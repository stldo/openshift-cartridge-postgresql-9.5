# OpenShift PostgreSQL 9.5 Cartridge

A cartridge for OpenShift containing PostgreSQL 9.5 with autovacuum activated. This package adds the option `track_address` to `postgresql.conf` ([patch gist](https://gist.github.com/sbtoledo/294c6f0c4fea31c153cd1d4413656554)), so an address for the statistics collector can be set and autovacuum enabled—circumventing the fact that OpenShift disables access to `localhost`. Its default configuration aims for better performance on small gears. To get the most out of larger instances, it's recommended to set [postgresql.conf](#configure) accordingly.

## Install

### Add to an existing application

```bash
rhc cartridge-add -a {myApplicationName} https://raw.githubusercontent.com/sbtoledo/openshift-cartridge-postgresql-9.5/master/metadata/manifest.yml
```

### Set default username

```bash
rhc cartridge-add -a {myApplicationName} --env OPENSHIFT_POSTGRESQL_USERNAME='{myUserName}' https://raw.githubusercontent.com/sbtoledo/openshift-cartridge-postgresql-9.5/master/metadata/manifest.yml
```

### Set default password

```bash
rhc cartridge-add -a {myApplicationName} --env OPENSHIFT_POSTGRESQL_PASSWORD='{myPassword}' https://raw.githubusercontent.com/sbtoledo/openshift-cartridge-postgresql-9.5/master/metadata/manifest.yml
```

### Add with contrib modules

```bash
rhc cartridge-add -a {myApplicationName} --env OPENSHIFT_POSTGRESQL_CONTRIB=true https://raw.githubusercontent.com/sbtoledo/openshift-cartridge-postgresql-9.5/master/metadata/manifest.yml
```

## Use

Use these environment variables to connect to your PostgreSQL server. This package doesn't store the password, so you'll need to provide it manually. The password is required for both connection types (either host or socket). If you forgot your password, see how to reset it in [Reset password](#reset-password) section.

- Host: `$OPENSHIFT_POSTGRESQL_HOST`
- Port: `$OPENSHIFT_POSTGRESQL_PORT`
- Socket: `$OPENSHIFT_POSTGRESQL_SOCKET`
- Username: `$OPENSHIFT_POSTGRESQL_USERNAME`

## Configure

You can edit the main configuration files in `$OPENSHIFT_POSTGRESQL_DIR/conf`, then simply reload the cartridge with `rhc cartridge-reload postgresql -a {myApplicationName}`. Alternatively, you can edit `$PGDATA/pg_hba.conf` and `$PGDATA/postgresql.conf` directly—in this case, don't forget that you must call `cartridge-restart` instead of `cartridge-reload` for the changes to take effect, otherwise your configuration will be overwritten by the default configuration file.

## Troubleshooting

### Reset password

You can reset the password of the default PostgreSQL user through SSH, issuing the command `pg_pwd {newPassword}`.

## License

The MIT License (MIT), © 2016 Saulo Toledo
