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
	* `WFWorkflowIconStartColor`: A number representing color of the workflow’s icon, see below.
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

### Workflow Icon Start Color
A number representing the start color of the workflow’s icon gradient. As an example, converting the value 4282601983 reveals an 8-digit hex color in the format RRGGBBAA, FF4351FF.

This works for all colors tested, except for dark gray (255).

Shortcuts does not appear to support custom icon colours when a workflow edited on a Mac is opened on iOS – the colour will instead default to gray.

Colors available in Shortcuts and their respective values (liable to change):
* Red: 4282601983
* Dark Orange: 4251333119
* Orange: 4271458815
* Yellow: 4274264319
* Green: 4292093695
* Turquoise: 431817727
* Cyan: 1440408063
* Blue: 463140863
* Navy: 946986751
* Violet: 2071128575
* Purple: 3679049983
* Dark Gray: 255
* Pink: 3980825855
* Taupe: 3031607807
* Gray: 2846468607

### Importing with a  `WFWorkflowMinimumClientVersion` greater than the client version

In Shortcuts, importing a file with a `WFWorkflowMinimumClientVersion` that is greater than the client version will result in a warning dialog, shown below. The imported file can be duplicated or deleted, but not edited or executed.

![Shortcut Format Too New Dialog](Shortcut%20Format%20Too%20New%20Dialog.png)

**Shortcut Format Too New**

This shortcut cannot be opened because it was created on a newer version of the Shortcuts app.

Update Shortcuts [opens App Store] | OK

# URL Schemes
Most Shortcuts URL schemes remain the same as Workflows, and can be used interchangeably by replacing 'shortcut' with 'workflow' and vice-versa.

* Import Shortcut from URL: `shortcuts://import-shortcut/`
	* Query parameters:
		* name (optional name for Shortcut)
		* url (required file download URL)
	* Example: `shortcuts://import-shortcut/?name=Awesome%20Shortcut&url=https%3A%2F%2Fdownloadwebsite.com`

* Run Shortcut: `shortcuts://x-callback-url/run-shortcut`
	* Query parameters:
		* name (string name for Shortcut)
	* Example: `shortcuts://x-callback-url/run-shortcut/?name=Shortcut%20to%20Run`
	* Notes:
		* Shortcuts added to the homescreen tend to use the URL with the extra parameters `id` (the internal UUID for the Shortcut) and `source`, which is always `homescreen`.

# Misc Notes
* The last Workflow version was 1.7.8
* The first Shortcuts version was 2.0.0

# Contributing
Pull requests with new and updated information or suggestions are welcome!