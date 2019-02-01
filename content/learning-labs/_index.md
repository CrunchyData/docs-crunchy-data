---
title: "Data Loading Tools Lab"
draft: false
weight: 8
---

Setting Up Your Environment
---------------------------

The following examples provide a general overview for setting up a
service and binding certain tools and applications to the service. These
topics are covered in-depth on the [Using Crunchy PostgreSQL for
PCF](/using-crunchy-postgresql-for-pcf) page.

**Note:** Some applications referenced in this document may only be
available to current Crunchy Data service subscribers, and accessed
through the Crunchy Customer Portal. For further information, contact
Crunchy Data Sales.

In order to start setting up the Crunchy PostgreSQL tools, some
environment details will need to be set up.

Initially, make sure you have access to whichever Pivotal Cloud Foundry
environment you will be working in. Whomever is your administrator or
supervisor should be able to provide you with the web URL and login
credentials.

This procedure requires the Cloud Foundry Command Line Interface (cf
CLI). To obtain the tool, do the following:

1.  Log in into your Pivotal Apps Manager.
2.  Click on Tools on the left hand side of the screen.
3.  On the screen, you will be provided a download link for the ‘cf cli’
    utility for your release.
4.  Download and follow the installation instructions to get it set up.
5.  If you experience any difficulty installing cf CLI, please use
    [reference guide](https://docs.cloudfoundry.org/cf-cli/cf-help.html)

Using the cf CLI, log into the CF instance you intend to use by running
`cf login -a API_URL -u USERNAME -p PASSWORD -o ORG -s SPACE`.

Setting Up a Crunchy PostgreSQL Database
----------------------------------------

To set up a Crunchy PostgreSQL Database

1.  Log in to Pivotal Apps Manager.
2.  Select the appropriate org on the left-hand side of the screen.
3.  Click **Marketplace** on the left-hand side of the screen.
4.  From the Marketplace, select the database cluster you wish to use.
5.  Set up the service.
    -   **Instance Name:** The name of your service instance. The name
        should be short but descriptive. The name should not contain any
        spaces.
    -   **Space:** The working space for the deployment of the
        application. If you have a production or development space
        allocated, this is where the space will be defined. It may be
        pre-defined depending on the access rights associated with your
        user account.
    -   **Bind to App:** For this deployment, the service is not bound
        to any application.

6.  Click **Show Advanced Options** to add the additional required
    fields.
7.  Create four additional fields as specified below:
    -   `db_username`: The username associated with the database.
    -   `db_name`: The name of the database.
    -   `owner_email`: The application owner’s email address.
    -   `owner_name`: The application owner’s full name.
    -   `postgis`: Set to `true` to include PostGIS extensions.

8.  Click **Add** once all fields have been completed.

Now that the database is set up and running on a service, you must set
up service keys. Service keys allow you to access the database with a
username and password.

To set up a service key, do the following:

1.  On the left-hand side of your screen, go to the org and spaces under
    which your service was created.
2.  On the right-hand side of the screen, under the list of services,
    you should see your newly created service. The second column
    contains the name you specified in previous steps.
3.  Go to the service.
4.  Go to **Overview \> Service Key Credentials** and click **Create
    Service Key**.
5.  Enter the name of the service key.
    -   Crunchy Data recommends naming the service key with the same
        name as your instance, followed by the suffix `_sk`. For
        example, if your instance name was `test_deployment`, then your
        first service key would be `test_deployment_sk`.

6.  Click **Create**. Your service key is created and listed under the
    service key credentials. Click your service key to see your service
    credentials.
7.  You will want to save the information on this page temporarily to
    set up the pgAdmin instance as well as the PostgreSQL Tools.

Crunchy pgAdmin4
----------------

**Note:** The version of CF-PGAdmin4 referenced in this document may
only be available to current Crunchy Data service subscribers and
accessed through the Crunchy Customer Portal. For further information,
contact Crunchy Data Sales.

To administer the database using the pgAdmin web interface, do the
following:

1.  Open up the `manifest.yml` file in an editor and change the
    following values:
    -   `name`: The name of the app deployment. It should be unique to
        the environment and not previously used.
    -   `memory`: 256 MB of memory should be sufficient to run this
        application.
    -   `disk_quota`: 1 GB of disk space should accommodate the setup.

2.  Save the file.
3.  Open the `config_local.py` file in an editor and change the
    following values:
    -   `PGADMIN_SETUP_EMAIL`: The email user account you use to log in
        to the pgAdmin interface. Crunchy Data recommends using the same
        email address you used in the database setup steps.
    -   `PGADMIN_SETUP_PASSWORD`: The password used to access the
        pgAdmin interface.

4.  Go the `crunchy-pgadmin` directory by running
    `cd ~/crunchy-pgadmin`.
5.  Push the app up to the Pivotal Cloud Foundry area by running
    `cf push`.

**Note:** During deployment of the app, its status shows as **Crashed**.
You must wait until your terminal returns to the start before it will
show as **Running**.

To set up the pgAdmin interface to communicate with the database, do the
following:

1.  In Apps Manager, on the left, select the org and spaces in which
    your app was deployed.
2.  On the right-hand side of the screen, you should now see your app
    name.
3.  Below the app name, you should see the word **Running** in green
    letters indicating that the app is running properly. If it is not,
    reach out to Crunchy Data for assistance.
4.  Click on the app.
5.  In the details of the app, go to the **Route** tab.
6.  Under the Route tab is a URL to your running pgAdmin interface. Save
    this URL.
7.  Open the URL in a separate browser window or tab.
8.  The login credentials are the email address and password you
    specified in `config_local.py` in the previous section.

To set up access to your previously created database, do the following:

1.  Right-click on **Servers** on the left and select **Create Server**
    from the dropdown.
2.  Under **General**, complete the following fields:
    -   **Name:** This name should match the name of the database
        instance created.
    -   \*\*Server Group: Select the Servers group.
    -   **Connect Now:** Leave this checkbox enabled.
    -   **Comments:** This is for details regarding the setup of this
        connection, such as brief summary of the database you are
        connecting to and its purpose.

3.  Under **Connection**, complete the following fields:
    -   `Host name/address`: Use the IP address provided by the service
        key.
    -   `Port`: Leave as `5432`.
    -   `Maintenance DB`: Use the `database` name provided by the
        service key.
    -   `User Name`: Use the `username` provided by the service key.
    -   `Password`: Use the `password` provided by the service key.
    -   `Save Password`: Enable this checkbox.
    -   `Role`: Leave this field blank.

4.  Click **Save**.

You should now be able to view your database and have the standard
PostgreSQL administrator tools at your disposal.
