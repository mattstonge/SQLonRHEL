# SQLonRHEL
Workshop for SQL on RHEL 8.x systems
Thanks in advance to Microsoft  for their excellent reference materials which made this possible
(https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-red-hat?view=sql-server-2017)

## Getting Started

### Prereqisites
You must have a RHEL 7.3, 7.4, 7.5, or 7.6 machine with at least 2 GB of memory.(We'll be using RHEL 8.x)

To install Red Hat Enterprise Linux on your own machine, go to https://access.redhat.com/products/red-hat-enterprise-linux/evaluation. You can also create RHEL virtual machines in Azure. See Create and Manage Linux VMs with the Azure CLI, and use --image RHEL in the call to az vm create.

If you have previously installed a CTP or RC release of SQL Server, you must first remove the old repository before following these steps. For more information, see Configure Linux repositories for SQL Server 2017 and 2019.

For other system requirements, see System requirements for SQL Server on Linux.

## Install SQlServer

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
