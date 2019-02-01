---
title: "Disaster Recovery"
draft: false
weight: 6
---

Backup and Restore
------------------

Crunchy PostgreSQL for Pivotal Cloud Foundry (PCF) Tile uses
[pgBackRest](https://github.com/pgbackrest/pgbackrest) as a dedicated
backup and archiving host. The tile comes pre-configured with nightly
physical backups of the database server for non-standalone plans:

  Day         Backup Type   Time
  ----------- ------------- ----------
  Sunday      Full          1 am UTC
  Monday      Incremental   1 am UTC
  Tuesday     Incremental   1 am UTC
  Wednesday   Incremental   1 am UTC
  Thursday    Incremental   1 am UTC
  Friday      Incremental   1 am UTC
  Saturday    Incremental   1 am UTC

**WARNING:** Archiving and backups are disabled by default in the
`standalone` plan. PCF users can set the `standalone_backups` parameter
to `true` to enable this feature.

Although backups only happen once a day, PostgreSQL is continuously
shipping the Write-Ahead-Logs (WAL) to the pgBackrest server. This means
that point-in-time recovery is possible, regardless of the schedule.

These backups not only offer peace of mind, but are used frequently by
the tile.

Crunchy PostgreSQL for PCF uses backups to create replicas in the stack.
By using backups in operations, we ensure that backup and restore
operations are functioning properly.

All archives from the database server are stored on the dedicated backup
host. This means that databases can be restored to specific points in
time.

Currently, individual databases cannot be restored. All databases are
restored in the shared cluster.

**Note:** Standalone plans do not include automatic backups by default.
When using a standalone plan, you should regularly perform manual
backups using external tools, or enable the backup using the service
creation/update parameter.

Restore Using Deltas
--------------------

These instructions demonstrate how the database can be restored using
only deltas between the backup and the database.

The following requires an administrator to SSH to the `postgresql`
servers of the service instance. For guidance on using SSH to connect to
servers in the service, see [Advanced Troubleshooting with the BOSH
CLI](https://docs.pivotal.io/pivotalcf/customizing/trouble-advanced.html).

**Note:** Pivotal recommends that you use BOSH CLI v2 or later when
running PCF v1.11 or later.

### Cluster Plans

1.  SSH into any `postgresql` server using the `bosh` tool:

    ```
    bosh ssh postgresql 0
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/0
    ```

2.  Identify the current primary by querying Consul:

    ```
    sudo su - vcap
    curl -sSL http://localhost:8500/v1/catalog/service/postgresql-zone1 \
    | jq -r '.[] | .Node, .ServiceTags'
    ```

3.  SSH into the `postgresql` server(s) identified above as replicas
    using the `bosh` tool:

    ```
    bosh ssh postgresql <index identified above>
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/<index identified above>
    ```

4.  Switch to the `root` user:

    ```
    sudo -i
    ```

5.  Stop monitoring `consul` and `postgresql` services:

    ```
    monit unmonitor consul
    monit stop consul
    monit unmonitor postgresql
    monit stop postgresql
    ```

6.  SSH into the `postgresql` server identified above as primary using
    the `bosh` tool:

    ```
    bosh ssh postgresql <index identified above>
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/<index identified above>
    ```

7.  Switch to the `root` user:

    ```
    sudo -i
    ```

8.  Stop monitoring `consul` and `postgresql` services:

    ```
    monit unmonitor consul
    monit stop consul
    sleep 60 # Give adequate time for consul to settle
    monit unmonitor postgresql
    monit stop postgresql
    ```

9.  Switch to the `vcap` user:

    ```
    su - vcap
    ```

10. Verify that backups are available:

    ```
    pgbackrest --stanza=main info
    ```

    Note that you may need to provide the `pgbackrest.conf` path:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf --stanza=main info
    ```

11. Restore the database:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf \
        --stanza=main \
        --delta \
        --log-level-console=detail \
        restore
    ```

12. Switch to `root`:

    ```
    sudo -i
    ```

13. Start the database:

    ```
    monit start postgresql
    monit monitor postgresql
    ```

14. Return the other systems to working order:

    ```
    monit start consul
    monit monitor consul
    ```

15. SSH into the `postgresql` server(s) identified above as replicas
    using the `bosh` tool:

    ```
    bosh ssh postgresql <index identified above>
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/<index identified above>
    ```

16. Switch to the `root` user:

    ```
    sudo -i
    ```

17. Resume monitoring `consul` and `postgresql` services:

    ```
    monit start consul
    monit monitor consul
    monit start postgresql
    monit monitor postgresql
    ```

### Standalone Plan

1.  SSH into the `postgresql` server using the `bosh` tool:

    ```
    bosh ssh postgresql 0
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/0
    ```

2.  Switch to the `root` user:

    ```
    sudo -i
    ```

3.  Stop monitoring the `postgresql` service:

    ```
    monit unmonitor postgresql
    monit stop postgresql
    ```

4.  Switch to the `vcap` user:

    ```
    su - vcap
    ```

5.  Verify that backups are available:

    ```
    pgbackrest --stanza=main info
    ```

    Note that you may need to provide the `pgbackrest.conf` path:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf --stanza=main info
    ```

6.  Restore the database:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf \
        --stanza=main \
        --delta \
        --log-level-console=detail \
        restore
    ```

7.  Switch to `root`:

    ```
    sudo -i
    ```

8.  Start the database:

    ```
    monit start postgresql
    monit monitor postgresql
    ```

Point in Time Recovery
----------------------

These instructions demonstrate how the database can be restored using a
point in time recovery.

The following requires an administrator to SSH to the `postgresql`
servers of the service instance. For guidance on using SSH to connect to
servers in the service, see [Advanced Troubleshooting with the BOSH
CLI](https://docs.pivotal.io/pivotalcf/2-2/customizing/trouble-advanced.html).

**Note:** Pivotal recommends that you use BOSH CLI v2 or later when
running PCF v1.11 or later.

### Cluster Plans

1.  SSH into any `postgresql` server using the `bosh` tool:

    ```
    bosh ssh postgresql 0
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/0
    ```

2.  Identify the current primary by querying Consul:

    ```
    sudo su - vcap
    curl -sSL http://localhost:8500/v1/catalog/service/postgresql-zone1 \
    | jq -r '.[] | .Node, .ServiceTags'
    ```

3.  SSH into the `postgresql` server(s) identified above as replicas
    using the `bosh` tool:

    ```
    bosh ssh postgresql <index identified above>
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/<index identified above>
    ```

4.  Switch to the `root` user:

    ```
    sudo -i
    ```

5.  Stop monitoring `consul` and `postgresql` services:

    ```
    monit unmonitor consul
    monit stop consul
    monit unmonitor postgresql
    monit stop postgresql
    ```

6.  SSH into the `postgresql` server identified above as primary using
    the `bosh` tool:

    ```
    bosh ssh postgresql <index identified above>
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/<index identified above>
    ```

7.  Switch to the `root` user:

    ```
    sudo -i
    ```

8.  Stop monitoring `consul` and `postgresql` services:

    ```
    monit unmonitor consul
    monit stop consul
    sleep 60 # Give adequate time for consul to settle
    monit unmonitor postgresql
    monit stop postgresql
    ```

9.  Switch to the `vcap` user:

    ```
    su - vcap
    ```

10. Verify that backups are available:

    ```
    pgbackrest --stanza=main info
    ```

    Note that you may need to provide the `pgbackrest.conf` path:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf --stanza=main info
    ```

11. Restore the database:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf \
        --stanza=main \
        --delta \
        --log-level-console=detail \
        --type=time "--target=2016-12-13 00:11:34.531619+00" \
        restore
    ```

12. Switch to `root`:

    ```
    sudo -i
    ```

13. Start the database:

    ```
    monit start postgresql
    monit monitor postgresql
    ```

14. Return the other systems to working order:

    ```
    monit start consul
    monit monitor consul
    ```

15. SSH into the `postgresql` server(s) identified above as replicas
    using the `bosh` tool:

    ```
    bosh ssh postgresql <index identified above>
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/<index identified above>
    ```

16. Switch to the `root` user:

    ```
    sudo -i
    ```

17. Resume monitoring `consul` and `postgresql` services:

    ```
    monit start consul
    monit monitor consul
    monit start postgresql
    monit monitor postgresql
    ```

### Standalone Plan

1.  SSH into the `postgresql` server using the `bosh` tool:

    ```
    bosh ssh postgresql 0
    ```

    If using the BOSH v2+ Client:

    ```
    bosh -e $ENV -d $DEPLOYMENT ssh postgresql/0
    ```

2.  Switch to the `root` user:

    ```
    sudo -i
    ```

3.  Stop monitoring the `postgresql` service:

    ```
    monit unmonitor postgresql
    monit stop postgresql
    ```

4.  Switch to the `vcap` user:

    ```
    su - vcap
    ```

5.  Verify that backups are available:

    ```
    pgbackrest --stanza=main info
    ```

    Note that you may need to provide the `pgbackrest.conf` path:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf --stanza=main info
    ```

6.  Restore the database:

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf \
        --stanza=main \
        --delta \
        --log-level-console=detail \
        --type=time "--target=2016-12-13 00:11:34.531619+00" \
        restore
    ```

    **Note:** The default behavior for restore is to use the last backup
    but an earlier backup can be specified with the –set option

    ```
    pgbackrest --config=/var/vcap/store/pgbackrest/config/pgbackrest.conf \
        --stanza=main \
        --delta \
        --log-level-console=detail \
        --type=time "--target=2016-12-13 00:11:34.531619+00" \
        --set=ID_OF_PREVIOUS_BACKUP
        restore
    ```

7.  Switch to `root`:

    ```
    sudo -i
    ```

8.  Start the database:

    ```
    monit start postgresql
    monit monitor postgresql
    ```

9.  Finally, confirm that replication is still working. Connect to the
    primary PostgreSQL server and run:

    ```
    sudo su vcap
    psql -d replicationdb
    select * from pg_stat_replication;
    ```

pgBackrest Documentation
------------------------

For more information about pgBackrest restorations, please refer to the
official [PGBackRest
documentation](http://www.pgbackrest.org/user-guide.html).
