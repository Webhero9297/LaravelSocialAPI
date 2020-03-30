# LaravelSocialAPI

- Prefix {BASE URI}/api/

## API List
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