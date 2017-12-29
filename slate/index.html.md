---
title: KLANG SENTRAL API Specification

language_tabs: # must be one of https://git.io/vQNgJ
  - cURL

includes:
  - errors

search: true
---

# Introduction

The technical document specifies the interaction point for Bus Operator System(BOS) and the Centralised Ticketing system Klang Terminal. The project will enable the Klang Sentral CTS to sell the Bus operator inventory via real time API integration. 

The Bus Operator System(BOS) integrating with Klang Sentral CTS needs to provide the following RESTFUL API with the mentioned JSON Structure. 

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
    "errorCode": 1,
    "errorMessage": SUCCESS,
    "data": [
        {
            "tripId": "2012778",
            "tripCode": "KLGKB",
            "fromCityId": "35",
            "toCityId": "6",
            "departTime": "2017-12-29 01:00:00",
            "arrivalTime": "2017-12-29 10:00:00",
            "adultFare": 100,
            "childFare": 90,
            "seniorFare": 80,
            "disabledFare": 70,
            "operatorCode": "CTBH"
        },
        {
            "tripId": "2012789",
            "tripCode": "UIKD",
            "fromCityId": "35",
            "toCityId": "6",
            "departTime": "2017-12-29 01:00:00",
            "arrivalTime": "2017-12-29 10:00:00",
            "adultFare": 80,
            "childFare": 60,
            "seniorFare": 60,
            "disabledFare": 50,
            "operatorCode": "UI"
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
curl "{getSeatMapApiURL}?tripId=0B101010&operatorCode=OPM&operatorId=421"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 1,
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
curl "{getSeatAvailabilityApiURL}?tripId=0B101010&seatNo=4B&operatorId=421&operatorCode=OPM&sourceCityId=42&destinationCityId=73&seatNumber=12&departDate=2017-12-12`
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
    "errorCode": 1,
    "errorMessage": "Success",
    "data": {
        "sourceCityId": "35",
        "destinationCityId": "6",
        "operatorCode": "CTBH",
        "departDate": "2017-12-29 01:00:00",
        "arrivalDate": "2017-12-29 10:00:00",
        "fareDetails": {
            "adultFare": "100.0",
            "childFare": "0.0",
            "seniorCitizenFare": "0.0",
            "disabledFare": "0.0",
            "currency": "MYR"
        },
        "seatAvailability": {
            "2B": true,
            "3C": true
        }
    }
}
```

CTS will call the Bus Operator System(BOS) before calling make booking. The system needs to ensure that the selected seats are available and no changes happened on the selected trip.

### HTTP Request

`GET {getSeatAvailabilityApiURL}?tripId=0B101010&operatorId=421&operatorCode=OPM&departDate=2017-12-12&seatNumber=2B&seatNumber=3C`

### Query Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
seatNumber | String | 4B, but if multiple seats we will send same parameter multiple times
operatorId | String | 421
operatorCode | String | OPM
departDate | String | 2017-12-12

### Response Format

Field | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | 0: Failure 1: Success
errorMessage | String | Brief description for failure
data | JSON object | Refer below

### Seat availability JSON object

Field | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
seatNumber | String | ["4B", "5B"]
operatorId | String | 421
operatorCode | String | OPM
sourceCityId | Integer | 1
destinationCityId | Integer | 2


<aside class="notice">
{getSeatAvailabilityApiURL} — Operator specific seat availability API URL that we'll call
</aside>

# Make booking

```cURL
curl "{makeBookingApiURL}"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
  -d "{
      "tripId": "123456",
      "tripCode": "0B101010",
      "sourceCityId": "42",
      "destinationCityId": "2",
      "operatorCode": "OPM",
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
  "data": {
      "tripId": "0B101010",
      "tripCode": "OP4273",
      "pnr": "OPM20180128",
      "totalAmount": "142.95",
      "operatorCode": "OPM",
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

### PassengerTicket Object

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
data | JSON Object | Refer Below

### MakeBooking Data Object

Field | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Trip id for validation
tripCode | String | Unique trip code to be printed in ticket
pnr | String | Unique booking ID
totalAmount | String | 154.00 (It will be used for data validity)
passengerTicketList | Array of PassengerDetails | Refer below

### PassengerTicket Object

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
curl "{waybillApiURL}?tripId=0B101010&operatorCode=OPM&departDate=2018-01-28"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 0,
  "errorMessage": "SUCCESS",
  "data": {
      "busNumber": "SBUS707",
      "SourceId": "42",
      "destinationId": "73",
      "departTime": "2018-01-28 20:15:00",
      "tripCode": "OP4273",
      "driverName": "Adrian Pang",
      "passengerTicketList": [
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
}
```

CTS will call the Bus Operator System(BOS) to get the full information (passenger with seat number and ticket number) about the trip or bus details at any given time. 

### HTTP Request

`GET {getSeatAvailabilityApiURL}?tripId=0B101010&operatorId=421&operatorCode=OPM&departDate=2017-12-12`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
operatorId | Integer | 12
operatorCode | String | OPM
departDate | String | yyyy-MM-dd

### Response Format

Parameter | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | 0:Failure 1: Success
errorMessage | String | Success / The reason for failure 
data | JSON Object | Refer below


### WayBill Object

Field | Type | Format / Example
--------- | ------- | -----------
pnr | String | PNR of the booking
name | String | Passenger name
phoneNumber | String | Passenger contact number
busNumber | String | Bus number
passengerTicketList | JSON Array | refer below

### PassengerTicket Object

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
{waybillApiURL} — Operator specific waybill API URL that we'll call
</aside>


# Retrieve booking

```cURL
curl "{retrieveBookingApiURL}?tripId=0B101010&operatorCode=OPM&pnr=OPM20180128A"
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
  "sourceCityId": 12,
  "destinationCityId": 13,
  "passengerTicketList": [
    {
        "name": "John Doe",
        "email": "john@doe.co",
        "phoneNumber": "+605864783920",
        "idNumber": "JohnDoe42",
        "category": "Adult",
        "seatNumber": "4B",
        "ticketFare": "42.95"
        "ticketNumber": "OPM4273201801284B"
    },
    {
        "name": "David Doe",
        "email": "david@doe.co",
        "phoneNumber": "+605864783921",
        "idNumber": "David1234",
        "category": "Adult",
        "seatNumber": "4B",
        "ticketFare": "42.95"
        "ticketNumber": "OPM4273201801284B"
    }
  ] 
}
```

CTS will call the Bus Operator System(BOS) at any given time to get the booking details of a successful booking using pnr.

### HTTP Request

`GET {retrieveBookingApiURL}?tripId=0B101010&OperatorCode=OPM&pnr=OPM20180128A`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
operatorCode | String | OPM
pnr | String | Unique booking ID 

### Response Format

Field | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | 0:Failure 1: Success
errorMessage | String | Success / The reason for failure 
data | JSON Object | Refer below

### Retrieve Booking Object

Field | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Trip id for validation
tripCode | String | Unique trip code to be printed in ticket
departTime | String | yyyy-MM-dd HH:mm:ss
arrivalTime | String | yyyy-MM-dd HH:mm:ss
sourceCityId | Integer | 10
destinationCityId | Integer | 20
passengerTicketList | JSON Array | refer below

### PassengerTicket Object

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
ticketFare | double | 10.0


### Error descriptions

Error code | Error message
--------- | -------
1 | Success
104 | PNR not found
105 | Any other error needs to be sent here.

<aside class="notice">
{retrieveBookingApiURL} — Operator specific retrieve booking API URL that we'll call
</aside>

# Cancel booking

```cURL
curl "{cancelBookingApiURL}?tripId=0B101010&operatorCode=OPM&pnr=OPM20180128A"
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

`GET {cancelBookingApiURL}?tripId=0B101010&operatorCode=OPM&pnr=OPM20180128A`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
operatorCode | String | OPM
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
{cancelBookingApiURL} — Operator specific cancel booking API URL that we'll call
</aside> 
