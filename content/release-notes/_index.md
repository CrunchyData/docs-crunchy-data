---
title: "Release Notes"
draft: false
weight: 9
---

This topic provides release notes for Crunchy PostgreSQL for Pivotal
Cloud Foundry (PCF) Tile.

v05.1006.003
------------

**Release Date:** December XX, 2018

Software features in this release:

-   [Software Component Update] PostgreSQL v10.6-1 to v10.6-2
-   [Software Component Update] postgres-exporter v0.4.5 to v0.4.7
-   [Software Component Update] python-psycopg2 v2.7.5 to v2.7.6.1
-   [Software Component Update] python-requests v2.20.0 to v2.20.1
-   [Software Component Update] ODB SDK v0.21.2 to v0.24.0

Additional notes about this release:
------------------------------------

-   This release addresses a critical vulnerability in the ODB SDK. This
    upgrade is strongly recommended. Further information on this CVE is
    available here:
    [https://pivotal.io/security/cve-2018-15759](https://pivotal.io/security/cve-2018-15759)

    **WARNING:** Crunchy PostgreSQL for PCF cannot be directly upgraded
    from v4 with PostgreSQL v9.5 to v5 with PostgreSQL v10. Instead,
    perform a migration upgrade, rather than an in-place upgrade. For
    more information regarding migrations, see
    [Migration](https://docs.pivotal.io/partners/data-migration).
    If continued support is needed for PostgreSQL v9.5, contact [Crunchy
    Data for PCF](mailto:crunchypcf@crunchydata.com). For additional
    consulting assistance to plan a data migration effort, contact us at
    [info@crunchydata.com](mailto:info@crunchydata.com).

v05.1006.002
------------

**Release Date:** November 12, 2018

Software features in this release:

-   [Software Component Update] Improved failover selection
-   [Stability Enhancement] Resolve issue with disk health reporting on
    instances using over 80% disk
-   [New Software Feature] Allow for end-user specified PostgreSQL
    Extension installation
-   [New Software Feature] Allow for end-user specified PostgreSQL
    settings configuration
-   [New Software Feature] Support for SCRAM-SHA-256 Authentication
    -   SCRAM-SHA-256 Authentication is enabled and used by default on
        all new service instances as of this release
    -   A service configuration parameter has been added to allow for
        per-instance override to fall back to MD5 authentication
    -   MD5 Authentication is considered deprecated, and may be removed
        in a future release.
-   [New Software Feature] New parameter added for enabling PostgreSQL
    syslog logging
-   [New Plan Structure] BOSH Errand upgrade-all-service-instances now
    requires a Crunchy Data provided Upgrade Token
-   [New Software Feature] Support for isolated networks, all support
    packages provided as part of this release
-   [Software Component Update] PGBackRest v2.04-2 to v2.06-1
-   [Software Component Update] PostgreSQL v10.05 to v10.06
-   [Software Component Update] node\_exporter v0.15.2 to v0.16.0
-   [Software Component Update] python\_certifi v2018.1.18 to
    v2018.10.15
-   [Software Component Update] python\_click v6.7 to v7.0
-   [Software Component Update] python\_itsdangerous v0.24 to v1.1.0
-   [Software Component Update] python\_requests v2.19.1 to v2.20.0
-   [Software Component Update] python\_urllib3 v1.23 to v1.24
-   [Software Component Update] wal2json v1.0-2 to v1.0-3

Notes
-----

-   Crunchy PostgreSQL for PCF can **NOT** be directly upgraded from v4
    with PostgreSQL 9.5 to v5 with PostgreSQL 10.
-   Crunchy PostgreSQL for PCF v5 relies on functionality released in
    PCF v2.0 – v2.2 to provide support for “on-demand broker”
    capability.

    **WARNING:** Crunchy PostgreSQL for PCF cannot be directly upgraded
    from v4 with PostgreSQL v9.5 to v5 with PostgreSQL v10. Instead,
    perform a migration upgrade, rather than an in-place upgrade. For
    more information regarding migrations, see
    [Migration](https://docs.pivotal.io/partners/data-migration).
    If continued support is needed for PostgreSQL v9.5, contact [Crunchy
    Data for PCF](mailto:crunchypcf@crunchydata.com). For additional
    consulting assistance to plan a data migration effort, contact us at
    [info@crunchydata.com](mailto:info@crunchydata.com).

v05.1005.001
------------

**Release Date:** August 14, 2018

Software features in this release:

-   [New Plan Structure] Addition of the general and standalone plans
    -   The small, med, large, and extra large plans have been removed
        in favor of using a general, custom, custom-monitor,
        general-monitor, standalone, and standalone-replica plans
-   [New Service Offering] Standalone and Standalone Replica Services
    -   The standalone and standalone replica service offerings provide
        a single PostgreSQL instance with local pgBackRest that can
        either function independently or connect to a parent cluster for
        replication.
    -   A new configuration option for a standalone service instance,
        standalone\_backups, that can be set to true to turn on local
        pgBackRest backups.
    -   The `standalone-replica` plan can connect across foundations
        provided the two service networks have connectivity.
-   [Software Component Update] PGBackrest (v1.X to 2.04)
-   [Software Component Update] Pivotal On Demand Broker SDK (v0.18.0 to
    v0.21.2)
-   [Software Component Update] Pivotal Stemcell (3468 to 3541)
-   [Software Component Update] Crunchy PostgreSQL On Demand Broker
    version 05
    -   Enhanced acceptance tests
-   [Software Component Update] Crunchy PostgreSQL Service Adapter
    version 05
    -   Addition of a remote replication service key creation.
    -   Allows users to upgrade a standalone instance to a full cluster
        instance.
    -   Fixes a bug that caused configuration options to reset when new
        parameters were passed in to the service instance.
-   [Software Component Update] Crunchy PostgreSQL BOSH version 05
    -   This new software version provides the updated service
        offerings, plan restructure, and remote replication.
    -   Fixes a bug that caused services instances to no longer be able
        to bind/unbind with applications.
    -   Fixes a bug that caused servers to not recover when rebooted
        outside the scope of BOSH.
    -   Fixes a bug that caused health check failures during heavy
        PostgreSQL transaction load times.
    -   Fixes a bug that caused multiple HAProxy process to remain
        active after receiving a kill command.
-   [Software Component Update] Python Updates
    -   python-flask version 1.0.2
    -   python-flask-httpauth version 3.2.4
    -   python-idna version 2.7
    -   python-psycopg2 version 2.7.5
    -   python-requests version 2.19.1
    -   python-urllib3 version 1.23
-   [Software Component Update] pgaudit-set-user version 1.6.1
-   [New Software Feature] `wal2json` library availability
-   [New Software Feature] `pgc_` configuration parameters can now be
    updated through `update-service`
-   [New Configuration Option] Parallel upgrades for service instances
-   [New Configuration Option] Standalone configurations:
    -   Option for backups by default
    -   Customizable VM type, persistent storage, connection limit
-   [Stability Enhancemnet] Improves recovery from IaaS storage
    disconnection

Additional notes about this release:

-   Crunchy PostgreSQL for PCF can **NOT** be directly upgraded from v4
    with PostgreSQL 9.5 to v5 with PostgreSQL 10.
-   Crunchy PostgreSQL for PCF v5 relies on functionality released in
    PCF v2.0 – v2.2 to provide support for “on-demand broker”
    capability.

**WARNING:** Crunchy PostgreSQL for PCF cannot be directly upgraded from
v4 with PostgreSQL v9.5 to v5 with PostgreSQL v10. Instead, perform a
migration upgrade, rather than an in-place upgrade. For more information
regarding migrations, see
[Migration](https://docs.pivotal.io/partners/data-migration). If
continued support is needed for PostgreSQL v9.5, contact [Crunchy Data
for PCF](mailto:crunchypcf@crunchydata.com). For additional consulting
assistance to plan a data migration effort, contact us at
[info@crunchydata.com](mailto:info@crunchydata.com)

v05.090514.001
--------------

**Release Date:** August 14, 2018

Software features in this release:

-   [New Plan Structure] Addition of the general and standalone plans
    -   The small, med, large, and extra large plans have been removed
        in favor of using a general, custom, custom-monitor,
        general-monitor, standalone, and standalone-replica plans
-   [New Service Offering] Standalone and Standalone Replica Services
    -   The standalone and standalone replica service offerings provide
        a single PostgreSQL instance with local pgBackRest that can
        either function independently or connect to a parent cluster for
        replication.
    -   A new configuration option for a standalone service instance,
        standalone\_backups, that can be set to true to turn on local
        pgBackRest backups.
    -   The `standalone-replica` plan can connect across foundations
        provided the two service networks have connectivity.
-   [Software Component Update] PGBackrest (v1.X to 2.04)
-   [Software Component Update] Pivotal On Demand Broker SDK (v0.18.0 to
    v0.21.2)
-   [Software Component Update] Pivotal Stemcell (3468 to 3541)
-   [Software Component Update] Crunchy PostgreSQL On Demand Broker
    version 05
    -   Enhanced acceptance tests
-   [Software Component Update] Crunchy PostgreSQL Service Adapter
    version 05
    -   Addition of a remote replication service key creation.
    -   Allows users to upgrade a standalone instance to a full cluster
        instance.
    -   Fixes a bug that caused configuration options to reset when new
        parameters were passed in to the service instance.
-   [Software Component Update] Crunchy PostgreSQL BOSH version 05
    -   This new software version provides the updated service
        offerings, plan restructure, and remote replication.
    -   Fixes a bug that caused services instances to no longer be able
        to bind/unbind with applications.
    -   Fixes a bug that caused servers to not recover when rebooted
        outside the scope of BOSH.
    -   Fixes a bug that caused health check failures during heavy
        PostgreSQL transaction load times.
    -   Fixes a bug that caused multiple HAProxy process to remain
        active after receiving a kill command.
-   [Software Component Update] Python Updates
    -   python-flask version 1.0.2
    -   python-flask-httpauth version 3.2.4
    -   python-idna version 2.7
    -   python-psycopg2 version 2.7.5
    -   python-requests version 2.19.1
    -   python-urllib3 version 1.23
-   [Software Component Update] pgaudit-set-user version 1.6.1
-   [New Software Feature] `wal2json` library availability
-   [New Software Feature] pgc\_ configuration parameters can now be
    updated through `update-service`
-   [New Configuration Option] Parallel upgrades for service instances
-   [New Configuration Option] Standalone configurations:
    -   Option for backups by default
    -   Customizable VM type, persistent storage, connection limit
-   [Stability Enhancemnet] Improves recovery from IaaS storage
    disconnection

Notes
=====

-   Crunchy PostgreSQL for PCF can be directly upgraded from v4 with
    PostgreSQL 9.5 to v5 with PostgreSQL 9.5.
-   Crunchy PostgreSQL for PCF v5 relies on functionality released in
    PCF v2.0 – v2.2 to provide support for “on-demand broker”
    capability.

**Note:** Crunchy PostgreSQL for PCF can be directly upgraded from v4 to
v5. Services using versions of Crunchy PostgreSQL for PCF prior to v3
should first upgrade to v4, then directly to v5 for maximum stability.

**WARNING:** Crunchy PostgreSQL for PCF cannot be directly upgraded from
v4 with PostgreSQL v9.5 to v5 with PostgreSQL v10. Instead, perform a
migration upgrade, rather than an in-place upgrade. For more information
regarding migrations, see
[Migration](https://docs.pivotal.io/partners/data-migration). If
continued support is needed for PostgreSQL v9.5, contact [Crunchy Data
for PCF](mailto:crunchypcf@crunchydata.com). For additional consulting
assistance to plan a data migration effort, contact us at
[info@crunchydata.com](mailto:info@crunchydata.com)

v04.090513.001
--------------

**Release Date:** May 11, 2018

Software features in this release:

-   [Software Component Update] - PostgreSQL Update from v9.5.12 to
    v9.5.13
-   This release contains a variety of fixes from v9.5.12. For a list of
    changes, see the [PostgreSQL v9.5.13 release
    notes](https://www.postgresql.org/docs/9.5/static/release-9-5-13.html).

    **Note:** Crunchy PostgreSQL for PCF can be directly upgraded from
    v3 to v4. Services using versions of Crunchy PostgreSQL for PCF
    prior to v3 should first upgrade to v3, then directly to v4 for
    maximum stability.

**Note:** Crunchy PostgreSQL for PCF v4 relies on functionality released
in PCF v1.11 through v2.1 to provide support for “on-demand broker”
capability. To implement this functionality, Crunchy Data has made
material modifications to the Crunchy PostgreSQL for PCF manifest and
does not currently provide a direct upgrade path from Crunchy PostgreSQL
for PCF v1 or v2 to Crunchy PostgreSQL for PCF v4. To avoid confusion
between the two implementations, Crunchy Data has removed the Crunchy
PostgreSQL for PCF v1 and v2 software versions from Pivotal Network. If
you previously downloaded and/or deployed an evaluation copy of Crunchy
PostgreSQL for PCF v1 or v2, Crunchy Data is happy to provide ongoing
commercial subscription support. If you are interested in commercial
subscription support for Crunchy PostgreSQL for PCF, contact
sales@crunchydata.com.

v04.090512.001
--------------

**Release Date:** April 27, 2018

Software features in this release:

-   [New Software Feature] - Compliance Profiles
    -   Provide users with the option to produce audit logs that comply
        with ICS 500-27 requirements.
-   [New Software Feature] - Inspec
    -   Implement tool to automate the verification of the DISA Security
        Technical Implementation.
-   [Software Component Update] - PGMonitor Update (v1.4 to v1.6)
-   [Software Component Update] - Pivotal On Demand Broker SDK (v0.17.0
    to v0.18.0)
-   [Software Component Update] - Pivotal Stemcell (v3363 to v3468)
-   [Software Component Update] - Crunchy PostgreSQL On Demand Broker
    (v3 to v4)
    -   Provide application owners ability to upgrade a service from a
        smaller to larger size.
    -   Enhance acceptance tests.
-   [Software Component Update] - Crunchy PostgreSQL Service Adapter (v3
    to v4)
    -   Redesign of service adapter allowing for increased system
        compatibility and additional features.
-   [Software Component Update] - Crunchy PostgreSQL BOSH Release (v3 to
    v4)
    -   Monitoring system now reports statistics on all PostgreSQL
        instances available in the cluster.
    -   Remove smoke-test errand and introduce the post-deploy errand.
    -   Provide option for operators to customize the log rotation
        settings.
    -   Fix issue preventing log rotation because of incorrect
        permissions.
    -   Fix issue preventing the Redis Foreign Data Wrapper from being
        created.
    -   Fix issue preventing the upgrade-all-service-instances errand
        from completing by causing fail-overs to occur during the
        upgrade errand.
    -   Fix issue preventing a replica from being restored to service if
        more than one replica was down.
    -   Fix issue preventing PGMonitor from reporting all data items.
    -   Fix issue preventing updates to the monitoring database when a
        new instance is created and a failover occurs on the original
        instance.
    -   Fix issue causing timeout failure during service creation when
        running the monitoring installation on a replica.
    -   Fix issue causing the PGAudit extension to be installed even if
        the option was specified to be false.
-   [Support Update] - Crunchy PostgreSQL for PCF is supported in PCF
    v2.1
-   [New Configuration Option] - Bloat Check Schedule
    -   Allow the service user to set the schedule for the bloat check
        statistics.
-   [New Configuration Option] - Logging Parameters
    -   Allows the operator to configure the logging rotation size, age,
        and filename.
-   [Security Update] - Implementation of MD5 password authentication
    for the monitoring user.
-   [Security Update] - Implementation of dynamic PGBackRest SSH keys to
    allow for dynamic role changes.

**Note:** Crunchy PostgreSQL for PCF can be directly upgraded from v3 to
v4. Services using versions of Crunchy PostgreSQL for PCF prior to v3
should first upgrade to v3, then directly to v4 for maximum stability.

**Note:** Crunchy PostgreSQL for PCF v4 relies on functionality released
in PCF v1.11 through v2.1 to provide support for “on-demand broker”
capability. To implement this functionality, Crunchy Data has made
material modifications to the Crunchy PostgreSQL for PCF manifest and
does not currently provide a direct upgrade path from Crunchy PostgreSQL
for PCF v1 or v2 to Crunchy PostgreSQL for PCF v4. To avoid confusion
between the two implementations, Crunchy Data has removed the Crunchy
PostgreSQL for PCF v1 and v2 software versions from Pivotal Network. If
you previously downloaded and/or deployed an evaluation copy of Crunchy
PostgreSQL for PCF v1 or v2, Crunchy Data is happy to provide ongoing
commercial subscription support. If you are interested in commercial
subscription support for Crunchy PostgreSQL for PCF, contact
sales@crunchydata.com.

v03.090512.109
--------------

**Release Date:** March 6, 2018

Software features in this release:

-   PostgreSQL minor release: v9.5.12
    -   This minor release addresses CVE-2018-1058: Uncontrolled search
        path element in pg\_dump and other client applications

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090511.108
--------------

**Release Date:** February 15, 2018

Software features in this release:

-   PostgreSQL minor release: v9.5.11
-   Component update: pgBackrest (v1.25 to v1.28)
-   Component update: node\_exporter (v0.15.1 to v0.15.2)
-   Support for customizing bloat check schedule
-   Bugfix: Health check error may cause unnecessary failovers
-   Bugfix: Prometheus series errors from fast failover

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090510.107
--------------

**Release Date:** January 25, 2018

Software features in this release:

-   PostgreSQL minor release: v9.5.10
-   Added ‘Monitoring’ feature: using Grafana and Prometheus
-   Support for PostgreSQL Foreign Data Wrapper
-   Support for Redis Foreign Data Wrapper
-   Support for optionally enabling PL/R extensions
-   Support for additional PostgreSQL configuration options

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090510.106
--------------

**Release Date:** November 9, 2017

Software features in this release:

-   PostgreSQL minor release: v9.5.10
-   Component upgrade: PG common 184 to 187
-   Component upgrade: Backrest v1.20 to v1.25
-   Component upgrade: pgaudit\_analyze v1.0.6 to v1.0.7
-   Fixed bug with SSH known hosts causing backrest failures
-   Fixed bug where replicas reported failing to BOSH
-   Tuned failover code to fence failed primaries

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090509.105
--------------

**Release Date:** September 15, 2017

Software features in this release:

-   Upgrade ODB SDK from v0.14.1 to v0.17.0
-   Added service quotas for plans
-   Added configurable replica counts

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090509.104
--------------

**Release Date:** August 31, 2017

New tags included in this release:

-   PostgreSQL v9.5.9

Software fixes in this release:

-   Fixed bug where monit triggered PostgreSQL to restart
-   Fixed bug where failover did not repair fenced servers

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090508.103
--------------

**Release Date:** August 16, 2017

Security fixes included in this release:

-   Update stemcell to patch Ubuntu Linux kernel security vulnerability
    (USN-3385-2)

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090508.102
--------------

**Release Date:** August 14, 2017

Security fixes included in this release:

-   Security Vulnerability CVE-2017-7546: Empty password accepted in
    some authentication methods
-   Security Vulnerability CVE-2017-7547: The “pg\_user\_mappings”
    catalog view discloses passwords to users lacking server privileges
-   Security Vulnerability CVE-2017-7548: lo\_put() function ignores
    ACLs

New tags included in this release:

-   PostgreSQL v9.5.8
-   PostGIS v2.3.3
-   PgAudit v1.0.6
-   pgBackrest v1.20
-   PLR v8.3.0.17

Software fixes in this release:

-   Fixed slow deprovision bug
-   Added primary to replica pool as a fallback for read queries
-   Added hourly job to repair fenced services
-   Fixed bug where HAProxy would not disconnect users after failover
-   Lifted replica limitation
-   Improved healthchecks for PostgreSQL

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090507.101
--------------

**Release Date:** July 14, 2017

Security fixes included in this release:

-   Prevent the tile from logging manifests generated by the broker when
    deploying PostgreSQL, because they contain sensitive information.

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v03.090507.100
--------------

**Release Date:** July 1, 2017

**WARNING:** This is the GA release, and is not compatible with the
previous releases, including the beta release. See detailed release
notes below for more information.

Features included in this release (for details on features, see the
feature overview document):

-   High-availability model
-   Auto-failover
-   On-demand single-tenant service

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v02.090507.101
--------------

**Release Date:** May 23, 2017

**WARNING:** This is an updated beta release of the on-demand
single-tenant service. This beta release is intended for evaluation
purposes only. The final release of this software will include some
changes that will likely break compatibility between this release and
the next release.

Features included in this release (for details on features, see the
feature overview document):

-   On-demand single-tenant service

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v02.090507.100
--------------

**Release Date:** May 12, 2017

**WARNING:** This is a beta release of the on-demand single-tenant
service. This beta release is intended for evaluation purposes only. The
final release of this software will include some changes that will
likely break compatibility between this release and the next release.

Features included in this release:

-   On-demand single-tenant service

**Note:** Crunchy PostgreSQL for PCF v3 relies on functionality released
in PCF v1.9 – v2.0 to provide support for “on-demand broker”
capability.\
\
 To implement this functionality, Crunchy Data has made material
modifications to the Crunchy PostgreSQL for PCF manifest and does not
currently provide a direct upgrade path from Crunchy PostgreSQL for PCF
v1 or v2 to Crunchy PostgreSQL for PCF v3. \
\
 To avoid confusion between the two implementations, Crunchy Data has
removed the Crunchy PostgreSQL for PCF v1 and v2 software versions from
Pivotal Network. If you previously downloaded and deployed an evaluation
copy of Crunchy PostgreSQL for PCF v1 or v2, Crunchy Data is happy to
provide ongoing commercial subscription support.\
\
 If you are interested in commercial subscription support for Crunchy
PostgreSQL for PCF, please contact sales@crunchydata.com.

v1.0.8
------

**Release Date:** March 10, 2017

Features included in this release:

-   Crunchy PostgreSQL v9.5 and PostGIS v2.3
-   Leader/follower PostgreSQL architecture
-   Load-balanced read replicas
-   Load-balanced brokers
-   Automated backups with pgBackrest
-   Self-managed databases

v1.0.6
------

**Release Date:** January 13, 2017

Features included in this release:

-   Crunchy PostgreSQL v9.5 and PostGIS v2.2
-   Leader/follower PostgreSQL architecture
-   Load-balanced read replicas
-   Load-balanced brokers
-   Automated backups with pgBackrest
-   Self-managed databases
