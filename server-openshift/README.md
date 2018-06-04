# Keycloak Openshift

Extends the Keycloak docker image to allow using it on Openshift as well as in s2i builds.

This image support the three execution modes:

- use an embedded database
- use an external postgres database
- use an external mysql database

## Usage

In order to be compatible with S2I builds, the startup command isn't fixed
in the entrypoint.

As a consequence, you have to include the startup script as the first parameter
of the `docker run` command.

The required startup script is: `start-keycloak.sh`.

To choose the execution mode, you can use the `DB_VENDOR` environment variable:

- `H2` for the embedded H2 database,
- `POSTGRES` for the Postgres database,
- `MYSQL` for the MySql database.

If no `DB_VENDOR` variable is set, then the startup script will fail with the following error message:
```
Missing mandatory environment variable DB_VENDOR. Allowed values are [H2, POSTGRES, MYSQL, MARIADB]
```

If no database-related environement variable is set, the script will use the embedded
H2 database by default.


### Use with an embedded database

    docker run -e DB_VENDOR=H2 jboss/keycloak-openshift start-keycloak.sh

or simply

    docker run jboss/keycloak-openshift start-keycloak.sh

The available environment variables and parameters are the same as for the `jboss/keycloak` image.

### Use with an external MySql database

    docker run --name keycloak --link mysql:mysql -e DB_DATABASE=keycloak -e DB_USER=keycloak -e DB_PASSWORD=password jboss/keycloak-openshift start-keycloak.sh

The available environment variables and parameters are the same as for the `jboss/keycloak-mysql` image.

### Use with an external Postgres database

    docker run --name keycloak --link postgres:postgres -e DB_DATABASE=keycloak -e DB_USER=keycloak -e DB_PASSWORD=password jboss/keycloak-openshift start-keycloak.sh

The available environment variables and parameters are the same as for the `jboss/keycloak-postgres` image.
