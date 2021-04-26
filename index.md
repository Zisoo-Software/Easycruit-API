# Zisoo Cruit
Zisoo Cruit is a developer friendly API for Visma Easycruit software. It makes custom vacancy applications forms posible with serverside validation.

Vacancies can be created through Zisoo Cruit Portal.
 <p align="center">
    <img src="https://assets.zisoo.nl/zisoo-cruit-portal-2.png" width="500px"/>
    <img src="https://assets.zisoo.nl/zisoo-cruit-vacancy-template.png" width="450px"/>
</p>


## Public Endpoints
Public endpoints are the endpoints used on your website to apply for a vacancy.

In order to connect with your Easycruit Environment, we need the credentials.
The credentials we need are:
- Customer
- User
- Password

You can provide these credentials in two different ways:
1. Add the credentials to Zisoo Cruit at https://cruit.zisoo.nl/settings
2. Send the credentials with the request. You can use the X-Auth-* header.
So your header will look something like: 
```javascript
{
  "Content-Type": "application/json",
  "X-Auth-Customer": "CUSTOMER",
  "X-Auth-User": "USER",
  "X-Auth-Password": "PASSWORD"
}
```
**Important note:** only use the second option in your backend code. **Never** expose your Easycruit credentials to the internet. In case you need to call the API from the frontend, use the first option.

#### Apply for vacancy
POST https://cruit-api.zisoo.nl/api/v1/:vacancyId/application
JSON Body example:
```javascript
// Apply for vacancy example
{
  "api_key": "PUBLIC_API_KEY",
  "personal_info": {
    "firstname": String,
    "lastname": String,
    "email": String,
    "title": String | Null
  },
  "default_attachments": {
    "picture": File,
    "cv": File,
    "motivation": File
  }
}
```

## Portal Endpoints
To access the Portal Endpoints you need to be authorized. You need a JWT token to authorize yourself. Use the authorization header to send your JWT token.
```javascript
// Authorized call
{
  "headers": {
    "authorization": "Bearer ..."
  }
}
```

#### Login
Get the JWT token
POST https://cruit-api.zisoo.nl/api/v1/portal/user_token
```javascript
{
  "auth": {
    "email": "test@example.com"
    "password": "YOUR_PASSWORD"
  }
}
```

### Vacancy

#### All vacancies
GET https://cruit-api.zisoo.nl/api/v1/portal/vacancy

#### Show vacancy
GET https://cruit-api.zisoo.nl/api/v1/portal/vacancy/:vacancyId

#### Download vacancy template
GET https://cruit-api.zisoo.nl/api/v1/portal/vacancy/:vacancyId/download

#### Create vacancy
POST https://cruit-api.zisoo.nl/api/v1/portal/vacancy
```javascript
{
  "general": {
    "name": "Example vacancy",
    "department_id": 12345,
    "recruiting_project_id": 12345
  }
}
```

#### Update vacancy
PATCH https://cruit-api.zisoo.nl/api/v1/portal/vacancy/:vacancyId
```javascript
{
  "general": {
    "name": "Example vacancy",
    "department_id": 12345,
    "recruiting_project_id": 12345
  },
  "personal_info": {
    "email": { "enabled": false, "required": false},
    "firstname": { "enabled": true, "required": true},
    "lastname": { "enabled": true, "required": false},
    "gender": { "enabled": false, "required": false},
    "title": { "enabled": true, "required": false},
    "phone": { "enabled": true, "required": true}
  },
  "default_attachments": {
    "picture": { "enabled": true, "required": true},
    "cv": { "enabled": true, "required": true},
    "motivation": { "enabled": true, "required": true}
  }
}
```

### Settings

#### Check credentials
POST https://cruit-api.zisoo.nl/api/v1/portal/check_credentials
```javascript
{
	"easycruit_customer": "test",
	"easycruit_username": "test",
	"easycruit_password": "test123"
}
```

#### Update settings
PATCH https://cruit-api.zisoo.nl/api/v1/portal/settings
```javascript
{
	"easycruit_customer": "test",
	"easycruit_username": "test",
	"easycruit_password": "test123"
}
```

### API info
GET https://cruit-api.zisoo.nl/api/v1/portal/api-info
