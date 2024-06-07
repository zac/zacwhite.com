---
title: Ideal Cross-Platform Return Key Behavior with SwiftUI's TextField
date: 2024-06-06-12:00:00-0700
author: Zac White
layout: post
description: Getting ideal return key behavior for a multi-line TextField for chat message input in SwiftUI on macOS Catalyst
---

I've recently been working on a cross-platform chat application for macOS and iOS written in SwiftUI. One of the challenges I ran into was getting the return key to behave as expected on all platforms in a multi-line `TextField`. The behavior I wanted was for the return key (↩) to send the message and for option return (⌥↩) to insert a newline. This is the behavior of a chat application like iMessage, and it's what I wanted to replicate.

I started with something like this:

```swift
struct EntryField: View {
    let titleKey: LocalizedStringKey
    @Binding var text: String

    let onSend: (String) -> Void

    var body: some View {
        TextField(titleKey, text: _text, axis: .vertical)
            .onSubmit {
                onSend(text)
            }
    }
}
```

If using an AppKit macOS application, you get the desired return key behavior for free. You also get it with iOS / iPadOS and a connected hardware keyboard. However, when using a Catalyst macOS app target (at least on Sonoma), the return key always inserts a return character and never triggers the `onSubmit` closure. 🤔

So as a workaround, you can add an invisible `Button` that responds to the return and performs the passed in closure.

```swift
TextField(titleKey, text: _text, axis: .vertical)
    .onSubmit {
        onSend(text)
    }
    .background {
        Button {
            onSend(text)
        } label: {
            EmptyView()
        }
        .keyboardShortcut(.defaultAction)
        .opacity(0)
    }
```

Now the field behaves as expected on all platforms. The return key sends the message, and option return inserts a newline. This is a simple solution (hack?) that works well for my little app, but if you have a better solution I'd love to hear it!