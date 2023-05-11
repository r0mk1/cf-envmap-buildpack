# cf-envmap-buildpack

The buildpack exposes a configuration from VCAP_SERVICES into
user-defined environment variables.


## Usage

Add the buildpack to the app's `manifest.yml`:

```yaml
    buildpacks:
    - https://github.com/r0mk1/cf-envmap-buildpack.git
    ...
```

Create a file `cfenvmap.yml` in the root directory of the application with required mappings, e.g.:

```yaml
---
postgresql-db:
  env:
    PGHOST: credentials.hostname
    PGDATABASE: credentials.dbname
    PGPORT: credentials.port
    PGUSER: credentials.username
    PGPASSWORD: credentials.password

objectstore:
  env:
    AWS_ACCESS_KEY_ID: credentials.access_key_id
    AWS_SECRET_ACCESS_KEY: credentials.secret_access_key
    BUCKET: credentials.bucket
```

After deploying the application, the environment variables with values from service bindings
are exported into the application's process.


## Alternatives

Libraries like `node-cfenv` and python's `cfenv` help to extract configuration from the
VCAP_SERVICES structure.  They are great when source code could be adopted to use
CloudFoundry's format.

Some applications though, rely on predefined environment variables, and it could be hard
to change them.  This buildpack helps to overcome the problem.


## License

Apache License 2.0
