---
title: Gauge and Taiko Automation Frameworks
date: 2020-05-31 12:40:36
tags: [Test Automation, QA, Tutorial, Github, JavaScript]
---

Hello, 
In this blog, I will be discussing abut **Gauge** and **Taiko** Open source automation JavaScript frameworks.

I was trying my hands on with these tools, and found that it is very easy to write and maintain tests using it. Lets dive in and learn more about these frameworks. 

**You can find the example code I created using Gauge+Taiko on github by clicking [here][githublink].**

  
# What is Gauge?  
{%asset_img gauge_img.jpeg Gauge%}  
Gauge is a free and open source test automation framework that takes the pain out of acceptance testing. It has been developed by ThoughtWorks.
Gauge helps you write the test specification in BDD manner and is quite easy to learn.
It has other helpful features like generating reports, Screenshots, Parallel execution, etc.

**Some of the key features of Gauge that makes it unique include:**

- Simple, flexible and rich syntax based on Markdown.
- Consistent cross platform/language support for writing test code.
- A modular architecture with plugins support.
- Extensible through plugins and hackable.
- Supports data driven execution and external data sources.
- Helps you create maintainable test suites.
- Great support for VS Code.

_For more details about Gauge Framework, [click here][GaugeWebsite]_

# What is Taiko?  
{%asset_img taiko_img.jpg Taiko%}  
Taiko is a free and open source browser automation tool built by the team behind Gauge by ThoughtWorks. 
It is a node library with a clear and concise API to automate the chrome browser. 

Taiko uses the Chrome DevTools API and is built ground up to test modern web applications.
Taiko is just a charm, it requires less programming knowledge and is quick and easy to learn.

The most interesting part of Taiko was its smart locators, you don't have to dig into the DOM of the page to search for locators, its locates the elements smartly.
Using Taiko alone, you can quickly write tests and run the smoke test round easily.  

**The only drawback I found in Taiko was that it does not support any other browser other than Chrome**. 

_For more details about Taiko Framework, [click here][TaikoWebsite]_

Overall, Gauge and Taiko are nice Open Source automation tools, you should try your hands on!!
To conclude, as mentioned in the official website of Gauge,  
Gauge + Taiko = Reliable browser automation for your JavaScript tests!

[githublink]: https://github.com/mfaisalkhatri/GaugeTaikoExample
[GaugeWebsite]: https://gauge.org/
[TaikoWebSite]: https://taiko.dev
