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
-------- | --------
success | false
message | Invalid Email Or Password
status  | HTTP_UNAUTHORIZED

In success case: 
-------- | --------
success | false
token | string
status  | HTTP_OK