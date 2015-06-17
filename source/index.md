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

This API is for administrators only.

### REQUEST BODY

Parameter | Default | Description
--------- | ------- | -----------
name | required | The hospital name.
region | required | The ID of region where the hospital is in.
address | null | The hospital address.
latlong | required | Two elements array that contains latitute and longitude of the hospital. It's required for search purpose.
email | required | Email for login.
password | required | Password for login.
insurances | null | List of insurances that the hospital supports.
phone | null | The hospital's phone number.
fax | null | The hospital's fax number.
doctors | null | List of doctors' ID who work in the hospital.
logo_url | null | URL of hospital logo. Width: 250px.

<aside class="warning">If you're not using an administrator API key, this API call will return `403 Forbidden`.</aside>

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

This API is for administrators only.

### REQUEST BODY

Parameter | Default | Description
--------- | ------- | -----------
name | required | The doctor's name.
str_no | required | Surat Tanda Registrasi.
email | required | The doctor's work email, must be unique.
phone | required | The doctor's work phone number.
specialization | required | The ID of doctor's specialization.
since | required | Registration date to Indokter. (format ISO8601 UTC)
until | required | Expiration date from Indokter. (format ISO8601 UTC)
schedules | null | List of doctor's schedule. (See `SCHEDULES BODY`)
hospitals | null | Hospitals' ID where the doctor works.

### SCHEDULES BODY

Parameter | Default | Description
--------- | ------- | -----------
day | required | Number of work day. (1: monday, 2: tuesday, etc)
hospital | required | The hospital ID for this schedule.
from | h:7, m:0 | Start time of working.
to | h:7, m:0 | End time of working.

<aside class="warning">If you're not using an administrator API key, this API call will return `403 Forbidden`.</aside>

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

This API is for administrators only.

### REQUEST BODY

Parameter | Default | Description
--------- | ------- | -----------
formal_name | required | Formal name of specialization. English version is required.
display_name | required | Display name of specialization. English version is required.
abbr | required | Abbreviated job title of specialization. English version is required.
category | required | Either `specialty`, `dentist`, or `generic`.
complaints | null | List of complaints in this specialty. English version is required.

<aside class="warning">If you're not using an administrator API key, this API call will return `403 Forbidden`.</aside>