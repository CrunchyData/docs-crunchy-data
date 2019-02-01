---
title: "Data Migration"
draft: false
weight: 7
---

About Migration
---------------

PostgreSQL minor releases never change the internal storage format and
are always compatible with earlier and later minor releases of the same
major version number. For example, v9.5.2 is compatible with v9.5,
v9.5.1, and v9.5.6. Upgrades between minor versions of PostgreSQL allow
for in-place upgrades because of this compatibility.

However, for major releases of PostgreSQL, the internal data storage
format is subject to change, thus complicating upgrades. The traditional
method for moving data to a new major version is to dump and reload the
database.

New major versions also typically introduce some user-visible
incompatibilities, so application programming changes may be required.
Cautious users test their client applications on the new version before
switching over fully. Therefore, Crunchy Data recommends setting up
concurrent installations of old and new versions.

Basic Migration Guidelines
--------------------------

To dump data from one major version of PostgreSQL and reload it in
another, you must use `pg_dump`. File system level backup methods will
not work. There are checks in place that prevent you from using a data
directory with an incompatible version of PostgreSQL, so no great harm
can be done by trying to start the wrong server version on a data
directory.

You can see the least downtime by installing the newer release on a
different instance, running both the old and new in parallel.

If there is network-peering and communication between the two, Crunchy
Data recommends you use the `pg_dump` and `pg_dumpall` programs from the
newer version of PostgreSQL to take advantage of enhancements that might
have been made in these programs.

Alternately, you can dump to an intermediate file from the old service,
and restore on the new service. You should ensure that the old database
is not updated after you begin to run `pg_dump` or `pg_dumpall`, or you
will lose those updates.

For more information, see [the Official PostgreSQL
documentation](https://www.postgresql.org/docs/9.0/static/migration.html).

### CF Apps-Based Tools

There are several CF apps developed that can assist with this type of
migration, such as:

-   cf-pgadmin4
-   cf-pgloader
