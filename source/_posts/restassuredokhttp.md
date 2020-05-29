---
title: API Testing using RestAssured and OkHttp.
date: 2020-05-29 13:06:31
tags: [API Testing, RestAssured, OKHttp, Test Automation, QA, Tutorial, Github] 
---

## What is REST-Assured?

REST Assured is a Java library that provides a domain-specific language (DSL) for writing powerful, maintainable tests for RESTful APIs.

## What is OkHttp?

OkHTTP is an open source project designed to be an efficient HTTP client:
- HTTP/2 support allows all requests to the same host to share a socket.
- Connection pooling reduces request latency (if HTTP/2 isn’t available).
- Transparent GZIP shrinks download sizes.
- Response caching avoids the network completely for repeat requests. 

## How to write API Tests using REST-Assured/ OkHttp?

As many of you know that REST-Assured and OkHttp are currently in demand for writing automation tests for APIs. I was learning about Rest-Assured and OkHttp, an idea just popped into my mind, why not save the code on github so it would serve as a learning material for beginners.

Hence, I created and posted the code which I wrote, on github, in the following repository. It has API testing example codes for GET, POST, PUT, PATCH and DELETE requests using REST-Assured as well as OkHttp:

[OkHttpRestAssuredExamples][githubrepo]

## REST-Assured or OkHttp?

Interesting question! While I was running the tests, I found an interesting thing which caught my attention. The execution speed of OkHttp when compared to Rest-assured was far better. 


**_The following image shows the Test execution time which a Get Request took:_**

{% asset_img GetTests.png Results of Get Request Tests %}

_“2.002 seconds”_ was taken by OkHttp, however for executing the same API test using REST-assured, it took _“3.131 seconds”._ Similarly for running the next test, OkHttp took “0.124 seconds”, and REST-assured took _“0.372 seconds”._

Now, lets jump to execution time taken for **Post** Request.


**_The following image shows the Test execution time which a Post Request took._**

{% asset_img PostTests.png Results of Post Request Tests %}

_“2.252 seconds”_ was taken by OkHttp, however for executing the same API test using REST-assured, it took _“4.521 seconds”._ Similarly for running the next test, OkHttp took “0.649 seconds”, and REST-assured took _“0.72 seconds”._

Going further, lets check execution time taken for **Put** Request.


**_The following image shows the Test execution time which a Put Request took._**

{% asset_img PutTests.png Results of Put Request Tests %}

_“5.226 seconds”_ was taken by OkHttp, however for executing the same API test using REST-assured, it took _“8.008 seconds”._ Similarly for running the next test, OkHttp took “0.555 seconds”, and REST-assured took _“0.77 seconds”._

Lets have a look at the **Patch** requests execution time.


**_The following image shows the Test execution time which a Patch Request took._**

{% asset_img PatchTests.png Results of Put Request Tests %}

_“2.171 seconds”_ was taken by OkHttp, however for executing the same API test using REST-assured, it took _“4.268 seconds”_. Similarly for running the next test, OkHttp took “0.623 seconds”, and REST-assured took _“0.683 seconds”._

Lets have a look at the **Delete** requests execution time.


**_The following image shows the Test execution time which a Delete Request took._**

{% asset_img DeleteTests.png Results of Put Request Tests %}


_“0.718 seconds”_ was taken by OkHttp, however for executing the same API test using REST-assured, it took _“7.353 seconds”_.


Considering the facts written above, I can say that OkHttp is faster and better in terms of execution than REST-assured.

However, in terms of writing the tests, I enjoyed writing the tests using REST-Assured as it has the method chaining feature which allows to write tests efficiently and in one go. However, that is not the case with OkHttp. 

**You can check the github link I have mentioned above to check how to write tests using REST-Assured and OkHttp.**

To conclude, I would mention that it all depends on the automation framework you choose for your test project, in my view, you should choose the one which you feel implementing and maintaining is easy. 

_For more details about OkHttp [click here][Okhttpdocs]_
_For more details about RestAssured  [click here][RestAssureddocs]_

[githubrepo]: https://github.com/mfaisalkhatri/OkHttpRestAssuredExamples
[Okhttpdocs]:https://square.github.io/okhttp/
[RestAssureddocs]: http://rest-assured.io/