---
title: "Installation and Configuration"
draft: false
weight: 3
---

This topic describes how to install and configure Crunchy PostgreSQL for
Pivotal Cloud Foundry (PCF) Tile. If performing an upgrade of the Tile,
please refer to the Upgrading section before proceeding.

**Warning:** If upgrading the Tile, an Upgrade Token must be obtained
from Crunchy Data prior to beginning an upgrade action. The upgrade
process will not be able to complete without an Upgrade Token. Failure
to obtain an Upgrade Token prior to the upgrade process will result in
broker errors.

Import the Tile to Ops Manager
------------------------------

1.  Download the Crunchy PostgreSQL for Pivotal Cloud Foundry (PCF) Tile
    file from [Pivotal
    Network](https://network.pivotal.io/products/crunchy-postgresql/).

2.  Navigate to the Ops Manager Installation Dashboard and click
    **Import a Product** to upload the product file.

3.  Under the **Import a Product** button, click **+** next to the
    version number of Crunchy PostgreSQL for PCF. This adds the tile to
    your staging area.

Configuring the Service
-----------------------

Click the newly added **Crunchy PostgreSQL** tile on the Ops Manager
Installation Dashboard to open the configuration panes.

![Crunchy PostgreSQL tile](/images/tile.png)

If necessary, import a stemcell as required by the service.

Configure each section as described below.

### Configure AZ and Network Assignments

To make Crunchy PostgreSQL highly available, you must balance the
service across multiple availability zones (AZs).

![Assign AZs and Networks pane](/images/assign-azs.png)

1.  Click **Assign AZs and Networks**.

2.  Choose an AZ to deploy singleton jobs. This is a private service
    network with a route to a public gateway.

3.  Choose AZ to balance jobs. This should be a private service network
    with a route to a public gateway. Crunchy Data recommends two or
    more AZs when possible.

4.  Choose the primary network where the On-Demand Broker will be
    deployed.

5.  Choose the service network that On-Demand Broker will use to deploy
    VMs. This is a private service network with a route to a public
    gateway.

6.  Click **Save**.

### Configure Global Properties

![Global Properties](/images/global-properties.png)

1.  Click **Global Properties**.

2.  Configure a username and passphrase for a UAA Client.

    -   Requires an UAA account that has client login and either BOSH
        read or BOSH admin access.
    -   If the account has BOSH client admin login, select the check box
        labeled `Account Has Admin`.
    -   Example with BOSH read:
        `<ops manager url>/api/v0/deployed/director/credentials/uaa_login_client_credentials`

3.  Configured the maximum number of upgrades that can be processed at a
    given time.

4.  Choose an AZ in which to deploy these services.

5.  Configure the availability of the Standalone PostgreSQL plan for
    organizations.

6.  Configure the number of Standalone PostgreSQL instances permitted in
    the environment.

7.  Configure the availability of the Replica PostgreSQL plan for
    organizations.

8.  Configure the number of Replica PostgreSQL instances permitted in
    the environment.

9.  Configure the availability of the General PostgreSQL plan for
    organizations.

10. Configure the number of General PostgreSQL instances permitted in
    the environment.

11. Configure the availability of the General-Monitored PostgreSQL plan
    for organizations.

12. Configure the number of General-Monitored PosgreSQL instances
    permitted in the environment.

13. Configure the availability of the Custom PostgreSQL plan for
    organizations.

14. Configure the number of Custom PostgreSQL instances permitted in the
    environment.

15. Configure the availability of the Custom-Monitored PostgreSQL plan
    for organizations.

16. Configure the number of Custom-Monitored PosgreSQL instances
    permitted in the environment.

17. Click **Save**.

### Configure Standalone Properties

The standalone plans made available to developers must be configured
prior to deployment.

![Standalone Properties](/images/standalone-properties.png)

1.  Click **Standalone Properties**.

2.  Select a default size for the standalone instances.

3.  Select a default disk size for the standalone instances.

4.  Optionally enable backups by default for all standalone instances.

5.  Select a default maximum connections per standalone instance.

**Note:** Do not set any PostgreSQL VMs to less than 4 GB of memory.

**Note:** Do not set any PostgreSQL VMs to fewer than 2 cores.

### Configure Cluster Properties

The standard plans made available to developers must be configured prior
to deployment.

![Cluster Properties](/images/cluster-properties.png)

1.  Select the default system specification for General plans.

2.  Select the default disk size for the General plans.

3.  Select the default number of PostgreSQL instances in a General plan.

4.  Select the default maximum number of connections to the PostgreSQL
    instances.

5.  Select the system specifications and disk types for the Consul,
    HAProxy, and errand Virtual Machines.

**Note:** Do not set any plan which uses PostgreSQL or pgBackrest VMs to
less than 4 GB of memory.

**Note:** Do not set any plan which uses PostgreSQL or pgBackrest VMs to
fewer than 2 cores.

### Configure PostgreSQL Properties

The standard plans made available to developers must be configured prior
to deployment.

![PostgreSQL Properties](/images/postgresql-properties.png)

1.  Select the Log File Name Pattern by which PostgreSQL will rotate the
    logs.

2.  Select the frequency with which PostgreSQL will rotate the logs.

3.  Select the size by which PostgreSQL will rotate the logs.

4.  Either enable or disable the option to allow developers to use MD5
    authentication when binding applications to the service instances.

5.  Select the PostgreSQL configuration parameters developers will be
    allowed to set while using a Crunchy PostgreSQL service instance.
    ![Configuration Parameters](/images/postgresql-configurations.png)

6.  Select the PostgreSQL extensions developers will be allowed to
    create while using a Crunchy PostgreSQL service instance.
    ![Extensions](/images/postgresql-extensions.png)

**Note:** Do not set any plan which uses PostgreSQL or pgBackrest VMs to
less than 4 GB of memory.

**Note:** Do not set any plan which uses PostgreSQL or pgBackrest VMs to
fewer than 2 cores.

### Configure Errands

1.  Click **Errands**.

2.  Configure the errands that should be run after deployment of the
    service. Crunchy Data recommends the defaults.

3.  Click **Save**.

### Configure Resources

1.  Click **Resource Config**.

2.  Review the pre-populated recommended server sizes and make any
    necessary changes. Crunchy Data recommends the defaults.

3.  Click **Save**.

Install the Tile
----------------

1.  Return to the Ops Manager Installation Dashboard.

2.  Click **Apply Changes** to install the Crunchy PostgreSQL for PCF
    tile.

Upgrading the Tile
------------------

![Upgrade Token](/images/upgrade-token.png)

1.  Obtain the Upgrade Code from Crunchy Data’s Customer Access Portal.
    -   Log in to the Customer Access Portal and select PCF Tile Upgrade
        Token
    -   Select the version to which you wish to upgrade
    -   Select the Generate Token button to receive the Upgrade Token
        ![Token Generator](/images/token-generator.png)

2.  Enter the Upgrade Code, exactly as provided, into the associated
    field on the Upgrade Token page of the Tile Installer
3.  Continue the steps as necessary from the Errands
    section.
