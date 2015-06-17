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

<aside class="warning">If you're not using an administrator API key, this API call will return <pre>403 Forbidden</pre>.</aside>

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

<aside class="warning">If you're not using an administrator API key, this API call will return <pre>403 Forbidden</pre>.</aside>

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

### REQUEST BODY

Field | Default | Description
--------- | ------- | -----------
formal_name | required | Formal name of specialization. English version is required.
display_name | required | Display name of specialization. English version is required.
abbr | required | Abbreviated job title of specialization. English version is required.
category | required | Either `specialty`, `dentist`, or `generic`.
complaints | null | List of complaints in this specialty. English version is required.

<aside class="warning">If you're not using an administrator API key, this API call will return <pre>403 Forbidden</pre>.</aside>

# Requested Appointments

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
        },
      }
    ]
  }
}
```

Get all requested appointments in a hospital, sorted ascending by appointment time and creation time. This list is paginated with 10 items per page.

### QUERY PARAMETERS

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | The page you want to fetch.
hospital | required | The hospital's ID.
include | null | Field inclusion: `user`, `doctor`, `hospital`, `visiting_reason`.
lang | en | The language to display the resource.

<aside class="warning">If you're not using an API key, this API call will return <pre>403 Forbidden</pre>.</aside>

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

Make an appointment to a doctor in a hospital. This is the appointment that hospital admin will reject or approve.

### REQUEST BODY

Field | Default | Description
--------- | ------- | -----------
user | required | The patient's ID. (Patient = user in our system)
hospital | required | The hospital's ID.
doctor | required | The doctor's ID.
visiting_reason | required | The complaint's ID. Can be found in `Specialization` collection.
from_time | required | Start time of the visit.
to_time | required | End time of the visit.

> If success, the response header is `201 Created` with the resource in the body and emit `new apt` event via socket.io to the related hospital channel.