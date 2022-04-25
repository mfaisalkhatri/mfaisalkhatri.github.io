---
title: Guide to Mobile Testing
date: 2022-03-31 15:30:21
tags: [API Testing, rest-assured, Test Automation, QA, Tutorial]
---

<p>&nbsp;</p>
{%asset_img cover_image.png Cover Image%}
<p>&nbsp;</p>

As everyone knows, Mobile industry has taken all over the world and is the fast emerging industry in terms of technology and business. It is possible to do all the tasks using Mobile phone for which earlier we had to use computer. Using Mobile phone, we can take photos, view movies, perform transactions, make and receive payments, talk and chat over apps, and the list goes on…

With the technology emerging so fast and the clients demanding the release very often it is necessary to test the application built for use on mobile devices to be tested thoroughly before it is released to end users.

## Quality is the Key

We need to check everything and all the possible permutations and combinations so we do not let any loopholes open. Since Mobile devices have personal data of the end users stored as well so its necessary to perform checks with respect to Security and Data Integrity.

Checking the performance of the app is equally important as today people are more interested in the speed of the app. Functionality is fine, but if the apps response time is too long, it might not attract the users. Hence, performance testing of the app is also an important factor to consider.

Coming to testing Mobile applications, I think we should figure out the Test Strategy first, as it will help us break down the testing phases and perform testing with quality and will help us avoid missing the tests.

<p>&nbsp;</p>
{%asset_img mobile_test_strategy.png Mobile Test Strategy%}
<p>&nbsp;</p>

## Defining a Test Strategy

Lets be clear about the application under test whether it is a Native Mobile app, Hybrid app or a Mobile Web app.

### What is a Native Mobile app?

A Native app is one that is installed directly onto the smartphone and can work, in most cases, with no internet connectivity depending on the nature of the app. Native apps are installed through an application store (such as Google Play or Apple’s App Store).

### What is Mobile Web app?

Mobile web apps are web apps optimized for a good phone experience. They aren’t mobile applications, but websites written in HTML/CSS and run by a browser. While they may be designed to resemble the feel of smartphone apps, they don’t have much in common.

### What is a Hybrid Mobile app?

The concept of the hybrid app is a mix of native and web-based apps. These are made to support web and native technologies across multiple platforms. Moreover, these apps are easier and faster to develop.

> _**Hybrid Mobile app = Native app + Mobile Web app.**_

For defining the test strategy in our case, lets consider the case of a hybrid mobile app.

Before I jump into the details of testing lets discuss some high level points related to the testing types which should be considered to test the mobile app.

<p>&nbsp;</p>
{%asset_img testing_types.png Testing Types%}
<p>&nbsp;</p>

## Testing Types:

In an ideal scenario, considering hybrid app, I think following testing types should be considered:

1. Functional Testing.
2. Performance Testing
3. Security Testing
4. Usability Testing
5. UI/UX Testing

> _**I have taken the testing types considering my experience with Mobile projects, you can add or subtract the types based on your requirements.**_

## When and How do I start testing?

Since we are in the age where agile is religiously followed in today’s software world, it is better to start testing as early as possible.

Testing should be done in every phase of software development lifecycle and not only when the feature is fully developed.

Being said that, always make sure that the developer is writing the Unit tests. Integration and Service layers tests should also be covered. Writing tests only doesn’t help, code coverage report should show that unit tests coverage are at least greater than 80% which could gradually be increased to 100%, if there is a possibility. Its good to have a pipeline in place which helps us monitoring the lifecycle easily and take corrective actions at every stage. So, Unless the build is green, its not good to move ahead with testing and make the required fixes as soon as possible.

<p>&nbsp;</p>
{%asset_img test_planning.png Test Planning%}
<p>&nbsp;</p>

## Test Plan

Its good to have a test plan in place, as it would be easier to keep a check on all the test activities, so we don’t miss anything and perform the testing smoothly and provide quality output.

> _**The first and most important case is whether user is able to install the app successfully using PlayStore/App Store.**_ _**Next, is to check if the app is opening correctly after the successful installation.**_

**Here is a general list of all the test activities we need to take care of while testing the mobile app:**

1. Checkout for the mobile OS platforms that are widely used in the region where the app is expected to be released.

2. Based on the result that is derived from Step 1 — Checkout which all are the popular mobile devices in that region and accordingly make a list of all the popular devices.

3. Out of the list generated in Step 2, take top 5 devices and consider them for testing purpose. Also, make sure you take the combination of big and small screen sizes so you could test out for the UI/UX as well. Testing different screen sizes is an important aspect as it helps with the usability of the app.l

4. Its important to consider the OS versions as well, so for example lets say for the region and OS that was considered previously in above Steps, iOS version 14 and 15 are the popular ones in that region. So, in that case make sure you are testing on iOS versions 14 and 15. One more important thing which most of the time gets missed is the minimum version support. So, in case your app supports iOS version 12 as minimum version, make sure you do a round of regression and sanity testing on iOS version 12 on big and small screen devices to keep a check on quality. As per my experience, around 1–2% of some nasty bugs comes from this area.

5. From the automation point of view, it would be good if you could run your tests in parallel on the selected devices and select some latest devices as per the criteria set in above steps. So, from regression point of view, all your end to end tests run in automation frequently and gives you an alarm in case if anything fails.

6. Specific tests which couldn’t be covered as a part of automation due to certain reasons, as we all know that not everything could be covered as a part of automation, that could be tested in manual exploratory tests.

### Some additional checks which you could do in the mobile app is to check:

1. While moving from one screen to another the same APIs doesn’t get called multiple times.

2. Debouncing could also be checked as it would eventually affect the performance of the app.
   For e.g. Doing a pull to refresh on a particular screen multiple times and checking of the request is sent every time a pull to refresh is done. It is expected here that though user does a pull to refresh multiple times, requests for refreshing the screen should only be sent at certain intervals and not every time. This will make sure that the performance of the app is not getting hit due to multiple requests within fraction of seconds.

3. Its good to keep an eye on the UI fonts/colors and text size as in mobile apps it matters a lot to the end users and even a sight change in the background color/font/font size will affect the overall usage of the app. So, checking for the uniformity of color combinations, fonts, font size etc is necessary to be tested.

4. I would suggest to involve the UI/UX folks in the testing session so they could fairly show us the issues/difference in case if there is any related to it.

5. Check out by running the app manually in 1–2 different devices to check for the functionality and performance of the app.

6. A check should also be kept on the logs getting generated for all the steps that are performed in the front-end. As it would help easy debugging of the issues in case if any arises in production.

7.Coming to logs, do check if any [personal info(PII)][personalinfo] is not getting logged in the logs as it would lead to legal consequences. 8. We can also check for the navigation of the app and the opening of the links within the web view, since its an hybrid app, we should expect that all the links within the app should get open in web view and not on the external browser.

9. Check out if the app is sending notifications correctly and those notifications are displayed correctly in the notifications area of the phone.

10. We can check if the notification messages are navigating user to the respective area in the app.

11. If the user is running some other app and notifications for this app appears in between, how does it look like? Whether it is user friendly, other this might be a pain area for end user which ultimately may lead to user uninstalling the app.

### Additional tests which you could perform for checking the overall quality of the app.

### Scenarios related to Mobile Battery usage:

1. Checking if the app is not consuming a lot of battery, as it would lead to the end user uninstalling the app. It is expected that App should be using minimum battery while working.

2. Check the battery usage of the app in the background. It should also be minimal.

### Scenarios related to Mobile Device left Idle:

1. If you have the login functionality implemented in the app, you can check if the app is not logging out the user after the app has been left idle by the user for some time and it goes into background.

2. If you have a payment related app and say for example which performing a payment if the mobile phone is left idle for some time then the app should function smoothly and perform the desired transaction successfully.

3. In case of mobile phone left idle as well, the App should not consume huge amount of battery and also we can check if the phone does not get heated, as it would lead to negative reviews and affect the overall business.

4. What happens when user keeps the app in background and tries to use it after a while from the background itself from the point where he left. This is purely a technical scenario which can be discussed with the team and accordingly implemented as per the technical requirement.

### Scenarios related to locking the phone:

1. We can also check for the phone lock scenario, like, for example, while fetching any data within the app, the app takes longer, and in meanwhile the users phone gets locked, then the app should remain active in background and perform the user desired transaction successfully.

2. Another scenario is, if user intentionally locks the phone during an ongoing transaction, in that case as well it should be expected that the transaction is completed successfully and does not get cancelled.

3. Another scenario to check would be if mobile phone is locked while using the app, again we can check for the battery usage which should be minimum.

### Scenarios related to internet connectivity/WiFi and Location services:

1. We can check that app does not break when in middle of some transaction when the internet on the phone goes off. Here, it is expected that appropriate screen with “No Internet” banner should be shown to the user.

2. Next, we can check when the internet is back and user is online once again, in that case, app should resume working correctly and “No Internet” banner should not be displayed.

3. User should be able to use the app while being connected to WiFi as well as mobile internet.

4. Another point to check would be by connecting the mobile phone to mobile hot-spot and checking if the app works smoothly, though this scenario need not be deep tested, however it is good to have this test performed.

5. We can check by suddenly turning on the Airplane mode and check the behavior of the app. If the app has some dependency like an app related to Smart Watch, the device should get disconnected without any error message and it should be smooth without crashing the app.

6. Similarly, by turning the Airplane mode Off, everything should be resuming back to normal with minimum user intervention required.

7. We can check by turning the Location services ON/OFF in the phone and checking if the app is able to discover that the location services are turned ON/OFF respectively, and showing appropriate permission message popup in the app.

### Conclusion

To conclude, all the things listed in this blog are from my personal experience and if you need you can add more tests as per your requirements. This document describes a way of how you can start testing and what all things you need to consider.

I have tried my best to cover all the aspects with regards to mobile testing, In case, if you need any help on any topic related to Mobile testing or automation please do reach out to me.

<p>&nbsp;</p>

### References:

1. https://www.toptal.com/android/developing-mobile-web-apps-when-why-and-how
2. https://www.app-press.com/blog/web-app-vs-native-app
3. https://en.wikipedia.org/wiki/Mobile_app

<p>&nbsp;</p>

[personalinfo]: https://www.imperva.com/learn/data-security/personally-identifiable-information-pii/
