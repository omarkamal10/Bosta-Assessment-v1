# Bost Assessment API

A Node.js-based RESTful API for for tracking the uptime of user-specified websites. It will allow a user to enter a URL that they want monitored and receive alerts when those resources go down or come back up. Functionality will be included for sending Email notifications to a user. Users will be able to sign up, sign in.

## Setup

### Environment

Generate .env file using .env.example in root folder.
Create a postgres Database

### Installation and Starting up

Run ➡ "npm install"
Run ➡ "npm run database:init "(To initialize DB sync)
Run ➡" npm run start" ➡ Production runtime environment.
Run ➡ "npm run dev" ➡ Development runtime environment. (Listening on src for changes)

### NPM scripts

npm run docker ➡ Docker runtime environment. (HAS UNRESOLVED ISSUES AT THE MOMENT SO RUN LOCAL ENVIRONMENT)
npm run lint ➡ Run linter. (Checking for mistakes)
npm run database:reset(To reset DB to original state after initializing)

I left .env intentionally for you to test it right away and apologies for the poor documentation by hand was in a bit of a hurry :D
There is a postman collection in the docs folder

### Documentation

Postman (collection and environment) are provided in ./docs/Postman folder.
Environment variables to be used:
1-A token variable -> Ex: "token" with value of token received when logging in
2- A url variable -> Ex: "base-URL" with value of "http://127.0.0.1:9000" and for each request add "/api/v1"

GET,UPDATE checks are made only by user who created the check
