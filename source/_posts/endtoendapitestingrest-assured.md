---
title: End to End API Testing using Rest-Assured
date: 2022-03-14 20:20:39
tags: [API Testing, rest-assured, Test Automation, QA, Tutorial]
---

{% asset_img coverimage.png Cover Image %}

<p>&nbsp;</p>

I am a huge fan of rest-assured framework and have been using it since past couple of years. My current project has the demand for Mobile Automation and hence I didn’t get much chance to work on API testing lately. So, as a part of revising the already known things I thought I should practice writing some API tests using rest-assured and make it public so others too, who want to learn more about API testing and rest-assured will get a chance to know more about it.

## Testing APIs

### What is API Testing?

> _I have created this [repository on github][manualtestcasesrepo] which has sample test cases explaining how to test API, checkout the file named [“TestCases_for_ApiTesting.xlsx”][apitestcases] from the repository_.

## What is rest-assured?

[REST-Assured][rest-assured] is a Java library that provides a domain-specific language (DSL) for writing powerful, maintainable tests for RESTful APIs. One thing I really like about rest-assured is its BDD style of writing tests and one can read the tests very comfortably like whats going on inside and how the tests will proceed.

## Test Automation Strategy

Let me tell you the details about the APIs we are going to automate and test automation strategy as we need to be very much clear what all things we are going to automate and what ROI(Return on Investment) it will provide to us.

Following are the list of APIs on the [restful-booker][restfulbooker] website which I have used to write end to end tests.

1. [Create Booking][create-booking]
2. [Get Booking][get-booking]
3. [Update Booking][update-booking]
4. [Partial Update Booking][partial-update-booking]
5. [Delete Booking][delete-booking]

For sending the Update, Partial Update and Delete Booking requests it requires an AUTH token which is generated using the [CreateToken][create-token] API.

Since its testing end to end scenarios, hence, I planned the following test automation strategy to test the APIs so it could be tested in an efficient way:

1. Create a Booking, all test data required to be sent in the body of the request will be picked from a separate `pojo` which will build the data for post request.
2. Assert the response of Create Booking request with the data posted using the **Step 1**.
3. Save the _Booking Id_ from create booking response in a variable so it could be used in further tests.
4. Get Booking details using the _Booking Id_ in **Step 2** and compare with the create booking response ideally from the data posted using **Step 1**.
5. Create a **token generation method** so it could be reused in Put, Patch and Delete requests.
6. Update all the booking details using the _Booking Id_ in **Step 2** and _AUTH token_ in **Step 4**. Generate a separate booking data using the booking data builder as discussed in **Step 1**.
7. Assert the response of update booking.
8. Update the `firstname` and `totalprice` using the Patch request.
9. Assert the response returned from Patch request using the updated data that was supplied. Here, only `firstname` and `totalprice` will be asserted using the **partial update data builder** and the remaining values in the response will be asserted using the data built in **Step 6**.
10. Delete the booking and assert the status code.
11. Get the _Booking Id_ which is deleted(`bookingId` from **Step 2**), here it is expected to send **status code 404** since the booking is already deleted.
    This step confirms that booking got successfully deleted from the system.

Lets get into the code now and see how the implementation of the above steps can be done!!

## Getting Started

I have created this project using [Maven][mavenwebsite]. [TestNG][testngwebsite] is used as test runner. Once the project is created we need to add the dependency for rest-assured in `pom.xml` file.

<p>&nbsp;</p>
{% asset_img 1_restassured_dependency.png Rest Assured Dependency %}
<p>&nbsp;</p>

I have used [Lombok][lombok] in this project which is a java library that automatically plugs into your editor and build tools, spicing up your java. Using Lombok, you can save time by writing less code and just putting up annotations like **@Getter @Setter** instead of writing the getters and setters for a **pojo** class. You can add Lombok plugin in intellij, add its dependency in `pom.xml` and you are all set to start using it in your code. My main intention of using Lombok was to get the extensive leverage of **@Builder** annotation which I have used for building the test data.

For test data generation on run time I have used Java Faker which helps generating fake data on run time.
Here are the dependencies I have added in `pom.xml` for _Java Faker_ and _Lombok_.

<p>&nbsp;</p>
{% asset_img 2_javafaker_lombok_dpndcy.png Java Faker and Lombok Dependency %}
<p>&nbsp;</p>

## End to End Tests

### Configuration

<p>&nbsp;</p>
{% asset_img 3_baseclasssetup.png Base Class Setup %}
<p>&nbsp;</p>

The first thing I did was creating a base setup class which has all the configuration settings for running the tests. Things like BaseURI, Request and Response Specifications, Common Headers, are all set here, using which you don’t need to set the respective specifications in tests separately and it helps in writing more clear and maintainable tests and also all configurations remain at one place.

We can also add the filters like _RequestLoggingFilter_ and _ResponseLogginFilter_.

> RequestLoggingFilter will log the request before it’s passed to HTTP Builder. This filter will only log things specified in the request specification.
>
> ResponseLoggingFilter will print the response body if the response matches a given status code.

I have added a condition in response specification to check that the response time to be less than _20000 milliseconds_, which means if it takes longer than that, the tests will fail.

## Setting up the Tests

<p>&nbsp;</p>
{% asset_img 4_testsetupclass.png Test Setup Class %}
<p>&nbsp;</p>

Before running any of the tests, some basic setup needs to be done for which I have created the
_testSetup()_ method. I have instantiated the booking data builder and token builder classes and have also created instances for creating the _new booking data, updated booking, partial update booking data_ builders. So, before any tests are run these classes are ready for being used in the tests. But before all, the _setup()_ method from _BaseSetup_ class will be called as the method is tagged with annotation @BeforeClass

> _Note: I have created an int bookingId variable which will store the booking Id value once a new booking is created._

## Creating a Booking

Before I begin talking about creating a new Booking, let me first make you understand how I have used builder pattern to create body for the post request, as it is required for creating a booking.

## Booking Data Builder

Following is the image of the curl request that is used for creating a booking. Considering the data that is passed in the post body, I have generated the **pojo class** which will help in parsing the json data in the post request.

<p>&nbsp;</p>
{% asset_img 5_curlforpost.png Curl for Post Request %}
<p>&nbsp;</p>

<p>&nbsp;</p>
{% asset_img 6_bookingdata.png Booking Data class %}
<p>&nbsp;</p>

<p>&nbsp;</p>
{% asset_img 7_bookingdates.png Booking Dates Class %}
<p>&nbsp;</p>

**Pojo class** _BookingData_ has another class _BookingDates_ in it. If you look closely in the post body data, you will see an object _BookingDates_ which has the field _checkin_ and _checkout_, hence new _pojo_ class was created to handle this fields as per json data required for creating a new booking.

> _Note the annotations @Data and @Builder above the pojo class. These are lombok annotations, as discussed in the Getting Started section above._

## Building the test data

<p>&nbsp;</p>
{% asset_img 8_bookingdatabuilder.png Booking Data Builder Class %}
<p>&nbsp;</p>

To generate the test data on run time, first of all we need to _create an instance of Faker class_. Once instance of faker class is created we can start using it for creating the data. Next, we can call the _BookingData_ builder class and use the method _builder()_ to build the test data and once you press _dot_ and press _Ctrl+Space_ IDE will show you the list of variables you have set inside the _BookingData_ class like _firstname, lastname, totalprice, etc.._ which you can use and set the test data by passing the required methods of _faker class_, so your test data gets generated on runtime once the tests are run.

## Creating a new Booking

[rest-assured]: https://rest-assured.io/
[manualtestcasesrepo]: https://github.com/mfaisalkhatri/Manual_Testing/tree/master/Templates
[apitestcases]: https://github.com/mfaisalkhatri/Manual_Testing/blob/master/Templates/TestCases_for_ApiTesting.xlsx
[restfulbooker]: https://restful-booker.herokuapp.com/apidoc/index.html
[create-booking]: https://restful-booker.herokuapp.com/apidoc/index.html#api-Booking-CreateBooking
[get-booking]: https://restful-booker.herokuapp.com/apidoc/index.html#api-Booking-GetBooking
[update-booking]: https://restful-booker.herokuapp.com/apidoc/index.html#api-Booking-UpdateBooking
[partial-update-booking]: https://restful-booker.herokuapp.com/apidoc/index.html#api-Booking-PartialUpdateBooking
[delete-booking]: https://restful-booker.herokuapp.com/apidoc/index.html#api-Booking-DeleteBooking
[create-token]: https://restful-booker.herokuapp.com/apidoc/index.html#api-Auth-CreateToken
[mavenwebsite]: https://maven.apache.org/
[testngwebsite]: https://testng.org/doc/
[lombok]: https://projectlombok.org/
