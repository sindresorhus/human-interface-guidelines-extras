# Human Interface Guidelines Extras

> Community additions to Apple's Human Interface Guidelines

Apple's [Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/) are amazingly well done. However, there are many details they don't clearly specify. This is an attempt to document the best conventions and patterns in the community.

**[Contribute →](contributing.md)**

## Contents

- [macOS](#macOS)
- [iOS](#ios)
- [watchOS](#watchos)
- [tvOS](#tvOS)

## macOS

### Don't remove default menu items

When you create a new macOS app project in Xcode, it includes a main menu with a lot of items. You might be tempted to remove stuff you don't need. For example, I have seen many apps remove “Undo” and “Redo” because their app doesn't support that. This is usually because they don't realize how the [responder chain](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/EventOverview/EventArchitecture/EventArchitecture.html) works. I made this mistake myself.

The reason you should not remove such menu items is that you cannot foresee what will need it. For example, if your app has a way to save a file, the filename text field in the save panel supports undo/redo and the “Undo” menu item works when the text field is key (focused). Another example, my [Black Out](https://sindresorhus.com/black-out) app has a “Quick Action” extension, and if the user runs it from Finder, the extension is able to use Finder's “Undo” and “Redo” menu items.

So what default menu items can be removed?

- The `Preferences` menu item in the app menu.
- All menu items in the `File` menu except for `Close`.
- `Find` and the below menu items in the `Edit` menu if your app doesn't include text editing.
- The `Format` menu.
- The toolbar and sidebar menu items in the `View` menu if your app doesn't need those.

### Drag and drop files

If your app has a drag and drop target for files, don't forget to also make it possible to click the drop area to open files through an open panel instead of dragging. Alternatively, add an “Open” button inside the drop target.

### “Always on top” functionality

<img width="391" src="https://user-images.githubusercontent.com/170270/96353866-5463ca00-10d0-11eb-96b9-28d52611c1d0.png">

You might want to let the user set your app's window to always be on top. You can find this behavior in the Simulator app.

The menu bar item that toggles this functionality should be named “Stay On Top” and should be in the “Window” menu below “Enter Full Screen”, or if it's not there, below “Tile Window to Right of Screen”.

The setting should be persisted in `UserDefaults`.

When enabled, the window should be set to `window.level = .floating`.

### Include an Internet Access Policy

If your app needs to access the internet for any reason, I would strongly recommend including a [Internet Access Policy](https://www.obdev.at/iap/index.html). This lets firewall apps present to the user what and why your app needs access to. It also makes it more likely the user will grant access.

[Example policy.](https://github.com/sindresorhus/Gifski/blob/main/Gifski/InternetAccessPolicy.json)

### Number text field convenience

If your app has a text field where the user can set a number, for example, for the width of something, make it convenient to increase/decrease the number.

1. Place a [stepper control](https://developer.apple.com/design/human-interface-guidelines/macos/selectors/steppers/) on the right side of the text field.
2. When the text field is focused, make it possible to use arrow up/down keys change the number by 1.
3. If the user also holds down the Option key, change the number by 10.
4. If the user reaches a lower or upper limit, shake the text field contents to indicate that.

You can find a real-world example of this in my [Gifski app](https://github.com/sindresorhus/Gifski) (missing the stepper though) ([source](https://github.com/sindresorhus/Gifski/blob/8ff31b387338b43f0c91cd481b3ebe0a4741b208/Gifski/IntTextField.swift)).

[Read more about text fields.](https://developer.apple.com/design/human-interface-guidelines/macos/fields-and-labels/text-fields/)

## iOS

### Settings sheet dismiss button

<img width="562" src="https://user-images.githubusercontent.com/170270/96354277-2c2a9a00-10d5-11eb-9354-85d0c1f953ea.png">

The most common conventions I have seen for a settings sheet dismissal button is either a “Done” button on the right side (primary position) of the navigation bar or an [“X” icon](https://twitter.com/sindresorhus/status/1317548489463762945/photo/2) on the left side (navigational position).

There are several benefits to using a “Done” button on the right side:

- Apple does that in most apps. You cannot go wrong following what Apple does.
- “Done” has a larger tap target.
- Friendlier. An “X” might make people think the settings will not be saved.
- Easier to reach for right-handed people, the majority.
- An informal [Twitter poll](https://twitter.com/sindresorhus/status/1317548882054742016) tells us that most users prefer this.

What you should definitely not do:

- “Done” button on the left side.
- “Cancel” button on either side.
- “Dismiss” button on either side.

<details>
<summary>Here's how you correctly do a settings sheet view in SwiftUI.</summary>

```swift
struct SettingsView: View {
	@Environment(\.presentationMode) private var presentationMode

	var body: some View {
		NavigationView {
			Form {
				Section(header: Text("Unicorn")) {
					// …
				}
			}
				.navigationTitle("Settings")
				.navigationBarTitleDisplayMode(.inline)
				.toolbar {
					// Note that it's `.confirmationAction` and not `.primaryAction`.
					ToolbarItem(placement: .confirmationAction) {
						Button("Done") {
							presentationMode.wrappedValue.dismiss()
						}
					}
				}
		}
	}
}
```
</details>

### Settings sheet title

People seem [undecided](https://twitter.com/sindresorhus/status/1317557474677882889) on whether to use a normal or large navigation bar title for a settings sheet.

[Discuss →](https://github.com/sindresorhus/human-interface-guidelines-extras/issues/1)

## watchOS

*Nothing yet*

## tvOS

*Nothing yet*
