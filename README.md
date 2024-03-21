# Enterprisey Workshop POC

## Setup Instructions

> [!NOTE]
> Node Version 20.x.x is recommended for this project.

```bash
git clone https://github.com/onehungrymind/enterprisey-apps.git
cd enterprisey-apps
npm install
npm start
```

There are four features (challenges, flashcards, notes, users) that will serve as remotes for the host application (dashboard).

To serve a feature, run the corresponding command.

```bash
npm run serve:challenges-feature
npm run serve:flashcards-feature
npm run serve:notes-feature
npm run serve:users-feature
```

The underlying NPM commands for a feature, are as follows.

```json
"serve:users-app": "npx nx serve users --open",
"serve:users-api": "npx nx serve users-api",
"serve:users-json": "json-server server/users.json --routes=server/routes.json --port=3400",
"serve:users-feature": "concurrently \"npm run serve:users-json\" \"npm run serve:users-app\""
```

We are currently serving the data for the feature from `json-server` but you could change the top-level command to use the Nest implementation if you wanted.

To run the dashboard, make sure the feature apps are up and running and then execute this command.

```
npx nx serve dashboard --open 
// or you can run which wraps the command above
npm start
```

## WIP: The Portal

The portal is designed to load in the remote URIs that an MFE is hosted at as well as the API URI. 

You can get to the portal by running

```
npm run serve:portal-feature
```

## Authentication
This feature uses the Users API to create and authenticate users. 

Start the service with `npm run s:users-api`.

Via curl, you can create a user like so:

```bash
curl -X 'POST' \
  'http://localhost:3400/api/users' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
"firstName": "test",
"lastName": "test last",
"email": "test@test.com",
"password": "test",
"role": "tester",
"company_id": "test"
}'
```

Once the user is created, you can log in like so:

```bash
curl -X POST http://localhost:3400/api/users/auth/login -d '{"email": "test@test.com", "password": "test"}' -H "Content-Type: application/json"
```

You will receive a token in the response. You can use this token to authenticate requests to the API.

## The Wizard

There is a tool that you can use to help accelerate development across features. It allows you to quickly pull code from one feature and generate an equivalent version for other features.

You can see the tool by running

```
npm run wizard
```

## Running the remote services with docker
### Build a single service by passing the name to the build command.
```
npm run docker:build-remote <service name> (users, notes, flashcards, challenges)
```

### Build all services (this will take a while, as it will build all the remote services in the monorepo.)
```
npm run docker:build-remote
```

### Run with docker-compose
After building all the images, run the full stack with the following command.
```
npm run docker:run-remote
```

