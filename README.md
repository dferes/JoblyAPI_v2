# Jobly API Documentation

The Jobly Api is a Node/Express application providing a REST
API that interfaces with a PostgreSQL database using node-postgres.

Deployed with heroku; the base URL is: https://dylan-feres-jobly.herokuapp.com/

## To get a local copy of the Api up and running:

### Clone this repository:

    https://github.com/dferes/JoblyAPI_v2.git

### Install (note that you will need `npm`, `postgresql` installed globally on your machine)

    npm install

### Create the database

    createdb jobly

### Create the test database

    createdb test

#### Seed the database (make sure you are in the root directory)

    psql jobly < jobly.sql
  

### Run the app

    nodemon server.js

### Run the tests

    jest -i

### Local url

    localhost:3001/


## The REST API endpoints are described below.
##### Note that the examples below use `http://localhost:3001` as the base url, but can be switched out with `https://dylan-feres-jobly.herokuapp.com` if the deployed version is preferable.

## /auth

### Registering a new user.

#### Request

`POST /auth/register`

    http://localhost:3001/auth/register

    {
      "username": "testUser2",
      "password": "password",
      "firstName": "Some",
      "lastName": "Guy",
      "email": "email@gmail.com"
    }

#### Response

    HTTP/1.1 201 Created
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 169
    ETag: W/"a9-W/S4Oy5xqp/xATNNNh9FtvCQNog"
    Date: Sat, 24 Jul 2021 03:54:42 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RVc2VyMiIsImlzQWRtaW4iOmZhbHNlLCJpYXQiOjE2MjcwOTg4ODJ9.-XCk58h0GGsY2eTNGDavqCyyAyU2F3vbvcw9tjPo-94"
}


### Authorizing a new user with username/password. Note that the username and password are from the admin seeded into the database.

#### Request

`POST /auth/token`

    {
      "username": "testadmin",
      "password": "password"
    }

#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 168
    ETag: W/"a8-/LtLJqk7hnSJhdG9gvVX0L+a0Q0"
    Date: Sat, 24 Jul 2021 04:21:50 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RhZG1pbiIsImlzQWRtaW4iOnRydWUsImlhdCI6MTYyNzEwMDUxMH0.sPOW4FxWU-07I5Xpm9VwGT1Usnl8milUMvQNjW3AwEc"
    }


## /users

### Create a new user. 
##### This is not the registration endpoint - instead, this is only for admin users to add new users. Only admins can create other admin users. Below, we use the bearer token for an admin seeded into the database (obtained in the previous example). Note that the first admin added to the database must be inserted into the users table manually with a SQL insert; this was an intentional design choice.

#### Request

`POST /users`

    {
      "username": "admin2",
      "password": "password",
      "firstName": "Mr",
      "lastName": "Smith",
      "email": "email2@gmail.com",
      "isAdmin": true
    }

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RhZG1pbiIsImlzQWRtaW4iOnRydWUsImlhdCI6MTYyNzEwMDExNX0.pWL7LASwfKmEAM6upkax4WpUS_qWf0QflnJ93AkTxB8
}

#### Response

    HTTP/1.1 201 Created
    Server: Cowboy
    Connection: keep-alive
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 168
    Etag: W/"a8-J+nKivBtF2MGu5yX/lZNsYwaTxY"
    Date: Sat, 24 Jul 2021 03:11:50 GMT
    Via: 1.1 vegur

    {
      "user": {
        "username": "admin2",
        "firstName": "Mr",
        "lastName": "Smith",
        "email": "email2@gmail.com",
        "isAdmin": true
    },
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluMiIsImlzQWRtaW4iOnRydWUsImlhdCI6MTYyNzEwMDY5MH0.Vumucp3_85vbx404gcFoG82vhuLoZlUx7DeQIP6G0Ns"
    }



### Get list of all users

##### Note that some of the following users are not initially seeded into the database when the repository is cloned. Admin token required.

#### Request

`GET /users/`
    
    http://localhost:3001/users

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RhZG1pbiIsImlzQWRtaW4iOnRydWUsImlhdCI6MTYyNzEwMDExNX0.pWL7LASwfKmEAM6upkax4WpUS_qWf0QflnJ93AkTxB8


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 634
    ETag: W/"27a-pOXr1kWzXuqNZSzJs2vTzv54Bf8"
    Date: Sat, 24 Jul 2021 04:27:55 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "users": [
        {
          "username": "admin2",
         "firstName": "Mr",
          "lastName": "Smith",
          "email": "email2@gmail.com",
          "isAdmin": true
        },
        {
          "username": "dferes23",
          "firstName": "Dylan",
          "lastName": "Feres",
          "email": "email@gmail.com",
          "isAdmin": false
        },
        {
          "username": "guy23",
          "firstName": "Guy",
          "lastName": "Man",
          "email": "guyMan@gmail.com",
          "isAdmin": false
        },
        {  
          "username": "testadmin",
          "firstName": "Test",
          "lastName": "Admin!",
          "email": "joel@joelburton.com",
          "isAdmin": true
        },
        {
          "username": "testuser",
          "firstName": "Test",
          "lastName": "User",
          "email": "joel@joelburton.com",
          "isAdmin": false
        },
        {
          "username": "testUser2",
          "firstName": "Some",
          "lastName": "Guy",
          "email": "email@gmail.com",
          "isAdmin": false
        }
      ]
    }

### Get a specific user by username. 

##### Token must belong to an admin or match to the username in the user information being requested (must be the same user).

#### Request

`GET /user/:username`

    http://localhost:3001/users/testUser2


    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RVc2VyMiIsImlzQWRtaW4iOmZhbHNlLCJpYXQiOjE2MjcwOTg4ODJ9.-XCk58h0GGsY2eTNGDavqCyyAyU2F3vbvcw9tjPo-94

#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 129
    ETag: W/"81-iEA3Ei0TyFBRL4jOjDKcGkAM/+k"
    Date: Sat, 24 Jul 2021 04:41:08 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "user": {
        "username": "testUser2",
        "firstName": "Some",
        "lastName": "Guy",
        "email": "email@gmail.com",
        "isAdmin": false,
        "applications": []
      }
    }

### Update a user's information

##### Token must belong to an admin or match to the username in the user information being requested (must be the same user).

##### Updatable fields include: firstName, lastName, password, email.

#### Request

`PATCH /users/:username`
   
    http://localhost:3001/users/testUser2

    {
      "firstName": "newFirst",
      "lastName": "newLast",
      "email": "newEmail@gmail.com",
      "password": "newPassword"  
    }

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RVc2VyMiIsImlzQWRtaW4iOmZhbHNlLCJpYXQiOjE2MjcwOTg4ODJ9.-XCk58h0GGsY2eTNGDavqCyyAyU2F3vbvcw9tjPo-94


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 122
    ETag: W/"7a-/+DrkkzIagvGe0NyWio3ftyUSbw"
    Date: Sat, 24 Jul 2021 06:22:58 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "user": {
        "username": "testUser2",
        "firstName": "newFirst",
        "lastName": "newLast",
        "email": "newEmail@gmail.com",
        "isAdmin": false
      }
    }


### Delete a user by username

#### Request

`DELETE /users/:username`
   
    http://localhost:3001/users/testUser

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RVc2VyMiIsImlzQWRtaW4iOmZhbHNlLCJpYXQiOjE2MjcwOTg4ODJ9.-XCk58h0GGsY2eTNGDavqCyyAyU2F3vbvcw9tjPo-94


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 23
    ETag: W/"17-fmYDO8KQtNq7nJG/jWxrxBXzkXA"
    Date: Sat, 24 Jul 2021 06:25:45 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "deleted": "testUser2"
    }

### Apply to a job with a username and job id

#### Request

`POST /users/:username/jobs/:id`
   
    http://localhost:3001/users/testUser2/jobs/800

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RVc2VyMiIsImlzQWRtaW4iOmZhbHNlLCJpYXQiOjE2MjcwOTg4ODJ9.-XCk58h0GGsY2eTNGDavqCyyAyU2F3vbvcw9tjPo-94


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 23
    ETag: W/"17-fmYDO8KQtNq7nJG/jWxrxBXzkXA"
    Date: Sat, 24 Jul 2021 06:25:45 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "deleted": "testUser2"
    }



