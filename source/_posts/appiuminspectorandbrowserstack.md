---
title: Inspecting elements of an app using Appium Inspector and BrowserStack!
date: 2022-02-27 17:08:52
tags: [appium, Mobile Testing, Browserstack]
---

<p>&nbsp;</p>
{% asset_img cover_image.png Cover Image%}
<p>&nbsp;</p>

Appium is not a new name in the automation world and every automation engineer knows it well as it is widely used for automating android and iOS apps.

Before I begin writing about inspecting the elements of the app, let me first introduce you to appium.

## What is Appium?

[Appium][appium] is an open source test automation framework for use with native, hybrid and mobile web apps.
It drives iOS, Android, and Windows apps using the WebDriver protocol.

## Getting Started

Appium supports iOS as well as Android apps. To install appium and get started with using it there are some steps which you need to follow to set it up on different OS versions like Windows and macOS.

[Checkout this blog to learn how to setup appium on macOS][appium_mac_setup]
[Checkout this blog to learn how to setup appium on Windows][appium_win_setup]

## What is Appium Inspector?

[Appium Inspector][appium_inspector] is basically just an Appium client (like WebdriverIO, Appium’s Java client, Appium’s Python client, etc…) with a user interface. There’s an interface for specifying which Appium server to use, which capabilities to set, and then interacting with elements and other Appium commands once you’ve started a session.

## Download and Installation

You can download appium inspector from this link and as per the OS you have you can go ahead and install it.

> _Download [Appium-Inspector-windows-2022.2.1.exe][winexe] for windows and install it._ > _Download [Appium-Inspector-mac-2022.2.1.dmg][macdmg] for macOS and install it._

## What is BrowserStack?

[BrowserStack][browserstack_website] is a cloud Service Platform provides instant access to 3,000+ real mobile devices and browsers on a highly reliable cloud infrastructure that effortlessly scales as testing needs grow. With BrowserStack, Dev and QA teams can move fast while delivering an amazing experience for every customer.

## What do we need to do to inspect element of an app using BrowserStack?

### Problem Statement

Consider a scenario where you need to inspect elements of the app for automating it and you don’t have the device physically available with you! Unless you have the locators of the app available you cannot proceed with automation stuff.

This has happened with me a lot like I own an android device and its pretty easy for me to connect using that device and find out the locators and write the automation tests. But in case if I want to write the automation tests for an iOS app then I am not in a good position to start with as I don’t own an iphone device and very much I am blocked to start with testing and writing the automation tests for it.

### So what do we do?

In such a situation, the cloud services like BrowserStack comes to the rescue where we can take leverage of the physical devices on BrowserStack to actually inspect the locators.

### Uploading the app on BrowserStack

The first step is to upload the app to BrowserStack so we could use it for testing. For uploading the app, we need to hit an [API which is provided by BrowserStack][browserstack_upload_api]. Once you hit the API, you should get an `app_url` in response which can be updated in the _app path_ in appium desired capabilities and used further to start the Appium session on BrowserStack.

To upload the app you must be an **authorized user and have an active account on BrowserStack**,
as _username_ and _accesskey_ are required to upload the app.

I am using Postman to upload the app in this example.

<p>&nbsp;</p>
{% asset_img postman_api_upload.png App Upload using Postman%}
<p>&nbsp;</p>

## Appium Inspector

Start the Appium Inspector and **Select Cloud Provider >> BrowserStack**

<p>&nbsp;</p>
{% asset_img appium_inspector.png Appium Inspector app%}
<p>&nbsp;</p>

We need to provide the appium desired capabilities for starting the respective device with the app as per the requirement. In this example, I am running the app on _Samsung Galaxy S21 Plus_ on _Android Version 11.0_. Since I have already uploaded the app to BrowserStack, I would be using the same **app_url** in the _app path_ in appium desired capabilities. Since, this is a demo, I am using android phone, you can use any other device like iphone or any other Samsung device, on BrowserStack as per your need.

**App Details:** I am using android version of [Saucelabs demo app][saucedemoapp]
Here are the appium desired capabilities I have used:

```
{
  "platformName": "Android",
  "appium:platformVersion": "11.0",
  "appium:deviceName": "Samsung Galaxy S21 Plus",
  "appium:automationName": "UiAutomator2",
  "appium:app": <app_url recieved in response on uploading the app>
}
```

You are all set now to **Start** the Session and inspect the element! Click on Start Session button to start the session and inspect the elements.

<p>&nbsp;</p>
{% asset_img session_started.png Appium Inspector Session Started%}
<p>&nbsp;</p>

Lets inspect the username field and see what all locators we get

<p>&nbsp;</p>
{% asset_img inspecting_locator_username.png Inspecting locator for username field%}
<p>&nbsp;</p>

We see the _accessibility id_ as _test-username_ which can be used in the test for locating _username_ field.
Now, lets inspect the password field and login button to check for their respective locators.

<p>&nbsp;</p>
{% asset_img inspecting_locator_password.png Inspecting locator for password field%}
<p>&nbsp;</p>

<p>&nbsp;</p>
{% asset_img inspecting_locator_loginbtn.png Inspecting locator for login button%}
<p>&nbsp;</p>

We see the _accessibility id_ as _test-Password_ for _Password_ field and _test-LOGIN_ as _accessibility id_ for _Login_ button.

> _Though we also see xpath as a locator, however, its not recommended to use xpath locator and always ask the developers to add a specific test-id or accessibility id for locating elements, incase if there are none._

Now, using these locators we are now all set to write the automation test script for login scenario. Similarly, we can use this session for fetching locators for another fields within the app.

Hope, you got to know how to inspect the elements by running the app on BrowserStack.
Please comment below in case if you need any help or clarification, I would be happy to help!

Happy Testing!

<p>&nbsp;</p>

[appium]: https://appium.io/
[appium_mac_setup]: https://wasiqbhamla.github.io/website/blog/2017/04/24/appium-automation-ios
[appium_win_setup]: https://wasiqb.wordpress.com/2017/04/13/beginners-guide-to-appium-automation-with-java-for-android-apps-part-1/
[appium_inspector]: https://github.com/appium/appium-inspector
[winexe]: https://github.com/appium/appium-inspector/releases/download/v2022.2.1/Appium-Inspector-windows-2022.2.1.exe
[macdmg]: https://github.com/appium/appium-inspector/releases/download/v2022.2.1/Appium-Inspector-mac-2022.2.1.dmg
[browserstack_website]: https://www.browserstack.com/
[browserstack_upload_api]: https://www.browserstack.com/docs/app-automate/api-reference/appium/apps#upload-an-app
[saucedemoapp]: https://github.com/saucelabs/sample-app-mobile/releases
