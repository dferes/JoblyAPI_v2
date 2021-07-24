# Jobly API Documentation

The Jobly Api is a Node/Express application providing a REST
API that interfaces with a PostgreSQL database using node-postgres.

Deployed with heroku; the base URL is: https://dylan-feres-jobly.herokuapp.com/

## To get a local copy of the Api up and running:

### Clone this repository:

    https://github.com/dferes/JoblyAPI_v2.git

### Install 
##### Note that you will need `npm`, `postgresql` installed globally on your machine.

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

##### Token must belong to an admin or match to the username in the user information being requested (must be the same user).

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

### Apply to a job with a username and job id.

##### Token must belong to an admin or match to the username in the user information being requested (must be the same user).

#### Request

`POST /users/:username/jobs/:id`
   
    http://localhost:3001/users/testUser2/jobs/805

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RVc2VyMiIsImlzQWRtaW4iOmZhbHNlLCJpYXQiOjE2MjcwOTg4ODJ9.-XCk58h0GGsY2eTNGDavqCyyAyU2F3vbvcw9tjPo-94


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 15
    ETag: W/"f-ia3hpIOZOfs/EqNLQKrzpaU7/J4"
    Date: Sat, 24 Jul 2021 07:05:32 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "applied": 805
    }


## /companies

### Creating a new company

##### Token must belong to an admin or match to the username in the user information being requested (must be the same user).

#### Request

`POST /companies`

    http://localhost:3001/companies

    { 
      "handle": "comp1", 
      "name": "Amaz-Net-oogle", 
      "description": "future monopoly", 
      "numEmployees": 10000000,
      "logoUrl": "https://google.com/some-picture.jpg"
    }

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RhZG1pbiIsImlzQWRtaW4iOnRydWUsImlhdCI6MTYyNzEwMDExNX0.pWL7LASwfKmEAM6upkax4WpUS_qWf0QflnJ93AkTxB8

#### Response

    HTTP/1.1 201 Created
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 158
    ETag: W/"9e-39fwiq9TAWC7zyPgimhG9DyEFGY"
    Date: Sat, 24 Jul 2021 22:29:04 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "company": {
        "handle": "comp1",
        "name": "Amaz-Net-oogle",
        "description": "future monopoly",
        "numEmployees": 10000000,
        "logoUrl": "https://google.com/some-picture.jpg"
      }
    }

### Get a list of all companies

##### Note that 50 companies are inserted into the company table when the database is seeded; for brevity, only a few company objects will be shown in the below example.

#### Request

`GET /companies`

    http://localhost:3001/companies


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 10191
    ETag: W/"27cf-FFyYdsnzeZFmrnRIezA8W7Oz9js"
    Date: Sat, 24 Jul 2021 22:35:15 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {  
      "companies": [
        {
          "handle": "comp1",
          "name": "Amaz-Net-oogle",
          "description": "future monopoly",
          "numEmployees": 10000000,
          "logoUrl": "https://google.com/some-picture.jpg"
        },
        {
          "handle": "anderson-arias-morrow",
          "name": "Anderson, Arias and Morrow",
          "description": "Somebody program how I. Face give away discussion view act inside. Your official relationship administration here.",
          "numEmployees": 245,
          "logoUrl": "/logos/logo3.png"
        },
        {
          "handle": "arnold-berger-townsend",
          "name": "Arnold, Berger and Townsend",
          "description": "Kind crime at perhaps beat. Enjoy deal purpose serve begin or thought. Congress everything miss tend.",
          "numEmployees": 795,
          "logoUrl": null
        },
        ...
    }

#### Get a list of all companies matching the optional filter term: minEmployees

#### Request

`GET /companies`

    http://localhost:3001/companies?minEmployees=925


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 1024
    ETag: W/"400-Pg4R49Inwu0QHYySVp4w3NwhibA"
    Date: Sat, 24 Jul 2021 22:59:55 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "companies": [
        {
          "handle": "comp1",
          "name": "Amaz-Net-oogle",
          "description": "future monopoly",
          "numEmployees": 10000000,
          "logoUrl": "https://google.com/some-picture.jpg"
        },
        {
          "handle": "garner-michael",
          "name": "Garner-Michael",
          "description": "Necessary thousand parent since discuss director. Visit machine skill five the.",
          "numEmployees": 940,
          "logoUrl": null
        },
        {
          "handle": "mueller-moore",
          "name": "Mueller-Moore",
          "description": "Edge may report though least pressure likely. Cost short appear program hair seven.",
          "numEmployees": 932,
          "logoUrl": "/logos/logo2.png"
        },
        {
          "handle": "owen-newton",
          "name": "Owen-Newton",
          "description": "Red compare try way. Bed standard again number wrong force. Stop exactly agent product economy someone. North describe site manager employee customer.",
          "numEmployees": 953,
          "logoUrl": null
        },
        {
          "handle": "scott-smith",
          "name": "Scott-Smith",
          "description": "Room newspaper foot. Student daughter their themselves top almost near. Wait time recently it street follow medical nothing.",
          "numEmployees": 993,
          "logoUrl": "/logos/logo2.png"
        }
      ]
    }

#### Get a list of all companies matching the optional filter term: maxEmployees

#### Request

`GET /companies`
   
    http://localhost:3001/companies?maxEmployees=30

#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 591
    ETag: W/"24f-o4zlGcRAPmqBopp8ljpG5fvRQQg"
    Date: Sat, 24 Jul 2021 23:03:35 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "companies": [
        {
          "handle": "carr-wells-jones",
          "name": "Carr, Wells and Jones",
          "description": "Human medical throw book pick possible. Maybe yeah word beat treatment impact campaign.",
          "numEmployees": 27,
          "logoUrl": "/logos/logo3.png"
        },
        {
          "handle": "davis-davis",
          "name": "Davis-Davis",
          "description": "Career participant difficult. Decide claim particular century society. Question growth two staff.",
          "numEmployees": 23,
          "logoUrl": null
        },
        {
          "handle": "martinez-daniels",
          "name": "Martinez-Daniels",
          "description": "Five source market nation. Drop foreign raise pass.",
          "numEmployees": 12,
          "logoUrl": "/logos/logo4.png"
        }
      ]
    }

#### Get a list of all companies matching the optional filter term: name

#### Request

`GET /companies`
   
    http://localhost:3001/companies?name=arn

#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 591
    ETag: W/"24f-o4zlGcRAPmqBopp8ljpG5fvRQQg"
    Date: Sat, 24 Jul 2021 23:03:35 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "companies": [
        {
          "handle": "arnold-berger-townsend",
          "name": "Arnold, Berger and Townsend",
          "description": "Kind crime at perhaps beat. Enjoy deal purpose serve begin or thought. Congress everything miss tend.",
          "numEmployees": 795,
          "logoUrl": null
        },
        {
          "handle": "garner-michael",
          "name": "Garner-Michael",
          "description": "Necessary thousand parent since discuss director. Visit machine skill five the.",
          "numEmployees": 940,
          "logoUrl": null
        }
      ]
    }

### Get a company by handle

#### Request

`GET /companies/:handle`

    http://localhost:3001/companies/boyd-evans


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 911
    ETag: W/"38f-tMGUxKueBe6xFGzvZlGTz0g64no"
    Date: Sat, 24 Jul 2021 23:12:13 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "company": {
        "handle": "boyd-evans",
        "name": "Boyd-Evans",
        "description": "Build respond generation tree. No five keep. Happy medical back fine focus suffer modern.",
        "numEmployees": 698,
        "logoUrl": "/logos/logo4.png",
        "jobs": [
          {
            "id": 825,
            "title": "Pharmacist, hospital",
            "salary": 194000,
            "equity": "0"
          },
          {
            "id": 859,
            "title": "Psychologist, forensic",
            "salary": 176000,
            "equity": "0"
          },
          {
            "id": 961,
            "title": "Accountant, chartered certified",
            "salary": 86000,
            "equity": "0.070"
          },
          {
            "id": 1025,
            "title": "Pharmacist, hospital",
            "salary": 194000,
            "equity": "0"
          },
          {
            "id": 1059,
            "title": "Psychologist, forensic",
            "salary": 176000,
            "equity": "0"
          },
          {
            "id": 1161,
            "title": "Accountant, chartered certified",
            "salary": 86000,
            "equity": "0.070"
          },
          {
            "id": 1225,
            "title": "Pharmacist, hospital",
            "salary": 194000,
            "equity": "0"
          },
          {
            "id": 1259,
            "title": "Psychologist, forensic",
            "salary": 176000,
            "equity": "0"
          },
          {
            "id": 1361,
            "title": "Accountant, chartered certified",
            "salary": 86000,
            "equity": "0.070"
          }
        ]
      }
    }

### Update a company's information

##### Updatable fields include: name, description, numEmployees, logo_url. Admin token required.

#### Request

`PATCH /companies/:handle`
   
    http://localhost:3001/companies/comp1

    {
      "name": "Amaz-Net-oogle-acebook",
      "description": "Mega Future Monopoly",
      "numEmployees": 50000000,
      "logoUrl": "https://google.com/NEW-picture.jpg"
    }

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RhZG1pbiIsImlzQWRtaW4iOnRydWUsImlhdCI6MTYyNzEwMDExNX0.pWL7LASwfKmEAM6upkax4WpUS_qWf0QflnJ93AkTxB8


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 170
    ETag: W/"aa-CgEwMeBDK5cFB64b8y04BLQwhZw"
    Date: Sat, 24 Jul 2021 23:28:20 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "company": {
        "handle": "comp1",
        "name": "Amaz-Net-oogle-acebook",
        "description": "Mega Future Monopoly",
        "numEmployees": 50000000,
        "logoUrl": "https://google.com/NEW-picture.jpg"
      }
    }

### Delete a company by handle

##### Admin token required.

#### Request

`DELETE /companies/:handle`
   
    http://localhost:3001/companies/garcia-ray

    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3RhZG1pbiIsImlzQWRtaW4iOnRydWUsImlhdCI6MTYyNzEwMDExNX0.pWL7LASwfKmEAM6upkax4WpUS_qWf0QflnJ93AkTxB8


#### Response

    HTTP/1.1 200 OK
    X-Powered-By: Express
    Access-Control-Allow-Origin: *
    Content-Type: application/json; charset=utf-8
    Content-Length: 24
    ETag: W/"18-3pi1NQWxWbmwsjIxzIHgmPkw9Lg"
    Date: Sat, 24 Jul 2021 23:31:19 GMT
    Connection: keep-alive
    Keep-Alive: timeout=5

    {
      "deleted": "garcia-ray"
    }

