# API Testing with Postman

This repository is a guide on how to perform API testing using Postman, a popular tool for testing and automating RESTful APIs. Whether you're a developer, QA engineer, or a DevOps professional, Postman simplifies the process of testing APIs and validating their functionality. This README will walk you through the basics of API testing with Postman.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Creating Requests](#creating-requests)
- [Working with Environments](#working-with-environments)
- [Writing Tests](#writing-tests)
- [Running Collections](#running-collections)
- [Automating Tests](#automating-tests)
- [Report](#report)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)
- [Contributing](#contributing)
- [License](#license)

## Prerequisites

Before you start API testing with Postman, make sure you have the following:

- **Postman**: Download and install [Postman](https://www.postman.com/downloads/), if you haven't already.

## API Documentation-
https://documenter.getpostman.com/view/29102059/2s9YXpWeqm

## Getting Started

- **Import the Environment**: Start by importing an environment that contains variables like API base URLs or authentication tokens. You can create your own or use existing ones.

- **Create a Request**: Build your first API request by specifying the HTTP method, URL, headers, and request body if needed.

## Creating Requests

In Postman, you can create and organize API requests as follows:

- **Request Type**: Select the HTTP method (GET, POST, PUT, DELETE, etc.) that corresponds to your API operation.

- **URL**: Enter the endpoint URL you want to test.

- **Headers**: Add headers, such as `Content-Type` or authentication tokens, as needed.

- **Request Body**: For POST and PUT requests, provide the payload in JSON, form-data, or other formats.

## Working with Environments

Environments in Postman allow you to manage variables for different scenarios (e.g., development, production, staging). Here's how to use them:

- Create an environment and define variables like `baseURL`.

- Reference these variables in your requests using double curly braces, like `{{baseURL}}`.

- Switch between environments to adapt your requests for different settings.

## Writing Tests

Postman allows you to write tests in JavaScript using the Postman scripting environment. Common assertions include checking the status code, response time, and specific JSON properties. Example test script:

Tests_scripts for POST
```javascript
var jsonData = pm.response.json()
pm.environment.set("id", jsonData.bookingid)
```
```javascript
var jsonData = pm.response.json()
pm.environment.set("token", jsonData.token)
```
Test_script for GET
```javascript
var statuscode = pm.response.code
console.log(statuscode)

if(statuscode==200){
    var jsonData = pm.response.json()
    pm.test("Status code validation", function(){
        pm.response.to.have.status(200)
    })

    pm.test("First Name validation", function(){
        pm.expect(jsonData.firstname).to.eql(pm.environment.get("firstName"))
    })

    pm.test("Last name validation", function(){
        pm.expect(jsonData.lastname).to.eql(pm.environment.get("lastName"))
    })

    pm.test("Total Price validation", function(){
        pm.expect(jsonData.totalprice).to.eql(parseInt(pm.environment.get("totalPrice")))
    })

    pm.test("Deposit Paid Validation", function(){
        pm.expect(jsonData.depositpaid).to.eql(Boolean(pm.environment.get("depositPaid")))
    })

    pm.test("checkIn dates validation", function(){
        pm.expect(jsonData.bookingdates.checkin).to.eql(pm.environment.get("checkIn"))
    })

    pm.test("checkOut dates validation", function(){
        pm.expect(jsonData.bookingdates.checkout).to.eql(pm.environment.get("checkOut"))
    })

    pm.test("additionalneeds validation", function(){
        pm.expect(jsonData.additionalneeds).to.eql(pm.environment.get("additionalNeeds"))
    })
}
else if(statuscode==404){
    pm.test("Not Found", function(){
        pm.to.have.status(404)
    })
}
else if(statuscode==403){
    pm.test("Forbidden", function(){
        pm.to.have.status(403)
    })
}
else{
    pm.test("Something wrong......")
}
```

Pre_requisites script for POST
```javascript
var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
console.log("First Name Value:" +firstName)
pm.environment.set("firstName", firstName)

var lastName = pm.variables.replaceIn("{{$randomLastName}}")
console.log("Last Name Value:" + lastName)
pm.environment.set("lastName", lastName)

var totalPrice = pm.variables.replaceIn("{{$randomInt}}")
console.log("Total Price Value:" + totalPrice)
pm.environment.set("totalPrice", totalPrice)

var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
console.log("Deposit Paid Value:" + depositPaid)
pm.environment.set("depositPaid", depositPaid)

const moment = require('moment')
const today = moment()
console.log(today.format('YYYY-MM-DD'))
pm.environment.set("checkIn", today.format('YYYY-MM-DD'))
console.log(today.add(5, 'd').format('YYYY-MM-DD'))
pm.environment.set("checkOut", today.format('YYYY-MM-DD'))

var additionalNeeds = pm.variables.replaceIn("{{$randomProduct}}")
console.log("additionalNeeds value:" + additionalNeeds)
pm.environment.set("additionalNeeds", additionalNeeds)
```

## Running Collections

A collection in Postman is a group of requests. To run a collection:

- Organize your requests into folders within the collection.

- Set up request dependencies if needed (e.g., a login request before an authenticated request).

- Execute the collection to run all requests in sequence.

- Review the test results and responses for each request.
 - Create Booking-
![create booking](https://github.com/tofayel143/API-Testing/blob/main/Image/p1.png)
- Get Booking
![Get booking](https://github.com/tofayel143/API-Testing/blob/main/Image/p4.png)
- Create Token
![Ctreate token](https://github.com/tofayel143/API-Testing/blob/main/Image/p5.png)
- Update Booking
![Update booking](https://github.com/tofayel143/API-Testing/blob/main/Image/p7.png)
- Get Booking Update
![Get booking update](https://github.com/tofayel143/API-Testing/blob/main/Image/p8.png)
- Delate Booking
![Delete](https://github.com/tofayel143/API-Testing/blob/main/Image/p9.png)

## Automating Tests

You can automate API tests in Postman by using:

- **Postman Monitors**: Schedule collections to run at specific intervals and receive notifications about test results.

- **Newman (Command-line Runner)**: Execute collections from the command line for continuous integration and automated testing.

Install Command:
```bash
npm install -g newman
```
Run Command:
```bash
newman run “Path/CollectionName.json” -e Path/EnvironmentName.json
newman run “Collection Link” -e “Path”/EnvironmentName.json
```
## Report
install
```bash
npm install -g newman-reporter-html
	npm install -g newman-reporter-htmlextra
```
Run Command: 
```bash
newman run CollectionName.json -e EnvironmentName.json -r cli,html
newman run CollectionName.json -e EnvironmentName.json -r cli,htmlextra
```
![API_Report](https://github.com/tofayel143/API-Testing/blob/main/Image/Web%20capture_1-10-2023_190_.jpeg)

## Best Practices

- Keep your collections and requests well-organized.

- Use variables and environments to make tests reusable and adaptable.

- Write comprehensive tests to ensure the API's functionality.

- Version control your Postman collections with Git for collaboration.

## Troubleshooting

If you encounter issues during API testing, check the following:

- Request configuration, including headers and variables.

- Test scripts for errors.

- API response for unexpected changes.

## Resources

- [Postman Documentation](https://learning.postman.com/docs/getting-started/introduction/)

- [Postman Learning Center](https://learning.postman.com/)

## Contributing

Contributions to this guide are welcome! If you have suggestions, improvements, or want to expand on a topic, please open an issue or submit a pull request.

## License

This guide is open-source and available under the [MIT License](LICENSE). Feel free to use, modify, and share it as needed.

Happy API testing with Postman! 🚀
