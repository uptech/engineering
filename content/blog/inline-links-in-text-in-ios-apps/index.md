+++
title = "Inline Links in Text in iOS Apps"
date = 2018-10-19T09:32:01-08:00
updated = 2018-10-19T09:32:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
+++

Today, I was working on a client project. As usual a couple of  designers had done some high fidelity designs in [Sketch](https://www.sketchapp.com) that I was riffing off. In the designs on the Sign Up screen near the bottom there was the classic notice stating that by signing up you are agreeing to the **User Agreement** and **Privacy Policy**.

![User Agreement Notice](user_agreement_notice_screenshot.jpg)

There are two interesting things about this. First and foremost the phrases **User Agreement** and **Privacy Policy** are visually called out using blue like they would be if they were anchor tags (a.k.a. links) in a website. Secondly, both these phrases are inline with other text. If I was doing this on a website this would be trivial as I would just put two anchor tags inline in a paragraph tag with the rest of  the text. However, on iOS things aren’t so straight forward.

## Assumptions
My assumptions going into this were that I was going to have to somehow detect a tap on each of  those phrases and then present a new modal View Controller containing a web view that loads the **User Agreement** or **Privacy Policy** respectively.  I knew off hand how to do all of these things except for the detection of the phrases being tapped. Though, I did have enough knowledge to know that with some work it could be done.

## Research
I started out searching for “UILabel tappable text”. It led me to a variety of half baked attempts at solving this with code that seemed way more complicated then it should need to be in my mind. I am talking like 60+ line solutions. So, I kept digging and stumbled across a post referencing the existence of some sort of `NSAttributedString` link feature. I thought this was interesting but sadly the post did not give me much direction in terms of actually making it happens. So, I searched a bit more and found an example of applying the `NSAttributedString.Key.link` attribute to an `NSMutableAttributedString`. An example is as follows:

```swift
let attributedString = NSMutableAttributedString(string: “by signing up, you are agreeing to Float’s User Agreement and Privacy Policy.”)
attributedString.addAttribute(NSAttributeString.Key.link, value: “https://example.com/policy”, range: NSRange(location: 43, length: 14)
```

When I applied the above and then assigned it to the `UILabel` ‘s `attributedText` property I got excited because it resulted in the **User Agreement** phrase showing up as blue with an underline. This was close to what I wanted design wise and gave me hope that it might actually do a thing. Sadly, tapping on what I believed was a link had no impact. So, I dug further and tried a number of various properties on `UILabel` such as `isUserInteractionEnabled` but no luck. 

After searching a bit more I stumbled upon an article that referenced further some delegate methods from `UITextViewDelegate` that people were using with the `NSAttributedString.Key.link` to handle opening them in a browser. This gave me a new path to explore.

## UITextView
So, step one I converted my `UILabel` to a `UITextView`. When I initially did this the view was `UITextView` was not visible and inspecting the view I could see that there was layout constraint issues. So, I gave it a hard coded height using auto layout constraints. Magically, it was visible and the layout constraint issue went away. So, now I knew that it was not being sized based on it’s content. After a quick search I was able to find out it is because it has a `isScrollEnabled` property that is by default set to `true`. So, I flipped it to `false` and got rid of the explicit height constraint. Like magic it was there and visible just liked I wanted with the correct styling.

Next I attempted to tap the link. But, to my surprise the keyboard popped up. Of course that is because there is another property `isEditable` that is by default `true`. So, after flipping that to `false` when I tapped the link it actually opened Safari to the URL of the link.

Finally some success! At this point my `UITextView` was configured as follows:

```swift
textView.isEditable = false
textView.isScrollEnabled = false
textView.backgroundColor = UIColor.clear
```

## Breaking Assumptions
At this point it was actually functioning though in a different fashion than I had originally assumed it would. This made me question my initial assumptions of the need for having a separate view be presented modally containing a web view that would load the content.

The default behavior I was seeing is actually pretty much inline with those concepts. It is basically modally presenting Safari with the content loaded in Safari. My immediate next thought was well how easy is it for the user to get back to the app where they left off in either scenario. In the separate view scenario they would have to hit a close button to dismiss the modal view and go back to the Sign Up view. In the Safari scenario the user simply hits the back to last app button in the upper left.

So, turns out that the default behavior experience feels great, is minimal effort and models how most apps on macOS handle links as well. At this point I am pretty confident this the ideal path and I am driving full force ahead. 

## Another Problem
Turns out there is one problem. I have been localizing this application up to this point using `NSLocalizedString`. Looking at the example:

```swift
let attributedString = NSMutableAttributedString(string: “by signing up, you are agreeing to Float’s User Agreement and Privacy Policy.”)
attributedString.addAttribute(NSAttributeString.Key.link, value: “https://example.com/policy”, range: NSRange(location: 43, length: 14)
```

We are clearly coupling the link to a specific starting location and length within the string. So, sadly this breaks localization because different languages will have different starting locations and different lengths, and with some googling there is no built in way for localization to support `NSAttributedString`. 

## Solution
But, wait there is still hope. At least in my opinion the above linking strategy is by far the best solution I have been able to find. So, if your app does not need localization you can use it straight away. If on the other hand your app does need localization. Don’t worry I have an idea.

The thought is to actually use a subset of [Markdown](https://daringfireball.net/projects/markdown/syntax) in the localized string content and then write a method that will fetch a `NSLocalizedString` and parse the subset of [Markdown](https://daringfireball.net/projects/markdown/syntax) into an `NSAttributedString`. This would in theory resolve the localization issue and potentially give localization a decent bit more functionality in terms of representing text.

In fact this can trivially be done with existing Markdown parsing libraries such as [iwasrobbed/Down](https://github.com/iwasrobbed/Down). You could use it as follows to get your attributed string with links.

```swift
let down = Down(markdownString: NSLocalizedString(“USER_AGREEMENT_NOTICE”, comment: “User Agreement Notice”))
let attributedString = try? down.toAttributedString()
```

The only down side to this approach, at least with this Markdown library, is that the `NSAttributedString` it produces has the links styled with the `NSAttributedString.Key.link` attribute as well as the underline style. This makes it look different than the designs.  With a quick read of the [iwasrobbed/Down](https://github.com/iwasrobbed/Down) README.md and some spelunking of the [DownAttributedStringRenderable.swift](https://github.com/iwasrobbed/Down/blob/master/Source/Renderers/DownAttributedStringRenderable.swift) file I quickly discovered that the `toAttributedString()` method has a `toAttriubtedString(stylesheet:)` variant that allows passing a `String` of CSS in to control the styling. In my case this just meant that I need to remove the `text-decoration` on anchor tags.

I thought about making a library to support this. However, now that I have seen how trivial it is to implement I decided I would just implement a helper method to do this. The helper I now write looks as follows:

```swift
func localizedMarkdownString(_ key: String, comment: String) -> NSAttributedString? {
	let down = Down(markdownString: NSLocalizedString(key, comment: comment))
	return try? Down.toAttributedString(stylesheet: "a { text-decoration: none; }")
}
```

That’s it! Relish in the beauty of localized Markdown and `NSAttributedString` coming together so harmoniously to solve this problem.
