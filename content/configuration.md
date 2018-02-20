---
title: Configuration
menu:
    main:
        weight: 15
---
Kangaroo attempts to provide an out-of-the-box "sane default" configuration for development purposes only, yet provides
all the necessary configuration hooks for true, highly-available deployments. Below is a comprehensive list of our
direct configuration options, as well as the current defaults.

## Basic Configuration

| Commandline Flag               | Default       | Description                                                                                                                            |
|--------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `--kangaroo.host`              | `127.0.0.1`   | The IP address to which this server will bind itself.                                                                                  |
| `--kangaroo.port`              | `8080`        | The port on which this server will listen.                                                                                             |
| `--kangaroo.working_dir`       | `~/.kangaroo` | The filesystem path used for the server's working directory.                                                                           |
| `--kangaroo.html_app_root`     |               | Filesystem path to the directory to use as our HTML5 Application root. If not set, the server will not attempt to host an application. |

## SSL Certificates

| Commandline Flag               | Default       | Description                                                                                                                            |
|--------------------------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `--kangaroo.keystore_path`     |               | Path to an external keystore which contains your server certificates.                                                                  |
| `--kangaroo.keystore_password` | `kangaroo`    | The keystore's own password.                                                                                                           |
| `--kangaroo.keystore_type`     | `PKCS12`      | The keystore's type. For a list of options, please refer to `java.security.Keystore`                                                   |
| `--kangaroo.cert_alias`        | `kangaroo`    | The alias of the server certificate to use from the keystore.                                                                          |
| `--kangaroo.cert_key_password` | `kangaroo`    | The password for server certificate's private key.                                                                                     |

## Database

Rather than provide custom options to configure our ORM (hibernate), kangaroo instead passes all system properties to
Hibernate directly, including Hibernate's Search. For specific configuration options, please refer to the
[hibernate documentation](http://hibernate.org/orm/documentation) or the [hibernate search documentation](http://hibernate.org/search/documentation/).
Specific, popular database configuration options are listed below.

### H2 (Default)

By default, Kangaroo creates an on-disk H2 database. This is mostly for testing and local development purposes, and we
do not recommend using this configuration at scale. The default options are listed below:

| Commandline Flag                      | Default                            | Description                                          |
|---------------------------------------|------------------------------------|------------------------------------------------------|
| `-Dhibernate.connection.url`          | `jdbc:h2:file://~/.kangaroo/h2.db` | The JDBC connection string for the database.         |
| `-Dhibernate.connection.username`     | `oid`                              | Database user.                                       |
| `-Dhibernate.connection.password`     | `oid`                              | Database password.                                   |
| `-Dhibernate.connection.driver_class` | `org.h2.Driver`                    | The Driver class to use for the database connection. |
| `-Dhibernate.connection.dialect`      | `org.hibernate.dialect.H2Dialect`  | The database SQL dialect implementation class.       |

### MariaDB / MySQL

If you'd like to connect to a MySQL-compatible database instead, the below options should provide a good starting point.
Note that Kangaroo comes packaged only with the MariaDB driver, as the MySQL driver does not meet our licensing guidelines.
You will need to provide a database-specific driver via your system's JVM if this does not work for you.

| Commandline Flag                      | Default                                                    |
|---------------------------------------|------------------------------------------------------------|
| `-Dhibernate.connection.url`          | `jdbc:mariadb://127.0.0.1:3306/my_database?useUnicode=yes` |
| `-Dhibernate.connection.username`     | `my_username`                                              |
| `-Dhibernate.connection.password`     | `my_password`                                              |
| `-Dhibernate.connection.driver_class` | `org.mariadb.jdbc.Driver`                                  |
| `-Dhibernate.connection.dialect`      | `org.hibernate.dialect.MariaDBDialect`                     |

### Lucene Index

Kangaroo makes use of an on-disk lucene index for its search features. Truly highly-available configurations are currently
being considered for development.

| Commandline Flag                                | Default                      | Description                                   |
|-------------------------------------------------|------------------------------|-----------------------------------------------|
| `-Dhibernate.search.default.directory_provider` | `filesystem`                 | The lucene index provider to use.             |
| `-Dhibernate.search.default.indexBase`          | `~/.kangaroo/lucene_indexes` | Root directory for a lucene filesystem index. |