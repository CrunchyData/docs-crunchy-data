---
title: "Using the On-Demand Service"
draft: false
weight: 4
---

This topic describes how to use Crunchy PostgreSQL for Pivotal Cloud
Foundry (PCF) Tile.

Crunchy PostgreSQL Service
--------------------------

Crunchy PostgreSQL for Pivotal Cloud Foundry (PCF) Tile is an on-demand,
single-tenant PostgreSQL database service.

Crunchy PostgreSQL comes with one service: postgresql-10-odb

This service includes:

-   PostgreSQL
    -   Can be configured as either cluster (primary and replicas) or
        standalone

The cluster plans also include:

-   Load balancer
-   pgBackRest dedicated backup
-   Consul cluster

Backups performed by pgBackRest are stored within the service instance
by default. These backups can be used in the event of [disaster
recovery](/disaster-recovery), or to assist when migrating
from one service to another.

### PostgreSQL Plans

The `postgresql-10-odb` service comes with several preconfigured plans.

Each plan corresponds to different server size set up by the operator
when the service was deployed.

The following table provides information about the out-of-the-box
cluster configurations. Review each plan offering and choose the correct
database plan for the application.

  Plan Name              Description
  ---------------------- ----------------------------------------------------------------------------------------
  `standalone`           Single PostgreSQL server, useful for small development purposes
  `standalone-replica`   Single PostgreSQL server configured as a remote replica for an existing parent service
  `general`              Standard service plan, configured to fit most general use cases
  `general-monitor`      Same as `general`, but with added support for Grafana monitoring
  `custom`               Uses similar `general` plan settings, configurable to specific use cases
  `custom-monitor`       Same as `custom` but with added support for Grafana monitoring

The current limitations for these cluster configurations are as follows:

-   The number of replicas are only configurable during initial setup.
-   All replicas use `async` replication. `sync` replication is not yet
    configurable.
-   Standalone instances should be used with care, as they lack any of
    the integrated backup or recovery systems provided in the General
    plans.

#### Standalone Instances

A standalone instance may be used in one of two ways:

1.  Isolated single PostgreSQL instance, with no supporting software
2.  Isolated replica instance, allowing to ‘add on’ remote disaster
    recovery to an existing cluster.

**Note:** These instances are to be used with care, as they have no
internal support services that are typically deployed with the standard
service plans. In the event that any question or concern regarding
implementation arises, please contact Crunchy Support.

#### Using the Standalone-Replica Plan

There are several prerequisites when creating a service using the
standalone-replica plan type.

-   Services must be in the same VPC or their networks must be peered.
-   A 'parent’ cluster service (using any other service plan) must
    exist.

To enable remote replication on an existing cluster, do the following:

1.  Run the following command:
    ` cf update-service $SERVICE_NAME -c '{"allow_remote_replication": true}'`

2.  Create a service key from the parent cluster with the following
    parameters:
    ` cf create-service-key $CLUSTER_SERVICE_NAME \ $SERVICE_KEY_NAME -c '{"remote_replica": true}'`
    This service key will contain all of the relevant info for binding
    the standalone-replica instance to the parent cluster.

You can query this information directly from the service key by running
the following command:

```
cf service-key $CLUSTER_SERVICE_NAME $SERVICE_KEY_NAME
```

These parameters provided from the service key are required during the
creation of the standalone-replica service.

Because of the complexity of some of these parameters, Crunchy Data
recommends to compose a json file for creation:

```
{
        "db_name": "same_as_parent_service",
        "db_username": "same_as_parent_service",
        "owner_name": "John Q. Example",
        "owner_email": "example@fabrikam.com",
        "haproxy_ip": "192.168.1.1",
        "pgbackrest_ip": "192.168.1.2",
        "replication_user": "testuser123",
        "replication_password": "supersecure",
        "replication_private_key": "-----BEGIN RSA PRIVATE KEY-----...-----END RSA PRIVATE KEY-----"
}
```

The service can be created by:

```
cf create-service postgresql-10-odb standalone-replica \
$STANDALONE_SERVICE_NAME -c values.json
```

For more information, see Creating a New Service.

### Viewing Service Plans in Cloud Foundry

#### Using Cloud Foundry CLI

Run `cf marketplace -s postgresql-10-odb`. This will display information
about each service plan offered by the Crunchy service. Choose the plan
that best fits your application requirements.

#### Using Pivotal Apps Manager

1.  Select **Marketplace** on the left navigation menu.
2.  Find and select **Crunchy PostgreSQL** in the list of available
    services.
3.  Browse the available service plans and choose the one that best fits
    your application requirements.

Creating an On-Demand PostgreSQL Service
----------------------------------------

### Service Creation Parameters

#### Service Creation Parameters

The following service configuration parameters are supported during
service creation.

  Parameter                    Description                                                        Required?                                 Example Value                           Parameter Limitations                                                                                                         Notes
  ---------------------------- ------------------------------------------------------------------ ----------------------------------------- --------------------------------------- ----------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  `db_name`                    Name of the service database                                       Yes                                       `example_db`                            Value must begin with a lowercase alpha character and may contain only lowercase alpha, numeric, and underscore characters.   
  `db_username`                Name of the service database user                                  Yes                                       `john_example`                          Value must begin with a lowercase alpha character and may contain only lowercase alpha, numeric, and underscore characters.   This user will be the owner of the service database and be granted all privileges on the service database.
  `owner_name`                 Contact name for the service database                              Yes                                       `John Example`                                                                                                                                                        
  `owner_email`                Contact email for the service database                             Yes                                       `john.example@fabrikam.com`             Must be a valid email address                                                                                                 
  `allow_remote_replication`   Enables PostgreSQL archiving to a remote standalone-replica        Optional                                  `true`                                  Value must be boolean true or false                                                                                           
  `bloat_check_schedule`       Recurrence of database bloat checking                              Optional                                  `@weekly`                               Value must follow `cron`-style syntax                                                                                         Only valid if `monitoring` is true
  `compliance_profile`         Compliance logging level used in cluster                           Optional                                  `base_compliance`                       Valid values are `base_compliance`, `stig_compliance_full`, or `pgaudit_disabled`.                                            Specifying `stig_compliance_full` configures PgAudit as specified in the [PostgreSQL STIG](https://www.crunchydata.com/postgres-stig/PGSQL-STIG-9.5+.pdf), which may impact performance. Specifying `pgaudit_disabled` disables PgAudit logging and is not recommended. The configuration details for each profile can be found under Compliance Profiles.
  `db_encoding`                Database encoding for the service database                         Optional                                  `UTF8`                                                                                                                                                                
  `enable_syslog`              Enables PostgresQL logging to syslog daemon                        Optional                                  `true`                                  Value must be boolean true or false                                                                                           Enable if you plan to use syslog-release to forward your logs remotely.
  `hba_addresses`              Addresses authorized to connect to the service database            Optional                                  `10.244.9.101/32`                       Value must be space separated string of addresses in CIDR format                                                              
  `plr`                        Enables PL/R extensions                                            Optional                                  `true`                                  Value must be boolean true or false                                                                                           
  `postgis`                    Enable PostGIS and related extensions                              Optional                                  `true`                                  Value must be boolean true or false                                                                                           Enabling PostGIS will install `postgis`, `postgis_tiger_geocoder`, `postgis_topology`, and `fuzzystrmatch`
  `postgres_fdw`               Enables PostgreSQL Foreign Data Wrapper                            Optional                                  `true`                                  Value must be boolean true or false                                                                                           
  `redis_fdw`                  Enables Redis Foreign Data Wrapper                                 Optional                                  `true`                                  Value must be boolean true or false                                                                                           
  `standalone_backups`         Enables PostgreSQL archiving and PgBackRest automated backups      Optional                                  `true`                                  Value must be boolean true or false                                                                                           Only valid for `standalone` plan
  `haproxy_ip`                 Replication value for replication cluster haproxy instance         Yes (Only for `stanalone-replica` Plan)   `10.244.9.114`                          Value must be string with valid IP address                                                                                    Only valid for `standalone-replica` plan
  `pgbackrest_ip`              Replication value for replication cluster pgbackrest instance      Yes (Only for `stanalone-replica` Plan)   `10.244.9.112`                          Value must be string with valid IP address                                                                                    Only valid for `standalone-replica` plan
  `replication_password`       Replication value password to perform remote cluster replication   Yes (Only for `stanalone-replica` Plan)   `securepassword`                        Value must be a valid string                                                                                                  Only valid for `standalone-replica` plan
  `replication_private_key`    Replication key pair to allow remote cluster replication           Yes (Only for `stanalone-replica` Plan)   `-----BEGIN RSA PRIVATE KEY-----\n..`   Value must be valid private key, all newlines escaped in string with `\n`                                                     Only valid for `standalone-replica` plan
  `replication_user`           Replication value user to perform remote cluster replication       Yes (Only for `stanalone-replica` Plan)   `john_example`                          Value must begin with a lowercase alpha character and may contain only lowercase alpha, numeric, and underscore characters.   Only valid for `standalone-replica` plan

### Optional PostgreSQL Settings Parameters

#### PostgreSQL Internal Settings

The default configuration settings for PostgreSQL attempt to apply the
most tuned settings for each plan and service instance.

It should not be necessary to change any of the low-level settings
unless a service instance is being used in a very advanced
configuration.

However, if further configuration is found to be necessary, the
following settings can be overridden manually:

-   `pgc_autovacuum_analyze_scale_factor`
-   `pgc_autovacuum_analyze_threshold`
-   `pgc_autovacuum_freeze_max_age`
-   `pgc_autovacuum_max_workers`
-   `pgc_autovacuum_vacuum_cost_delay`
-   `pgc_autovacuum_vacuum_scale_factor`
-   `pgc_autovacuum_vacuum_threshold`
-   `pgc_checkpoint_completion_target`
-   `pgc_log_checkpoints`
-   `pgc_log_connections`
-   `pgc_log_disconnections`
-   `pgc_log_duration`
-   `pgc_log_hostname`
-   `pgc_log_line_prefix`
-   `pgc_log_min_duration_statement`
-   `pgc_log_min_error_statement`
-   `pgc_log_min_messages`
-   `pgc_log_timezone`
-   `pgc_maintenance_work_mem`
-   `pgc_max_locks_per_transaction`
-   `pgc_max_wal_senders`
-   `pgc_max_wal_size`
-   `pgc_max_worker_processes`
-   `pgc_random_page_cost`
-   `pgc_timezone`
-   `pgc_wal_buffers`
-   `pgc_wal_keep_segments`
-   `pgc_wal_keep_segments`
-   `pgc_work_mem`
-   `pgc_work_mem`

More information specific to the individual values can be found in the
[Official PostgreSQL
Documentation](https://www.postgresql.org/docs/9.5/static/runtime-config.html).

#### Example Usage for PostgreSQL Internal Settings

For example, if you wish to create a service with custom PostgreSQL
setting for `timezone` and `log_timezone`:

```
cf create-service postgresql-10-odb general myService -c '
{
    "db_name": "testdb",
    "db_username": "testuser",
    "owner_name": "Test User",
    "owner_email": "test.user@fabrikam.com",
    "pgc_timezone": "America/New_York",
    "pgc_log_timezone": "America/New_York"
}'
```

It is also possible to edit the PostgreSQL Config settings within an
existing service.

For example, the following `update-service` command would set `timezone`
and `log_timezone` to UTC:

```
cf update-service myService -c '
{
    "pgc_timezone": "UTC",
    "pgc_log_timezone": "UTC"
}'
```

#### Optional Extensions

The following extensions are available to install on the service.

The extensions are prefixed with `ext_` as part of the parameter passed
into the `create-service` or `update-service` command.

-   `ext_address_standardizer_data_us`
-   `ext_address_standardizer`
-   `ext_adminpack`
-   `ext_amcheck`
-   `ext_autoinc`
-   `ext_bloom`
-   `ext_btree_gin`
-   `ext_btree_gist`
-   `ext_chkpass`
-   `ext_citext`
-   `ext_cube`
-   `ext_dblink`
-   `ext_dict_int`
-   `ext_dict_xsyn`
-   `ext_earthdistance`
-   `ext_file_fdw`
-   `ext_fuzzystrmatch`
-   `ext_hstore_plperl`
-   `ext_hstore_plperlu`
-   `ext_hstore_plpython3u`
-   `ext_insert_username`
-   `ext_intagg`
-   `ext_intarray`
-   `ext_isn`
-   `ext_lo`
-   `ext_ltree`
-   `ext_moddatetime`
-   `ext_pageinspect`
-   `ext_pg_buffercache`
-   `ext_pg_freespacemap`
-   `ext_pg_prewarm`
-   `ext_pg_stat_statements`
-   `ext_pg_trgm`
-   `ext_pg_visibility`
-   `ext_pgrowlocks`
-   `ext_pgstattuple`
-   `ext_plperl`
-   `ext_plperlu`
-   `ext_plpgsql`
-   `ext_plpython3u`
-   `ext_plr`
-   `ext_postgis_tiger_geocoder`
-   `ext_postgis_topology`
-   `ext_postgis`
-   `ext_postgres_fdw`
-   `ext_redis_fdw`
-   `ext_refint`
-   `ext_seg`
-   `ext_set_user`
-   `ext_sslinfo`
-   `ext_tablefunc`
-   `ext_tcn`
-   `ext_timetravel`
-   `ext_tsm_system_rows`
-   `ext_tsm_system_time`
-   `ext_unaccent`
-   `ext_uuid-ossp`
-   `ext_xml2`

#### Unsupported Extensions

The following extensions are no longer supported due to deprecation from
the BOSH Stemcells:

-   hstore\_plpython2u
-   hstore\_plpythonu
-   ltree\_plpython2u
-   ltree\_plpythonu
-   plpython2u
-   plpythonu
-   pltcl
-   pltclu

#### Example Usage for Optional Extensions

For example, if you wish to create a service with the `btree_gist`
extension installed:

```
cf create-service postgresql-10-odb general myService -c '
{
    "db_name": "testdb",
    "db_username": "testuser",
    "owner_name": "Test User",
    "owner_email": "test.user@fabrikam.com",
    "ext_btree_gist": true
}'
```

It is also possible to add or remove extensions from an existing
service.

For example, the following `update-service` command would install
`postgis`, and remove `btree_gist`:

```
cf update-service myService -c '
{
    "ext_postgis": true,
    "ext_btree_gist": false
}'
```

### Creating a New Service

Follow the steps below to create and bind a service instance of Crunchy
PostgreSQL to use with your app.

**Note:** With an on-demand service, new servers are built and
configured when a new service is created. Therefore, this step takes
some time to complete. You may check the status of your service from the
space dashboard in the Services tab. Ensure that the status of the
service is **create succeeded** before proceeding.

1.  Create a service instance named myService using the following
    command as a template:
    ` cf create-service postgresql-10-odb general myService -c '{    "db_name": "exampledb",    "db_username": "exampleuser",    "owner_name": "Example User",    "owner_email": "example.user@company.com" }'`

2.  Bind the service instance to your app:
    ` cf bind-service <APP_NAME> myService`

3.  Run the following commands to restage your app so that it can use
    the service: ` cf restage <APP_NAME>` The service will then be
    available for use within any bound application.

A more advanced example, using custom plans:

```
cf create-service postgresql-10-odb custom customService -c '{
    "db_name": "exampledb",
    "db_username": "exampleuser",
    "owner_name": "Example User",
    "owner_email": "example.user@company.com",
    "server_type": "crunchy-small",
    "num_of_replicas": 2,
    "disk_size": "64GB",
    "allow_remote_replication": true,
    "db_connection_limit": 200
}'
```

Further, in instances where `"` chars might cause issues in the command
prompt (such as with Windows CMD), instances where consistent service
parameters are used, or in instances where private data would be passed
in the parameters, it is possible to create a separate JSON file with
all of the parameters to be used in service creation.

Create a JSON file with required values: `param.json`

```
{
        "db_name": "testdb",
        "db_username": "testdb",
        "owner_name": "Example User",
        "owner_email": "example.user@company.com",
        "super_secret_value": "dont_tell_anyone"
}
```

Then use this file as the JSON argument in the service creation:

```
cf create-service postgresql-10-odb general myService -c param.json
```

Updating an On-Demand Service
-----------------------------

### Configuration Parameters

#### Standard Service Plans

The following service configuration parameters are supported during
service update:

  Parameter                    Description                                                                           Required?              Example Value       Parameter Limitations                                                            Notes
  ---------------------------- ------------------------------------------------------------------------------------- ---------------------- ------------------- -------------------------------------------------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  `add_hba_addresses`          Add additional addresses authorized to connect to the service database                Optional               `10.244.9.101/32`   Value must be space separated string of addresses in CIDR format                 
  `allow_remote_replication`   Enable database replication to another service                                        Optional (See Notes)   `true`              Value must be boolean true or false                                              This parameter is a required prerequisite for enabling standalone-replica instances to communicate with the service.
  `compliance_profile`         The compliance level to use for the database cluster                                  Optional               `base_compliance`   Value must be `base_compliance`, `stig_compliance_full`, or `pgaudit_disabled`   The default and recommended value is `base_compliance`. Specifying `stig_compliance_full` configures PgAudit as specified in the [PostgreSQL STIG](https://www.crunchydata.com/postgres-stig/PGSQL-STIG-9.5+.pdf), which may impact performance. Specifying `pgaudit_disabled` disables PgAudit logging and is not recommended. The configuration details for each profile can be found under Compliance..
  `delete_hba_addresses`       Remove addresses that were previously authorized to connect to the service database   Optional               `10.244.9.101/32`   Value must be space separated string of addresses in CIDR format                 You can only remove addresses that have been added to the service through the `hba_addresses` and/or `add_hba_addresses` parameters.
  `enable_syslog`              Enables PostgreSQL logging to syslog daemon                                           Optional               `true`              Value must be boolean true or false                                              This parameter is required to forward PostgreSQL logs with the syslog-release plugin.
  `standalone_backups`         Enables PostgreSQL archiving and PgBackRest automated backups                         Optional               `true`              Value must be boolean true or false                                              Only valid for `standalone plan`.

#### PostgreSQL Internal Settings

The default configuration settings for PostgreSQL attempt to apply the
most tuned settings for each plan and service instance.

It should not be necessary to change any of the low-level settings
unless a service instance is being used in a very advanced
configuration.

However, if further configuration is found to be necessary, the
following settings can be overridden manually:

-   `pgc_autovacuum_analyze_scale_factor`
-   `pgc_autovacuum_analyze_threshold`
-   `pgc_autovacuum_freeze_max_age`
-   `pgc_autovacuum_max_workers`
-   `pgc_autovacuum_vacuum_cost_delay`
-   `pgc_autovacuum_vacuum_scale_factor`
-   `pgc_autovacuum_vacuum_threshold`
-   `pgc_checkpoint_completion_target`
-   `pgc_log_checkpoints`
-   `pgc_log_connections`
-   `pgc_log_disconnections`
-   `pgc_log_duration`
-   `pgc_log_hostname`
-   `pgc_log_line_prefix`
-   `pgc_log_min_duration_statement`
-   `pgc_log_min_error_statement`
-   `pgc_log_min_messages`
-   `pgc_log_timezone`
-   `pgc_maintenance_work_mem`
-   `pgc_max_locks_per_transaction`
-   `pgc_max_wal_senders`
-   `pgc_max_wal_size`
-   `pgc_max_worker_processes`
-   `pgc_random_page_cost`
-   `pgc_timezone`
-   `pgc_wal_buffers`
-   `pgc_wal_keep_segments`
-   `pgc_wal_keep_segments`
-   `pgc_work_mem`
-   `pgc_work_mem`

More information specific to the individual values can be found in the
[Official PostgreSQL
Documentation](https://www.postgresql.org/docs/9.5/static/runtime-config.html).

#### Example Usage for PostgreSQL Internal Settings

For example, if you wish to create a service with custom PostgreSQL
setting for `timezone` and `log_timezone`:

```
cf create-service postgresql-10-odb general myService -c '
{
    "db_name": "testdb",
    "db_username": "testuser",
    "owner_name": "Test User",
    "owner_email": "test.user@fabrikam.com",
    "pgc_timezone": "America/New_York",
    "pgc_log_timezone": "America/New_York"
}'
```

It is also possible to edit the PostgreSQL Config settings within an
existing service.

For example, the following `update-service` command would set `timezone`
and `log_timezone` to UTC:

```
cf update-service myService -c '
{
    "pgc_timezone": "UTC",
    "pgc_log_timezone": "UTC"
}'
```

#### Optional Extensions

The following extensions are available to install on the service.

The extensions are prefixed with `ext_` as part of the parameter passed
into the `create-service` or `update-service` command.

-   `ext_address_standardizer_data_us`
-   `ext_address_standardizer`
-   `ext_adminpack`
-   `ext_amcheck`
-   `ext_autoinc`
-   `ext_bloom`
-   `ext_btree_gin`
-   `ext_btree_gist`
-   `ext_chkpass`
-   `ext_citext`
-   `ext_cube`
-   `ext_dblink`
-   `ext_dict_int`
-   `ext_dict_xsyn`
-   `ext_earthdistance`
-   `ext_file_fdw`
-   `ext_fuzzystrmatch`
-   `ext_hstore_plperl`
-   `ext_hstore_plperlu`
-   `ext_hstore_plpython3u`
-   `ext_insert_username`
-   `ext_intagg`
-   `ext_intarray`
-   `ext_isn`
-   `ext_lo`
-   `ext_ltree`
-   `ext_moddatetime`
-   `ext_pageinspect`
-   `ext_pg_buffercache`
-   `ext_pg_freespacemap`
-   `ext_pg_prewarm`
-   `ext_pg_stat_statements`
-   `ext_pg_trgm`
-   `ext_pg_visibility`
-   `ext_pgrowlocks`
-   `ext_pgstattuple`
-   `ext_plperl`
-   `ext_plperlu`
-   `ext_plpgsql`
-   `ext_plpython3u`
-   `ext_plr`
-   `ext_postgis_tiger_geocoder`
-   `ext_postgis_topology`
-   `ext_postgis`
-   `ext_postgres_fdw`
-   `ext_redis_fdw`
-   `ext_refint`
-   `ext_seg`
-   `ext_set_user`
-   `ext_sslinfo`
-   `ext_tablefunc`
-   `ext_tcn`
-   `ext_timetravel`
-   `ext_tsm_system_rows`
-   `ext_tsm_system_time`
-   `ext_unaccent`
-   `ext_uuid-ossp`
-   `ext_xml2`

#### Unsupported Extensions

The following extensions are no longer supported due to deprecation from
the BOSH Stemcells:

-   hstore\_plpython2u
-   hstore\_plpythonu
-   ltree\_plpython2u
-   ltree\_plpythonu
-   plpython2u
-   plpythonu
-   pltcl
-   pltclu

#### Example Usage for Optional Extensions

For example, if you wish to create a service with the `btree_gist`
extension installed:

```
cf create-service postgresql-10-odb general myService -c '
{
    "db_name": "testdb",
    "db_username": "testuser",
    "owner_name": "Test User",
    "owner_email": "test.user@fabrikam.com",
    "ext_btree_gist": true
}'
```

It is also possible to add or remove extensions from an existing
service.

For example, the following `update-service` command would install
`postgis`, and remove `btree_gist`:

```
cf update-service myService -c '
{
    "ext_postgis": true,
    "ext_btree_gist": false
}'
```

### Updating an Existing Service

#### Using Cloud Foundry CLI

To update an existing on-demand service instance’s configuration
parameters, run `cf update-service SERVICE-INSTANCE -c SERVICE-PARAMS`,
substituting `SERVICE-INSTANCE` with the name of your service, and
`SERVICE-PARAMS` with a JSON object containing the parameters to update.
For example:

```
cf update-service myService -c '{
    "add_hba_addresses": "192.168.0.24/32 192.168.1.0/24"
}'
```

To update the plan of an existing on-demand service instance, run
`cf update-service SERVICE-INSTANCE -p PLAN-NAME`, substituting
`PLAN-NAME` with the name of the desired service plan. For example, to
switch to the `general-monitor` plan:

```
cf update-service myService -p general-monitor
```

**Note:** New servers may be created and/or reconfigured when an
on-demand service is updated. Therefore, this step will take some time
to complete. You can check the status of your service my running
`cf services` and looking for `SERVICE-INSTANCE` or with
`cf service SERVICE-INSTANCE`.

#### Using Pivotal Apps Manager UI

1.  From the space dashboard, go to the **Services** tab.
2.  Select your service.
3.  Go to the **Settings** tab.
4.  In the fields under **Configure Instance**, add any service update
    supported parameters. Click **+** to create additional fields.
5.  Once the desired parameters have been added, click **Update**.

**Note:** New servers may be created and/or reconfigured when an
on-demand service is updated. Therefore, this step will take some time
to complete. You can check the status service from the space dashboard,
in the **Services** tab.

**WARNING:** Prior to upgrading a cluster-type plan (`general`,
`custom`, etc) to a standalone plan, it is **imperative** that the
cluster is healthy and `postgresql/0` server is healthy. Failure to do
so will result in data loss. For guidance, contact Crunchy Support prior
to upgrading the service.

### Updating the Service Plan

#### Using Cloud Foundry CLI

To change the service plan of an existing on-demand service instance,
run `cf update-service SERVICE_INSTANCE -p SERVICE_PLAN`, substituting
`SERVICE_INSTANCE` with the name of your service and `SERVICE_PLAN` with
the desired service plan. For example:

```
cf update-service myService -p large
```

When updating to a standard service plan, service configuration
parameters are optional. When updating to the custom service plan, you
must specify the required parameters listed
under the Custom Service Plan section. For example:

```
cf update-service myService -p custom -c '{
    "db_connection_limt": 500,
    "disk_size": "512GB",
    "num_of_replicas": 2,
    "server_type:" "crunchy-large"
}'
```

**IMPORTANT!** The on-demand service currently only supports updating
from a standard service plan to the custom service plan.

New servers may be created and/or reconfigured when an on-demand service
is updated. Therefore, this step will take some time to complete. You
can check the status of your service my running `cf services` and
looking for `SERVICE_INSTANCE` or with `cf service SERVICE_INSTANCE`.

Integrating Applications
------------------------

The following sections provide information on integrating an application
with a newly created service.

### Service Bindings and Service Keys

Service bindings are used for Cloud Foundry applications and services.
When an application is bound to a service, the service credentials are
automatically added to the `VCAP_SERVICES` environment variable within
the application container. Unique credentials are generated for each
service binding and are removed when the binding is deleted.

**WARNING:** Applications must obtain the service binding credentials
dynamically from the application container environment. Do not use
service binding credentials outside of a Cloud Foundry application.
Instead, use service keys for this use case.

Service keys are used for external applications that need to use Cloud
Foundry services. Creating a service key returns the service credentials
JSON object, which can be used by the external application as
appropriate.

### Service Credentials

The following keys are available in the service credentials JSON object:

-   **db\_host**: IP address for database write commands
-   **db\_name**: database name
-   **db\_port**: TCP port for database write commands
-   **jdbc\_read\_uri**: JDBC connection URI for database read commands;
    `jdbc:postgresql://db_host:db_port/db_name`
-   **jdbc\_uri**: JDBC connection URI for database write commands;
    `jdbc:postgresql://read_host:read_port/db_name`
-   **password**: password for database user
-   **read\_host**: IP address for database read commands
-   **read\_port**: TCP port for database read commands
-   **read\_uri**: standard postgresql connection URI for database read
    commands; `postgresql://username:password@db_host:db_port/db_name`
-   **service\_id**: service instance identifier
-   **service\_role**: the database role that is granted privileges to
    the service instance database; this role can not be used to connect
    to the database directly
-   **uri**: standard postgresql connection URI for database write
    commands;
    `postgresql://username:password@read_host:read_port/read_name`
-   **username**: the database role used to connect to the service
    instance database; this role inherits privileges from
    **service\_role** and is unique for each binding

The following keys are available in cluster plans:

-   **dashboard\_user**: username for the dashboard application
-   **dashboard\_password**: password for the dashboard application

The following keys are available if one of the monitoring plans were
selected:

-   **prometheus\_host**: host of the Prometheus server for monitoring
    data
-   **prometheus\_port**: connection port for the Prometheus server
-   **prometheus\_username**: username for accessing Prometheus
-   **prometheus\_password**: password for accessing Prometheus

### Managing Service Keys

#### Creating a Service Key

To create a service key for an external application, run
`cf create-service-key SERVICE_NAME SERVICE_KEY_NAME`, substituting
`SERVICE_NAME` with the name of your service and `SERVICE_KEY_NAME` with
a name for the service key, such as `SERVICE_NAME_sk`. The service
credentials JSON object can be viewed by running
`cf service-key SERVICE_NAME SERVICE_KEY_NAME`. For example:

```
cf create-service-key myService myService_sk
cf service-key myService myService_sk
```

Service Keys presently accept one optional parameter:

-   “remote\_replica”: true

The `create-service-key` command expects these parameters as JSON:

```
cf create-service-key myService myService_sk -c '{"remote_replica": true}'
```

**Note:** Service key creation is not currently supported for
\`standalone-replica\` plan types.

#### Deleting a Service Key

To delete a service key, run
`cf delete-service-key SERVICE_NAME SERVICE_KEY_NAME`, substituting
`SERVICE_NAME` with the name of your service and `SERVICE_KEY_NAME` with
the name of the service key. For example:

```
cf delete-service-key myService myService_sk
```

#### Database Roles

##### Service Role

The service role is created at the same time as the service instance.
The name of the database role matches the value of the **db\_username**
parameter given to `cf create-service`. This role is granted ownership
of the service instance database and the `public` schema. The service
role is identified by the **service\_role** key of the binding
credentials object.

**Note:** The service role has the `NOLOGIN` attribute, so it cannot be
used directly to connect to the service instance database. The binding
role should be used to connect to the database.

##### Binding Role

A binding role is created for each service binding. This ensures each
binding or service key is a unique set of credentials. It is granted the
service role and inherits the service role’s privileges. The binding
role is identified by the **username** key of the binding credentials
object.

##### Database Object Ownership

A database object is owned by the role that creates it. Database objects
may only be altered or dropped by the owner or a role granted the
owner’s role.

The binding role will own any database objects it creates. Privileges
for these objects are granted to the service role. This allows
subsequent binding roles to access an object with the same privileges as
the binding role that created the object.

When a binding is destroyed, object ownership is transferred from the
binding role to the service role. This allows subsequent binding roles
to alter or drop database objects created by previous binding roles.

Services with multiple active bindings that create database objects may
have objects owned by multiple roles. In this scenario, an attempt to
alter or drop an object that is not owned by the current binding role or
the service role may fail with an error like the one below:

```
ERROR:  must be owner of ...
```

Therefore, it is recommended to create database objects as the service
role instead of the binding role. This can be accomplished by using the
`SET ROLE` command. For example:

```
SET ROLE <service_role>; CREATE TABLE t (id SERIAL, text TEXT);
```

If this is not possible, `SET ROLE` may be used when an object needs to
be altered or dropped:

```
SET ROLE <service_role>; ALTER TABLE t ADD COLUMN moretext TEXT;
```

#### Accessing the Primary Database Server

To access the primary database server, which should be used for database
write operations, use the `db_host` and `db_port` keys to construct a
connection string. The `uri` and `jdbc_uri` may also be used to connect
to the primary database server when supported by the client application
or library. For more information, see [Selectively Accessing
Clusters](/feature-overview#ports).

#### Accessing the Replica Database Server(s)

To access the replica database server(s), which should be used for
database read operations, use the `read_host` and `read_port` keys to
construct a connection string. The `read_uri` and `jdbc_read_uri` may
also be used to connect to the replica database server(s) when supported
by the client application or library. For more information, see
[Selectively Accessing Clusters](/feature-overview#ports).

### Binding a PCF App to an On-Demand Service

#### Using Cloud Foundry CLI

When the status of your service is **create succeeded** you can bind an
app to the service by running
`cf bind-service APP_NAME SERVICE_INSTANCE`, substituting `APP_NAME`
with the name of your PCF app and `SERVICE_INSTANCE` with the name of
your service. For example:

```
cf bind-service myApp myService
cf restage myApp
```

If the app does not support SCRAM-SHA-256 authentication, you can
optionally enable MD5 Auth methods at binding time:

```
cf bind-service myApp myService -c '{"use_md5_auth": true}'
cf restage myApp
```

**Note:** You must restage or restart your app before it will have
access to the service credentials.

**Note:** MD5 Authentication is considered cryptographically broken. Any
and all efforts should be made to utilize SCRAM-SHA-256 authentication
where possible.

#### Using Pivotal Apps Manager

1.  From the space dashboard, select the **Services** tab, then select
    your service.

2.  Select the **Overview** tab, then click the **Bind Apps** button.

3.  Under the **Bound Apps** section, tick the checkbox for the app you
    would like to bind to your service, then click **Save**.

### Unbinding a PCF App from an On-Demand Service

#### Using Cloud Foundry CLI

To unbind your app from your service, run
`cf unbind-service APP-NAME SERVICE-INSTANCE`, substituting `APP-NAME`
with the name of your PCF app and `SERVICE-INSTANCE` with the name of
your service. For example:

```
cf unbind-service myApp myService
```

#### Using Pivotal Apps Manager

1.  From the space dashboard, go to the **Services** tab.
2.  Select your service.
3.  Go to the **Overview** tab.
4.  Click **Bind Apps**.
5.  Under the **Bound Apps** section, click the **Edit Bindings** button
    and disable the checkbox for the app you want to unbind from your
    service.
6.  Click **Save**.

Deleting an On-Demand Service
-----------------------------

**WARNING:** This is a destructive process and will destroy the entire
service instance, including your database and backups. Ensure that you
export any data you need before performing this step.

### Using Cloud Foundry CLI

**Note:** You are required to unbind a service before the service can be
deleted.

To delete your service instance, run
`cf delete-service SERVICE_INSTANCE`, substituting `SERVICE_INSTANCE`
with the name of your service. For example:

```
cf delete-service myService
```

### Using Pivotal Apps Manager

1.  From the space dashboard, select the **Services** tab, then select
    your service.

2.  Select the **Settings** tab, then click the **Delete Service
    Instance** button.

Appendix
--------

### Common Errors

As part of the toolset to ensure that the Crunchy PostgreSQL for PCF
services are highly available, a number of internal checks and processes
run frequently to ensure the health of the system.

It’s possible that some of these checks fail due to actions performed
against the service.

The PostgreSQL Healthcheck ensures the following: - Role is expected
value: primary/replica - PostgreSQL is ready - Recovery Status, if
failover occurs - Replication Status - Filesystem usage

If any of the above tests return an unexpected value, the BOSH job is
alerted of this condition, allowing Platform Administrators to quickly
and easily identify troubled service instances.

### Connecting to an On-Demand Service from an External VPC

In order to connect to an on-demand service from an external VPC, you
must ensure that the external VPC and PaaS VPC are peered. This process
allows traffic to be routed between VPCs. You may see connection timeout
or connection refused errors if the VPCs are not peered.

Additionally, the client IP address or subnet must be added to the
`pg_hba` configuration of the service instance. This can be accomplished
during service creation with the `hba_addresses` parameter or during
service update with the `add_hba_addresses` parameter.

### Security and Compliance

The PostgreSQL Audit Extension (PgAudit) provides detailed session
and/or object audit logging via the standard PostgreSQL logging
facility. The goal is to provide Crunchy PostgreSQL users with
capability to produce audit logs that comply with ICS 500-27
requirements.

**WARNING:** Depending on settings, it is possible for PgAudit to
generate an enormous volume of logging. If log levels are set too high,
disk space and other system resources may become overutilized, which can
have a negative impact on PostgreSQL performance and stability.
Therefore, it is important to determine your exact audit requirements to
avoid logging too much.

The following table provides more information on the configuration
provided for each compliance profile.

-   **Profile** is the name of the `compliance_profile`.
-   **pgaudit.log** is the value(s) for `pgaudit.log`, which determine
    the type of statements that are captured in the audit log.
    -   **READ**: `SELECT` and `COPY` when the source is a relation or a
        query.
    -   **WRITE**: `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, and `COPY`
        when the destination is a relation.
    -   **FUNCTION**: Function calls and `DO` blocks.
    -   **ROLE**: Statements related to roles and privileges: `GRANT`,
        `REVOKE`, or `CREATE`, `ALTER`, and `DROP ROLE`.
    -   **DDL**: All `DDL` that is not included in the `ROLE` class.
    -   **MISC**: Miscellaneous commands, e.g. `DISCARD`, `FETCH`,
        `CHECKPOINT`, `VACUUM`.

  Profile                  pgaudit.log
  ------------------------ ------------------------------
  `base_compliance`        ddl, role, misc
  `stig_compliance_full`   ddl, role, misc, read, write
  `pgaudit_disabled`       N/A

For further information regarding Security and Compliance within this
service, see the [Security and Compliance](/security-and-compliance-information)
section.

### pgBackRest Documentation

For more information about pgBackRest restorations, see the official
[PGBackRest documentation](http://www.pgbackrest.org/user-guide.html).

### Crunchy PostgreSQL Manager

The Crunchy PostgreSQL Manager is a simple Cloud Foundry app to allow
for developer visibility into the service, showing VM health, disk
usage, and other useful information without the need for Platform
Administrator privileges.

**Note:** This app is only accessible to current Crunchy Data Customers.

1.  Obtain the source package from the Crunchy Data Access Portal.
2.  Follow the installation instructions contained within the tarball to
    deploy the PCF app.

**Note:** The Crunchy PostgreSQL Manager application may be requested to
bind to your service under certain circumstances for troubleshooting
purposes through Crunchy Support.
