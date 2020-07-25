---
title: Testing - Breaking the Myth
date: 2020-07-25 09:31:45
tags: [Software Testing, QA, Tutorial, Testing]
---

<p>&nbsp;</p>
As I was thinking about a new topic to post a blog upon, a thought struck my mind about sharing the professional experience about my work that would be helpful to the beginners in the QA industry.

Let me tell you all that I am into Software Testing for the last 12+ years, and I have worked with both Automation as well as Manual Testing teams. Domains are Financial Market, E-Commerce, Healthcare and Payments.

When I say, **"Testing"**, what comes to your mind?? 
It sounds like verifying a scientific liquid, with some lab formulas, like, H2so4, blah blah.. (I am not much familiar with science and its things, lol ). But, this isn't the thing we would be talking about here, in this blog.
<p>&nbsp;</p>

{% asset_img Testing_world.png Testing World %}

### Software Testing is basically Verification and Validation.

1. Verification is the process of evaluating the work-products of a development phase to determine whether they meet the specified requirements. verification ensures that the product is built according to the requirements and design specifications. [Click Here to read more on verification][Verification]

2. Validation means actually executing the tests and checking whether it is working as per the business requirement raised by the client. It is checking whether the product meets the customer's needs. [Click here to read more on validation][Validation]

The first thing that the tester should have a look at, is the requirement document. 
He should analyze it thoroughly, understand what the client needs, does it sound right for the application Or would that fit into the current application that's already in place. The Tester should always raise his concern if he finds something in the requirement which would hamper the existing functionality and might lead to chaos after it is being released. Minor things like the message text, window title should also be thoroughly analyzed. Since the requirement document is the first step of the development phase it should be well documented and clearly understood by everyone in the team.

The reason I am emphasizing more on the requirement document is, If you ask about my experience, I many a time have come across the case that development was thought from another angle of what was written in the requirement document, and then came the need to revisit the software changes which lead to delay in software being released and also an equal pressure on the team to make up for the time loss and deliver it as early as possible.

### If you ask me, What Testing is?
I would say, Testing means to verify and check the software is bug-free and that it can be shipped to the client. 
But you see, I missed something in the above phrase, can you identify it?? 
If you are an experienced tester, you may have judged it, yes, I am talking about "Quality". 
No, a question comes to the mind, 
**What is Quality??** 
Five major views according to (Kitchenham and Pfleeger, 1996; Pfleeger et al., 2002) are:
transcendental, user, manufacturing, product, and value-based views, as outlined below:

- **_In the transcendental view_**, "Quality is hard to define or describe in abstract terms, but can be recognized if it is present. It is generally associated with some intangible properties that delight users."**

- **_In the manufacturing view_**, "Quality is confirmation to the Standards."**

- **_In user's view_**, "Quality is fitness for purpose or meeting user's need."**

- **_In the product view_**, "The focus is on the inherent characteristics in the product itself in the hope that controlling these internal quality indicators will result in improved external product behavior."*

- **_In value-based view_**, "Quality is the customer's willingness to pay for a software."**

### How do I Test?
While testing something always keep in mind the three basic rules.

I can say, if you thoroughly practice this, you would be at least at par in testing the software.
<p>&nbsp;</p>

{% asset_img Test_sign.jpg Test %}

**Rule No.1:** Analyse the requirement, measure the changes done with the Business/Sales Team.
**Rule No.2:** Never ask the requirement to the developers, especially, Don't ask them what is to be tested!
**Rule No.3:** Use Input--> Process --> Output basics to test the software.

### Elaborating Rule No.1
I mean to say that you should always pressurize the Business Team to prepare required documents so that tracking the business cases and functionality would be an easier task. Also, demand for use cases.

### Talking about Rule No. 2.
You should never ask the developers what you need to test!! You should ask this question to the Business Analyst, who can assist you with business and use cases. To some extent, the Development team can be approached to get knowledge upon technical things, but as a Black-box tester, you don't need to know what is going inside the code. All you need to check is the functionality of the software. But having knowledge about technical things would be an added advantage. If you are into automation testing, you will have to understand the technical flow of the application and accordingly design your test suite for automated testing. Use cases would help here to design the functional and regression tests. Adding more, you can also have an idea about which part of the application would require performance testing.

### Rule No.3 is all about checking the functionality. 
1. Input here means to enter the data into the system. This input could be through any of master records window, for example, client master, address master, account master, etc.. Or it could be one of the windows for data entry part like, if you take financial markets, Trade Entry window, would be a good example here.

2. Process means to Run the Processes/calculations of the data input in the system. Processes here means running the calculation process within the application. In terms of Financial markets, it would be Margin calculation/ Payin-Payout calculation.

3. And finally, Output is to verify the processed data of the software through reports. This is straight forward, it refers to retrieving the data you entered through the master window, is correctly displayed in the reports after the calculation process is run. For example, you need to check that the margins are correctly calculated based on the inputs you provided to the application. 

### How to be a Good Tester ?
A good tester is required to test that application is NOT doing what it is NOT supposed to do. I mean to say, regression testing! Always, perform rounds of Regression testing on the changed software, many a time it happens that the development team and Business team works too hard to develop the functionality, but they ignore the existing working part of the system, hence regression is always useful! Automated regression is always suggested as it saves lots of time and effort.

The need for an Independent Test team arises to verify that the requirement raised by the clients is correctly implemented in the software. Hence, just testing the software is not enough, quality is equally important to be delivered.

Coming to the end, I would advise that, upgrading yourself with the latest market trends will help you survive in the all-time changing economy. If you don't upgrade yourself, you would be left behind in this always competing world!!

[Verification]: https://www.tutorialspoint.com/software_testing_dictionary/verification_testing.htm
[Validation]: https://www.tutorialspoint.com/software_testing_dictionary/validation_testing.htm
[** -  _Software Quality Engineering - Testing, Quality Assurance and Quantifiable Improvement - Jeff Tian_]