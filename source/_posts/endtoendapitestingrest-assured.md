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

> **RequestLoggingFilter** will log the request before it’s passed to HTTP Builder. This filter will only log things specified in the request specification.
>
> **ResponseLoggingFilter** will print the response body if the response matches a given status code.

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

<p>&nbsp;</p>
{% asset_img 9_createbooking.png Create Booking %}
<p>&nbsp;</p>

Post requests are used to create new data in the system. It is very simple to use the rest-assured methods to create a post request. Like I said in the very beginning, tests are written in BDD style using rest-assured so its quite simple to understand this framework and write the tests. It begins with given() method and then continues further using _when(), post(), get(), put(), delete(), etc.._ Tests can be written in [fluent interface][fluentinterface] style with chaining the methods one after another which actually helps in reading the intention of the method clearly and even a beginner would be able to understand what actually the tests is going to perform.

In the create booking test, _(checkout the image above)_, I have used the _given()_ method then called the _body()_ method which takes the _post body_ as parameter and once this is done, _when()_ method is called and then using _post()_ request method passing the path of the API endpoint.

Next, we can start the assertions after calling _then()_ method and checking if _status code is 200_. Once this assertion is passed, we can make sure that the Post API is working fine and we are good to move
ahead asserting the response body.

> _If status code is not matching 200 then test will fail and it doesn’t make any sense to check the response body._

Now, comes an important point, as per the test strategy we defined in the start of this blog, Saving the _bookingid_ in `bookingId` variable.

For doing that I have used the _extract()_ method and extracted the _bookingid_ from response body using _path()_ method and assigned it to variable `bookingId`.

If we run the Post API tests now, it will generate the booking and save the _bookingid_ in `bookingId` variable.

> _Note: Only the endpoint is passed in the post() method, the baseURI will be picked from BaseSetup configuration class which is extended in the test class. BaseSetup class will also set the required headers in the test._

## Updating a Booking

For updating a resource, PUT API is used. PUT requests needs all the data in the body to be sent, you can not send only the required fields you need to update unlike PATCH.

The pattern of calling the rest-assured methods for PUT request remains the same. First you need to call _given()_ after that _body()_ and passing the put _body_ as parameter, then calling _when()_ method. Here, since there is an additional requirement of passing the token without which the PUT request won’t be successful.
Let me show you how I have handled this in the test.

## Token Builder

<p>&nbsp;</p>
{% asset_img 10_curlfortokenrqst.png Curl for Token Request %}
<p>&nbsp;</p>

<p>&nbsp;</p>
{% asset_img 11_tokencreds.png Token Credentials class %}
<p>&nbsp;</p>

<p>&nbsp;</p>
{% asset_img 12_tokenbuilderclass.png Token Builder class %}
<p>&nbsp;</p>

Looking at the curl request for creating an AUTH token, we see only 2 fields in namely, `username` and `password` which the API will consume and provide us with the **AUTH Token** which will be further used in PUT, PATCH and DELETE API requests.

> _What is an AUTH Token?_
>
> _Access tokens are used in token-based authentication to allow an application to access an API. The application receives an access token after a user successfully authenticates and authorizes access, then passes the access token as a credential when it calls the target API. The passed token informs the API that the bearer of the token has been authorized to access the API and perform specific actions specified by the scope that was granted during authorization._
>
> _Reference - https://auth0.com/docs/secure/tokens/access-tokens_

## Token Generation

<p>&nbsp;</p>
{% asset_img 13_generatetoken.png Generate Token Method %}
<p>&nbsp;</p>

In the _TokenBuilder_ class I have passed the actual value for the fields username and password. Since this is a demo project hence I have passed the static values in the class itself while building the data. Recommended way to pass the secure data is to use the _String Interpolation_ and pass the data on run time.

As the token generated would be used in 3 different requests, hence it is recommended to generate a single method which would be called in the different requests instead of writing duplicate codes to generate token every time. Having said that, I have created a _private method generateToken()_ which consumes the credentials and returns the token in String format.

## Updating the Booking

<p>&nbsp;</p>
{% asset_img 14_updatebookingtest.png Update Booking test %}
<p>&nbsp;</p>

As mentioned above, the pattern to write the test is same as for Post API, the only difference here is, we are calling _put()_ method and passing the `/booking/` endpoint with `bookingId` which we extracted at the time of creating a booking. Also to note the _header()_ method which passes the _Cookie key and AUTH token value_.
Assertions are made using the newly created _updatedBooking_ data builder.

## Updating a Booking Partially

<p>&nbsp;</p>
{% asset_img 15_updatebookingpartially.png Update Booking Partially test %}
<p>&nbsp;</p>

Using PATCH request you can modify only a few respective fields which needs to be updated, we don’t need to send all the fields like we do in PUT request.

`firstname` and `totalprice` fields are updated using PATCH request.The pattern to write the _Partial update of the booking_ test is the same as we did for Update booking test. Starting with _given()_ method and then asserting the status code and finally the response body. Taking a close look at the _image above for partial update of the booking_ test you will notice that in assertions I have used the instance of _partialUpdateBooking builder_ for asserting the `firstname` and `totalprice` values, for rest of the fields instance of `updateBooking` builder is used, as it is the latest builder used for the test data. Remember, we updated the booking after creating it!

## Deleting a Booking

<p>&nbsp;</p>
{% asset_img 16_deletebookingtest.png Delete Booking test %}
<p>&nbsp;</p>

Now, comes the final stage of the test strategy we planned!
Its time now to delete the booking and check if the delete API is working as expected. The other way of looking at this is, we should always clear the data we generated using automated tests, if we don’t do it, we would be creating junk data in the system which would be useless and unnecessarily increase our server load and size of the DB, since every test run will keep on generating new set of booking records.

_delete()_ method is used for _deleting the booking_. We are using the same `bookingId` which we saved while creating the booking, so we delete the same booking which we created using this end to end tests. Once delete API is called we assert that it should return _status code 201(This is as per the API documentation of restful booker)_ from which we come to know that booking was deleted successfully. Ideally for _delete status code returned is_ **204**.

To make sure that booking was actually deleted, I have created one more test which will call the Get request and try to fetch the booking using the `bookingId` which was deleted and here we expect that it should return **status code 404** which means data was not found and it assures us that the booking was actually deleted.

This marks the end of our end to end test scenarios!

## Conclusion

To Conclude, we performed end to end for the restful booker APIs by creating a new booking, updating the newly created booking, partially updating the booking by modifying the _firstname_ and _totalprice_ values and then finally deleting the booking created. We also added a method which would generate _token_ so we could use that method while updating and deleting the booking entries, as those entries require user authorization.

Hope you got a better understanding of rest-assured framework and how to perform API Testing and also test automation strategy!

Happy Testing!

## \*\*Github Repository

<p>&nbsp;</p>
{% asset_img 17_githubrepo.png Github Repository %}
<p>&nbsp;</p>

All of the code which is showcased above is located in this [github repository][githubrepo].
Do mark a ⭐ and make the project popular so it reaches to the people who want to learn and upgrade and get better understanding about API Test Automation using rest-assured.

> ## Paid Trainings
>
> _Contact me for Paid trainings related to Test Automation and Software Testing_.

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
[fluentinterface]: https://en.wikipedia.org/wiki/Fluent_interface
[githubrepo]: https://github.com/mfaisalkhatri/rest-assured-examples
