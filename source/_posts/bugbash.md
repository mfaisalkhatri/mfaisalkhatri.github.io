---
title: What is a Bug Bash?
date: 2021-10-02 00:08:52
tags: [Testing, Release Planning, Bug bash]
---


{% asset_img BugBash.png Bug Bash Poster%}  
<p>&nbsp;</p>


## What is a Bug bash?

In software development, a bug bash is a procedure where all the developers, testers, program managers, usability researchers, designers, documentation folks, and even sometimes marketing people, put aside their regular day-to-day duties and "pound on the product" — that is, each exercises the product in every way they can think of. Because each person will use the product in slightly different (or very different) ways, and the product is getting a great deal of use in a short amount of time, this approach may reveal bugs relatively quickly. _[Wikipedia]_ 

## What is the Objective behind organising a Bug bash?

As the name and definition already mentions, it is bug bash, so the major objective is to find bugs hidden in the application and resolve them before it reaches the end users of the product. However, The Developers, Business Analyst, Project Managers and the Quality Analysts should all be on the same page and there should not be any blame game once bug is found as this is a collective team effort to make the application more stable and robust by providing faster feedback, so as to release a quality product in the market.

## Why should you conduct a Bug bash?

- Most importantly, Bug bashes are a relatively cheaper way to find a large number of bugs in a short span.
- Bug bashes provide an opportunity for everyone in the organization to learn about the different parts of the product they are less familiar with.
- It improves cross-team collaborations, communication, and relationships.
- Helps testing the product with wide variety and combination of devices/browsers/mobile versions/mobile OS's which in general is very difficult to test in a short span.
- People with different experiences in the team can collaborate and also with different perspective product can be tested effectively.

## Who facilitates the Bug bash?

Ideally, it is the Tester or the Test Lead who should facililate this.

## When to conduct a Bug bash?

It should be conducted before a major release or if there is any crtitical release which may impact the overall working of the product, Bug bashes are advised. The time to schedule may vary according to the collective decision made by the team. Normally, it should be conducted before a week of release or even sooner. A point to note here is that all the cards/tickets that are tagged for the release should be QA Done or either Dev Done before the bug bash is scheduled. It doesnt make any sense to have a bug bash if the feature that is going to be released is half baked.

## How to run a Bug bash?

### Pre Bug bash

- Define the facilitator for the Bug bash, it is ideal if 2 QAs could pair and lead this.
- The owners of the Bug bash should setup a preparation meeting with the team and explain the agenda of Bug bash to all the participants and also to setup the pre-requisite, if any. If any team member requires any access related to product/application, this could also possibly be figured out in preparation call. 
- It would be an added advantage, if a representative from client side could join the Bug bash. It would help in terms of business requirement.
- Send out a Calendar Invite for Bug bash to all the Participants and ask them to RSVP to it, so you could plan out the event successfully.
Following points needs to be considered while sending the Calendar invite:
    - Mention the Scope of the bugbash.
    - Place where Bug bash is scheduled to happen. Mention the meeting room details, else if it is a Zoom/Teams Call, update the link in it.
    - Mention the details about the test environment and also test data which can be used for testing.
    - Attach the link to the Bug bash sheet which has all details related to pre-requisite, OS/Browser/Tools setup/Description about features of the product.
    - If it is a mobile app, do share the link from where the latest build should be downloaded for ios as well as android.
    - Check if all the participants have access to the Bug bash sheet as well the required links to download the artifacts(in case of mobile app)/ links to the website under test.

### Bug bash Session

It should an hour session, but could be increased to 90 minutes depending upon the requirement. It all depends on how well it works for you and your team.

- Facilitator should start by welcoming everyone to the session and explaining them the scope and details about Bug bash and ask them to start with it.
- Once initiated, facilitator should monitor the participants activities by checking if they could perform the testing  without any blocker. If someone is blocked, he should help them in resolving the queries.
- Continuous Monitoring of the Bug bash sheet where issues are recorded should be done.
- It should be thoroughly checked that participants are adding the bug details correctly, with proper Test Steps, Screenshots, Device/OS/Browser details and also their respective name, else it would be difficult to reproduce the same once Bug bash is completed.
- Keep an eye on the time as well, and once the decided time is reached. A Call out should be done by the facilitator if someone needs more time to perform tests. Accordingly the session should be extended, if required. Facilitator should Thank everyone for their participation and giving their valuable time for Bug bash.

### Post Bug bash Session

This is the most crucial and most important session that needs to be set up. Most importantly, in this session, the Business Analyst and QAs should *prioritize* the issues reported.

This session doesn't require whole team to be present. Business Analysts and QA can come up and analyze the issues reported.
They might also need to reproduce the issues and update the steps, if any, in the sheet. 

It should also be noted that all observations reported in the bug bash may not be a bug, so appropriate clarifications may be required to be taken from the reporter as what is their perspective in reporting the respective observation, once that is done and understood, accordingly action would be taken to mark it as *Not a Bug*. 

Once the Priorities are defined for the bugs reported, Tickets/Cards should be created in the Sprint board labelling them as Bugbash bugs. The ones with Higher Priority should be taken in the current sprint and resolved at the earliest before the release.
Accordingly, tickets/cards should be prepared for the lower priority issues as well and should be placed in the later sprints.

## Bug bash template

A sample template for Bug bash has been added in this [github repository][github_link] which might help you in Bug bashing!!

You can find **"Bugbash_Template.xlsx"** inside **"Templates"** folder.

## Conclusion

To conclude, Bug bash is great way to do exploratory testing. It brings in people with different experiences from different teams. It has also a variety of Device/OS/Browser coverage in a short span of time which might help in uncovering the hidden issues. Having someone participating from the client side would help us in getting faster feedback before releasing the product to end user. 

I have tried to explain Bug bash as per my experience in this blog, however, if you feel if I am missing something or you need to getting in depth understanding about some areas please feel free to ping me. I would be Happy to help!

Happy Bug bashing!!
<p>&nbsp;</p>


[Wikipedia]: https://en.wikipedia.org/wiki/Bug_bash
[github_link]:  https://github.com/mfaisalkhatri/Manual_Testing/
