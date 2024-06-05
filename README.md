# Hotel-Booking-API-Testing
This project demonstrates API testing using Postman, providing a collection of tests to validate various API endpoints. 

### **Features**

- Tests for GET, POST, PUT,PATCH, DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations

## API Documentation:
-https://documenter.getpostman.com/view/34981174/2sA3QzYTSR
  
### **Technology used:**
- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
 ```console 
  git clone https://github.com/farihahoque/Hotel-Booking-API-Testing.git
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

## **Testing**

## Test Case Scenarios:

## _**1. Create New Booking**_

### Request URL: https://restful-booker.herokuapp.com/booking/
### Request Method: POST
### Pre-request Script:
```console 
    var firstName=pm.variables.replaceIn('{{$randomFirstName}}')
console.log(firstName)
pm.environment.set("firstName",firstName)

var LastName=pm.variables.replaceIn('{{$randomLastName}}')
console.log(LastName)
pm.environment.set("LastName",LastName)

var number=Math.floor(Math.random() * 90) + 10;
var totalprice =pm.variables.replaceIn(number)

console.log(totalprice)
pm.environment.set("totalprice",totalprice)
const arr = ["true","false"];
const randomElement = arr[Math.floor(Math.random() * arr.length)];
var depositpaid =pm.variables.replaceIn(randomElement)
console.log(depositpaid)
pm.environment.set("depositpaid",depositpaid)

const moment =require('moment')
const today = moment()

var checkIn= today.format("YYYY-MM-DD")
pm.environment.set('checkIn',checkIn)

var checkOut= today.format("YYYY-MM-DD")
pm.environment.set('checkOut',checkOut)

var additionalneed=pm.variables.replaceIn('{{$randomProduct}}')
console.log(additionalneed)
pm.environment.set("additionalneed",additionalneed)
```
  **Request Body:** 
 ```console 
  
{
	"firstname": "{{firstName}}",
	"lastname": "{{LastName}}",
	"totalprice": {{totalprice}},
	"depositpaid": {{depositpaid}},
	"bookingdates": {
    	"checkin": "{{checkIn}}",
    	"checkout": "{{checkOut}}"
	},
	"additionalneeds": "{{additionalneed}}"
}

```
  **Response Body:**
 ```console 
{
    "bookingid": 1419,
    "booking": {
        "firstname": "Mohamed",
        "lastname": "Orn",
        "totalprice": 78,
        "depositpaid": false,
        "bookingdates": {
            "checkin": "2024-06-04",
            "checkout": "2024-06-04"
        },
        "additionalneeds": "Ball"
    }
}
```
### Post-Request Script:
```console 
   var jsonData=pm.response.json()
pm.environment.set("id",jsonData.bookingid)

pm.test('Verify status code',function(){
    pm.response.to.have.status(200);
})

pm.test('first name validation',function(){
    pm.expect(jsonData.booking.firstname).to.eql(pm.environment.get('firstName'))
    pm.response.to.have.status(200);
})

pm.test('last name validation',function(){
    pm.expect(jsonData.booking.lastname).to.eql(pm.environment.get('LastName'))
})

pm.test('price validation',function(){
    pm.expect(jsonData.booking.totalprice).to.eql(parseInt(pm.environment.get('totalprice')))
    
})

pm.test('depositpaid validation',function(){
    console.log(jsonData.booking.depositpaid)
    console.log(pm.environment.get('depositpaid'))
    pm.expect(jsonData.booking.depositpaid).to.eql(JSON.parse(pm.environment.get('depositpaid')))
})

pm.test('checkIn validation',function(){
    pm.expect(jsonData.booking.bookingdates.checkin).to.eql(pm.environment.get('checkIn'))
})

pm.test('checkOut validation',function(){
    pm.expect(jsonData.booking.bookingdates.checkout).to.eql(pm.environment.get('checkOut'))
})

pm.test('additionalneed validation',function(){
    pm.expect(jsonData.booking.additionalneeds).to.eql(pm.environment.get('additionalneed'))
})

```
 ## _**2. Get Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/id
### Request Method: GET
### Response Body:
 ```console 
{
    "firstname": "Mohamed",
    "lastname": "Orn",
    "totalprice": 78,
    "depositpaid": false,
    "bookingdates": {
        "checkin": "2024-06-04",
        "checkout": "2024-06-04"
    },
    "additionalneeds": "Ball"
}
```
### Post-Request Script:
```console 
 var status = pm.response.code;
var jsonData = pm.response.json();


switch (status) {
    case 200:
   
        pm.test('Verify status code', function () {
            pm.response.to.have.status(200);
        });

        pm.test("first name validation", function () {
            pm.expect(jsonData.firstname).to.eql(pm.environment.get('firstName'));
        });

        pm.test("last name validation", function () {
            pm.expect(jsonData.lastname).to.eql(pm.environment.get('LastName'));
        });

        pm.test('totalprice validation', function () {
            console.log(jsonData.totalprice)
            console.log(pm.environment.get('totalprice'))
            pm.expect(jsonData.totalprice).to.eql(parseInt(pm.environment.get('totalprice')));
        });

        pm.test('depositpaid validation', function () {
            console.log(jsonData.depositpaid.toString())
            console.log(pm.environment.get('depositPaid'))
            pm.expect(jsonData.depositpaid.toString()).to.eql((pm.environment.get('depositpaid')));
        });

        pm.test('checkIn validation', function () {
            pm.expect(jsonData.bookingdates.checkin).to.eql(pm.environment.get('checkIn'));
        });

        pm.test('checkOut validation', function () {
            pm.expect(jsonData.bookingdates.checkout).to.eql(pm.environment.get('checkOut'));
        });

        pm.test('additionalneed validation', function () {
            pm.expect(jsonData.additionalneeds).to.eql(pm.environment.get('additionalneed'));
        });
         
        break;

    case 404:
        pm.test('not found');
        break;

    case 400:
        pm.test('bad request');
        break;
}

```
## _**3. Create A Token For Authentication.**_
### Request URL: https://restful-booker.herokuapp.com/auth
### Request Method: POST
### Pre-request Script: None
### Request Body:
 ```console 
{
	"username": "admin",
	"password": "password123"
}
```
  **Response Body:**
 ```console 
{
    "token": "91ed0feec9226c0"
}
```


 ## _**4. Update the Booking Details**_
### Request URL: https://restful-booker.herokuapp.com/booking/id
### Request Method: PUT

  **Request Body:** 
 ```console   {
    "firstname": "mimma",
    "lastname": "hoque",
    "totalprice": 1167798,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2013-02-23",
        "checkout": "2014-10-23"
    },
    "additionalneeds": "Breakfast"
}
```
  **Response Body:**
 ```console 
{
    "firstname": "mimma",
    "lastname": "hoque",
    "totalprice": 1167798,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2013-02-23",
        "checkout": "2014-10-23"
    },
    "additionalneeds": "Breakfast"
}
```
 ## _**5. Update a particular Booking Detail**_
### Request URL: https://restful-booker.herokuapp.com/booking/id
### Request Method: PATCH

  **Request Body:** 
 ```console   {
{
    "firstname": "jubayer"
    
}
```
  **Response Body:**
 ```console
{
    "firstname": "jubayer",
    "lastname": "hoque",
    "totalprice": 1167798,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2013-02-23",
        "checkout": "2014-10-23"
    },
    "additionalneeds": "Breakfast"
}
```
 ## _**6. Check after Updating Detail**_
### Request URL: https://restful-booker.herokuapp.com/booking/id
### Request Method: GET

  **Response Body:**
 ```console
{
    "firstname": "jubayer",
    "lastname": "hoque",
    "totalprice": 1167798,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2013-02-23",
        "checkout": "2014-10-23"
    },
    "additionalneeds": "Breakfast"
}
```

  **Post-Request Script:** 
 ```console   {
var status = pm.response.code;
console.log(status);
var jsonData= pm.response.json

pm.test('first name validation',function(){
    pm.expect(pm.environment.get('firstname')).to.eql(jsonData.firstname)
})

pm.test('last name validation',function(){
    pm.expect(pm.environment.get('lastname')).to.eql(jsonData.lastname)
})
```
 ## _**7. Delete Booking Record**_

### Request URL: https://restful-booker.herokuapp.com/booking/id
### Request Method: DELETE
### Response Body: None 

## Run Command:  
- Run Command for Console: 
```console 
newman run Booking_Testing.postman_collection.json -e env1.postman_environment.json
```
- Run Command for Report: 
```console 
newman run Booking_Testing.postman_collection.json -e env1.postman_environment.json -r cli,htmlextra
```

## Newman Report Summary:
![image](https://github.com/farihahoque/Hotel-Booking-API-Testing/assets/49908132/31a918c7-e8b7-46e4-92bd-d0a0a7e4103c)
![image](https://github.com/farihahoque/Hotel-Booking-API-Testing/assets/49908132/26561510-670e-45f2-8e0e-8768e427043f)
![image](https://github.com/farihahoque/Hotel-Booking-API-Testing/assets/49908132/de223a06-ac04-47e2-af2f-1f2a29bc36f9)
![image](https://github.com/farihahoque/Hotel-Booking-API-Testing/assets/49908132/deacc02f-428d-4df8-b778-f17d9527ff31)



