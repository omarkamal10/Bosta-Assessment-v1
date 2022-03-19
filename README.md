# Bost Assessment API

A Node.js-based RESTful API for for tracking the uptime of user-specified websites. It will allow a user to enter a URL that they want monitored and receive alerts when those resources go down or come back up. Functionality will be included for sending Email notifications to a user. Users will be able to sign up, sign in.

## Setup

Environment
Generate .env file using .env.example in root folder.
Create a postgres Database

Installation and Starting up
Run ➡ "npm install"
Run ➡ "npm run database:init "(To initialize DB sync)
Run ➡" npm run start" ➡ Production runtime environment.
Run ➡ "npm run dev" ➡ Development runtime environment. (Listening on src for changes)

NPM scripts
npm run docker ➡ Docker runtime environment. (HAS UNRESOLVED ISSUES AT THE MOMENT SO RUN LOCAL ENVIRONMENT)
npm run lint ➡ Run linter. (Checking for mistakes)
npm run database:reset(To reset DB to original state after initializing)

I left .env intentionally for you to test it right away and apologies for the poor documentation by hand was in a bit of a hurry :D
There is a postman collection in the docs folder

Documentation
Postman (collection and environment) are provided in ./docs/Postman folder.
Environment variables to be used:
1-A token variable -> Ex: "token" with value of token received when logging in
2- A url variable -> Ex: "base-URL" with value of "http://127.0.0.1:9000" and for each request add "/api/v1"

GET,UPDATE checks are made only by user who created the check

-REgister
-request POST "/users" -> body Example:
{
"firstName":"omarr",
"middleName":null ---->optional
"lastName":"omarr",
"userName":"omar",
"email":"fesikhe80@gmail.com",
"mobile":"1234",
"password":"1234"
}
-response -> Example:
{
"message": "ok",
"data": {
"user": {
"verified": false,
"id": 1,
"firstName": "omarr",
"lastName": "omarr",
"userName": "omar",
"email": "fesikhe80@gmail.com",
"mobile": "1234rr",
"middleName": null,
"dateOfBirth": null
},
"token": {
"id": 1,
"userId": 1,
"token": "289ca8dbfa3f6e79d63634175a63caadaec7e67fc527bb7ab403bcf2f6583175"
}
}
}

-Verify
An email is sent to user email to verify using below request using the token above
request GET "/users/verify/:userId/:token"

response ->
{
"message": "ok",
"data": "email-verified"
}

-Login:
request POST "/users/login"-> body:
{
"userName":"omar",
"password":"1234"
}

response ->
{
"message": "ok",
"data": {
"user": {
"id": 1,
"firstName": "omarr",
"middleName": null,
"lastName": "omarr",
"userName": "omar",
"email": "fesikhe80@gmail.com",
"mobile": "1234rr",
"dateOfBirth": null,
"verified": true
},
"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjEsImVtYWlsIjoiZmVzaWtoZTgwQGdtYWlsLmNvbSIsImlhdCI6MTY0NzczMDc2NSwiZXhwIjoxNjc5Mjg4MzY1fQ.0_8ILgiSlNznvha-wTf9psKbkzb242hjbE1JZdfPk00"
}
}

-User creates url to check(Needs to be authenticated with Bearer token)
request POST "/url-check/create"-> body:
{
"name":"google",
"url":"www.google.com",
"protocol":"https",
"tags":"search engine"
"ignoreSSL":true --->optional
}
response ->:
{
"message": "ok",
"data": {
"id": 1,
"userId": 1,
"name": "google",
"url": "www.google.com",
"protocol": "https",
"tags": "search engine",
"ignoreSSL": true
}
}

-User checks if url created above is down or up and responds with some details
request POST "/url-check/check" -> body:
{
"urlToCheckId":1 ---> id of url created above
}
response -> depends if check is successful or not(in this case it is successful). Email is sent notifying if up or down
{
"message": "ok",
"data": {
"id": 1,
"urlId": 1,
"visitedAt": "2022-03-19T23:03:33.749Z",
"responseTime": 268,
"successful": true
}
}

-Get checks by tags
request GET "/url-check/tags/:tag" example: "/url-check/tags/search engine"

response ->
{
"message": "ok",
"data": [
{
"id": 1,
"userId": 1,
"name": "google",
"url": "www.google.com",
"protocol": "https",
"tags": "search engine",
"ignoreSSL": null,
"UserURLChecks": [
{
"id": 1,
"urlId": 1,
"visitedAt": "2022-03-19T23:03:33.749Z",
"responseTime": 268,
"successful": true
}
]
}
]
}

-Get all checks
request GET "/url-check/"
response -> all checks with their details

-Modify URL check
request PUT "/url-check/:id" put any field of URL check in body to updated it
response-> updated url check
