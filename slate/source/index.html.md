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

# {integrationUrl}

This is going to be the operator integration rest api base url and api endpoints must be the same as given below.

For example, If base url is `http://api.testoperator.com/integration/klang/`, then we'll call `http://api.testoperator.com/integration/klang/trips` to get the trips.

### Content-Type

All the api response should return `Content-Type` header as `application/json`

# Get trips

```cURL
curl "{integrationUrl}/trips?sourceCityId=42&destinationCityId=73&departDate=2018-01-28"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
    "errorCode": 1,
    "errorMessage": "SUCCESS",
    "data": [
        {
            "tripId": "2012778",
            "tripCode": "KLGKB",
            "fromCityId": "35",
            "toCityId": "6",
            "departTime": "2017-12-29 01:00:00",
            "arrivalTime": "2017-12-29 10:00:00",
            "availableSeats": 30,
            "adultFare": 100,
            "childFare": 90,
            "seniorFare": 80,
            "disabledFare": 70,
            "insurance": 2,
            "operatorCode": "CTBH"
        },
        {
            "tripId": "2012789",
            "tripCode": "UIKD",
            "fromCityId": "35",
            "toCityId": "6",
            "departTime": "2017-12-29 01:00:00",
            "arrivalTime": "2017-12-29 10:00:00",
            "availableSeats": 4,
            "adultFare": 80,
            "childFare": 60,
            "seniorFare": 60,
            "disabledFare": 50,
            "insurance": 2,
            "operatorCode": "UI"
        }
    ]
}
```

CTS will call the Bus Operator System(BOS) to get the trip list for the given search criteria (source city, destination city, journey date). The response will have trip information and fare details.

### HTTP Request

`GET {integrationUrl}/trips?sourceCityId=42&destinationCityId=73&operatorCode=CTBH&operatorId=1&departDate=2018-01-28`

### City List

You can get the cts system supported city list from below link

`http://api.klangsentral.com/pub/city`

### Query Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
sourceCityId | Integer | 42
destinationCityId | Integer | 73
operatorCode | String | operator identifier CTBH
operatorId | Integer | 1
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
fromCityId | String | source city id
toCityId | String | destination city id
departTime | String | yyyy-MM-dd HH:mm:ss
arrivalTime | String | yyyy-MM-dd HH:mm:ss
availableSeats | Integer | Number of seats available for booking, exculde booked already
adultFare | double | 42.0
childFare | double | 37.0
seniorFare | double | 37.0
disabledFare | double | 37.0
insurance | double | 2.0 (Optional) Not inclusive of above fares
operatorCode | String | operator identifier


### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure - Send error message if any
101 | Trips Cancelled
103 | Trip does not exist


<aside class="notice">
{integrationUrl}/trips — Operator specific API URL that we'll call
</aside>


# Get seat map

```cURL
curl "{integrationUrl}/seatmap?tripId=0B101010&operatorCode=OPM&operatorId=421"
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
                    "seatNumber": "1",
                    "status": 0,
                    "rowIndex": 0,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 0,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "2",
                    "status": 0,
                    "rowIndex": 0,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "3",
                    "status": 0,
                    "rowIndex": 0,
                    "columnIndex": 3
                }
            ],
            [
                {
                    "seatNumber": "4",
                    "status": 0,
                    "rowIndex": 1,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 1,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "5",
                    "status": 4,
                    "rowIndex": 1,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "6",
                    "status": 0,
                    "rowIndex": 1,
                    "columnIndex": 3
                }
            ],
            [
                {
                    "seatNumber": "7",
                    "status": 0,
                    "rowIndex": 2,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 2,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "8",
                    "status": 0,
                    "rowIndex": 2,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "9",
                    "status": 0,
                    "rowIndex": 2,
                    "columnIndex": 3
                }
            ],
            [
                {
                    "seatNumber": "10",
                    "status": 4,
                    "rowIndex": 3,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 3,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "11",
                    "status": 4,
                    "rowIndex": 3,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "12",
                    "status": 4,
                    "rowIndex": 3,
                    "columnIndex": 3
                }
            ],
            [
                {
                    "seatNumber": "13",
                    "status": 0,
                    "rowIndex": 4,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 4,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "14",
                    "status": 4,
                    "rowIndex": 4,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "15",
                    "status": 0,
                    "rowIndex": 4,
                    "columnIndex": 3
                }
            ],
            [
                {
                    "seatNumber": "16",
                    "status": 0,
                    "rowIndex": 5,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 5,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "17",
                    "status": 0,
                    "rowIndex": 5,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "18",
                    "status": 4,
                    "rowIndex": 5,
                    "columnIndex": 3
                }
            ],
            [
                {
                    "seatNumber": "19",
                    "status": 0,
                    "rowIndex": 6,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 6,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "20",
                    "status": 0,
                    "rowIndex": 6,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "21",
                    "status": 0,
                    "rowIndex": 6,
                    "columnIndex": 3
                }
            ]
        ],
        "upperDeck": [
            [
                {
                    "seatNumber": "22",
                    "status": 0,
                    "rowIndex": 0,
                    "columnIndex": 0
                },
                {
                    "seatNumber": "#",
                    "status": 0,
                    "rowIndex": 0,
                    "columnIndex": 1
                },
                {
                    "seatNumber": "23",
                    "status": 0,
                    "rowIndex": 1,
                    "columnIndex": 2
                },
                {
                    "seatNumber": "24",
                    "status": 0,
                    "rowIndex": 1,
                    "columnIndex": 3
                }
            ]
        ]
    }
}
```

CTS will call the Bus Operator System(BOS) to get seat map or bus seat details for the trip using tripId

### HTTP Request

`GET {integrationUrl}/seatmap?tripId=0B101010&operatorCode=OPM&operatorId=42`

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

### NOTE

* upperdeck will be empty jsonArray if not available
* # as a seatNumber can be passed to show empty or walking space in the seat map


### Seatmap Status

* status => 0 - AVAILABLE

* status => 1 - BOOKED

* status => 2 - BLOCKED

* status => 3 - RESERVED

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure - Any other error needs to be sent here
101 | Trips Cancelled
103 | Trip does not exist

<aside class="notice">
{integrationUrl}/seatmap — Operator specific seat map API URL that we'll call
</aside>


# Get seat availability

```cURL
curl "{integrationUrl}/seatAvailability?tripId=0B101010&operatorCode=OPM&sourceCityId=42&destinationCityId=73&seatNumber=12&departDate=2017-12-12`
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
            "adultFare": 100.0,
            "childFare": 0.0,
            "seniorFare": 0.0,
            "disabledFare": 0.0,
            "insurance": 2.0,
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

`GET {integrationUrl}/seatAvailability?tripId=0B101010&operatorCode=OPM&departDate=2017-12-12&seatNumber=2B&seatNumber=3C`

### Query Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
seatNumber | String | 4B, but if multiple seats we will send same parameter multiple times
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
sourceCityId | String | source city id
destinationCityId | String | destination city id
departDate | String | yyyy-MM-dd HH:mm:ss
arrivalDate | String | yyyy-MM-dd HH:mm:ss
fareDetails | JSONObject | refer below
operatorCode | String | OPM
seatAvailability | JSONObject | key will be the seatNumber and value will be the boolean(true if available, false if not available) Example: "4B": true

### Fare Details JSON Object
Field | Type | Format / Example
--------- | ------- | -----------
adultFare | double | 20.0
childFare | double | 10.0
seniorFare | double | 12.0
disabledFare | double | 10.0
insurance | double | 2.0 (Optional) Not inclusive of above fares
currency | String | currency code (iso format)

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure - Any other error needs to be sent here

<aside class="notice">
{integrationUrl}/seatAvailability — Operator specific seat availability API URL that we'll call
</aside>

# Make booking

```cURL
curl "{integrationUrl}/makebooking"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
  -d "{
      "tripId": "123456",
      "tripCode": "0B101010",
      "sourceCityId": "42",
      "destinationCityId": "2",
      "operatorCode": "OPM",
      "departDate": "2018-01-28 04:00:00",
      "passengerTicketList": [
        {
          "name": "John Doe",
          "email": "john@doe.co",
          "phoneNumberCode": 60,
          "phoneNumber": "5864783920",
          "idNumber": "JohnDoe42",
          "category": "A",
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
  "errorCode": 1,
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
              "phoneNumberCode": 60,
              "phoneNumber": "5864783920",
              "idNumber": "JohnDoe42",
              "category": "A",
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

### HTTP Request

`POST {integrationUrl}/makebooking`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
tripCode | String | Unique Identifier representing the whole trip
sourceCityId | String | 42
destinationCityId | String | 73
operatorCode | String | OPM
departDate | String | yyyy-MM-dd HH:mm:ss
passengerTicketList | Array of PassengerDetails | Refer below


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
operatorCode | String | OPM
passengerTicketList | Array of PassengerDetails | Refer below

### PassengerTicket Object

Field | Type | Format / Example
--------- | ------- | -----------
name | String | John Doe
email | String | john@doe.co
phoneNumberCode | Integer | phone number country code iso format
phoneNumber | String | 5864783920
idNumber | String | JohnDoe42
category | String | A - Adult / C - Child / S - Senior Citizen / O - Disabled
seatNumber | String | 4B
gender | String | M - Male / F - Female / U - Unspecified
nationality | String | Malaysian / Singaporean etc
ticketNumber | String | OPM4273201801284B

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure - Any other error needs to be sent here

<aside class="notice">
{integrationUrl}/makebooking — Operator specific make booking API URL that we'll call
</aside>


# Get WayBill

```cURL
curl "{integrationUrl}/waybill?tripId=123456&tripCode=KLGKB&operatorCode=OPM&departDate=2018-01-28"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
    "errorCode": 1,
    "errorMessage": "SUCCESS",
    "data": {
        "operator": "Ctb Holidays",
        "completeRoute": "Klang - Shah Alam - Kuala Lumpur - Bentong - Gua Musang - Kuala Krai - Kok Lanas - Pengkalan Chepa - Pengkalan Kubor - Teluk Mesira - Kota Bharu",
        "tripId": "123456",
        "tripCode": "KLGKB",
        "busDriver1": "Driver Name 1",
        "busDriver2": "Driver Name 2",
        "departDate": "2017-12-29",
        "departTime": "01:00:00.0",
        "busPlateNo": "AB-1234",
        "passengerTicketList": [
            {
                "idNumber": "83884343",
                "name": "Ashrith test",
                "sourceCityId": 35,
                "destinationCityId": 6,
                "route": "Klang - Kota Bharu",
                "phoneNumber": "601234567890",
                "category": "A",
                "seatNumber": "4C",
                "ticketNumber": "CTBH10047",
                "pnr": "ABC1234",
                "ctsBooking": 1
            },
            {
                "idNumber": "83884343",
                "name": "John Smith",
                "sourceCityId": 35,
                "destinationCityId": 6,
                "route": "Klang - Kota Bharu",
                "phoneNumber": "601234567890",
                "category": "A",
                "seatNumber": "4A",
                "ticketNumber": "CTBH10049",
                "pnr": "CBA321",
                "ctsBooking": 0
            }
        ],
        "pickUp": [
            {
                "pointName": "Klang",
                "noOfPax": 2
            }
        ],
        "dropOff": [
            {
                "pointName": "Kota Bharu",
                "noOfPax": 2
            }
        ]
    }
}
```

CTS will call the Bus Operator System(BOS) to get the full information (passenger with seat number and ticket number) about the trip or bus details at any given time.

### HTTP Request

`GET {integrationUrl}/waybill?tripId=123456&tripCode=KLGKB&operatorId=421&operatorCode=OPM&departDate=2017-12-12`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
tripId | String | Unique Identifier representing the trip
tripCode | String | Unique Identifier representing the whole trip
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
operator | String | bus operator name
completeRoute | String | complete route of the trip (Concatenation pickup and dropoff city names in a sequence order)
tripId | String | unique identifier of the trip
tripCode | String | unique identifier of the whole trip
departDate | String | bus depart date (yyyy-MM-dd)
departTime | String | bus depart time (HH:mm:ss)
busDriver1 | String | optional
busDriver2 | String | optional
busPlateNo | String | bus number plate number
passengerTicketList | JSON Array | refer below
pickUp | JSON Array | refer below
dropOff | JSON Array | refer below

### PassengerTicket Object

Field | Type | Format / Example
--------- | ------- | -----------
name | String | John Doe
phoneNumber | String | +605864783920
idNumber | String | JohnDoe42
category | String | A - Adult / C - Child / S - Senior Citizen / O - Disabled
seatNumber | String | 4B
ticketNumber | String | OPM4273201801284B
sourceCityId | Integer | 35
destinationCityId | Integer | 6
route | String | Passenger travelling route(pickup and dropoff city name)
pnr | String | Unique identifier for a booking
ctsBooking | Integer | 1 - Booked by Klang terminal / 0 - Booked by others

### NOTE

* If the trip has not sold any tickets, return empty passengerTicketList

### Waybill Point Object
Field | Type | Format / Example
--------- | ------- | -----------
pointName | String | pick up or drop off point name
noOfPax | int | number of passengers picking up or dropping off for this point name

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure - Any other error needs to be sent here.

<aside class="notice">
{integrationUrl}/waybill — Operator specific waybill API URL that we'll call
</aside>


# Retrieve booking

```cURL
curl "{integrationUrl}/retrieveBooking?operatorCode=OPM&pnr=OPM20180128A"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
```

> The above command should return JSON structured like this:

```json
{
    "errorCode": 1,
    "errorMessage": "Success",
    "data": {
        "tripId": "2012778",
        "tripCode": "KLGKB",
        "departTime": "2017-12-29 01:00:00",
        "arrivalTime": "2017-12-29 10:00:00",
        "sourceCityId": 35,
        "destinationCityId": 6,
        "busNumber": "AB1234",
        "soldDate": "2018-01-12 04:00:00",
        "bookingStatus": 3,
        "passengerTicketList": [
            {
                "name": "John Smith",
                "email": "john@example.com",
                "phoneNumberCode": 60,
                "phoneNumber": "1234567890",
                "idNumber": "83884343",
                "seatNumber": "4A",
                "category": "A",
                "gender": "M",
                "nationality": "Malaysian",
                "ticketNumber": "CTBH10049",
                "ticketFare": 100,
                "insurance": 2
            }
        ]
    }
}
```

CTS will call the Bus Operator System(BOS) at any given time to get the booking details of a successful booking using pnr.

### HTTP Request

`GET {integrationUrl}/retrieveBooking?operatorCode=OPM&pnr=OPM20180128A`

### Parameters

Parameter | Type | Format / Example
--------- | ------- | -----------
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
busNumber | String | bus plate number
soldDate | String | The date ticket was sold (yyyy-MM-dd HH:mm:ss)
bookingStatus | Integer | 3 => booked / -3 => cancelled / 4 => reserved / 0 => other
passengerTicketList | JSON Array | refer below

### PassengerTicket Object

Field | Type | Format / Example
--------- | ------- | -----------
name | String | John Doe
email | String | john@doe.co
phoneNumberCode | Integer | phone number country code iso format
phoneNumber | String | 5864783920
idNumber | String | JohnDoe42
category | String | A - Adult / C - Child / S - Senior Citizen / O - Disabled
seatNumber | String | 4B
gender | String | M - Male / F - Female / U - Unspecified
nationality | String | Malaysian / Singaporean etc
ticketNumber | String | OPM4273201801284B
ticketFare | double | 10.0
insurance | double | 2.0 (Optional) Not inclusive of ticketFare


### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure - Any other error needs to be sent here.
104 | PNR not found

<aside class="notice">
{integrationUrl}/retrieveBooking — Operator specific retrieve booking API URL that we'll call
</aside>

# Cancel booking

```cURL
curl "{integrationUrl}/cancelbooking"
  -u "sk_test_BQokikJOvBiI2HlWgH4olfQ2:"
  -d "{
        "pnr":"K3WFW42",
        "operatorCode": "OPW",
        "operatorId": 12,
        "reason":"No reason testing"
      }"
```

> The above command should return JSON structured like this:

```json
{
  "errorCode": 1,
  "errorMessage": "SUCCESS"
}
```

CTS will call the Bus Operator System(BOS) to cancel the booking using pnr which was given when make booking. Additionally, we might send related trip information as well.

### HTTP Request

`POST {integrationUrl}/cancelbooking`

### Cancel Booking Object

Parameter | Type | Format / Example
--------- | ------- | -----------
pnr | String | Unique booking ID
operatorCode | String | OPM
operatorId | Integer | unique identifier of operator
reason | String | reason for canceling

### Response Format

Parameter | Type | Format / Example
--------- | ------- | -----------
errorCode | Integer | Refer below
errorMessage | String | Refer below

### Error descriptions

Error code | Error message
--------- | -------
1 | Success
0 | Failure - Any other error needs to be sent here.
-1 | PNR not found
-2 | Already cancelled
-3 | Failed to cancel

<aside class="notice">
{integrationUrl}/cancelbooking — Operator specific cancel booking API URL that we'll call
</aside>

# City List

```cURL
curl "http://api.klangsentral.com/pub/city"
```

```json
[
    {
        "id": 350,
        "name": "Jawi",
        "code": "JAWI"
    },
    {
        "id": 230,
        "name": "Yong Peng",
        "code": "YP"
    },
    {
        "id": 351,
        "name": "Universiti Malaysia Pahang",
        "code": "UMP"
    }
]
```

###

City List to map
