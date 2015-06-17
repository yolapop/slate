---
title: Indokter (Temp) API Documentation

language_tabs:
  - http

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Internal use only. All traffic through this API must be in HTTPS. TODO: make this [JSON API](http://jsonapi.org/format) compliant.

# Authentication

TODO: make API key.


# Hospitals

## Get All Hospitals

Not yet implemented.

## Create New Hospital

```http
POST /api/hospitals HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "name": "Siloam Hospitals",
  "region": "557f8ac64d35ac3835b52919",
  "address": "Jalan Garnisun Dalam Setiabudi, Jakarta Selatan",
  "latlong": [ -6.219401, 106.81649 ],
  "email": "admin@siloamhospitals.com",
  "password": "abcde",
  "insurances": [ "AXA", "BPJS" ],
  "phone": "+622125668000",
  "fax": "+62215475890",
  "doctors": [ "557b2acf0dba53980b5eaa53", "557f8b6b4d35ac3835b5291f" ],
  "logo_url": "http://domain.com/photo250.jpeg"
}
```

> If success, the response header is `201 Created` with the resource in the body

**This API is for administrators only.**

### HTTP Request

`POST http://domain.com/api/hospitals`

### REQUEST BODY

Field | Default | Description
--------- | ------- | -----------
name | required | The hospital name.
region | required | The ID of region where the hospital is in.
address | null | The hospital address.
latlong | required | Two elements array that contains latitute and longitude of the hospital. It's required for search purpose.
email | required | Email for login. Must be unique.
password | required | Password for login.
insurances | null | List of insurances that the hospital supports.
phone | null | The hospital's phone number.
fax | null | The hospital's fax number.
doctors | null | List of doctors' ID who work in the hospital.
logo_url | null | URL of hospital logo. Width: 250px.

<aside class="warning">If you're not using an administrator API key, this API call will return <code>403 Forbidden</code>.</aside>

# Doctors

## Get All Doctors

Forbidden.

## Get All Doctors In A Hospital

Not yet implemented.

## Create New Doctor

```http
POST /api/doctors HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "name": "Eric R. Tate",
  "str_no": "1234567890",
  "email": "EricRTate@rhyta.com",
  "phone": "+19375656998",
  "specialization": "557b4a94d0cd9dcc10e35a5b",
  "since": "2015-06-16T00:00:00.000Z",
  "until": "2016-06-16T00:00:00.000Z",
  "schedules": [
    {
      day: 1,
      hospital: "557b27f5d407310f2a74942f"
      from: { hour: 8, minute: 0 },
      to: { hour: 9, minute: 0 }
    },
    {
      day: 2,
      hospital: "557d6c4bf6d022100c1bd2fb"
      from: { hour: 11, minute: 30 },
      to: { hour: 12, minute: 45 }
    }
  ],
  "hospitals": [ "557b27f5d407310f2a74942f", "557d6c4bf6d022100c1bd2fb" ]
}
```

> If success, the response header is `201 Created` with the resource in the body

**This API is for administrators only.**

### HTTP Request

`POST http://domain.com/api/doctors`

### REQUEST BODY

Field | Default | Description
--------- | ------- | -----------
name | required | The doctor's name.
str_no | required | Surat Tanda Registrasi.
email | required | The doctor's work email, must be unique.
phone | required | The doctor's work phone number.
specialization | required | The ID of doctor's specialization.
since | required | Registration date to Indokter. (format ISO8601 UTC)
until | required | Expiration date from Indokter. (format ISO8601 UTC)
schedules | null | List of doctor's schedules. (See `SCHEDULES BODY`)
hospitals | null | List of hospitals' ID where the doctor works.

### SCHEDULES BODY

Field | Default | Description
--------- | ------- | -----------
day | required | Number of work day. (1: monday, 2: tuesday, etc)
hospital | required | The hospital ID for this schedule.
from | h:7, m:0 | Start time of working.
to | h:7, m:0 | End time of working.

<aside class="warning">If you're not using an administrator API key, this API call will return <code>403 Forbidden</code>.</aside>

# Specializations

## Get All Specializations

Not yet implemented.

## Create New Specialization

```http
POST /api/specializations HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "formal_name": { en: "Pediatrician", id: "Spesialis Anak" },
  "display_name": { en: "Kids", id: "Anak" },
  "abbr": { en: "MD", id: "SpA" },
  "category": "specialty",
  "complaints": [
    { en: "Kids consultation", id: "Konsultasi anak" },
    { en: "Cough", id: "Batuk" }
  ]
}
```

> If success, the response header is `201 Created` with the resource in the body

**This API is for administrators only.**

### HTTP Request

`POST http://domain.com/api/specializations`

### REQUEST BODY

Field | Default | Description
--------- | ------- | -----------
formal_name | required | Formal name of specialization. English version is required.
display_name | required | Display name of specialization. English version is required.
abbr | required | Abbreviated job title of specialization. English version is required.
category | required | Either `specialty`, `dentist`, or `generic`.
complaints | null | List of complaints in this specialty. English version is required.

<aside class="warning">If you're not using an administrator API key, this API call will return <code>403 Forbidden</code>.</aside>

# Requested Appointments

Requested Appointments is Waiting List, i.e the appointments that have to be **approved** or **rejected** by a hospital admin.

## Get A Requested Appointment

```http
GET /api/appointments/557d6c43f6d022100c1bd2fa HTTP/1.1
Accept: application/vnd.api+json
```

> The above command returns JSON structured like this:

```json
{
  "code": 200,
  "message": {
    "_id": "557d6c43f6d022100c1bd2fa",
    "created_at": "2015-06-14T11:57:55.440Z",
    "updated_at": "2015-06-14T14:40:01.185Z",
    "user": "557b2bc80dba53980b5eaa55",
    "hospital": "557b291afaef37c8147d717d",
    "doctor": "557b2acf0dba53980b5eaa53",
    "from_time": "2015-07-10T11:00:00.000Z",
    "to_time": "2015-07-10T12:30:00.000Z",
    "visiting_reason": "557b27f5d407310f2a74942f"
  }
}
```

Fetch a single requested appointment, i.e. an appointment that is not yet **approved** nor **rejected** by a hospital admin.

If the appointment is already **approved** or **rejected** you will get a `301 Moved Permanently` response with the associated `Location` header.

### HTTP Request

`GET http://domain.com/api/appointments/<ID>`

### URL PARAMETERS

Parameter | Default | Description
--------- | ------- | -----------
ID | required | ID of the appointment to retrieve.

### QUERY PARAMETERS

Parameter | Default | Description
--------- | ------- | -----------
include | null | Field inclusion: `user`, `doctor`, `hospital`, `visiting_reason`.
lang | en | The language to display the resource. Either `en` or `id`.

## Get All Requested Appointments

Forbidden.

## Get All Requested Appointments In A Hospital

```http
GET /api/appointments?hospital=557b4a8dd0cd9dcc10e35a59&page=1&include=visiting_reason HTTP/1.1
Accept: application/vnd.api+json
```

> The above command returns JSON structured like this:

```json
{
  "code": 200,
  "message": {
    "page_count": 1,
    "item_count": 1,
    "current_page": "1",
    "list": [
      {
        "_id": "557d6c43f6d022100c1bd2fa",
        "created_at": "2015-06-14T11:57:55.440Z",
        "updated_at": "2015-06-14T14:40:01.185Z",
        "user": "557b2bc80dba53980b5eaa55",
        "hospital": "557b291afaef37c8147d717d",
        "doctor": "557b2acf0dba53980b5eaa53",
        "from_time": "2015-07-10T11:00:00.000Z",
        "to_time": "2015-07-10T12:30:00.000Z",
        "visiting_reason": {
          "_id": "557b27f5d407310f2a74942f",
          "en": "Kids consultation"
        }
      }
    ]
  }
}
```

Get all requested appointments in a hospital, sorted ascending by appointment time and creation time. This list is paginated with 10 items per page.

### HTTP Request

`GET http://domain.com/api/appointments?hospital=<ID>&page=<PAGE>&include=<FIELDS>`

### QUERY PARAMETERS

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | The page you want to fetch.
hospital | required | The hospital's ID.
include | null | Field inclusion: `user`, `doctor`, `hospital`, `visiting_reason`.
lang | en | The language to display the resource. Either `en` or `id`.

<aside class="warning">If you're not using an API key, this API call will return <code>403 Forbidden</code>.</aside>

## Request An Appointment To A Hospital

```http
POST /api/appointments HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "user": "557b2bc80dba53980b5eaa55",
  "hospital": "557b291afaef37c8147d717d",
  "doctor": "557b2acf0dba53980b5eaa53",
  "visiting_reason": "557b27f5d407310f2a74942f",
  "from_time": "2015-07-10T11:00:00.000Z",
  "to_time": "2015-07-10T12:30:00.000Z"
}
```

> If success, the response header is `201 Created` with the resource in the body.

> Event `new apt` will be emitted to the related hospital channel with data below.

```json
{
  "data": { "user": "..." },
  "count": 10
}
```

> `data` is the newly created appointment, `count` is total item in the Waiting List.

Make an appointment to a doctor in a hospital. This is the appointment that a hospital admin will reject or approve.

### HTTP Request

`POST http://domain.com/api/appointments`

### REQUEST BODY

Field | Default | Description
--------- | ------- | -----------
user | required | The patient's ID. (patient = user in our system)
hospital | required | The hospital's ID.
doctor | required | The doctor's ID.
visiting_reason | required | The complaint's ID. Can be found in `Specialization` collection.
from_time | required | Start time of the visit.
to_time | required | End time of the visit.

<aside class="warning">If you're not using an API key, this API call will return <code>403 Forbidden</code>.</aside>

# Rejected Appointments

## Get A Rejected Appointment

Not yet implemented.

## Reject An Appointment

```http
POST /api/rejected-appointments HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "id": "557d6c43f6d022100c1bd2fa"
}
```

> If success, the response header is `201 Created` with the resource in the body.

> Event `rjct apt` will be emitted to the related hospital channel with data below.

```json
{
  "data": { "user": "..." },
  "count": 10
}
```

> `data` is the newly rejected appointment, `count` is total item in the Waiting List.

Make an appointment to a doctor in a hospital. This is the appointment that a hospital admin will reject or approve.

### HTTP Request

`POST http://domain.com/api/rejected-appointments`

### REQUEST BODY

Field | Default | Description
--------- | ------- | -----------
ID | required | The ID of the appointment you want to reject.

<aside class="warning">If you're not using an API key, this API call will return <code>403 Forbidden</code>.</aside>

# Approved Appointments

## Get An Approved Appointment

```http
GET /api/approved-appointments/557b32d8ec6036e00e57995a HTTP/1.1
Accept: application/vnd.api+json
```

> The above command returns JSON structured like this:

```json
{
  "code": 200,
  "message": {
    "_id": "557b32d8ec6036e00e57995a",
    "created_at": "2015-06-12T19:28:24.986Z",
    "updated_at": "2015-06-14T07:19:58.135Z",
    "user": "557b2bc80dba53980b5eaa55",
    "hospital": "557b291afaef37c8147d717d",
    "doctor": "557b2acf0dba53980b5eaa53",
    "from_time": "2015-07-10T11:00:00.000Z",
    "to_time": "2015-07-10T12:30:00.000Z",
    "visiting_reason": "557b27f5d407310f2a74942f",
    "approved_time": "2015-06-16T07:19:00.000Z",
    "approved_at": "2015-06-16T02:50:00.000Z",
    "status": "accepted"
  }
}
```

### HTTP Request

`GET http://domain.com/api/approved-appointments/<ID>?include=<FIELDS>&lang=<LANG>`

### URL PARAMETERS

Parameter | Default | Description
--------- | ------- | -----------
ID | required | ID of the appointment to retrieve.

### QUERY PARAMETERS

Parameter | Default | Description
--------- | ------- | -----------
include | null | Field inclusion: `user`, `doctor`, `hospital`, `visiting_reason`.
lang | en | The language to display the resource. Either `en` or `id`.

### RESPONSE BODY
Field | Description
--------- | -----------
status | `waiting`: the patient not yet confirmed the appointment time. `accepted`: the patient accepted the appointment and ready to go to hospital. `cancelled`: the patient cancelled the appointment.

<aside class="warning">If you're not using an API key, this API call will return <code>403 Forbidden</code>.</aside>

## Get All Approved Appointment In A Hospital

```http
GET /api/approved-appointments?hospital=557b291afaef37c8147d717d HTTP/1.1
Accept: application/vnd.api+json
```

> The above command returns JSON structured like this:

```json
{
  "code": 200,
  "message": {
    "page_count": 1,
    "item_count": 1,
    "current_page": 1,
    "list": [
      {
        "_id": "557b32d8ec6036e00e57995a",
        "created_at": "2015-06-12T19:28:24.986Z",
        "updated_at": "2015-06-14T07:19:58.135Z",
        "user": "557b2bc80dba53980b5eaa55",
        "hospital": "557b291afaef37c8147d717d",
        "doctor": "557b2acf0dba53980b5eaa53",
        "from_time": "2015-07-10T11:00:00.000Z",
        "to_time": "2015-07-10T12:30:00.000Z",
        "visiting_reason": "557b27f5d407310f2a74942f",
        "approved_time": "2015-06-16T07:19:00.000Z",
        "approved_at": "2015-06-16T02:50:00.000Z",
        "status": "waiting"
      }
    ]
  }
}
```

Get all approved appointments in a hospital, sorted ascending by approved time and creation time. This list is paginated with 10 items per page.

### HTTP Request

`GET http://domain.com/api/approved-appointments?hospital=<ID>&page=<PAGE>&include=<FIELDS>&lang=<LANG>`

### QUERY PARAMETERS

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | The page you want to fetch.
hospital | required | The hospital's ID.
include | null | Field inclusion: `user`, `doctor`, `hospital`, `visiting_reason`.
lang | en | The language to display the resource. Either `en` or `id`.

### RESPONSE BODY
Field | Description
--------- | -----------
status | `waiting`: the patient not yet confirmed the appointment time. `accepted`: the patient accepted the appointment and ready to go to hospital. `cancelled`: the patient cancelled the appointment.

<aside class="warning">If you're not using an API key, this API call will return <code>403 Forbidden</code>.</aside>

## Approve An Appointment

```http
POST /api/approved-appointments HTTP/1.1
Content-Type: application/vnd.api+json
Accept: application/vnd.api+json

{
  "id": "557b32d8ec6036e00e57995a",
  "approved_time": "2015-06-16T07:19:00.000Z"
}
```

> If success, the response header is `201 Created` with the resource in the body.

> Event `ok apt` will be emitted to the related hospital channel with data below.

```json
{
  "data": { "user": "..." },
  "count": 10
}
```

> `data` is the newly approved appointment, `count` is total item in the Waiting List.

Approve an appointment in the Waiting List. Must supply the approved time that's in the range of start time and end time.

### HTTP Request

`POST http://domain.com/api/approved-appointments`

### REQUEST BODY

Parameter | Default | Description
--------- | ------- | -----------
id | required | The appointment's ID.
approved_time | required | The exact time to be proposed to the patient.

<aside class="warning">If you're not using an API key, this API call will return <code>403 Forbidden</code>.</aside>