Tutorial: Design an Azure Database for PostgreSQL using the Azure portal
03/20/2018
6 minutes to read
Contributors
Rachel Agyemang  Andrea Lam  Jason H  Alma Jenks  jan-eng all
Azure Database for PostgreSQL is a managed service that enables you to run, manage, and scale highly available PostgreSQL databases in the cloud. Using the Azure portal, you can easily manage your server and design a database.

In this tutorial, you use the Azure portal to learn how to:

Create an Azure Database for PostgreSQL server
Configure the server firewall
Use psql utility to create a database
Load sample data
Query data
Update data
Restore data
Prerequisites
If you don't have an Azure subscription, create a free account before you begin.

Log in to the Azure portal
Log in to the Azure portal.

Create an Azure Database for PostgreSQL
An Azure Database for PostgreSQL server is created with a defined set of compute and storage resources. The server is created within an Azure resource group.

Follow these steps to create an Azure Database for PostgreSQL server:

Click Create a resource in the upper left-hand corner of the Azure portal.
Select Databases from the New page, and select Azure Database for PostgreSQL from the Databases page. Azure Database for PostgreSQL - Create the database

Fill out the new server details form with the following information:

Create a server

Server name: mydemoserver (name of a server maps to DNS name and is thus required to be globally unique)
Subscription: If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.
Resource group: myresourcegroup
Server admin login and password of your choice
Location
PostgreSQL Version
 Important

The server admin login and password that you specify here are required to log in to the server and its databases later in this tutorial. Remember or record this information for later use.

Click Pricing tier to specify the pricing tier for your new server. For this tutorial, select General Purpose, Gen 4 compute generation, 2 vCores, 5 GB of storage and 7 days backup retention period. Select the Geographically Redundant backup redundancy option to have your server's automatic backups stored in geo-redundant storage. Azure Database for PostgreSQL - pick the pricing tier

Click Ok.

Click Create to provision the server. Provisioning takes a few minutes.

On the toolbar, click Notifications to monitor the deployment process. Azure Database for PostgreSQL - See notifications

 Tip

Check the Pin to dashboard option to allow easy tracking of your deployments.

By default, postgres database gets created under your server. The postgres database is a default database meant for use by users, utilities, and third-party applications.

Configure a server-level firewall rule
The Azure Database for PostgreSQL service uses a firewall at the server-level. By default, this firewall prevents all external applications and tools from connecting to the server and any databases on the server unless a firewall rule is created to open the firewall for a specific IP address range.

After the deployment completes, click All Resources from the left-hand menu and type in the name mydemoserver to search for your newly created server. Click the server name listed in the search result. The Overview page for your server opens and provides options for further configuration.

Azure Database for PostgreSQL - Search for server 

In the server page, select Connection security.

Click in the text box under Rule Name, and add a new firewall rule to whitelist the IP range for connectivity. Enter your IP range. Click Save.

Azure Database for PostgreSQL - Create Firewall Rule

Click Save and then click the X to close the Connections security page.

 Note

Azure PostgreSQL server communicates over port 5432. If you are trying to connect from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall. If so, you cannot connect to your Azure SQL Database server unless your IT department opens port 5432.

Get the connection information
When you created the Azure Database for PostgreSQL server, the default postgres database was also created. To connect to your database server, you need to provide host information and access credentials.

From the left-hand menu in the Azure portal, click All resources and search for the server you just created.

Azure Database for PostgreSQL - Search for server 

Click the server name mydemoserver.

Select the server's Overview page. Make a note of the Server name and Server admin login name.

Azure Database for PostgreSQL - Server Admin Login

Connect to PostgreSQL database using psql in Cloud Shell
Let's now use the psql command-line utility to connect to the Azure Database for PostgreSQL server.

Launch the Azure Cloud Shell via the terminal icon on the top navigation pane.

Azure Database for PostgreSQL - Azure Cloud Shell terminal icon

The Azure Cloud Shell opens in your browser, enabling you to type bash commands.

Azure Database for PostgreSQL - Azure Shell Bash Prompt

At the Cloud Shell prompt, connect to your Azure Database for PostgreSQL server using the psql commands. The following format is used to connect to an Azure Database for PostgreSQL server with the psql utility:

bash

Copy
psql --host=<myserver> --port=<port> --username=<server admin login> --dbname=<database name>
For example, the following command connects to the default database called postgres on your PostgreSQL server mydemoserver.postgres.database.azure.com using access credentials. Enter your server admin password when prompted.

bash

Copy
psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
Create a new database
Once you're connected to the server, create a blank database at the prompt.

bash

Copy
CREATE DATABASE mypgsqldb;
At the prompt, execute the following command to switch connection to the newly created database mypgsqldb.

bash

Copy
\c mypgsqldb
Create tables in the database
Now that you know how to connect to the Azure Database for PostgreSQL, you can complete some basic tasks:

First, create a table and load it with some data. Let's create a table that tracks inventory information using this SQL code:

SQL

Copy
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
You can see the newly created table in the list of tables now by typing:

SQL

Copy
\dt
Load data into the tables
Now that you have a table, insert some data into it. At the open command prompt window, run the following query to insert some rows of data.

SQL

Copy
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
You have now two rows of sample data into the inventory table you created earlier.

Query and update the data in the tables
Execute the following query to retrieve information from the inventory database table.

SQL

Copy
SELECT * FROM inventory;
You can also update the data in the table.

SQL

Copy
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
You can see the updated values when you retrieve the data.

SQL

Copy
SELECT * FROM inventory;
Restore data to a previous point in time
Imagine you have accidentally deleted this table. This situation is something you cannot easily recover from. Azure Database for PostgreSQL allows you to go back to any point-in-time for which your server has backups (determined by the backup retention period you configured) and restore this point-in-time to a new server. You can use this new server to recover your deleted data. The following steps restore the mydemoserver server to a point before the inventory table was added.

On the Azure Database for PostgreSQL Overview page for your server, click Restore on the toolbar. The Restore page opens.

Azure portal - Restore form options

Fill out the Restore form with the required information:

Azure portal - Restore form options

Restore point: Select a point-in-time that occurs before the server was changed
Target server: Provide a new server name you want to restore to
Location: You cannot select the region, by default it is same as the source server
Pricing tier: You cannot change this value when restoring a server. It is same as the source server.
Click OK to restore the server to a point-in-time before the table was deleted. Restoring a server to a different point in time creates a duplicate new server as the original server as of the point in time you specify, provided that it is within the retention period for your pricing tier.
Next steps
In this tutorial, you learned how to use the Azure portal and other utilities to:

Create an Azure Database for PostgreSQL server
Configure the server firewall
Use psql utility to create a database
Load sample data
Query data
Update data
Restore data
