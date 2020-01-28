# SQLonRHEL
Workshop for SQL on RHEL 8.x systems
Thanks in advance to Microsoft  for their excellent reference materials which made this possible
(https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-red-hat?view=sql-server-2017)

## Getting Started

### Prereqisites
- Access to RHPDS
- Initiate the Definitieve RHEL 8 Hands-on Lab (we will be piggy-backing on that deployment)
- View the inventory provided to you via email
We will be using those systems as follows:

Workstation - self explanatory

NODE1 - this is where we will install SQL first

NODE2 - this will be used when we do clustering

NODE3 - UNUSED 



To install Red Hat Enterprise Linux on your own machine, go to https://access.redhat.com/products/red-hat-enterprise-linux/evaluation. You can also create RHEL virtual machines in Azure. See Create and Manage Linux VMs with the Azure CLI, and use --image RHEL in the call to az vm create.

If you have previously installed a CTP or RC release of SQL Server, you must first remove the old repository before following these steps. For more information, see Configure Linux repositories for SQL Server 2017 and 2019.

For other system requirements, see System requirements for SQL Server on Linux.

## Install SQLServer

1. Download the Microsoft SQL Server 2017 Red Hat repository configuration file:

sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo

2. Run the following commands to install SQL Server:

sudo yum install -y mssql-server

3. After the package installation finishes, run mssql-conf setup and follow the prompts to set the SA password and choose your edition.

sudo /opt/mssql/bin/mssql-conf setup

4. Once the configuration is done, verify that the service is running:

systemctl status mssql-server

5. To allow remote connections, open the SQL Server port on the firewall on RHEL. The default SQL Server port is TCP 1433. If you are using FirewallD for your firewall, you can use the following commands:

sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent

sudo firewall-cmd --reload


## Install the Commandline Tools

To create a database, you need to connect with a tool that can run Transact-SQL statements on the SQL Server. The following steps install the SQL Server command-line tools: sqlcmd and bcp.

1. Download the Microsoft Red Hat repository configuration file.

sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo

2. If you had a previous version of mssql-tools installed, remove any older unixODBC packages.

sudo yum remove unixODBC-utf16 unixODBC-utf16-devel

3. Run the following commands to install mssql-tools with the unixODBC developer package.

sudo yum install -y mssql-tools unixODBC-devel

4. For convenience, add /opt/mssql-tools/bin/ to your PATH environment variable. This enables you to run the tools without specifying the full path. Run the following commands to modify the PATH for both login sessions and interactive/non-login sessions:

echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile

echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc

source ~/.bashrc


## Connect To Your New SQL Server

The following steps use sqlcmd to locally connect to your new SQL Server instance.

1. Run sqlcmd with parameters for your SQL Server name (-S), the user name (-U), and the password (-P). In this tutorial, you are connecting locally, so the server name is localhost. The user name is SA and the password is the one you provided for the SA account during setup.

sqlcmd -S localhost -U SA

You will be prompted to enter your password. Then you should get to a sqlcmd command prompt.  If you get a connection failure, first attempt to diagnose the problem from the error message. Then review the connection troubleshooting recommendations.





