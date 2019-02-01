---
title: "Crunchy PostgreSQL for Pivotal Cloud Foundry"
draft: false
weight: 1
---

Crunchy PostgreSQL for Pivotal Cloud Foundry
==========================

Using the cloud-native platform Pivotal Cloud Foundry (PCF), Crunchy
Data engineers have created a service offering that provides a secure,
reliable, and robust database infrastructure delivered as
“Database-as-a-Service” (DBaaS). Crunchy Certified PostgreSQL for PCF
provides a Marketplace service and complete PostgreSQL infrastructure to
enable any applications deployed to PCF to utilize PostgreSQL databases
in a self-service, on-demand manner. This allows developers to focus on
coding and not the underlying infrastructure.

This documentation describes the Crunchy PostgreSQL for Pivotal Cloud
Foundry (PCF) tile. Installing this tile creates a service that allows
PCF developers to use Crunchy PostgreSQL databases with their apps. In
order to provide our customers with the best possible environment,
Crunchy Data engineers have created an infrastructure that provides the
following key features:

**WARNING:** Crunchy PostgreSQL for PCF cannot be directly upgraded from
v4 with PostgreSQL v9.5 to v5 with PostgreSQL v10. Instead, perform a
migration upgrade, rather than an in-place upgrade. For more information
regarding migrations, see
[Migration](https://docs.pivotal.io/partners/data-migration). If
continued support is needed for PostgreSQL v9.5, contact [Crunchy Data
for PCF](mailto:crunchypcf@crunchydata.com). For additional consulting
assistance to plan a data migration effort, contact us at
[info@crunchydata.com](mailto:info@crunchydata.com).

Key Features
------------

In order to provide our customers with the best possible environment,
Crunchy Data engineers have created an infrastructure that provides the
following key features:

-   On-demand, single-tenant service
-   Crunchy PostgreSQL 10.6 and PostGIS 2.4
-   Primary/replica PostgreSQL architecture
-   Load-balanced read replicas
-   Automated backups with PgBackRest
-   Disaster recovery
-   High availability
-   SCRAM-SHA-256 Authentication between internal components

**Note:** Apps Manager support is only available if you are using PCF
Ops Manager and Elastic Runtime v1.10 or later.

For details on the features of this tile, see the [feature
overview](/feature-overview).

For more information about Crunchy PostgreSQL, see the [Crunchy
Data](http://crunchydata.com/) website.

Service Introduction Video
--------------------------

[Watch PivNET Overview of the Crunchy PostgreSQL
Service](https://pivotal.io/platform/services-marketplace/data-management/crunchy)

Product Snapshot
----------------

**Note:** As of PCF v2.0, Elastic Runtime is renamed Pivotal Application
Service (PAS).

The following table provides version and version-support information
about Crunchy PostgreSQL for PCF:

Element

Details

Tile version

05.1006.003

Release date

December 6, 2018

Software component version

PostgreSQL v10.6

Compatible Ops Manager version(s)

v2.0.x, v2.1.x, v2.2.x, and v2.3.x

Compatible Pivotal Application Service version(s)

v2.0.x, v2.1.x, v2.2.x, and v2.3.x

IaaS support

AWS, Azure, GCP, OpenStack, and vSphere

Limitations
-----------

Crunchy PostgreSQL for PCF currently includes the following limitation:

-   You must manually restore backups.

Feedback
--------

To report bugs, request features, or ask questions, send an email to
[Crunchy Data](mailto:crunchypcf@crunchydata.com).
