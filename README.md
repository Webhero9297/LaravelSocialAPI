# LaravelSocialAPI

- Prefix {BASE URI}/api/

## API List
### 1. Register
#### User register API. Not social register
  **route: **
    {Prefix}/register
  **request param:**
    param name | type | isOptional? | meaning
    ---------- | ---- | ----------- | -------
    name | string | No | User name
    email | string | No | User email
    password | string | No | User password
  **response param**
    param | type | meaning
    ----- | ---- | -------
    success | bool | true - register successfully, false - fail
    
