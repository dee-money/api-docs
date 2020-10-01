---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to DeeMoney API! Your gateway to a financial toolkit.

# Endpoints

DeeMoney API is split into 2 main endpoints


## Ticket Booth

This endpoint handles all authentication.

```
https://ticket-booth-staging.api.dee.money
```

## Grand Central

This endpoint is the transaction engine.

```
https://grand-central-staging.api.dee.money
```

<aside class="notice">
Note that the endpoints provided here are for our UAT / Sandbox environment. Once you have completed your integration you may request us for production endpoints.
</aside>

# Authentication

> You can generate the token by doing something like this:

```shell
echo -n 'client_id:client_secret' | base64 | sed 's/+/-/g; s/\//_/g'
```

DeeMoney uses JWT standard for authentication token. You will be provided with a `client_id` and a `client_secret`.

You will need to construct the initial auth token by joining the `client_id` with `client_secret` and encode using base64

Once you have the `auth_token` you can use the following request.

## Ticket Booth Access Token

`POST /auth/app_identity/callback`

In the response header you will be given the access token.

The `expires_at` field will give you the time when the token will expire.

> Request

```shell
curl -X "POST" "https://ticket-booth-staging.api.dee.money/auth/app_identity/callback" \
     -H 'Content-Type: application/json' \
     -d $'{"token": "{{auth_token}}"}' 
```

> Response Header

```
Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...
```

> Response Body

```json
{
  "data": {
    "expires_at": "2020-08-31T09:22:06Z",
    "id": 12
  }
}
```

## Grand Central Access Token

`POST /auth/sessions`

You will need to use the Ticket Booth token to authenticate with Grand Central. You will be responded with a header with another token which is the Grand Central token. Once you have this token you can make transactions, view how much funds in your wallet and much more.

The `expires_at` field will give you the time when the token will expire.

> Request

```shell
curl -X "POST" "https://grand-central-staging.api.dee.money/auth/sessions" \
     -H 'Content-Type: application/json' \
     -d $'{"token": "Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9..."}'
```

> Response Header

```
Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9...
```

> Response Body

```json
{
  "data": {
    "expires_at": "2020-08-31T08:27:34Z",
    "id": "4584be75-f9c4-4218-b057-09066a44a54a"
  }
}
```

# Kittenss

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

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
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
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

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
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

