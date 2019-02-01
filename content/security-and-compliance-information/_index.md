---
title: "Security and Compliance Information"
draft: false
weight: 5
---

Concepts
--------

The Pivotal Services Marketplace provides users with platform add-on
services to enhance, secure, and manage applications. The catalog
includes solutions from Pivotal, our Partners, and the Cloud Foundry
community providing a curated selection of capabilities from data
persistence, caching, messaging to continuous integration, security, and
much more.

Crunchy PostgreSQL for PCF brings the tools and expertise software
engineers need to make their open source database environments
successful and manageable. Developers can now easily deploy a
production-ready PostgreSQL cluster within a Pivotal Cloud Foundry (PCF)
environment through a PCF Service Tile.

Crunchy PostgreSQL for Cloud Foundry provides PostgreSQL necessary
infrastructure to support enterprise requirements including:

-   24/7 enterprise support
-   No forks: 100% open-source, native PostgreSQL
-   Predefined and Custom plans for any size system
-   On-Demand (Dedicated) or shared database clusters
-   Automated backups through pgBackrest
-   Load-balanced read-only replicas for scalability
-   Graphical database administration tools for simple point-and-click
    database management
-   PostGIS, robust open-source GIS functionality, included by default

Security and ATO:

-   Automated STIG Compliance Job
-   1400+ Automated STIG Compliance Checks
-   Integrated with STIG Viewer for expedited ATO
-   ICS 500-27 security events are extracted through pgAudit (over 100
    security events are captured)
-   Automated Keys Management

Encryption
----------

### FIPS 140-2 Compliance

Encryption can be used to secure data both in transit across a network
and when stored on physical media (“at rest”). PostgreSQL uses a system
library called OpenSSL to perform encryption. A version of the OpenSSL
library is available which is FIPS 140-2 certified.

**Note:** Accreditation of the OpenSSL library is the PaaS’s
responsibility. Crunchy Service inherits this functionality through the
use of the stemcell.

### pgCrypto

The pgCrypto module provides cryptographic functions for PostgreSQL. For
more information, see the official [pgCrypto
Documentation](https://www.postgresql.org/docs/10/static/pgcrypto.html).

### BOSH Data at Rest Encryption

**Note:** Integrating with IaaS encryption and keys management API is
the PaaS’s responsibility. Crunchy Service inherits this functionality
through the use of BOSH.

Crunchy Service uses BOSH Disk encryption properties to enable
encryption at rest for every database disk. Encryption allows operators
to use full disk encryption for all persistent disks on BOSH-deployed
VMs. You can use this feature to meet data-at-rest encryption
requirements or as a security best practice.

### Third-Party Encryption Tools

While use of the `pgcrypto` PostgreSQL extension may be sufficient for
specific data-at-rest encryption requirements, it is often desirable to
encrypt all of the metadata (the names of files, the names of
directories, the definition of tables, etc) associated with the
database. As this involves encryption data which exists outside of the
database (file names, directory names), whole-disk and/or filesystem
level encryption is recommended.

PostgreSQL is supported and works well with most whole-disk and
filesystem-level encryption solutions.

For further information, contact Crunchy Data Sales.

Common Criteria Certification
-----------------------------

Certified Crunchy PostgreSQL is the first open-source relational
database to be certified at Common Criteria EAL 2+.

For more information, see the [Crunchy Data press
release](https://www.postgresql.org/about/news/1685/).

DISA Security Technical Implementation
--------------------------------------

Security Technical Implementation Guides (STIGs) are the configuration
standards for United States Department of Defense (DoD) Information
Assurance (IA) and IA-enabled devices/systems published by the United
States Defense Information Systems Agency (DISA). Since 1998, DISA has
played a critical role enhancing the security posture of DoD’s security
systems by providing the STIGs. The STIGs contain technical guidance to
“lock down” information systems/software that might otherwise be
vulnerable to a malicious computer attack.

Importantly, compliance with the STIG guidance requires only open source
software and documentation. The PostgreSQL STIG is based on open source,
unmodified PostgreSQL v9.x used in conjunction with certain open source
PostgreSQL extensions – most notably, pgAudit.

For more information, see [Announcing the PostgreSQL
STIG](http://info.crunchydata.com/blog/postgres-stig-disa-security-guide).

### DISA PostgreSQL STIG Automation

Developed to reduce the time it takes to secure authority to operate
certification for cloud services, the technology leverages open-source
software to provide automated compliance testing. In this case, the
compliance testing and subsequent review and approval was accomplished
within 72 hours, which is a major reduction in effort compared to
previous attempts.

The PostgreSQL STIG Automation project utilizes the [InSpec
Project](https://www.inspec.io/), which provides an open source
compliance, security and policy testing framework, to dynamically
extract system configuration information. This information is checked
against strict security DoD guidelines crafted by industry-leading
PostgreSQL security experts. The PostgreSQL STIG Automation project also
electronically supplements the Body of Evidence required to verify NIST
800-53 and the government’s compliance requirements.

For more information, see [Crunchy Data Automates Security Compliance
with DISA PostgreSQL Security Technical Implementation
Guide](http://info.crunchydata.com/news/crunchy-data-automates-security-compliance-with-disa-security-technical-implementation-guide).

### Automation Process Overview

![](https://i.imgur.com/jOe9vQ8.png)

1.  DISA STIG is loaded and saved as `*.ckl` file.
2.  Tools are used to convert STIG Viewer’s `*.ckl` file into an InSpec
    tests collection.
3.  Each STIG test case is transferred into either a base profile or an
    overlay.
4.  Base profile and overlay tests are integrated into a BOSH job and
    integrated with a service job.
5.  Once the user creates an instance of the Crunchy Service, a STIG
    compliance job is automatically configured to scan that system.
6.  Full, basic, and ICS 500-27 reports are automatically generated and
    are available for the security team (for each service).
7.  The Platform Administrator can use scripts to convert this report
    into a `*.ckl` (STIG Viewer) file for analysis (see below). For more
    information, see [Learn more about PostgreSQL STIG InSpec Base
    Profile](https://github.com/CrunchyData/pgstigcheck-inspec).

### Using DISA STIG Viewer

Once a Compliance Report is generated, tools can be used to convert it
into a `*.ckl` (Checklist) format. At this point, a System Compliance
Assessor can use STIG Viewer to verify results of every automated test.
Results of each test are appended to the Finding Details field.

![](https://i.imgur.com/j8cbLRq.png)

ICS 500-27 Audit with Crunchy pgAudit
-------------------------------------

Crunchy’s pgAudit configuration enhances PostgreSQL’s logging abilities
by allowing administrators to audit specific classes of operations or
choosing specific objects to monitor and provides detailed session
and/or object audit logging.

Session auditing allows administrators to choose classes of statements
to log:

-   `READ`: `SELECT` and `COPY` when the source is a relation or a query
-   `WRITE`: `INSERT`, `UPDATE`, `DELETE`, `TRUNCATE`, and `COPY` when
    the destination is a relation
-   `FUNCTION`: Functions and `DO` blocks
-   `ROLE`: `GRANT`, `REVOKE`, `CREATE/ALTER/DROP ROLE`
-   `DDL`: All `DDL` not included in `ROLE`
-   `MISC`: `DISCARD`, `FETCH`, `CHECKPOINT`, `VACUUM`

For example:

```
2017-07-14 13:42:00.976 UTC psql crunchy stig_test_db 5968ca28.230f 2017-07-14 13:42:00 UTC localhost(41390) 8975 >LOG:  AUDIT: SESSION,1,1,ROLE,CREATE ROLE,,,CREATE ROLE bob; SET ROLE bob; CREATE TABLE stig_test_schema.test_table(id INT);,<none>
AUDIT: SESSION,1,1,ROLE,CREATE ROLE,,,CREATE ROLE bob; SET ROLE bob; CREATE TABLE stig_test_schema.test_table(id INT);,<none>
```

### ICS 500-27 Controls Inheritance

For more information, see [View All Inherited ICS 500-27
Controls](/ics500_27_controls).

### ICS 500-27 Audit Statement Anatomy

```
AUDIT: SESSION,1,1,ROLE,CREATE ROLE,,,CREATE ROLE bob; SET ROLE bob; CREATE TABLE stig_test_schema.test_table(id INT);,<none>
```

-   `AUDIT_TYPE` - `SESSION` or `OBJECT`.
-   `STATEMENT_ID` - Unique statement ID for this session. Each
    statement ID represents a backend call. Statement IDs are sequential
    even if some statements are not logged. There may be multiple
    entries for a statement ID when more than one relation is logged.
-   `SUBSTATEMENT_ID` - Sequential ID for each sub-statement within the
    main statement. For example, calling a function from a query.
    Sub-statement IDs are continuous even if some sub-statements are not
    logged. There may be multiple entries for a sub-statement ID when
    more than one relation is logged.
-   `CLASS` - e.g. `READ`, `ROLE` (see `pgaudit.log`).
-   `COMMAND` - e.g. `ALTER TABLE`,
    `SELECT.OBJECT_TYPE - TABLE, INDEX, VIEW`, etc. Available for
    `SELECT`, `DML` and most `DDL` statements.
-   `OBJECT_NAME` - The fully-qualified object name (e.g.
    `public.account`). Available for `SELECT`, `DML` and most `DDL`
    statements.
-   `STATEMENT` - Statement executed on the backend.
-   `PARAMETER` - If `pgaudit.log_parameter` is set, this field will
    contain the statement parameters as quoted CSV.

### Auditable Event Prefix Anatomy

```
2017-07-14 13:42:00.976 UTC psql crunchy stig_test_db 5968ca28.230f 2017-07-14 13:42:00 UTC localhost(41390) 8975 >LOG:  AUDIT: SESSION,1,1,ROLE,CREATE ROLE,,,CREATE ROLE bob; SET ROLE bob; CREATE TABLE stig_test_schema.test_table(id INT);,<none>
2017-07-14 13:42:00.976 UTC psql crunchy stig_test_db 5968ca28.230f 2017-07-14 13:42:00 UTC localhost(41390) 8975 >
```

```
%m %a %u %d %p %c %s %r

 %m Time stamp with milliseconds
 %u User name
 %d Database name
 %p Process ID
 %s Process start time stamp
 %r Remote host name or IP address, and remote port
```

### Compliance Profiles

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

Protocols
---------

### PaaS Protocols

All traffic from the public internet to the Pivotal Cloud Foundry (PaaS)
happens over HTTPS. Inside the boundary of the system, components
communicate over a publish-subscribe (pub-sub) message bus NATS, HTTP,
and SSL/TLS.

The Cloud Controller authenticates every request with the Service Broker
API using HTTP or HTTPS, depending on which protocol that you specify
during broker registration. The Cloud Controller rejects any broker
registration that does not contain a username and password.

### PostgreSQL Internal Protocols

PostgreSQL uses a
[`message-based protocol`](https://www.postgresql.org/docs/9.5/static/protocol.html)
for communication between front-ends and back-ends (clients and
servers).

The protocol is supported over TCP/IP and also over Unix-domain sockets.
Port number 5432 has been registered with IANA as the customary TCP port
number for servers supporting this protocol, but in practice any
non-privileged port number can be used.

### PostgreSQL Replication Protocols

This is an internal component with peer-to-peer secured communication
within an established System Boundary. User Accounts do not have any
access to this component.

To initiate streaming replication, the front-end sends the replication
parameter in the startup message. A Boolean value of true tells the
back-end to go into walsender mode, wherein a small set of replication
commands can be issued instead of SQL statements. Only the simple query
protocol can be used in walsender mode. Replication commands are logged
in the server log when `log_replication_commands` is enabled. Passing
database as the value instructs walsender to connect to the database
specified in the dbname parameter, which will allow the connection to be
used for logical replication from that database.

[Streaming Replication
Protocol](https://www.postgresql.org/docs/9.5/static/protocol-replication.html)

### Crunchy Cluster Manager Protocols

This is an internal component with peer-to-peer secured communication
within an established System Boundary. User Accounts do not have any
access to this component.

Crunchy Cluster Manager Consul uses a
[`gossip`](https://www.consul.io/docs/internals/gossip.html) protocol to
manage membership and broadcast messages to the cluster. The security
mechanisms of Consul have a common goal: to provide confidentiality,
integrity, and authentication. The gossip protocol is powered by Serf,
which uses a symmetric key, or shared secret, cryptosystem.

### Crunchy Backup Manager Protocols

This is an internal component with peer-to-peer secured communication
within an established System Boundary. User Accounts do not have any
access to this component.

Instead of relying on traditional backup tools like tar and rsync,
pgBackRest implements all backup features internally and uses a custom
protocol for communicating with remote systems. Removing reliance on tar
and rsync allows for better solutions to database-specific backup
challenges. The custom remote protocol allows for more flexibility and
limits the types of connections that are required to perform a backup
which increases security.

pgBackRest uses trusted SSH to enable communication between the hosts.

Managing User Access with Role-Based Access Control
---------------------------------------------------

### PostgreSQL Role-Based Access Control

Client authentication in the Crunchy Service Tile is controlled by a
configuration file, which traditionally is named `pg_hba.conf` and is
stored in the database cluster’s data directory. HBA stands for
host-based authentication.

Each record specifies a connection type, a client IP address range (if
relevant for the connection type), a database name, a user name, and the
authentication method to be used for connections matching these
parameters. The first record with a matching connection type, client
address, requested database, and user name is used to perform
authentication. There is no “fall-through” or “backup”: if one record is
chosen and the authentication fails, subsequent records are not
considered. If no record matches, access is denied.

Example `pg_hba.conf` Entries:

```
# Allow any user on the local system to connect to any database with
# any database user name using Unix-domain sockets (the default for local
# connections).
#
# TYPE  DATABASE        USER            ADDRESS                 METHOD
local   all             all                                     trust
```

SSH Access
----------

Only Platform Administrators with BOSH Admin access can ssh into the
database system. There is no direct connect mechanism available for any
User Accounts to any system component.

Accounts
--------

### System Accounts

Authentication with a DoD-approved PKI certificate does not imply
authorization to access the Database System. PostgreSQL manages database
access permissions using System Accounts only (Database ROLES).

Service instances bound to an app contain credential data. Users specify
the binding credentials for user-provided service instances, while
third-party brokers specify the binding credentials for managed service
instances. The
[`VCAP_SERVICES`](/using-crunchy-postgresql-for-pcf#service-credentials) environment
variable contains credential information for any service bound to an
app. Cloud Foundry constructs this value from encrypted data that it
stores in the Cloud Controller Database (CCDB).

PCF Apps hosted within the PaaS System Boundary and Legacy Applications
running within IaaS boundaries can ONLY access Crunchy service through a
load balancer. [`Bindings`](/using-crunchy-postgresql-for-pcf#binding-key) and
[`Service keys`](/using-crunchy-postgresql-for-pcf#binding-key) are used to
dynamically generate system accounts for the database.

### Admin System Accounts

  Account                 Description
  ----------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------
  VCAP                    Automated PCF Superuser account. Only PCF Platform Administrators (not DBA nor DO nor IO) have local login abilities to access this system account.
  CRUNCHY                 Automated Superuser Database Admin Account. Used by the Crunchy Broker and is NOT exposed to anyone (DBA or DO or IO)
  REPLICATION / HAPROXY   Automated Accounts used to configure replication and load balancer. NOT exposed to anyone (DBA or DO or IO)

### Other User Accounts

**IMPORTANT!** Crunchy PostgreSQL Service Tile does not utilize General,
Privileged, Group, Temporary, or Emergency accounts. Only System
Accounts are used within the system.
