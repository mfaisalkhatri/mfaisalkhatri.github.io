---
title: What is API testing?
date: 2020-08-08 11:24:08
tags: [API Testing, RestAssured, OKHttp, Test Automation, QA, Tutorial, Github] 
---


{% asset_img APITesting.png API Testing %}  
<p>&nbsp;</p>

### What is an API?

An API is an application programming interface. It is a set of rules that allow programs to talk to each other. 
The developer creates the API on the server and allows the client to talk to it.

### What are the different HTTP methods?

Following are the different HTTP Methods:
- GET
- POST
- PUT
- PATCH
- DELETE

The two most common HTTP methods are: GET and POST.

### GET Method

The GET method is used to fetch data from the specified resource.
It is the most commonly used HTTP method.

### POST Method

POST is used to send data to a server to create/update a resource.
It is also the most commonly used HTTP method.

### PUT Method

PUT is used to update a resource.
The main difference between a POST and a PUT request is that POST is used to create a resource while PUT is used to update a resource. Calling a POST repeatedly will create duplicate resources while calling PUT multiple times will always produce the same result.

### DELETE Method

The DELETE method deletes the specified resource.


### How to test APIs?

Believe me, it is not a tough task to test an API. You need to understand what the respective API does, test validation of the input fields while making a request and verify that you are getting the expected data in the response. You can also check the data integrity between the APIs by creating contracts between APIs. 

An example would be, if you delete something using the DELETE API, you can test by fetching the same record using GET API and expect that no records are returned in the response.
Similarly, You can check that the data is posted correctly when you HIT the POST/PUT APIs, likewise correct data is generated in response for the GET APIs. 

### Sample Test cases for API Testing

API testing these days, has gathered a lot of attention and most of the Testers find it difficult to make it to what to test in an API, so to give an outline and help with basic cases to test, I have prepared some sample test cases and have uploaded it on Github.

**[Checkout the sample cases here.][github_link]**  

You can find **"TestCases_for_ApiTesting.xlsx"** inside **"Templates"** folder.<p>&nbsp;</p>

[github_link]: https://github.com/mfaisalkhatri/Manual_Testing/