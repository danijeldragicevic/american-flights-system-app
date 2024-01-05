# American Flights System App (Demo)

Welcome to the American Flights System App! <br>

This Mule 4 application serves as implementation for the [American Flights System API](https://github.com/danijeldragicevic/american-flights-system-api), providing a way to interact with the collection of flights.

Application have implemented scheduler, to refresh the database every day at 05:00AM, UTC time.

# Demo Disclaimer
This application is a demo created for educational purposes and is associated with a blog post. It is not representing a fully functional or production-ready system. Please refer to the [blog post](https://www.example.com/blog-post) for insights into the concepts and use cases demonstrated.

# Technology
- Mule 4.4.0
- MySQL database

# To run application
## 1. Change database credentials

Applicaiton connects to the remote database by using the secured credentials. <br>
To be able to run the application first you have to provide encryption key and change the database configuration (URL, username, password) with some of your own.

In order to achieve that, please follow instructions on this link: https://docs.mulesoft.com/mule-runtime/latest/secure-configuration-properties

In short:
- download [Secure Properties Tool Jar file](https://docs.mulesoft.com/mule-runtime/latest/secure-configuration-properties#:~:text=Secure%20Properties%20Tool%20Jar%20file.).
- navigate to the directory where you downloaded it and run following command:
```
java -cp secure-properties-tool.jar com.mulesoft.tools.SecurePropertiesTool \
> string \
> encrypt \
> Blowfish \
> CBC \
> yourEncryptionKey \
> "yourValueToProtect"
```
The tool vill return encrypted value of your username and/or password (i.e. *b3mCTv+dsC7FlcD0Ua4Hhg==*). <br>
To be able to use it in your application, put it between angle brackets started wit the exclamation mark (i.e. ![b3mCTv+dsC7FlcD0Ua4Hhg==]) in your **.secure.yaml* file.

When this step is done, you can proceed further, to run application on standalone or embeded Mule Runtime.

## 2. Run on Mule Standalone Runtime

First make sure that you have Mule Runtime installed and correctly configured on your machine (for details how to do it please see: https://docs.mulesoft.com/mule-runtime/latest/starting-and-stopping-mule-esb). <br>

In short:
- download [Mule Standalone Runtime](https://www.mulesoft.com/platform/soa/mule-esb-enterprise)
- configure `MULE_HOME` (*export MULE_HOME=/path/to/your/downloaded/mule-standalone-runtime*)
- configure `PATH` (*export PATH=$PATH:$MULE_HOME/bin*)

<br>

Second step is to build your application archive (`.jar`) file and copy to the /apps directory: <br>
- navigate to the root directory of your project (i.e. *cd ~/Downloads/american-flights-system-app/*)
- build the application archive (*mvn clean install*)
- copy generated file to the /apps (*cp target/american-flights-system-app-1.0.0-SNAPSHOT-mule-application.jar ~/path/to/your/downloaded/mule-standalone-runtime/apps*)

Last step is to run Mule Runtime, by setting some specific VM arguments (the last two are actually yours):
- run the Mule Runtime (*mule deploy -M-Dmule.forceConsoleLog -M-Dmule.testingMode -M-XX:-UseBiasedLocking -M-Dfile.encoding=UTF-8 -M-XX:+UseG1GC -M-XX:+UseStringDeduplication -M-Dcom.ning.http.client.AsyncHttpClientConfig.useProxyProperties=true -M-Dmule.debugger.test.port=8000 -M-Dmule.debug.enable=true console0 -M-Dencryption.key=yourEncryptionKey -M-Denv=yourAppEnvironmentName*)

As result you should se in the Console that Mule Runtime is up and running and your application successfully deployed:
```
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+ Mule is up and kicking (every 5000ms)                                        +
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

**********************************************************************
*              - - + DOMAIN + - -               * - - + STATUS + - - *
**********************************************************************
* default                                       * DEPLOYED           *
**********************************************************************

*******************************************************************************************************
*            - - + APPLICATION + - -            *       - - + DOMAIN + - -       * - - + STATUS + - - *
*******************************************************************************************************
* american-flights-system-app-1.0.0-SNAPSHOT-mu * default                        * DEPLOYED           *
*******************************************************************************************************

INFO  2023-12-22 15:02:53,297 [[MuleRuntime].uber.01: [american-flights-system-app-1.0.0-SNAPSHOT-mule-application].uber@org.mule.runtime.module.extension.internal.runtime.source.ExtensionMessageSource.lambda$reallyDoStart$17:462 @65487513] [processor: ; event: ] org.mule.runtime.module.extension.internal.runtime.source.ExtensionMessageSource: Message source 'listener' on flow 'american-flights-system-api-console' successfully started
INFO  2023-12-22 15:02:53,297 [[MuleRuntime].uber.04: [american-flights-system-app-1.0.0-SNAPSHOT-mule-application].uber@org.mule.runtime.module.extension.internal.runtime.source.ExtensionMessageSource.lambda$reallyDoStart$17:462 @b69b951] [processor: ; event: ] org.mule.runtime.module.extension.internal.runtime.source.ExtensionMessageSource: Message source 'listener' on flow 'american-flights-system-api-main' successfully started
```

## 3. Run on Embeded Mule Runtime 
If you don't want to bother with setting up the runtime engine, you can use some of the free IDEs that MuleSoft offers. <br>
There is well known [Anypoint Studio](https://www.mulesoft.com/platform/studio) and a new, cloud-native one - [Anypoint Code Builder (Beta)](https://www.mulesoft.com/platform/api/anypoint-code-builder). <br>

They both comes with embeded runtime, so all you have to do is to import the project and run it. <br>
Please, be aware that you still have to configure VM arguments that will be passed before the application starts. <br>

# To test the application
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
<br>

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
<br>

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

In case you get a Bed Request, please create your JSON body following this data structure:
```
#%RAML 1.0 DataType

type: object
properties: 
    code:
        type: string
        pattern: '^[A-Za-z]{4}\d{4}$'
    price:
        type: number
        minimum: 0
        maximum: 999.99
    departureDate:
        type: date-only
    origin:
        type: string
        minLength: 3
        maxLength: 3
    destination:
        type: string
        enum: ['SFO', 'LAX', 'CLE']
    emptySeats:
        type: integer
        minimum: 0
        maximum: 853
    plane:
        type: object
        required: false
        properties:
            type: string
            totalSeats:
                type: integer
                minimum: 1
                maximum: 853
```
<br>

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
<br>

**Example 5:** DELETE /api/flights/1 <br>
To delete a specific flight.

Response: 200 OK
```
{
  "message": "Flight successfully deleted"
}
```
<br>

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

Feel free to explore the application and use the provided examples to understand how to interact with each endpoint. If you have any questions or issues, please refer to the API documentation or contact the application maintainers.

# Licence
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
