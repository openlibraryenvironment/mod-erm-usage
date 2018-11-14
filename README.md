# mod-erm-usage

Copyright (C) 2018 The Open Library Foundation

This software is distributed under the terms of the Apache License,
Version 2.0. See the file "[LICENSE](LICENSE)" for more information.


# Installation

```
git clone ...
cd mod-erm-usage
mvn clean install
```

# Run plain jar

### `mod-erm-usage-server`
```
cd mod-erm-usage-server
env \
DB_USERNAME=folio_admin \
DB_PASSWORD=folio_admin \
DB_HOST=localhost \
DB_PORT=5432 \
DB_DATABASE=okapi_modules \
java -jar target/mod-erm-usage-server-fat.jar
```

### `mod-erm-usage-harvester`

```
cd mod-erm-usage-harvester
java -jar target/mod-erm-usage-harvester-fat.jar -conf target/config.json
```

configuration via json file:
```json
{
  "okapiUrl": "http://localhost:9130",
  "tenantsPath": "/_/proxy/tenants",
  "reportsPath": "/counter-reports",
  "providerPath": "/usage-data-providers",
  "aggregatorPath": "/aggregator-settings",
  "moduleId": "mod-erm-usage-harvester-0.0.2-SNAPSHOT"
}
```

# Run via Docker

## `mod-erm-usage-server`

Build docker image

```
$ cd mod-erm-usage-server
$ docker build -t mod-erm-usage-server .
```

Register ModuleDescriptor

```
$ curl -w '\n' -X POST -D - -H "Content-type: application/json" -d @ModuleDescriptor.json http://localhost:9130/_/proxy/modules
```

Register DeploymentDescriptor

```
$ curl -w '\n' -X POST -D - -H "Content-type: application/json" -d @DeploymentDescriptor.json http://localhost:9130/_/discovery/modules
```

Activate module for tenant

```
$ curl -w '\n' -X POST -D - -H "Content-type: application/json" -d '{ "id": "mod-erm-usage-server-0.0.3-SNAPSHOT"}' http://localhost:9130/_/proxy/tenants/diku/modules
```

## `mod-erm-usage-harvester`

Build docker image

```
$ cd mod-erm-usage-harvester
$ docker build -t mod-erm-usage-harvester .
```

Register ModuleDescriptor

```
$ curl -w '\n' -X POST -D - -H "Content-type: application/json" -d @ModuleDescriptor.json http://localhost:9130/_/proxy/modules
```

Activate module for tenant (do this before registering DeploymentDescriptor)

```
$ curl -w '\n' -X POST -D - -H "Content-type: application/json" -d '{ "id": "mod-erm-usage-harvester-0.0.3-SNAPSHOT"}' http://localhost:9130/_/proxy/tenants/diku/modules
```

Register DeploymentDescriptor

```
$ curl -w '\n' -X POST -D - -H "Content-type: application/json" -d @DeploymentDescriptor.json http://localhost:9130/_/discovery/modules
```

## Additional information

### Issue tracker

See project [MODERM](https://issues.folio.org/browse/MODERM)
at the [FOLIO issue tracker](https://dev.folio.org/guidelines/issue-tracker).

### Other documentation

Other [modules](https://dev.folio.org/source-code/#server-side) are described,
with further FOLIO Developer documentation at [dev.folio.org](https://dev.folio.org/)

