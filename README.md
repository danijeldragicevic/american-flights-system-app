# American Flights System App (Demo for Blog Post)

Welcome to the American Flights System App! <br>

This Mule 4 application serves as implementation for the [American Flights System API](https://github.com/danijeldragicevic/american-flights-system-api), providing a way to interact with the collection of flights.

Application have implemented scheduler, to cleanup the database every day at 05:00AM, UTC time.

# Demo Disclaimer
This application is a demo created for educational purposes and is associated with a blog post. It may not represent a fully functional or production-ready system. Please refer to the [blog post](https://www.example.com/blog-post) for insights into the concepts and use cases demonstrated.

# Technology
- Mule 4.4.0
- MySQL database

# To run application:
This application is developed in Anypoint Code Builder (Beta).
To run the application you will need Visual Studio Code with Anypoint Extension Pack installed. Please see [this page](https://docs.mulesoft.com/anypoint-code-builder/start-acb) for more details.

# Exposed endpoints:
By default, service will run on **http://localhost:8081** <br/>
Following endpoints will be exposed:

| Methods | Urls                                 | Actions                     |
|---------|--------------------------------------|-----------------------------|
| GET     | /api/flights?destination=[keyword]   | Retrieve a list of flights  |
| POST    | /api/flights                         | Create a new flight         |
| GET     | /api/flights/:id                     | Get flight by :id           |
| DELETE  | /api/flights/:id                     | Delete flight by :id        |

# Examples

**Example 1:** GET /api/flights <br>
To get all flights from the database. <br>

Response: 200 OK
```
[
  {
    "ID": 1,
    "code": "rree0001",
    "price": 400.99,
    "departureDate": "2017-07-26",
    "origin": "MUA",
    "destination": "SFO",
    "emptySeats": 0,
    "plane": {
      "type": "Boeing 737",
      "totalSeats": 150
    }
  },
  {
    "ID": 2,
    "code": "eefd0123",
    "price": 540.99,
    "departureDate": "2017-07-27",
    "origin": "MUA",
    "destination": "LAX",
    "emptySeats": 54,
    "plane": {
      "type": "Boeing 777",
      "totalSeats": 300
    }
  }
]
```

**Example 2:** GET /api/flights?destination=SFO <br>
To get flights by desired destination. <br>
Destination is an enum type, so only 'SFO', 'LAX' and 'CLE' values are allowed (please see [American Flights System API](https://github.com/danijeldragicevic/american-flights-system-api) for details)

Response: 200 OK
```
[
  {
    "ID": 1,
    "code": "rree0001",
    "price": 400.99,
    "departureDate": "2017-07-26",
    "origin": "MUA",
    "destination": "SFO",
    "emptySeats": 0,
    "plane": {
      "type": "Boeing 737",
      "totalSeats": 150
    }
  }
]
```

**Example 3:** POST /api/flights <br>
To create a flight.
Add JSON body like this:
```
{
  "code": "rree0001",
  "price": 400.99,
  "departureDate": "2017-07-26",
  "origin": "MUA",
  "destination": "SFO",
  "emptySeats": 0,
  "plane": {
    "type": "Boeing 737",
    "totalSeats": 150
  }
}
```
Response: 201 Created
```
{
  "message": "Flight successfully created"
}
```

**Example 4:** GET /api/flights/1 <br>
To get flight by ID.

Resonse: 200 OK
```
{
  "ID": 1,
  "code": "rree0001",
  "price": 400.99,
  "departureDate": "2017-07-26",
  "origin": "MUA",
  "destination": "SFO",
  "emptySeats": 0,
  "plane": {
    "type": "Boeing 737",
    "totalSeats": 150
  }
}
```

**Example 5:** DELETE /api/flights/1 <br>
To delete a specific flight.

Response: 200 OK
```
{
  "message": "Flight successfully deleted"
}
```

# Error Handling
Every endpoint can throw some of those errors:

| Http Status | Content type   | Erorr body                          |
|---------|--------------------|-------------------------------------|
| 400     | application/json   | {"error": "Bad Request"}            |
| 404     | application/json   | {"error": "Resource not found"}     |
| 405     | application/json   | {"error": "Method not allowed"}     |
| 406     | application/json   | {"error": "Not acceptable"}         |
| 415     | application/json   | {"error": "Unsuporrted media type"} |
| 500     | application/json   | {"error": "Internal server error"}  |
| 501     | application/json   | {"error": "Not implemented"}        |

Feel free to explore the application and use the provided examples to understand how to interact with each endpoint. If you have any questions or issues, please refer to the API documentation or contact the API maintainers.

# Licence
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)