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

Manually setting these properties every time we apply a style can lead to repetitive and messy code. Instead of
extending Font for each style, it’s more efficient to centralize all typography settings—such as font size, weight, line
spacing, and kerning—into a custom SwiftUI modifier. This approach allows for easy reuse of your design system’s
typography styles across the app without cluttering the codebase.

Here’s an example of what the typography enum might look like:

```swift
public enum TypographyStyle {
	case subheadingS
	case bodyM
	case captionMStrong

	var font: Font {
		switch self {
		case .subheadingS: return .system(size: 16, weight: .semibold)
		case .bodyM: return .system(size: 15)
		case .captionMStrong: return .system(size: 11, weight: .medium)
		}
	}

	var lineSpacing: CGFloat? {
		switch self {
		case .subheadingS: return nil
		case .bodyM: return 1.27
		case .captionMStrong: return nil
		}
	}

	var kerning: CGFloat? {
		switch self {
		case .subheadingS: return nil
		case .bodyM: return 1.1
		case .captionMStrong: return nil
		}
	}
}
```

## Creating a SwiftUI Modifier for Typography

We can create a SwiftUI modifier that allows us to easily apply a custom typography style wherever needed. The modifier
would take care of applying not just the font, but also attributes like lineSpacing and kerning, all pulled from the
design system. This approach lets us make typography changes in one place without having to refactor dozens of text
views.

```swift
public struct Typography: ViewModifier {
	let style: TypographyStyle

	public init(style: TypographyStyle) {
		self.style = style
	}

	public func body(content: Content) -> some View {
		return content
		.font(style.font)
		.ifLet(style.lineSpacing) {
			$0.lineSpacing($1)
		}
		.ifLet(style.kerning) {
			$0.kerning($1)
		}
	}
}

extension View {
	public func typography(_ style: TypographyStyle) -> some View {
		self.modifier(Typography(style: style))
	}
}

```

## Wrapping up

By extending SwiftUI’s Font with custom styles from your design system and incorporating parameters like line spacing
and kerning, you can maintain a consistent and cohesive look throughout your app. Creating a reusable modifier for
typography helps eliminate redundancy, allowing you to apply design system-compliant text styles effortlessly.