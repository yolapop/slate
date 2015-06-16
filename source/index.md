---
title: Indokter (Temp) API Documentation

language_tabs:
  - http
  - ruby
  - python
  - shell

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Internal use only.

# Authentication

TODO

# Hospitals

## Get All Hospitals

Not implemented

## Create New Hospital

```http
POST /hospitals HTTP/1.1
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

> If success, the response header is 201 with the resource in the body

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

