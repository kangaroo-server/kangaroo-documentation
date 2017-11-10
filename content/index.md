---
title: Kangaroo
type: index
menu:
    main:
        weight: 1
        name: "Welcome"
---

Kangaroo is an open source, multi-tenant, OAuth2 Authorization server. It
fully supports [RFC 6749](https://tools.ietf.org/html/rfc6749) (The original OAuth2 Specification), and aspires to 
be a reference implementation thereof. We intend to expand the scope of this project to include other OAuth2 related RFC's.

``` bash
docker run -it -p 8080:8080 krotscheck/kangaroo-authz:latest
```

The above command will start a standalone server at [https://localhost:8080/](https://localhost:8080/]).
This server will sign its own SSL certificate on startup; Your browser will notify you of the 
untrusted nature of this certificate, every time the process is restarted. To maintain application state
between restarts, provide a volume mount at `/var/lib/kangaroo`. For example:

``` bash
docker run -it -v `pwd`/kangaroo:/var/lib/kangaroo -p 8080:8080 krotscheck/kangaroo-authz:latest
```
