# LaravelSocialAPI

- Prefix = {BASE URL}/api/

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

**route:**
route: /register

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
username | string | No | User name
email | string | No | User email
password | string | No | User password

**response param**
param | type | description
----- | ---- | -------
success | bool | true - register successfully, false - fail
error | string | error - [code, message]
token | string | token string

### 2. Login
#### User Login API. Not social register

**route:**
route: /login

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
username | string | No | Username
password | string | No | Password

**response param**
param | type | description
----- | ---- | -------
success | bool | true - 200, false - error
error | array | [code, message]
token | token string | -

***Example***

In fail case: 
param | content
-------- | --------
success | false
stratus | 404
message | Invalid Email Or Password
status  | HTTP_UNAUTHORIZED

In success case:
param | content 
-------- | --------
success | true
status | 200
token | token string
status  | HTTP_OK

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

**route:**
{Prefix}/logout

**request param:**
NONE

**response param**
param | type | description
----- | ---- | -------
success | bool | true - register successfully, false - fail
status | int | logout status code
message | string | logout status string

***Example***

In fail case: 
param | content
-------- | --------
success | false
status | 404
message | Sorry, the user cannot be logged out
status  | HTTP_INTERNAL_SERVER_ERROR

In success case:
param | content 
-------- | --------
success | true
status | 200
message | User logged out successfully
status  | HTTP_OK

### 2. Store Base Order Status
#### Base Order Status Insert/Update API

**route:**
{Prefix}/store_base_order_status

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
status_id | string | No | 'NULL' - insert, int - update
status_name | string | No | status name string

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
status | int | result status code
message | string | action result message string

***Example***

In fail case: 
***status_id is Int and it is not exists in the DB***

param | content
-------- | --------
success | false
status | 404
message | Spcified order status record not found
status  | HTTP_INTERNAL_SERVER_ERROR

***status_id is 'NULL' and DB connection is fail***

param | content
-------- | --------
success | false
status | 402
message | Spcified order status record cannot create
status  | HTTP_INTERNAL_SERVER_ERROR

In success case:
param | content 
-------- | --------
success | true
status | 200
message | Order Status record created successfully
status  | HTTP_OK

### 3. Delete Base Order Status
#### Base Order Status Delete API

**route:**
{Prefix}/delete_base_order_status

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
status_id | string | No | status record id as int 

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
status | int | status code
message | string | action result message string

***Example***

In fail case: 
param | content
-------- | --------
success | false
status | 404
message | Spcified order status record not found
status  | HTTP_INTERNAL_SERVER_ERROR

In success case:
param | content 
-------- | --------
success | true
status | 200
message | Spcified order Status record just deleted successfully
status  | HTTP_OK

### 3. List of Base Order Status
#### Base Order Status List API

**route:**
{Prefix}/list_base_order_status

**request param:**
NONE

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
status | int | status code
message | string | action result message string
status | int | rest API execution status code
data | array | order status data array { [status_id, status_name], ...}

***Example***

In fail case: 
param | content
-------- | --------
success | false
status | 401
message | DB connection error
data | []
status  | HTTP_INTERNAL_SERVER_ERROR

In success case:
param | content 
-------- | --------
success | true
status | 200
message | OK
data | array
status  | HTTP_OK

### 4. Store Public Profile
#### Public profile insert/update API

**route:**
{Prefix}/store_public_profile

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
profile_id | string | No | if 'NULL'-insert, otherwise - update
name | string | No | public profile name
address | string | Yes | address
contact_email | string | Yes | contact email address
contact_phone_number | string | Yes | contact email phone number
country | string | Yes | country if not defined, 'US'
photo | file | Yes | public profile avatar image, if not defined, default unknown image link
bio | file | NO | bio
is_distributor | int | Yes | default 0, 0 - merchant, 1- distributor
is_available_payment | int | Yes | default 0

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
status | int | status code
message | string | action result message string
status | int | rest API execution status code
code | string | error code

### 5. Update Public Profile available payment
####  Update Public Profile available payment

**route:**
{Prefix}/update_available_payment

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
is_available_payment | int | Yes | default 0

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
status | int | status code
message | string | action result message string
status | int | rest API execution status code
code | string | error code

### 6. Update Public Profile distributor status
####  Update Public Profile distributor status

**route:**
{Prefix}/update_distributor

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
is_distributor | int | Yes | default 0

**response param**
param | type | description
----- | ---- | -------
success | bool | true - success, false - fail
status | int | status code
message | string | action result message string
status | int | rest API execution status code
code | string | error code
