---
title: API Basics
menu:
    main:
        weight: 20
        name: "API Basics"
---

### Required Headers
Every request made against the administration API requires the following headers:

* `Authorization`: This header is required for all protected resources, and
  should contain the bearer token. For example: 
  `Authorization: Bearer my_granted_access_token`.
* `X-Requested-By`: This header needs to be sent with every request, in
  order to guard against CSRF attacks. The content may be anything, we
  recommend you use some human-readable string that identifies your application.
* `Accept`: This header should be set to `application/json`.

All resources exposed by the administration api follow the same request structure,
only the payload and URI root are different. The below examples use the `/role/` resource,
and request patterns can be assumed to transfer to all resources.

## Authorization

## Search and Browse

We make a distinction between 'browsing' a resource and 'searching' a resource. The former
is intended to be exclusive, showing everything until filters have been applied. The latter
is designed to be inclusive, only showing those resources that meaningfully match the
query criteria. The responses are identical, only the requests are different:

**To search:**
```
GET /v1/role/search HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```

**To browse:**
```
GET /v1/role/ HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```

**Search and Browse response**
```
Response body
```

## CRUD Operations

### Create a new resource
```
POST /v1/role/ HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```

### Read a resource
```
GET /v1/role/<resource_id> HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```

### Update an existing resource
```
PUT /v1/role/<resource_id> HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```

### Delete a resource
```
DELETE /v1/role/<resource_id> HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```
## Resource Linking


### Linking two resources
```
DELETE /v1/role/<resource_id> HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```

### Unlinking two resources
```
DELETE /v1/role/<resource_id> HTTP/1.1
Host: example.com
Accept: application/json
Authorization: Bearer <your_bearer_token>
X-Requested-By: Your Application Name
```
