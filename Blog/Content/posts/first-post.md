---
date: 2024-09-27 10:00
description: A description of my first post.
tags: SwiftUI, Swift, Mobile, Apple
---

# Customizing Typography in SwiftUI

When building modern iOS apps, typography plays a critical role in creating a consistent and polished user experience.
SwiftUI provides a built-in set of text styles through the Font extension—such as `.largeTitle`, `.headline`,
`.caption`, and more. It might seem intuitive to extend Font to add your custom text styles from a design system like
this:

```swift
extension Font {
	static func customCallout() -> Font {
		Font.custom("YourCustomFontName", size: UIFont.preferredFont(forTextStyle: .callout).pointSize)
	}
}
```

Yet this approach can quickly become limiting and difficult to maintain, especially as your design system evolves.

Many modern branded design systems go beyond simple font size and weight adjustments. They include additional parameters
like line height and kerning. These finer typography details add another layer of complexity. If you examine the Text
Styles panel in Figma, you’ll notice that each style defines not only font size and weight but also properties like line
height. When for example line height isn’t set to “Auto” and is expressed as a specific value, this needs to be
accounted for when implementing your typography in SwiftUI.
