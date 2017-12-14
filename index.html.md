---
title: KLANG SENTRAL API Specification

language_tabs: # must be one of https://git.io/vQNgJ
  - cURL

includes:
  - errors

search: true
---

# Introduction

The technical document specifies the interaction point for Bus Operator System(BOS) and the Centralised Ticketing system Klang Terminal. The project will enable the stake holder of Centralised Ticketing system sell the Bus operator inventory via real time API integrations. 

The Bus Operator System(BOS) integrating with Klang Centralised System need to provide the following RESTFUL API with the mentioned JSON Structure. 

# Authentication and Security

Authenticate your account when using the API by including your secret API key in the request. Your API keys carry many privileges, so be sure to keep them secret! Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.
Authentication to the API is performed via HTTP Basic Auth. Provide your API key as the basic auth username value. You do not need to provide a password.

If you need to authenticate via bearer auth (e.g., for a cross-origin request), use: `-H "Authorization: Bearer sk_test_BQokikJOvBiI2HlWgH4olfQ2"` 
 instead of: `-u sk_test_BQokikJOvBiI2HlWgH4olfQ2:`

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

# Get trips

```cURL
curl "{getTripsApiURL}?sourceCityId=42&destinationCityId=73&departDate=2018-01-28"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
    "errorCode": 0,
    "errorMessage": "Success",
    "data": [
    {
      "tripId": "0B101010",
      "tripCode": "OP4273",
      "departTime": "2018-01-28 20:15:00",
      "arrivalTime": "2018-01-29  02:15:00",
      "adultFare": "42.0",
      "childFare": "37.0",
      "seniorFare": "37.0",
      "disabledFare": "37.0",
    },
    {
      "tripId": "0B101012",
      "tripCode": "OP4274",
      "departTime": "2018-01-28 22:15:00",
      "arrivalTime": "2018-01-29  04:15:00",
      "adultFare": "42.5",
      "childFare": "37.5",
      "seniorFare": "37.5",
      "disabledFare": "37.5",
    }
    ]
}
```

CTS will call the Bus Operator System(BOS) to get the trip list for the given search criteria (source city, destination city, journey date). The response will have trip information and fare details.

### HTTP Request

`GET {getTripsApiURL}?sourceCityId=42&destinationCityId=73&departDate=2018-01-28`

### Query Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
sourceCityId | Integer | 42
destinationCityId | Integer | 73
departDate | String | yyyy-MM-dd


### Response Format

Field | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | Refer below
errorMessage | String | Refer below
data | Array of Trips | Refer below


### Trip Object

Field | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip 
tripCode | String | Trip Identifier for bus operator usage. Will be printed in the ticket.  MLM .. etc
departTime | String | yyyy-MM-dd HH:mm:ss
arrivalTime | String | yyyy-MM-dd HH:mm:ss
adultFare | double | 42.0
childFare | double | 37.0
seniorFare | double | 37.0
disabledFare | double | 37.0


### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure
101 | Trips Cancelled
103 | Trip does not exist
104 | Any other error needs to be sent here


<aside class="notice">
{getTripsApiURL} — Operator specific trips API URL that we'll call
</aside>


# Get seat map

```cURL
curl "{getSeatMapApiURL}?TripId=0B101010&OperatorCode=OPM&OperatorId=421"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 0,
  "errorMessage": "SUCCESS",
  "data": {
    "lowerDeck": [
      [
        {
          "seatNumber": "1A",
          "status": 1,
          "rowIndex": 0,
          "columnIndex": 0
        },
        {
          "seatNumber": "#"
        },
        {
          "seatNumber": "1B",
          "status": 1,
          "rowIndex": 0,
          "columnIndex": 1
        },
        {
          "seatNumber": "1C",
          "status": 1,
          "rowIndex": 0,
          "columnIndex": 2
        }
      ],
      [
        {
          "seatNumber": "2A",
          "status": 1,
          "rowIndex": 1,
          "columnIndex": 0
        },
        {
          "seatNumber": "#"
        },
        {
          "seatNumber": "2B",
          "status": 1,
          "rowIndex": 1,
          "columnIndex": 1
        },
        {
          "seatNumber": "2C",
          "status": 1,
          "rowIndex": 1,
          "columnIndex": 2
        }
      ]
    ],
    "upperDeck": [
      [
        {
          "seatNumber": "21A",
          "status": 1,
          "rowIndex": 0,
          "columnIndex": 0
        },
        {
          "seatNumber": "#"
        },
        {
          "seatNumber": "21B",
          "status": 1,
          "rowIndex": 0,
          "columnIndex": 1
        },
        {
          "seatNumber": "21C",
          "status": 1,
          "rowIndex": 0,
          "columnIndex": 2
        }
      ],
      [
        {
          "seatNumber": "22A",
          "status": 1,
          "rowIndex": 1,
          "columnIndex": 0
        },
        {
          "seatNumber": "#"
        },
        {
          "seatNumber": "22B",
          "status": 1,
          "rowIndex": 1,
          "columnIndex": 1
        },
        {
          "seatNumber": "22C",
          "status": 1,
          "rowIndex": 1,
          "columnIndex": 2
        }
      ]
    ]
  }
}
```

CTS will call the Bus Operator System(BOS) to get seat map or bus seat details for the trip using tripId

### HTTP Request

`GET {getSeatMapApiURL}?tripId=0B101010&operatorCode=OPM&operatorId=42`

### Query Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
operatorCode | String | OPM
operatorId | String | 421


### Response Format

Field | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | Refer below
errorMessage | String | Refer below
data | JSON object | As mentioned in the response example

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure
101 | Trips Cancelled
103 | Trip does not exist
104 | Any other error needs to be sent here

<aside class="notice">
{getSeatMapApiURL} — Operator specific seat map API URL that we'll call
</aside>


# Get seat availability

```cURL
curl "{getSeatAvailabilityApiURL}?tripId=0B101010&seatNo=4B&operatorId=421&operatorCode=OPM&sourceId=42&destinationId=73"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 0,
  "errorMessage": "SUCCESS",
  "data": {
    "tripId": "0B101010",
    "seatNo": "4B",
    "operatorId": "421",
    "operatorCode": "OPM"
  }
}
```

CTS will call the Bus Operator System(BOS) before call make booking we need to make sure the selected seats are available and no changes happened on selected trip

### HTTP Request

`GET {getSeatAvailabilityApiURL}?TripId=0B101010&SeatNo=4B&OperatorId=421&OperatorCode=OPM&SourceId=42&destinationId=73`

### Query Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
seatNo | String | 4B
operatorId | String | 421
operatorCode | String | OPM
sourceId | String | 42
destinationId | String | 73

### Response Format

Field | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | 0: Failure 1: Success
errorMessage | String | Brief description for failure
data | JSON object | Refer below

### Seat availability JSON object
tripId | String | Unique Identifier representing the trip
seatNo | String | 4B
operatorId | String | 421
operatorCode | String | OPM

<aside class="notice">
{getSeatAvailabilityApiURL} — Operator specific seat availability API URL that we'll call
</aside>

# Make booking

```cURL
curl "{makeBookingApiURL}"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
  -d "{
      "tripId": "0B101010",
      "SourceId": "42",
      "destinationId": "73",
      "OperatorCode": "OPM",
      "departDate": "2018-01-28",
      "passengerTicketList": [
        {
          "name": "John Doe",
          "email": "john@doe.co",
          "phoneNumber": "+605864783920",
          "idNumber": "JohnDoe42",
          "category": "Adult",
          "seatNumber": "4B",
          "gender": "M",
          "nationality": "Malaysian"
        }
      ]
    }"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 0,
  "errorMessage": "SUCCESS",
  "tripId": "0B101010",
  "tripCode": "OP4273",
  "pnr": "OPM20180128",
  "totalAmount": "142.95",
  "passengerTicketList": [
    {
      "name": "John Doe",
      "email": "john@doe.co",
      "phoneNumber": "+605864783920",
      "idNumber": "JohnDoe42",
      "category": "Adult",
      "seatNumber": "4B",
      "gender": "M",
      "nationality": "Malaysian",
      "ticketNumber": "OPM4273201801284B"
    }
  ]
}
```

CTS will call the Bus Operator System(BOS) to make the booking for the given request. Request will have passenger details, seat numbers selected, trip information. We'll get pnr and ticket numbers if the booking is successful.

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
TripId | String | Unique Identifier representing the trip
SourceId | String | 42
destinationId | String | 73
OperatorCode | String | OPM
OperatorId | String | 421
departDate | String | yyyy-MM-dd
passengerTicketList | Array of PassengerDetails | Refer below

### PassengerDetails Object

Field | Type | Format / Example
--------- | ------- | -----------
name | String | John Doe
email | String | john@doe.co
phoneNumber | String | +605864783920
idNumber | String | JohnDoe42
category | String | Adult / Child / Senior Citizen / Disabled
seatNumber | String | 4B
gender | String | M / F
nationality | String | Malaysian / Singaporean etc


### Response Format

Parameter | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | 0:Failure 1: Success
errorMessage | String | Success / The reason for failure
TripId | String | Unique Trip id for validation
TripCode | String | Unique trip code to be printed in ticket
pnr | String | Unique booking ID
totalAmount | String | 154.00 (It will be used for data validity)
passengerTicketList | Array of PassengerDetails | Refer below

### PassengerDetails Object

Field | Type | Format / Example
--------- | ------- | -----------
name | String | John Doe
email | String | john@doe.co
phoneNumber | String | +605864783920
idNumber | String | JohnDoe42
category | String | Adult / Child / Senior Citizen / Disabled
seatNumber | String | 4B
gender | String | M / F
nationality | String | Malaysian / Singaporean etc
ticketNumber | String | OPM4273201801284B


<aside class="notice">
{makeBookingApiURL} — Operator specific make booking API URL that we'll call
</aside>


# Get WayBill

```cURL
curl "{waybillApiURL}?tripId=0B101010&OperatorCode=OPM&DepartDate=2018-01-28"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 0,
  "errorMessage": "SUCCESS",
  "busNumber": "SBUS707",
  "SourceId": "42",
  "destinationId": "73",
  "departTime": "2018-01-28 20:15:00",
  "tripCode": "OP4273",
  "driverName": "Adrian Pang",
  "waybillList": 
  [
    {
      "pnr": "OPM20180128A",
      "name": "John Doe",
      "phoneNumber": "+605864783920",
      "idNumber": "JohnDoe42",
      "category": "Adult",
      "seatNumber": "4B",
      "ticketNumber": "OPM4273201801284B"
    },
    {
      "pnr": "OPM20180128B",
      "name": "Jane Doe",
      "phoneNumber": "+605864783929",
      "idNumber": "JaneDoe42",
      "category": "Adult",
      "seatNumber": "4A",
      "ticketNumber": "OPM4273201801284A"
    }
  ]
}
```

CTS will call the Bus Operator System(BOS) to get the full information (passenger with seat number and ticket number) about the trip or bus details at any given time. 

### HTTP Request

`GET {getSeatAvailabilityApiURL}?TripId=0B101010&SeatNo=4B&OperatorId=421&OperatorCode=OPM&SourceId=42&destinationId=73`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
OperatorCode | String | OPM
departDate | String | yyyy-MM-dd

### Response Format

Parameter | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | 0:Failure 1: Success
errorMessage | String | Success / The reason for failure 
SourceId | String | 42
destinationId | String | 73
tripCode | String | Unique trip code to be printed in ticket
driverName | String |Bus river name for the trip
departTime | String | yyyy-MM-dd HH:mm:ss
waybillList | Array of WayBill | Refer below

### WayBill Object

Field | Type | Format / Example
--------- | ------- | -----------
pnr | String | PNR of the booking
name | String | Passenger name
phoneNumber | String | Passenger contact number
busNumber | String | Bus number
idNumber | String | JohnDoe42
category | String | Adult / Child / Senior Citizen / Disabled
seatNumber | String | Passenger seat number
ticketNumber | String | Passenger ticket number (unique to each passenger in a bus)


<aside class="notice">
{waybillApiURL} — Operator specific waybill API URL that we'll call
</aside>


# Retrieve ticket

```cURL
curl "{retrieveTicketApiURL}?tripId=0B101010&OperatorCode=OPM&pnr=OPM20180128A"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 0,
  "errorMessage": "SUCCESS",
  "tripId": "0B101010",
  "tripCode": "OP4273",
  "departTime": "2018-01-28 20:15:00",
  "arrivalTime": "2018-01-29  02:15:00",
  "name": "John Doe",
  "email": "john@doe.co",
  "phoneNumber": "+605864783920",
  "idNumber": "JohnDoe42",
  "category": "Adult",
  "seatNumber": "4B",
  "ticketFare": "42.95"
  "ticketNumber": "OPM4273201801284B"
}
```

CTS will call the Bus Operator System(BOS) at any given time to get the booking details of a successful booking using pnr.

### HTTP Request

`GET {retrieveTicketApiURL}?tripId=0B101010&OperatorCode=OPM&pnr=OPM20180128A`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
OperatorCode | String | OPM
pnr | String | Unique booking ID 

### Response Format

Parameter | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | Refer below
errorMessage | String | Refer below
TripId | String | Unique Trip id for validation
TripCode | String | Unique trip code to be printed in ticket
departTime | String | yyyy-MM-dd HH:mm:ss
arrivalTime | String | yyyy-MM-dd HH:mm:ss
name | String | John Doe
email | String | john@doe.co
phoneNumber | String | +605864783920
idNumber | String | JohnDoe42
category | String | Adult / Child / Senior Citizen / Disabled
seatNumber | String | 4B
ticketFare | String | Fare for the ticket
ticketNumber | String | OPM4273201801284B

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
104 | PNR not found
105 | Any other error needs to be sent here.

<aside class="notice">
{retrieveTicketApiURL} — Operator specific retrieve ticket API URL that we'll call
</aside>

# Cancel ticket

```cURL
curl "{cancelTicketApiURL}?tripId=0B101010&OperatorCode=OPM&pnr=OPM20180128A"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 0,
  "errorMessage": "SUCCESS"
}
```

CTS will call the Bus Operator System(BOS) to cancel the booking using pnr which was given when make booking. Additionally, we might send related trip information as well.

### HTTP Request

`GET {cancelTicketApiURL}?tripId=0B101010&OperatorCode=OPM&pnr=OPM20180128A`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
OperatorCode | String | OPM
pnr | String | Unique booking ID 

### Response Format

Parameter | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | Refer below
errorMessage | String | Refer below

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
-1 | PNR not found
-2 | Already cancelled
-3 | Failed to cancel

<aside class="notice">
{cancelTicketApiURL} — Operator specific cancel ticket API URL that we'll call
</aside>