---
title: ScrollView Content Offsets with SwiftUI
date: 2019-09-30T14:00:00-07:00
author: Zac White
layout: post
description: How to get a content offset from a ScrollView in SwiftUI
---

Scroll views are an incredibly important view type for displaying content on all the Apple platforms. Unfortunately, `ScrollView` in SwiftUI has some pretty glaring and difficult to work around limitations. Some of the pretty important features lacking out of the box are:
1. Offset change callback (via `UIScrollViewDelegate.scrollViewDidScroll(_ scrollView: UIScrollView)` in UIKit)
1. Scroll end point control for pagination (via `UIScrollViewDelegate.scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>)` in UIKit)
1. Animation / setting of offset (via `UIScrollView.setContentOffset(_ contentOffset: CGPoint, animated: Bool)` in UIKit)

Luckily, for #1 we can use `GeometryReader` and our own container view to achieve the same view containment behavior as `ScrollView`, but add a `Binder<CGPoint>` which can give us the offset.

To begin, we start with all the same API available to `ScrollView`:

```swift
public struct OffsetScrollView<Content>: View where Content : View {

    /// The content of the scroll view.
    public var content: Content

    /// The scrollable axes.
    ///
    /// The default is `.vertical`.
    public var axes: Axis.Set

    /// If true, the scroll view may indicate the scrollable component of
    /// the content offset, in a way suitable for the platform.
    ///
    /// The default is `true`.
    public var showsIndicators: Bool

    ...
}
```

But we want to add a `Binder<CGPoint>` to the mix and an initializer that initializes all our properties and a private `@State` to hold the initial offset:

```swift
/// The initial offset of the view as measured in the global frame
@State private var initialOffset: CGPoint?

/// The offset of the scroll view updated as the scroll view scrolls
@Binding public var offset: CGPoint

public init(_ axes: Axis.Set = .vertical, showsIndicators: Bool = true, offset: Binding<CGPoint> = .constant(.zero), @ViewBuilder content: () -> Content) {
    self.axes = axes
    self.showsIndicators = showsIndicators
    self._offset = offset
    self.content = content()
}
```

Now in the `body` property, we'll want to return a normal `ScrollView`, but add our own view above the provided `content` that can utilize `GeometryReader` to get a callback for position. By passing the frame origin of this empty, 0x0 view into our `Binder<CGPoint>` we can get a callback for geometry changes that will allow us to update the binding and subsequently any listeners to the binding:

```swift
public var body: some View {
    ScrollView(axes, showsIndicators: showsIndicators) {
        VStack(alignment: .leading, spacing: 0) {
            GeometryReader { geometry in
                Run {
                    let globalOrigin = geometry.frame(in: .global).origin
                    self.initialOffset = self.initialOffset ?? globalOrigin
                    let initialOffset = (self.initialOffset ?? .zero)
                    let offset = CGPoint(x: globalOrigin.x - initialOffset.x, y: globalOrigin.y - initialOffset.y)
                    self.offset.wrappedValue = offset
                }
            }.frame(width: 0, height: 0)

            content
        }
    }
}
```

You'll notice a `Run` 'view' which is pretty simple and lets us just execute a bit of code without affecting the layout:

```swift
struct Run: View {
    let block: () -> Void

    var body: some View {
        DispatchQueue.main.async(execute: block)
        return AnyView(EmptyView())
    }
}
```

Putting it all together, we now have a way to access the offset of the `ScrollView` and update a `@State` wrapped property. This can be used within other Views in our hierarchy to achieve some interesting effects.

In this watchOS example, we can use it to achieve an effect similar to the built-in News app where as you scroll the content, a header recedes by changing opacity and position based on the scroll offset.

This kind of behavior wasn't possible on previous versions of watchOS, so this serves as a perfect example of why watchOS + SwiftUI is an awesome combo! 🎉

<video height="auto" controls="controls" preload="auto" onclick="this.play()">
  <source src="/assets/images/corgi-news-watch.mp4" type="video/mp4">
</video>

<hr />

If you'd like to mess around with the watchOS project demonstrating this, [here's the zip][1]

### *Update 10/28/19*
Thanks to [@dmcgloin](https://twitter.com/dmcgloin) on Twitter for pointing out that Xcode 11.2 beta 2 was throwing a helpful warning that the original code was "Modifying state during view update, this will cause undefined behavior." One workaround was to kick the closure inside the `Run` view to run asynchronously. The above inline code and the sample zip have been updated with this workaround.

[1]:{{ site.url }}/files/Headlines.zip
