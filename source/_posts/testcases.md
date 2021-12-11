---
title: Generic Test Cases for Testing
date: 2020-07-11 10:22:57
tags: [Test Cases, QA, Tutorial, Github, Testing]
---
<p>&nbsp;</p>

Recently I was reviewing the test cases of one of my colleague who has joined as a fresher in testing team. Though he had tried his best to cover up all the test cases as per the functionality and WireFrame provided in the Business Requirement Specification, I found that cases written were not descriptive to be understood in one go. I had to ask him multiple times what he meant to say in Case 1, then in Case 3 and so on...<p>&nbsp;</p>

{% asset_img Testing.jpg Testing %}  

A thought came to my mind that why should I not create some sample test cases which could help beginners giving a fair idea about writing test cases descriptively. This could help them in writing test cases effectively considering what all things are necessary while drafting test cases to test an application.  

### To begin with, lets first understand what does a Test case mean?  

IEEE Standard 610 (1990) defines test case as follows:
    “(1) A set of test inputs, execution conditions, and expected results developed for a
    particular objective, such as to exercise a particular program path or to verify compliance
    with a specific requirement.
    “(2) (IEEE Std 829-1983) Documentation specifying inputs, predicted results, and a set
    of execution conditions for a test item.”  

Its basically what you are testing, with what input, what you expect in return and finally recording the actual outcome of the test.<p>&nbsp;</p>

{% asset_img fail_pass.jpg TestCase %}  

### How to write a good test case?  

 A tester should always create a test case considering end user in mind. An effective test case is the one which can uncover defect, its not necessary that you can find bugs only in a complex test case. A Bug could be hidden in the title of the page, you just need an effective test case and a tester's eye to uncover it.  

Always mention the Steps in the test case, it makes the developers work easier.  

Always see to it that you perform one check in a test case. Your test case should not be pointing to two different things. If required, split the test case in two, rather than summing up all in one.  

Your cases should be self explanatory so the person who views your test cases shouldn't feel the need to contact you for making him understand what you have written.  

Try and write independent and small test cases which can be reused later.  

### Sample Test Cases  

{% asset_img helping.jpg Help %}  

Adding to the above, I have created a repository in Github, and have added sample generic test cases for field level validation, e.g. Text boxes, checkbox, Dropdown box, Multi dropdown box, Date field, etc.., that can be used for testing.

**Checkout the sample cases [here.][github_link]**  
You can find **"Generic_Field_Validation_Testcases.xlsx"** inside **"Templates"** folder.<p>&nbsp;</p>

[github_link]: https://github.com/mfaisalkhatri/Manual_Testing/