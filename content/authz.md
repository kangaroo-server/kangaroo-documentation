---
title: "API: Authorization Management"
menu:
    main:
        weight: 25
---

This API allows you to configure, and manage, the applications and their authorization
methods.

## Application

Applications are our top-level data constructs, which collect resources, clients, authenticators, etc.,
which all share the same user database.

* **Root URI:** /v1/application/
* **Scopes:** kangaroo:application, kangaroo:application_admin
* **Authorization:** Required

| Name         | Type                 | Description                                                           |
|--------------|----------------------|-----------------------------------------------------------------------|
| id           | Resource Identifier  | The unique identifier for this entity.                                |
| createdDate  | ISO-8601 Date String | The time this entity was created (read-only).                         |
| modifiedDate | ISO-8601 Date String | The time this entity was last modified (read-only).                   |
| owner        | Resource Identifier  | The ID of the user who created this entity (read-only).               |
| defaultRole  | Resource Identifier  | The ID of the default user role, to which new users will be assigned. |
| name         | String[3-255]        | A human-readable name for this application.                           |
| description  | String[0-255]        | A human-readable description for this application.                    |

```json
{
    "id": "0bea995629fac30d64b215c099f211a2",
    "createdDate": "2017-11-10T22:58:05Z",
    "modifiedDate": "2017-11-10T22:58:05Z",
    "owner": "3dfba52b24e1b70f0881942322771f32",
    "defaultRole": "0c16fd7a62133a4b1c3034835b325256",
    "name": "Kangaroo",
    "description": "The kangaroo administration application."
}
```

## Role

Roles are collections of scopes (permissions), to which a user may belong. A user who belongs to a role may request, and
be granted, any scope within that role. Users may only be members of one role at a time.

* **Root URI:** /v1/role/
* **Scopes:** kangaroo:role, kangaroo:role_admin
* **Authorization:** Required

| Name         | Type                 | Description                                           |
|--------------|----------------------|-------------------------------------------------------|
| id           | Resource Identifier  | The unique identifier for this entity.                |
| createdDate  | ISO-8601 Date String | The time this entity was created (read-only).         |
| modifiedDate | ISO-8601 Date String | The time this entity was last modified (read-only).   |
| application  | Resource Identifier  | The ID of the application to which this role belongs. |
| name         | String[3-255]        | A human-readable name for this role.                  |

```json
{
    "id":"58c1561d80d3cbd776bab952d3d19ae4",
    "createdDate":"2017-11-11T18:14:43Z",
    "modifiedDate":"2017-11-11T18:14:43Z",
    "application":"7d24c31aab1bbce750f29ea243783a27",
    "name":"Role Name"
}
```

## Authenticator

An authenticator describes a method of linking the Kangaroo Authz server to a remote IdP. Multiple
authenticators may be added to each client.

* **Root URI:** /v1/authenticator/
* **Scopes:** kangaroo:authenticator, kangaroo:authenticator_admin
* **Authorization:** Required

| Name          | Type                 | Description                                           |
|---------------|----------------------|-------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).         |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).   |
| client        | Resource Identifier  | The ID of the client which uses this authenticator.   |
| type          | String[enum]         | The type of authenticator.                            |
| configuration | Map[string:string]   | A string/string map of configuration values.          |

```json
{
    "id": "62eaab94de8d1d9c00ba28083411e216",
    "createdDate": "2017-11-11T00:11:29Z",
    "modifiedDate": "2017-11-11T00:11:29Z",
    "client": "2d532c9f97ba9207e28ea06fe0d9f26e",
    "type": "Facebook",
    "configuration": {
        "config_value_1": "authenticator_specific_setting",
        "config_value_2": "authenticator_specific_setting"
    }
}
```

## Client

A client describes an application - such as a mobile client, a web application, a device,
or some other user-facing interface - which needs to request auth tokens for the greater Application.
For example: A game may have an iOS client, an Android client, or a web client.

* **Root URI:** /v1/client/
* **Scopes:** kangaroo:client, kangaroo:client_admin
* **Authorization:** Required

| Name          | Type                 | Description                                             |
|---------------|----------------------|---------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                  |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).           |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).     |
| application   | Resource Identifier  | The ID of the application to which this client belongs. |
| name          | String[3-255]        | A human-readable name.                                  |
| type          | String[enum]         | The type of client.                                     |
| configuration | Map[string:string]   | A string/string map of configuration values.            |

```json
{
    "id": "2e338aef6f60bb9a98e115484bceb2ec",
    "createdDate": "2017-11-11T00:11:41Z",
    "modifiedDate": "2017-11-11T00:11:41Z",
    "application": "100063b46f5a09834afcfaecef8bb79f",
    "name": "Example Client",
    "type": "AuthorizationGrant",
    "configuration":{
        "authorization_code_expires_in": 600,
        "access_token_expires_in": 600,
        "refresh_token_expires_in": 2592000
    }
}
```
## Client Redirect

A client redirect is one of the URI's permitted to be used in the OAuth2 authorization flow.
All elements in the URI must be present in the client's request, though additional query parameters may be added.

* **Root URI:** /v1/client/<client_id>/redirect/
* **Scopes:** kangaroo:client, kangaroo:client_admin
* **Authorization:** Required

| Name          | Type                 | Description                                             |
|---------------|----------------------|---------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                  |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).           |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).     |
| uri           | String[URL]          | The fully-qualified URI to which redirects may be sent. |

```json
{
    "id": "29932edb564649f1fd849cf3b789f9c8",
    "createdDate": "2017-11-11T00:11:33Z",
    "modifiedDate": "2017-11-11T00:11:33Z",
    "uri": "http://redirect.example.com/"
}
```

## Client Referrer

A client referrer manages Cross-Origin-Resource-Sharing (CORS) permissions for the OAuth2 and Aanagement API. If the
kangaroo server lives on a separate origin host, you may make direct API requests from the client host by
adding the host's URL as a valid referrer.

* **Root URI:** /v1/client/<client_id>/referrer/
* **Scopes:** kangaroo:client, kangaroo:client_admin
* **Authorization:** Required

| Name          | Type                 | Description                                                  |
|---------------|----------------------|--------------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                       |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).                |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).          |
| uri           | String[URL]          | The fully-qualified URI from which API requests may be made. |

```json
{
    "id": "3a8bc39a1d43cf3fd11b063debf57b4b",
    "createdDate": "2017-11-11T00:11:33Z",
    "modifiedDate": "2017-11-11T00:11:33Z",
    "uri": "http://referrer.example.com/"
}
```
## Token

The database representation of an OAuth2 token.

* **Root URI:** /v1/token/
* **Scopes:** kangaroo:token, kangaroo:token_admin
* **Authorization:** Required

| Name          | Type                 | Description                                                  |
|---------------|----------------------|--------------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                       |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).                |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).          |
| identity      | Resource Identifier  | The ID of the user identity to which this token was issued.  |
| client        | Resource Identifier  | The ID of the client to which this token was issued.         |
| tokenType     | String[enum]         | The type of token.                                           |
| expiresIn     | Integer              | Seconds from createdDate when this token will expire.        |

```json
{
    "id": "3b3c3d9fc670236be824bead26f859ab",
    "createdDate": "2017-11-11T00:11:44Z",
    "modifiedDate": "2017-11-11T00:11:44Z",
    "identity": "7f7e275638cc1866002278104026ecb8",
    "client": "7e1b29edf7f09c6e1bb663fda83c9d0a",
    "tokenType": "Bearer",
    "expiresIn": 100
}
```

## Scope

A scope, or permission, available within an application. These may be explicitly requested for each token,
and must be unique within an application.

* **Root URI:** /v1/scope/
* **Scopes:** kangaroo:scope, kangaroo:scope_admin
* **Authorization:** Required

| Name          | Type                 | Description                                                  |
|---------------|----------------------|--------------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                       |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).                |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).          |
| application   | Resource Identifier  | The ID of the application to which this scope belongs.       |
| name          | String[3-255]        | The token name, no spaces.                                   |

```json
{
    "id": "7840f4cef92d5115dcc84af78e9a03e1",
    "createdDate": "2017-11-11T00:11:44Z",
    "modifiedDate": "2017-11-11T00:11:44Z",
    "application": "2b45fce637228dcb16388f03839e550c",
    "name": "permission:scope"
}

```

## User

A user is the parent for a collection of remote User identities. For instance, a user may have a facebook,
linkedin, and google identity. Multiple Id's of each type are permitted.

* **Root URI:** /v1/user/
* **Scopes:** kangaroo:user, kangaroo:user_admin
* **Authorization:** Required

| Name          | Type                 | Description                                                  |
|---------------|----------------------|--------------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                       |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).                |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).          |
| application   | Resource Identifier  | The ID of the application to which this scope belongs.       |
| role          | Resource Identifier  | This user's role, from which it is granted permitted scopes. |

```json
{
    "id": "10be785d1481ef5095bd6af784a55286",
    "createdDate": "2017-11-11T18:05:45Z",
    "modifiedDate": "2017-11-11T18:05:45Z",
    "application": "1d096bea5c66a7a1670c651883d2d935",
    "role": "191e961fbf7420624c735183a218aee4"
}
```
## UserIdentity

A UserIdentity is an explicit link between a kangaroo user and a third party identity provider. It contains
the remote id, as well as any third party claims issued by that IdP. The "Password" identity type has special meaning:
it only works with the Owner Credentials flow, and may only be written.

* **Root URI:** /v1/identity/
* **Scopes:** kangaroo:identity, kangaroo:identity_admin
* **Authorization:** Required

| Name          | Type                 | Description                                                  |
|---------------|----------------------|--------------------------------------------------------------|
| id            | Resource Identifier  | The unique identifier for this entity.                       |
| createdDate   | ISO-8601 Date String | The time this entity was created (read-only).                |
| modifiedDate  | ISO-8601 Date String | The time this entity was last modified (read-only).          |
| user          | Resource Identifier  | The ID of the user to which this identity belongs.           |
| type          | String[enum]         | The identity type, correlates with authenticator type.       |
| remoteId      | String               | The unique identifier for this user for the remote system.   |
| password      | String               | The user's new password. (write-only)                        |
| claims        | Map[string:string]   | A string/string map of remote IdP claims.                    |

```json
{
    "id": "41e142f3b93920220b79f058e00d000d",
    "createdDate": "2017-11-11T18:07:09Z",
    "modifiedDate": "2017-11-11T18:07:09Z",
    "user": "32c2048510b7ec967cf86c37d3e37229",
    "type": "Password",
    "remoteId": "0148524924c91ad7794fe5880bb2aa10",
    "claims":{
        "idp": "provided",
        "claims": "list"
    }
}
```