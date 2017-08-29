# README for the Developer Gadgets App for Splunk

## Table of Contents

- Overview
    - About
    - Release Notes
    - Support
    - License
- Installation and Configuration
    - Requirements
    - Installation
    - Configuration
- User Guide
    - Concepts
    - Usage
    - Upgrading

---

## Overview

The Developer Gadgets App App for Splunk is a library to extend Splunk dashboards with tools and features to help dashboard developers.

Here is a summary of the features in this app:

- Accordion Panel (a.k.a. Toggle Panel) library
- Dashboard Slider library
    - Token Debugger
    - Input Hiding for all inputs
    - Permanent Submit Button
- CSS Improvements
    - Footer Cleanup
    - Form Input Width Settings
    - Remove Splunk "Hide Filters" link

### About

Author: Ryan Thibodeaux<br/>
Version: 1.0.0<br/>
License: BSD 3-clause "New" or "Revised" License<br/>
Folder Name: developer_gadgets<br/>


| Splunk Components | Added?|
| --- | --- |
| Indexes | FALSE |
| Inputs | FALSE |
| Summarization | FALSE |
| Scripts | FALSE |
| Binaries | FALSE |
| Data Models / Sets | FALSE |
| Saved Searches | FALSE |


### Release Notes

#### v1.0.0

Initial release with support for token debugging, dashboard slider, and custom CSS.

### Support

Additional information and previous releases can be found on the [Github page](https://github.com/thibodux/developer_gadgets).

### License

This app and the author's content are released under the BSD 3-Clause "New" or "Revised" License. 

#### Third-party Software

The Dashboard Slider and Toggle Panel libraries are under copyright of OctoInisght Inc. (previously included in the Layer8Insight App for Splunk), and was authored by this app's author, Ryan Thibodeaux.

The token debugger component of this app comes from a public Splunk app (Splunk 6.x Dashboard Examples App for Splunk) that does not have any copyright or licensing associated with it. An inquiry has been made to Splunk about the licensing of this component and others in their public apps since there are some inconsistencies compared to other public apps they release under the MIT license.

-----

## Installation and Configuration

### Building

Skip this if you downloaded the app from Splunkbase.

Assuming you are checking out the source from Github, here are the steps to build the app tarball to then install in Splunk (assumes Linux or Mac). Note, make sure you keep the top-level repo folder name as "developer_gadgets" or you will potentially build a poorly structured application package.

```
$ cd <ROOT_DIRECTORY_OF_YOUR_CHOICE>
$ git clone https://github.com/thibodux/developer_gadgets developer_gadgets
$ tar c --exclude='.git' --exclude='.gitignore' developer_gadgets > developer_gadgets.tar
$ gzip -q developer_gadgets.tar
```

### Software requirements

The included demo / example dashboards require Splunk 6.3 or newer

The included software library in the app requires Splunk 6.2 or newer.

The [Splunk Enterprise system requirements](https://docs.splunk.com/Documentation/Splunk/latest/Installation/Systemrequirements) apply.

### Installation

This app can be installed using the Splunk GUI, CLI, or Deployer. 

This app is intended to extend Splunk dashboards, so it should be installed on Splunk Search Head components, such as:

* Stand-alone Splunk Enterprise servers
* Splunk Search-Head servers in distributed or clustered environments

This app should NOT be installed on Indexers, Heavy Forwarders, or Universal Forwarders.

### Configuration

No extra steps are required in order to make the app / library function properly following installation.

-----

## User Guide

### Concepts

This app provides a convenient way to extend Splunk dashboards with UX improvements and development tools.

### Usage

See the Demo dashboard in the app for more details about using this app's features.

#### Accordion Panel

The Accordion Panel library is designed to apply the feature to existing Splunk panels, i.e., you don't need to instantiate a new class of view.

To turn a SimpleXML panel into an accordion panel, give the panel an HTML ID that includes the string `_togglepanel_`.

By default, the accordion panel does not alter the visibility of the panel's   content. To hide or collapse the contents at dashboard loading, you need to use the HTML ID string `_togglepanel_true`. 

If you are using the `TogglePanel` class in JavaScript, you convert an existing panel into an Accordion panel using the following template where    the boolean value indicates if you want to initially hide the contents: `new TogglePanel("<PANEL_ID>").setup(boolean)`.

#### Dashboard Slider

The Dashboard Slider library creates the "Gadgets" widget you should see in the bottom right of the screen.

Click on the title or triangle icon to trigger the Dashboard Slider.

The Dashboard Slider includes three components for developers and users.

1. "Enable Debug" toggle
1. "Hide Inputs" toggle
1. Submit button

These components are extensions to existing SimpleXML and do not change any core functions.

**Debugger:** When set the "Enable Debug" checkbox will define a token named "show_debug", and it reveals the Token Debugger from the Splunk 6.x Dashboard Examples App for Splunk.

**Hide Inputs:** When set the "Hide Inputs" checkbox will define a token named "hide_inputs",
and it hides all `<fieldset>` elements in the Splunk dashboard, i.e., all form inputs at the top of the dashboard and inside panels will be made invisible.

**Submit Button:** The Submit Button in the Gadget Dashboard Slider emulates the Submit button often found at the top of the dashboard. It has no other side effects.

#### CSS Improvements

This app includes some CSS settings that help developers with little things.

All of these are defined in this app's load.css file.

**Footer Cleanup:** Removes the left-hand footer from Splunk dashboards that includes Spunk-specific Support links.

**Form Input Width Settings:** Changes CSS settings for Splunk dropdowns, text boxes, and multi-select boxes to make it easier to allow developers to override their default width with a single CSS change to the `width` of the top-level form input. 

**Remove Splunk "Hide Filters" link:** Obfuscates the "Hide Filters" link that appears in Splunk dashboards staring in Splunk 6.5.

#### Loading the Libraries

If you want to use the custom CSS or JavaScript libraries from this app in your Splunk dashboards, you must include the top-level app files `load.css` and `load.js`, respectively. Note, these files are independent, i.e., you don't have to include both.

In the case of SimpleXML, update the top-level XML to pull in the files, e.g.,

`<dashboard stylesheet="developer_gadgets:load.css" script="developer_gadgets:load.js">` 

OR 

`<form stylesheet="developer_gadgets:load.css" script="developer_gadgets:load.js">` 

For HTML dashboards, you need to add a the following element before the closing `</body>` tag, 

`<script src="{{SPLUNKWEB_URL_PREFIX}}/static/app/developer_gadgets/load.js" type="text/javascript"></script>`,

and this should go before the closing `/head` tag,

`<link rel="stylesheet" type="text/css" media="all" href="{{SPLUNKWEB_URL_PREFIX}}/static/app/developer_gadgets/load.css">`

#### Copying the libraries

If you want to use the libraries in the `/appserver/static/components/` folder in your own app (i.e., you just want to use the code without installing this Splunk app), you must copy the contents of `/appserver/static/components/` into your app's `/appserver/static/components/` folder. You must then copy the contents of the included file `/appserver/static/load.js` to setup the RequireJS paths to suit your app's needs. Naming conventions outside of this app's library do not matter - you must update the paths accordingly.

### Upgrading

Upgrade the app by installing new versions over previous installations. Restarts may or may not be required depending on what new files / features have been added. Splunk should indicate if a restart is required.
