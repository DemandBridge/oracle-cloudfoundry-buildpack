# CloudFoundry Buildpack for Oracle

CloudFoundry buildpack for setting up Oracle Instant Client and the `LD_LIBRARY_PATH` to support language level bindings.

# Usage

## Add a detect hook (required)

In order for this buildpack to execute, it will look for a `.oracle.yml` file in your app's root.  The file can be empty but it must exist.

    touch .oracle.yml

## Add Buildpack

You'll need to use multiple buildpacks. This buildpack will need to be invoked first, followed by the buildpack of your choice. CloudFoundry API v3 supports configuring multiple buildbacks, or you can use [multi-buildpack](https://github.com/cloudfoundry/multi-buildpack).

### Setup using multi-buildpack

The benefit to using [multi-buildpack](https://github.com/cloudfoundry/multi-buildpack) is that you can version-control your environment changes. Inside `manifest.yml` add

```yml
buildpack: https://github.com/cloudfoundry/multi-buildpack
```

Then inside `multi-buildpack.yml`, add the following contents:

```yml
buildpacks:
- https://github.com/EricHorton/oracle-cloudfoundry-buildpack
- <other buildpack>
```

# Configuration (Optional)

It is sometimes desirable to use `ldap.ora`, `tnsnames.ora`, or `sqlnet.ora` to configure how Oracle connects to a database or to use `sqlnet.ora` to configure connection wallets. These files are often located in `$ORACLE_HOME/network/admin`.  This buildpack will correctly setup `$ORACLE_HOME` and `$TNS_ADMIN` to point to `$ORACLE_HOME/network/admin`.  A source location can be configured inside `.oracle.yml`

```yml
ldap.ora: config/tnsnames.ora
tnsnames.ora: config/tnsnames.ora
sqlnet.ora: config/sqlnet.ora
```

The files will be symlinked into `vendor/oracle-instantclient/network/admin`
