---
title: Templates in Swift with ExpressibleByStringInterpolation
date: 2019-05-13T14:00:00-07:00
author: Zac White
layout: post
description: How to build better type-safe strings
---

In Swift, we're always striving for type-safety. The less we can 'stringly' type the better. When it comes to situations where we need to define a string that inserts various elements, the canonical example being a URL path, there have been plenty of approaches in the past. In Objective-C, you might see something like this:

{% highlight Swift %}
Template *template = [[Template alloc] initWithTemplateString:@"/users/:username:/:id:"];

NSString *fullString = [template fill:@[
    @"usernane": "zac",
    @"id": 42
]];

// now fullString should be: "/users/zac/42"

{% endhighlight %}

What's wrong with that? Well, first of all the identifiers in the template string have to match the strings in the dictionary. If one is wrong, then the other won't be inserted and you'll get a runtime issue. In fact, I snuck in "usernane" above instead of "username" and I bet you didn't notice!

The other issue that can come up is what kinds of values are supported? If you are a particularly lazy developer, you might define the parameters of the fill method as being: `NSDictionary<NSString *, id>`. And if someone passes in an unsupported type, just ignore it I guess?

### We can do better with Swift.

What if we utilize [`ExpressibleByStringInterpolation`](https://developer.apple.com/documentation/swift/expressiblebystringinterpolation) and [key paths](https://developer.apple.com/documentation/swift/keypath) to create something that doesn't have the above issues?

{% highlight Swift %}

struct Values {
  let username: String
  let id: Int
}

let template: Template<Values> = "/users/\(keyPath: \.username)/\(keyPath: \.id)"
let fullString = template.fill(with: Values(username: "zac", id: 42))

{% endhighlight %}

So now we have something that shows exactly what value should go where in the string. And more importantly, those values are strongly typed to be something that can definitely be inserted into the template. If I mistype "usernane" again, I'll get a compiler error.

There are a couple down-sides though:

1. It requires a definition of a type to 'contain' the inserted values so the key paths have something to reference.
2. Because the interpolation syntax in Swift uses `\()` and the key paths use `\.` there are a lot of backslashes. Couple that with our current use-case of a path which has several forward slashes and visually parsing the string can be difficult.

(1) doesn't bother me too much, but (2) makes the string a little tough to read. I'm not sure what would be better, but another approach is to use overloading to use "+" as a concatenation operator. So you'd end up with something like this: `"/users/" + \.username + "/" + \.id`. Is this better? 🤷‍♂️

### How to build it?

We first need to control the type being passed to us to fill values in the template, but in a way that allows developers to extend support for custom types in the future. We can accomplish this with a protocol:

{% highlight Swift %}
public protocol TemplateInsertable {
    var stringValue: String { get }
}

extension String: TemplateInsertable {
    public var stringValue: String { return self }
}

extension Int: TemplateInsertable {
    public var stringValue: String { return "\(self)" }
}
{% endhighlight %}

Next we'll need the `Template` type which can handle 'appending' `TemplateInsertable`s or can append key paths to `TemplateInsertable`s. We have to store these components in a way that allows us to go through and construct the final template string with different passed in values. Below is one way to accomplish that where we keep a string internally of all the strings that have been appended to the template. And when a key path is appended, the future index in that string where the value will be inserted is stored along with the key path itself.

This lets us enumerate through those key paths and insert the string values into the positions which were defined in the interpolation.

{% highlight Swift %}
public struct Template<T> {

    private var template: String = ""
    private var keyPaths: [(offset: String.IndexDistance, keyPath: PartialKeyPath<T>)] = []

    mutating func append<U: TemplateInsertable>(keyPath: KeyPath<T, U>) {
        keyPaths.append((offset: template.count, keyPath: keyPath))
    }

    mutating func append(string: TemplateInsertable) {
        template.append(string.stringValue)
    }

    public func fill(with value: T) -> String {
        var fullString = template
        var indexOffset: String.IndexDistance = 0
        for item in keyPaths {
            let stringValue = (value[keyPath: item.keyPath] as! TemplateInsertable).stringValue
            let insertionIndex = fullString.index(fullString.startIndex, offsetBy: item.offset + indexOffset)
            fullString.insert(contentsOf: stringValue, at: insertionIndex)
            indexOffset += stringValue.count
        }

        return fullString
    }
}
{% endhighlight %}

Now comes the `ExpressibleByStringInterpolation` piece. The protocol requires declaring a type conforming to `StringInterpolationProtocol` which will be called by the standard library based on the components in the order required to 'build up' the full instance of your type. An example from [the docs](https://developer.apple.com/documentation/swift/stringinterpolationprotocol) explains that if you have `("The time is \(time)." as MyString)` in your source, the compiled code will be similar to:

{% highlight Swift %}
var interpolation = MyString.StringInterpolation(literalCapacity: 13, 
                                                 interpolationCount: 1)

interpolation.appendLiteral("The time is ")
interpolation.appendInterpolation(time)
interpolation.appendLiteral(".")

MyString(stringInterpolation: interpolation)
{% endhighlight %}

If we implement `StringInterpolationProtocol`, we can call our internal `append()` functions. Note the type of the appendInterpolation function. We get to define the number of parameters, the parameter name and importantly for us, the type constraints on the parameters. Now we can be sure that all elements in our string interpolation will definitely be from our generic type `T` and be pointing to a value of type `U` which conforms to `TemplateInsertable`.

{% highlight Swift %}
extension Template: ExpressibleByStringInterpolation {

    public init(stringInterpolation: StringInterpolation) {
        self = stringInterpolation.template
    }

    public struct StringInterpolation: StringInterpolationProtocol {

        fileprivate var template: Template<T>

        public init(literalCapacity: Int, interpolationCount: Int) {
            template = Template<T>()
        }

        mutating public func appendLiteral(_ literal: String) {
            template.append(string: literal)
        }

        mutating public func appendInterpolation<U: TemplateInsertable>(keyPath: KeyPath<T, U>) {
            template.append(keyPath: keyPath)
        }
    }
}
{% endhighlight %}

### Conclusion

`ExpressibleByStringInterpolation` is a powerful way to build some interesting functionality out of syntax that appears like it's part of the language itself. Apple is going to be allowing more hooks like this in the future. Along similar lines, [Property Delegates](https://github.com/apple/swift-evolution/blob/master/proposals/0258-property-delegates.md) is another possible upcoming feature of Swift which could allow developers to define how properties store their values.

Type-safety can be tricky and require jumping through some hoops. Fortunately, they are hoops you jump through once when creating interfaces for your future self or other developers to use. Then, no more hoops. Just auto-completable, typo-proof code.

If you'd like to mess around with the Playground, [here's the zip][1]

[1]:{{ site.url }}/files/Template.playground.zip