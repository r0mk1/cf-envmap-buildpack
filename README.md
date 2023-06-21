# cf-envmap-buildpack

The buildpack exposes a configuration from VCAP_SERVICES and
VCAP_APPLICATION into user-defined environment variables.

It creates a `.profile`-like file from a provided
[ERB](https://docs.ruby-lang.org/en/master/ERB.html) template.

## Usage

Add the buildpack to the app's `manifest.yml`:

```yaml
    buildpacks:
    - https://github.com/r0mk1/cf-envmap-buildpack.git
    ...
```

Create a file `cfenvmap.erb` in the root directory of the application with required commands, e.g.:

```erb
<% S3 = VCAP_SERVICES["objectstore"][0]["credentials"] -%>

export AWS_ACCESS_KEY_ID=<%= S3["access_key_id"] %>
export AWS_SECRET_ACCESS_KEY=<%= S3["secret_access_key"] %>
export BUCKET=<%= S3["bucket"] %>

<% DB = VCAP_SERVICES["postgresql-db"].find { |instance| instance["name"] == "test-db" }["credentials"] -%>

export PGHOST=<%= DB["hostname"] %>
export PGDATABASE=<%= DB["dbname"] %>
export PGPORT=<%= DB["port"] %>
export PGUSER=<%= DB["username"] %>
export PGPASSWORD=<%= DB["password"] %>
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
