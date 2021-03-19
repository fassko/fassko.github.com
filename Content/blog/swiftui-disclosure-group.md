---
date: 2021-03-05 00:00
title: How to show and hide content with DisclosureGroup using SwiftUI
description:
tags: swift, ios, swiftui, DisclosureGroup
---

Showing and hiding some parts of information is a very important feature in mobile apps. Taking into consideration that screens for our phones are much smaller than for the laptops or desktop computers.

Now with new SwiftUI capabilities we can collapse content with `DisclosureGroup`. Let's see how we could use it in various ways.

## Display a collapsable content

Let's start with the easiest way how to set up a collapsable view that we could show or hide. It comes with with a nice disclosure arrow indicator and animation.

During this blog post let's use an example showing weather conditions that would be a SwiftUI view `WeatherDetailsView` and would show temperature and wind information.

If we would like to show or hide that information we can use the `DisclosureGroup` initializer by just passing a string value and content which is out `WeatherDetailsView` view.

```swift
DisclosureGroup("Current Weather Details") {
	WeatherDetailsView()
}
```

![SwiftUI DisclosureGroup](/assets/disclosuregroup-swiftui/disclosuregroup-string-init.gif)

## Modify `DisclosureGroup`

Now let's check out how we can modify the `DisclosureGroup`. Currently we can't do much right now and it is pretty limited, but we can change the accent color and disable it.

Let's start with disabling the option to show and hide the weather information. We could do this by using the `disabled` modifier.

```swift
DisclosureGroup("Current Weather Details") {
	WeatherDetailsView()
}
.disabled(true)
```

![SwiftUI DisclosureGroup](/assets/disclosuregroup-swiftui/disclosuregroup-disabled.png)

By default the disclosure arrow comes in the blue color. By using the `accentColor` modifier we could change to our desired color. We could use a system color or a defined color in the Assets catalog.

![SwiftUI DisclosureGroup](/assets/disclosuregroup-swiftui/disclosuregroup-accent-color.png)

## Configure `DisclosureGroup` title

So far we have only changed the inner content of the disclosure group view. How about if would like to change the title to a label view. The new [Label](https://developer.apple.com/documentation/swiftui/label) comes with an initializer where we can pass the textual title and a system image, let's use that. For that we can use a special `DisclosureGroup` initializer providing custom label.

```swift
DisclosureGroup(
content: {
  WeatherDetailsView()
},
label: {
  Label("Current Weather Details", systemImage: "thermometer")
    .font(.headline)
}
)
```

![SwiftUI DisclosureGroup](/assets/disclosuregroup-swiftui/disclosuregroup-custom-label.png)

## Manually control show / hide state

Right now to either show or hide the content of the disclosure group we relied on the user to click on the arrow. There can be cases where we would like to change this behaviour using a toggle button. To to that we could use a state boolean variable that indicates either the disclosure group is expanded or isn't.

```swift
@State private var isExanded = false

/// ...

Toggle("Show Current Weather Details", isOn: $isExanded)

DisclosureGroup("Current Weather Details", isExpanded: $isExanded1) {
  WeatherDetailsView()
}
```

![SwiftUI DisclosureGroup](/assets/disclosuregroup-swiftui/disclosuregroup-isExpanded-toggle.gif)

One thing personally I think is quite weird is that once you press on the title it does not expand instead we need to point our finger and press on the disclosure arrow button. We could fix it by implementing the title as button and control the `isExpaned` property.

```swift
DisclosureGroup(
        isExpanded: $isExanded,
        content: { WeatherDetailsView() },
        label: {
          Button("Current Weather Details") {
            withAnimation {
              isExanded.toggle()
            }
          }
        }
      )
```

![SwiftUI DisclosureGroup](/assets/disclosuregroup-swiftui/disclosuregroup-isExpanded-label-button.gif)

## TL;DR

Showing and hiding some parts of information is a nice touch for the mobile applications because of limiting screen size.

New SwiftUI version gives provides us a component out of the box specifically for that - `DisclosureGroup`.

Currently it is quite limited, but offers some possibilities to customize it if we need to.

## Links

* [Sample code](https://github.com/fassko/swiftui-DisclosureGroup)

[Official documentation](https://developer.apple.com/documentation/swiftui/disclosuregroup)
* [How to hide and reveal content using DisclosureGroup](https://www.hackingwithswift.com/quick-start/swiftui/how-to-hide-and-reveal-content-using-disclosuregroup)
* [SwiftUI DisclosureGroup Tutorial](https://www.ioscreator.com/tutorials/swiftui-disclosure-group-tutorial)
* [SwiftUI’s GroupBox, OutlineGroup, and DisclosureGroup in iOS 14](https://betterprogramming.pub/swiftuis-groupbox-outlinegroup-and-disclosuregroup-in-ios-14-cf9fb127cdc0)
