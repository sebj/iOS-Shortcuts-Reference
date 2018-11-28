# Shortcuts File Format
Shortcuts are exported from the iOS 12 Shortcuts app with a `.shortcuts` extension. Workflows exported from the Workflows app use a `.wflow` extension, but share the same internal format.

## File Structure
The format of a shortcut and a workflow file are the same – the file format does not seem to have changed going from the Workflow app to Apple’s iOS 12 Shortcuts app. Many strings remain prefixed with `WFWorkflow`.

The file is in a binary property list format. It’s possible to open it by transferring to a Mac and changing the file extension to `.plist`, where it can then be viewed in Xcode or other supported apps.

* `WFWorkflowClientVersion`: A number representing the version of the client app used to create the workflow (e.g. 700).
* `WFWorkflowClientRelease`: A string representing the release semantic version of the client app used to create the workflow (e.g. “1.7.8” or “2.0.0”)
* `WFWorkflowMinimumClientVersion`: An optional string representing the minimum client app version required to run the workflow (e.g. 411)
* `WFWorkflowName`: An optional string name for the workflow. If this is not present, an imported workflow’s filename will be used
* `WFWorkflowIcon`: An object containing:
	* `WFWorkflowIconStartColor`: A number representing color of the shortcut's icon, see below.
	* `WFWorkflowIconImageData`: Data
	* `WFWorkflowIconGlyphNumber`: A number
* `WFWorkflowInputContentItemClasses`: An array of strings, representing the types of input content accepted
	* `WFAppStoreAppContentItem`
	* `WFArticleContentItem`
	* `WFContactContentItem`
	* `WFDateContentItem`
	* `WFEmailAddressContentItem`
	* `WFGenericFileContentItem`
	* `WFImageContentItem`
	* `WFiTunesProductContentItem`
	* `WFLocationContentItem`
	* `WFDCMapsLinkContentItem`
	* `WFAVAssetContentItem`
	* `WFPDFContentItem`
	* `WFPhoneNumberContentItem`
	* `WFRichTextContentItem`
	* `WFSafariWebPageContentItem`
	* `WFStringContentItem`
	* `WFURLContentItem`
* `WFWorkflowTypes`:  An array of strings, representing the available usage types of the workflow, one or more of
	* `ActionExtension` (“Show in Share Sheet”
	* `NCWidget`  (“Show in [Notification Center] Widget”)
	* `WatchKit` (watch availability, supported in the old Workflow app & Shortcuts during some iOS 12 betas)
* `WFWorkflowActions`: Array of actions, each in the format:
	* `WFWorkflowActionIdentifier`: An action identifier string, formatted in reverse domain name notation, e.g. `is.workflow.actions.address`
	* `WFWorkflowActionParameters`: Array of parameters [String | Number | Object], each an identifier and a value 

### Shortcut Icon Start Color
The start color of the shortcut's icon gradient is stored as an RGBA-8 number. As an example, converting the stored value `4282601983` reveals the 8-digit hex color `FF4351FF` in the format `RRGGBBAA`.

Shortcuts does not appear to support custom icon colours when a shortcut edited on a Mac is opened on iOS – the colour will instead default to gray.

Colors available in Shortcuts and their respective values (liable to change):
* Red: `0xFF4351FF` / `4282601983`
* Dark Orange: `0xFD6631FF` / `4251333119`
* Orange: `0xFE9949FF` / `4271458815`
* Yellow: `0xFEC418FF` / `4274264319`
* Green: `0xFFD426FF` / `4292093695`
* Teal: `0x19BD03FF` / `431817727`
* Light Blue: `0x55DAE1FF` / `1440408063`
* Blue: `0x1B9AF7FF` / `463140863`
* Dark Blue: `0x3871DEFF` / `946986751`
* Violet: `0x7B72E9FF` / `2071128575`
* Purple: `0xDB49D8FF` / `3679049983`
* Dark Gray: `0x000000FF` / `255`
* Pink: `0xED4694FF` / `3980825855`
* Taupe: `0xB4B2A9FF` / `3031607807`
* Gray: `0xA9A9A9FF` / `2846468607`

### Importing with a  `WFWorkflowMinimumClientVersion` greater than the client version

In Shortcuts, importing a file with a `WFWorkflowMinimumClientVersion` that is greater than the client version will result in a warning dialog, shown below. The imported file can be duplicated or deleted, but not edited or executed.

![Shortcut Format Too New Dialog](Shortcut%20Format%20Too%20New%20Dialog.png)

**Shortcut Format Too New**

This shortcut cannot be opened because it was created on a newer version of the Shortcuts app.

Update Shortcuts [opens App Store] | OK

# Key URL Schemes
Most Shortcuts URL schemes remain the same as Workflows, and can be used interchangeably by replacing 'shortcut' with 'workflow' and vice-versa.

See the ["Use URL Schemes" section, under Advanced Shortcuts in Apple's Shortcuts User Guide](https://support.apple.com/en-gb/guide/shortcuts/about-url-schemes-apd621a1ad7a/ios) for more.

* Import Shortcut from URL: `shortcuts://import-shortcut/?url=[url]&name=[name]`
	* Parameters:
		* url: file download URL
		* name (optional): name for shortcut, defaults to shortcut filename
		* silent (optional): `true` to import without opening the shortcut, `false` by default to open and display the shortcut to the user
	* Example: `shortcuts://import-shortcut/?name=Awesome%20Shortcut&url=https%3A%2F%2Fdownloadwebsite.com`
	* 

* Run Shortcut: `shortcuts://run-shortcut`
	* Query parameters:
		* name: string name for shortcut
		* input (optional): initial input into the shortcut, a text string or the word `clipboard` to use the contents of the clipboard
	* Example: `shortcuts://run-shortcut/?name=Shortcut%20to%20Run`
	* Notes:
		* Shortcuts added to the homescreen tend to use the URL with the extra parameters `id` (the internal UUID for the Shortcut) and `source`, which is always `homescreen`.

* Open Shortcuts:
	* `shortcuts://` to launch app to last-used state
	* `shortcuts://create-shortcut` to create a new shortcut
	* `shortcuts://open-shortcut?name=[name]` to open the app to the shortcut of a given name

# Misc Notes
* The last Workflow version was 1.7.8
* The first Shortcuts version was 2.0.0

# Contributing
Pull requests with new and updated information or suggestions are welcome!