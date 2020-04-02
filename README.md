# LaravelSocialAPI

- Prefix = {BASE URL}/api

*** Error code ***
Code | description
---------- | ---- 
6001 | Invalid param
6002 | Invalid distributor
6003 | Order fail
6004 | Bad request

## UnAuthorized API List
### 1. Register
#### User register API. Not social register

**route:** /register

**request param:**
param name | type | description
---------- | ---- | -------
username | string | User name
email | string | User email
password | string | User password

**response param**
param | type | description
----- | ---- | -------
success | bool | true - register successfully, false - fail
error | string | error - [code, message]
token | string | token string

### 2. Login
#### User Login API. Not social login

**route:**  /login

**request param:**
param name | type | description
---------- | ---- | -------
username | string | Username
password | string | Password

**response param**
param | type | description
----- | ---- | -------
success | bool | true - 200, false - error
error | array | [code, message]
token | token string | -


### 3. Social Login or Register
#### Social Login API

**route:**
{Prefix}/social/redirect/{provider}
#### Provider - [apple, google, facebook, twitter]

**request param:**
NONE

**response param**
response result format is the same with login

These APIs are unathorized APIs, so when use these apis on the endpoint, don't worry about the authorized token.

## Authorized API List
When use the follwing apis, you have to define 'Authorization' item in the request header.
refer this screen.

![Screen](/images/header_authorized.png)

Regarding 'Authorization' header item, when these apis are called on the endpoint, exceptions occured from the backend

status | success | message
------ | ------- | -------
401 | false | Token is Invalid
402 | false | Token is Expired
403 | false | Authorization Token is not found

### 1. Logout
#### User logout API

**route:** /logout

**request param:**
NONE

**response param**
param | type | description
----- | ---- | -------
success | bool | true - register successfully, false - fail
error | array | logout error status [code, message]
message | string | logout status string

### 2. Insert/Update API for Public Profile
#### Action: insert/update

**route:** /upsert_public_profile

**request param:**
param name | type | description
---------- | ---- | -------
name | string | public profile name (Optional when update)
address | string | address  (Optional when update)
contact_email | string | contact email address  (Optional when update)
contact_phone_number | string | contact email phone number (Optional)
country | string | country if not defined, 'US' (Optional)
photo | file | public profile avatar image, if not defined, default unknown image link (Optional)
bio | string | bio (Optional)
is_distributor | int | default 0, 0 - merchant, 1- distributor (Optional)
is_available_payment | int | default 0 (Optional)

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]

### 3. Delete API for Public Profile
#### Action: delete

**route:** /delete_public_profile

**request param:**
param name | type | description
---------- | ---- | -------
profile_id | int | profile id

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | result message

### 4. Update Public Profile available payment
####  Update is_available_payment property of Public Profile

**route:** /update_is_available_payment

**request param:**
param name | type | description
---------- | ---- | -------
is_available_payment | int | default 0

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 5. Update Public Profile distributor status
####  Update is_distributor property of Public Profile

**route:** /update_is_distributor

**request param:**
param name | type | description
---------- | ---- | -------
is_distributor | int | default 0

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 6. Upsert Product
####  Action: Product Insert/Update
#### This API is valid for the only users of which is_distributor is 1. Otherwise, exception will occur.

**route:** /upsert_product

**request param:**
param name | type | description
---------- | ---- | -------
product_id | string | If 'NULL' - Insert, else int - update, otherwise - exception, this field is optional. But if omitted, API will perform insert action.
name | string | product name
unit | string | product unit (optional)
photo | file array | product photo (optional)
price | float | price (optional) - if omitted, price will be setted 0.
stock | float | stock (optional) - if omitted, stock will be setted 0.
discount_percentage | float | discount_percentage (optional) - if omitted, dicount_percentage will be setted 0.
discription | float | discription (optional) - if omitted, description will be setted 0.

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 7. Delete Product
####  Action: Product Delete
#### This API is valid for the only users of which is_distributor is 1. Otherwise, exception will occur.

**route:** /delete_product

**request param:**
param name | type | description
---------- | ---- | -------
product_id | int | If int - delete, otherwise - exception

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 8. Upsert Product Photo
####  Action: Product Photo Insert or Update
#### This API is valid for the only users of which is_distributor is 1. Otherwise, exception will occur.

**route:** /upsert_product_photo

**request param:**
param name | type | description
---------- | ---- | -------
product_id | int | product_id
product_photo_id | int | product_photo_id
photo | file | photo image file (Optional)

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 9. Delete Product Photo
####  Action: Product Photo Delete
#### This API is valid for the only users of which is_distributor is 1. Otherwise, exception will occur.

**route:** /delete_product_photo

**request param:**
param name | type | description
---------- | ---- | -------
product_photo_id | int | product_photo_id

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 10. New Order API
####  Action: Insert / Update of Order
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /upsert_order

**request param:**
param name | type | description
---------- | ---- | -------
distributor_id | int | distributor_id
order_id | string | If 'NULL' - Insert, else int - update, otherwise - exception. this field is optional. But if omitted, API will perform insert action.
order_product | json array | JSON format of order product.

order_product ex: ) 
```
  [
    {
      "order_product_id": "NULL", ---> Insert, if this item is omitted, Insert
      "product_id": 2,
      "price": 2,
      "count": 1,
      "product_name": "Food 1"
    },
    {
      "product_id": 3, ---> Update
      "price": 20,
      "count": 2,
      "product_name": "Food 2"
    }
  ]
```
**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 11. Delete Order API
####  Action: delete the specific order that the current user made.
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /delete_order

**request param:**
param name | type | description
---------- | ---- | -------
order_id | int | order_id

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string


### 12. change order product status
####  Action: change arrived/not arrived status of product in specific order
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /change_order_product_status

**request param:**
param name | type | description
---------- | ---- | -------
order_product_id | int | order_product_id
product_status | int | product_status (0, 1)

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 13. Rate to distributor
####  Action: with order, insert or update rate to specific distributor
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /upsert_rate

**request param:**
param name | type | description
---------- | ---- | -------
distributor_id | int | distributor_id
order_id | int | order_id
score | int | 1-5
title | string | rate title
comment | string | rate comment

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 14. delete rate
####  Action: delete rate
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /delete_rate

**request param:**
param name | type | description
---------- | ---- | -------
rate_id | int | rate_id

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 15. like distributor
####  Action: like/unlike distributor
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /like_distributor

**request param:**
param name | type | description
---------- | ---- | -------
distributor_id | int | distributor_id

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 16. like product
####  Action: like/unlike product
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /like_product

**request param:**
param name | type | description
---------- | ---- | -------
product_id | int | product_id

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 17. Helpful
####  Action: helpful
#### This API is valid for the only users of which is_distributor is 0. Otherwise, exception will occur.

**route:** /helpful

**request param:**
param name | type | description
---------- | ---- | -------
rate_id | int | rate_id

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 18. RateReport
####  Action: RateReport
#### This API is valid for the only users of which is_distributor is 1. Otherwise, exception will occur.

**route:** /rate_report

**request param:**
param name | type | description
---------- | ---- | -------
rate_id | int | rate_id
title | int | title
comment | int | comment

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 19. ProfileReport
####  Action: ProfileReport
#### This API is valid for the only users of which is_distributor is 1. Otherwise, exception will occur.

**route:** /profile_report

**request param:**
param name | type | description
---------- | ---- | -------
distributor_id | int | distributor_id
title | int | title
comment | int | comment

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
message | string | action result message string

### 20. Get My Profile
####  Action: Get Public profile of Auth user
#### This API is valid for the only users of which is_distributor is 1. Otherwise, exception will occur.

**route:** /get_my_profile

**request param:**

NONE

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
profile | array | profile data

```
{
    "status": 200,
    "success": true,
    "profile": {
        "name": "Tester1",
        "country": "US",
        "address": "Tester1 Address",
        "contact_email": "mail.com",
        "contact_phone_number": null,
        "photo": "http://192.168.1.12:5000/public/uploads//unknown_public_profile_avatar.png",
        "bio": "This is good for us. Hello",
        "is_distributor": 1,
        "is_available_payment": 0
    }
}
```

### 21. Get Distributors
####  Action: Get Distributor list 
#### This API can be called from Authed or unauthed user

**route:** /get_distributors

**request param:**
param name | type | description
---------- | ---- | -------
search_filter | string | Optional, if omitted, it will be 'all', This field must be in one of [all, top_rated, suggested, favorites]
pos | int | Optional, if omitted, it will be 0, 0, 20, 40, 60, ,,,
limit | int | Optional, if omitted, it will be 20, 


**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
error | array | error status - [code, message]
profile | array | profile data

```
{
    "success": true,
    "data": [
        {
            "distributor_id": 7,
            "name": "User5",
            "photo": "http://192.168.1.12:5000/public/uploads/public_profiles/profile_avatar_1585662833_608928871.jpg",
            "rate_count": 2,
            "avg_score": "5.0",
            "product_count": 5,
            "pending_orders_count": 5,
            "processed_order_count": 1,
            "last_delivery_date": "2020-04-01 18:14:20",
            "like_status": 1,
            "like_date": "2020-03-31 19:37:13"
        },
        {
            "distributor_id": 9,
            "name": "Josph Lee",
            "photo": "http://192.168.1.12:5000/public/uploads/unknown_public_profile_avatar.png",
            "rate_count": 0,
            "avg_score": "0.0",
            "product_count": 0,
            "pending_orders_count": 3,
            "processed_order_count": 0,
            "last_delivery_date": "-",
            "like_status": 0,
            "like_date": "-"
        },
        {
            "distributor_id": 10,
            "name": "Tester1",
            "photo": "http://192.168.1.12:5000/public/uploads/unknown_public_profile_avatar.png",
            "rate_count": 0,
            "avg_score": "0.0",
            "product_count": 1,
            "pending_orders_count": 5,
            "processed_order_count": 0,
            "last_delivery_date": "-",
            "like_status": 0,
            "like_date": "-"
        }
    ]
}
```