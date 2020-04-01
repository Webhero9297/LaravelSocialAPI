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
profile_id | string |if 'NULL'-insert, otherwise - update (Optional)
name | string | public profile name (Optional when update)
address | string | address  (Optional when update)
contact_email | string | contact email address  (Optional when update)
contact_phone_number | string | contact email phone number (Optional)
country | string | country if not defined, 'US' (Optional)
photo | file | public profile avatar image, if not defined, default unknown image link (Optional)
bio | file | bio (Optional)
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
unit | string | product unit
photo | file array | product photo(optional)
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
```
order_product ex: ) 
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