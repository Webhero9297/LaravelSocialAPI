# LaravelSocialAPI

- Prefix {BASE URI}/api/

## UnAuthorized API List
### 1. Register
#### User register API. Not social register

**route:**
{Prefix}/register

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
name | string | No | User name
email | string | No | User email
password | string | No | User password

**response param**
param | type | description
----- | ---- | -------
success | bool | true - register successfully, false - fail

### 2. Login
#### User Login API. Not social register

**route:**
{Prefix}/register

**request param:**
param name | type | isOptional? | description
---------- | ---- | ----------- | -------
email | string | No | User email
password | string | No | User password

**response param**
param | type | description
----- | ---- | -------
success | bool | -
message | string | -
token | string | -

***Example***

In fail case: 
param | content
-------- | --------
success | false
message | Invalid Email Or Password
status  | HTTP_UNAUTHORIZED

In success case:
param | content 
-------- | --------
success | true
token | string
status  | HTTP_OK

### 3. Social Login or Register
#### Social Login API

**route:**
{Prefix}/social/redirect/{provider}
####Provider - [apple, google, facebook, twitter]

**request param:**
NONE

**response param**
response result format is the same with login

These APIs are unathorized APIs, so when use these apis on the endpoint, don't worry about the authorized token.

## Authorized API List
When use the follwing apis, you have to define 'Authorization' item in the request header.
refer this screen
[link](https://prnt.sc/rpjlxf)
Regarding 'Authorization' header item, when these apis are called on the endpoint, exceptions occured from the backend

params | content
------ | -------
status | Token is Invalid
status | Token is Expired
status | Authorization Token is not found

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
message | string | logout status string

***Example***

In fail case: 
param | content
-------- | --------
success | false
message | Sorry, the user cannot be logged out
status  | HTTP_INTERNAL_SERVER_ERROR

In success case:
param | content 
-------- | --------
success | true
message | User logged out successfully
status  | HTTP_OK