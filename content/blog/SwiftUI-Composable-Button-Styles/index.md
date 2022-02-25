+++
title = "SwiftUI: Composable Button Styles"
date = 2021-11-08T06:00:01-08:00
updated = 2021-11-08T06:00:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
thumbnail = "/img/posts/thumbnails/logo-swiftui.png"
+++

One frustrating/annoying things about SwiftUI Buttons for me has been the
mechanism for styling them. In order to get information about the state of the
button (e.g. if it is pressed, not pressed) you have to build a `struct`
that implements the `ButtonStyle` protocol. This would generally look something
like the following.

```swift
struct MyColoredButtonStyle: ButtonStyle {
	let pressedBackgroundColor: Color = ...
	let notPressedBackgroundColor: Color = ...
	
	func makeBody(configuration: Configuration) -> some View {
		return configuration.label.background(configuration.isPressed ? pressedBackgroundColor : notPressedBackgroundColor)
	}
}
```

Then to use it you simply do the following:

```swift
Button(...).buttonStyle(MyColoredButtonStyle())
```

## The Problem

This is great and all until you decide that you also want your button to have
an animated scale effect tied to the `isPressed` state as well. You might think
you could just create a new `MyAnimatedScaleButtonStyle` struct and tack it on
like so.

```swift
Button(...)
	.buttonStyle(MyColoredButtonStyle())
	.buttonStyle(MyAnimatedScaleButtonStyle())
```

Sadly SwiftUI only takes the first button style in the chain and applies
it. The rest are ignored. This means you basically have to create a super
unique button style for each particular use case and have all your styling
slammed into it.

## A Solution (A Micro Framework)

Taking from Functional Programming we can effectively use Function Composition
to facilitate making a tiny lightweight framework to facilitate building
composable button styles.

```swift
typealias ButtonStyleClosure<A: View, B: View> = (ButtonStyleConfiguration, A) -> B

precedencegroup ForwardComposition {
	associativity: left
}

infix operator >>>: ForwardComposition

func >>> <A: View, B: View, C: View>(
    _ f: @escaping ButtonStyleClosure<A, B>,
    _ g: @escaping ButtonStyleClosure<B, C>
) -> ButtonStyleClosure<A, C> {
    return { configuration, a in
        g(configuration, f(configuration, a))
    }
}

struct ComposableButtonStyle<B: View>: ButtonStyle {
    let buttonStyleClosure: ButtonStyleClosure<ButtonStyleConfiguration.Label, B>

    init(_ buttonStyleClosure: @escaping ButtonStyleClosure<ButtonStyleConfiguration.Label, B>) {
        self.buttonStyleClosure = buttonStyleClosure
    }

    func makeBody(configuration: Configuration) -> some View {
        return buttonStyleClosure(configuration, configuration.label)
    }
}

extension Button {
    func composableStyle<B: View>(_ buttonStyleClosure: @escaping ButtonStyleClosure<ButtonStyleConfiguration.Label, B>) -> some View {
        return self.buttonStyle(ComposableButtonStyle(buttonStyleClosure))
    }
}
```

This allows us to define custom `ButtonStyleClosure` functions. The follow are
just a few examples. But the idea is that you can create whatever of these
`ButtonStyleClosure`s you want and then use Function Composition to combine
them.

```swift
struct ButtonStateColors {
    let pressed: Color
    let notPressed: Color
}

func scaledButtonStyle<A: View>(_ configuration: ButtonStyleConfiguration, _ view: A) -> some View {
    return view
        .scaleEffect(configuration.isPressed ? 0.98 : 1)
        .animation(.spring(response: 0.2, dampingFraction: 0.5, blendDuration: 0.5))
}

func roundedButtonStyle<A: View>(_ configuration: ButtonStyleConfiguration, _ view: A) -> some View {
    return view.cornerRadius(8)
}

func defaultPaddingButtonStyle<A: View>(_ configuration: ButtonStyleConfiguration, _ view: A) -> some View {
    return view.padding()
}

func coloredButtonStyle<A: View>(_ configuration: ButtonStyleConfiguration, _ view: A) -> some View {
    let backgroundColors = ButtonStateColors(
        pressed: Color("cta_button_highlight_color"),
        notPressed: Color("cta_button_color"))
    let foregroundColors = ButtonStateColors(
        pressed: Color("cta_button_text_color"),
        notPressed: Color("cta_button_text_color"))

    return view
        .background(configuration.isPressed ? backgroundColors.pressed : backgroundColors.notPressed)
        .foregroundColor(configuration.isPressed ? foregroundColors.pressed : foregroundColors.notPressed)
}
```

Allowing us to simply use these with our Function Composition operator we
defined in the framework as follows:

```swift
Button(...)
	.composableButtonStyle(defaultPaddingButtonStyle >>> coloredButtonStyle >>> roundedButtonStyle >>> scaledButtonStyle)
```

## Conclusion

This gives us lots of reuse and facilitates making more generic button styles
while allowing us to use them throughout our app rather than having to make
unique button styles for each case.

It does have a limitation though. It would be ideal if we could create
higher level functions by nesting closures. Which would give us even more
power. For example we could create a more general
`foregroundColorButtonStyle(colors:)` function like the following.

```swift
func foregroundColorButtonStyle<A: View>(_ colors: ButtonStateColors) -> (ButtonStyleConfiguration, A) -> some View {
	return { configuration, view in
		view.foregroundColor(configuration.isPressed ? colors.pressed : colors.notPressed)
	}
}
```

In theory this would enable us to be able to be even more composible. However,
it turns out Swift 5.4.1 doesn't support `some View` in closures. So the above
sadly won't work. I briefly tried to come up with a nice workaround but wasn't
able to get anything nice to work.

Despite this one limitation it is still far superior in my mind compared to
what SwiftUI provides out of the box.

