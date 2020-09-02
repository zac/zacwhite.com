---
title: Detecting SwiftUI Previews
date: 2020-09-02T08:00:00-07:00
author: Zac White
layout: post
description: How to figure out if your View is being displayed in a SwiftUI preview
---

It shouldn't be often, but sometimes you need to know within a View or some code that you are running within the SwiftUI preview system. There isn't a built-in way to do this, so you have to infer this yourself. It's not too terribly difficult though, since Apple includes some process environment values that can clue you into the exact context in which your view is running.

To start, declare an `EnvironmentValues` extension which includes a read-only `isPreview` value. This peeks into the current process environment and looks for `XCODE_RUNNING_FOR_PREVIEWS`, which is set to "1" if Xcode is running your preview. As an optimization, you can optionally compile this code out if the project isn't in the `DEBUG` configuration. This saves checking the process info for a value that definitely isn't there.

```swift
public extension EnvironmentValues {
    var isPreview: Bool {
        #if DEBUG
        return ProcessInfo.processInfo.environment["XCODE_RUNNING_FOR_PREVIEWS"] == "1"
        #else
        return false
        #endif
    }
}
```

Then usage is pretty straightforward. Declare your `@Environment` var and use your new-found Bool to alter your view based on it being in preview mode.

```swift
struct MyView: View {
    @Environment(\.isPreview) var isPreview

    var body: some View {
        Group {
            if isPreview {
                Text("This is a preview!")
            } else {
                Text("This is not a preview!")
            }
        }
    }
}
```

Bonus tip: In figuring out what environment is available in a preview, I stumbled upon this [super useful trick](https://developer.apple.com/news/?id=8vkqn3ih) (directly from Apple!) that I somehow missed when it was shared. It's not often you should need to debug something happening in a preview, but now you can!