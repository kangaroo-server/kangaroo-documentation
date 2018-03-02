---
title: Kangaroo
type: index
menu:
    main:
        weight: 1
        name: "Welcome"
---
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/net.krotscheck/kangaroo/badge.svg)](https://maven-badges.herokuapp.com/maven-central/net.krotscheck/kangaroo) [![Build Status](https://jenkins.krotscheck.net/buildStatus/icon?job=Kangaroo/kangaroo/develop)](https://jenkins.krotscheck.net/job/Kangaroo/job/kangaroo/job/develop) [![Jenkins tests](https://img.shields.io/jenkins/t/https/jenkins.krotscheck.net/job/Kangaroo/job/kangaroo/job/develop.svg)](https://jenkins.krotscheck.net/job/Kangaroo/job/kangaroo/job/develop/) [![Known Vulnerabilities](https://snyk.io/test/github/kangaroo-server/kangaroo/badge.svg?targetFile=/pom.xml)](https://snyk.io/test/github/kangaroo-server/kangaroo?targetFile=/pom.xml)

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
