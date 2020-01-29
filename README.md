[![MattTheITGuru](https://avatars0.githubusercontent.com/u/22283700?s=100)](https://MattTheITGuru.com)
A Set of Lab Exercises organized by Matt St. Onge & John Tietjen

# SQLonRHEL
Workshop for SQL on RHEL 8.x systems
Thanks in advance to Microsoft  for their excellent reference materials which made this possible
(https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-red-hat?view=sql-server-2017)

# TABLE OF CONTENTS
LAB ONE

  [Getting Started](https://github.com/mattstonge/SQLonRHEL/blob/master/README.md#prereqisites)

  [Install SQLServer](https://github.com/mattstonge/SQLonRHEL/blob/master/README.md#install-sqlserver)
     
  [Install the Commandline Tools](https://github.com/mattstonge/SQLonRHEL#install-the-commandline-tools)
  
  [Connect to SQLServer](https://github.com/mattstonge/SQLonRHEL#connect-to-your-new-sql-server)
  
  
     
     



## Getting Started

### Prereqisites
- Access to [RHPDS](https://rhpds.redhat.com)

[![screenshot](https://github.com/mattstonge/SQLonRHEL/blob/master/images/rhpds_login.png)

- Initiate the Definitive RHEL 8 Hands-on Lab (we will be piggy-backing on that deployment)

[![screenshot](https://github.com/mattstonge/SQLonRHEL/blob/master/images/def-rhel-8-lab-order.png)

- View the inventory provided to you via email (don't loose this - You're going to need it!)


We will be using those systems as follows:

Workstation - self explanatory

NODE1 - this is where we will install SQL first

NODE2 - this will be used when we do clustering

NODE3 - UNUSED 


### Please Note
IF YOU ARE ATTENDING A HOSTED WORKSHOP - the RHPDS provisioning has been done for you... Please make note of your systems and login credentials and move on to the Install section...

However, if you have no access to RHPDS or the workshop environment, you may install Red Hat Enterprise Linux on your own machine, go to https://access.redhat.com/products/red-hat-enterprise-linux/evaluation. You can also create RHEL virtual machines in Azure. See Create and Manage Linux VMs with the Azure CLI, and use --image RHEL in the call to az vm create.

### Accessing the Lab Environment
Review your environment information.
You will need to SSH from your laptop to the WORKSTATION then to NODE1 so we can begin work. (you'll be promted for the password)


---

ssh student@"<workstation_External_Hostname>" 

ssh student@node1
  
---






## Install SQLServer (on Node1)

1. On NODE1 - Download the Microsoft SQL Server 2017 Red Hat repository configuration file:

---

sudo yum -y install python2

sudo yum -y install compat-openssl10

sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo

sudo yum download mssql-server

---

2. Run the following commands to install SQL Server:

---

sudo rpm -Uvh --nodeps mssql-server*rpm

sudo /opt/mssql/bin/mssql-conf setup

     When prompted:
     * Select 2 - Developer Edition
     * Accept (YES) the license agreements
     * DB password = r3dh4t1! 


---

3. After the package installation finishes, run mssql-conf setup and follow the prompts to set the SA password and choose your edition.

---

sudo /opt/mssql/bin/mssql-conf setup

---

4. Once the configuration is done, verify that the service is running:

---

systemctl status mssql-server

---


5. To allow remote connections, open the SQL Server port on the firewall on RHEL. The default SQL Server port is TCP 1433. If you are using FirewallD for your firewall, you can use the following commands:

---
sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent


sudo firewall-cmd --reload

---

## Install the Commandline Tools (on NODE1)

To create a database, you need to connect with a tool that can run Transact-SQL statements on the SQL Server. The following steps install the SQL Server command-line tools: sqlcmd and bcp.

1. Download the Microsoft Red Hat repository configuration file.

---

sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo

---

2. If you had a previous version of mssql-tools installed, remove any older unixODBC packages.

---

sudo yum remove unixODBC-utf16 unixODBC-utf16-devel

---

3. Run the following commands to install mssql-tools with the unixODBC developer package.

---

sudo yum install -y mssql-tools unixODBC-devel

---

4. For convenience, add /opt/mssql-tools/bin/ to your PATH environment variable. This enables you to run the tools without specifying the full path. Run the following commands to modify the PATH for both login sessions and interactive/non-login sessions:

---

echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile

echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc

source ~/.bashrc

---

## Connect To Your New SQL Server (on NODE1)

The following steps use sqlcmd to locally connect to your new SQL Server instance.

1. Run sqlcmd with parameters for your SQL Server name (-S), the user name (-U), and the password (-P). In this tutorial, you are connecting locally, so the server name is localhost. The user name is SA and the password is the one you provided for the SA account during setup.

---

sqlcmd -S localhost -U SA

---

You will be prompted to enter your password. Then you should get to a sqlcmd command prompt.  If you get a connection failure, first attempt to diagnose the problem from the error message. Then review the connection troubleshooting recommendations.





