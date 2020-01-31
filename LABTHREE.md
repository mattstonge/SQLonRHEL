A Set of Lab Exercises organized by Matt St. Onge, Jack Waterworth, & John Tietjen

# SQLonRHEL
Workshop for SQL on RHEL 8.x systems
Thanks in advance to Microsoft  for their excellent reference materials which made this possible
(https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-red-hat?view=sql-server-2017)

# TABLE OF CONTENTS
LAB ONE - Introducing MS SQL Server

  [Getting Started](https://github.com/mattstonge/SQLonRHEL/blob/master/README.md#prereqisites)

  [Install SQLServer](https://github.com/mattstonge/SQLonRHEL/blob/master/README.md#install-sqlserver)

  [Install the Commandline Tools](https://github.com/mattstonge/SQLonRHEL#install-the-commandline-tools)

  [Connect to SQLServer](https://github.com/mattstonge/SQLonRHEL#connect-to-your-new-sql-server)


LAB TWO  - Importing/Recovery of a Database

  [Exercises](https://github.com/mattstonge/SQLonRHEL/blob/master/LABTWO.md)

LAB THREE  - Security / Best Practices

  [EXERCISES](https://github.com/mattstonge/SQLonRHEL/blob/master/LABTHREE.md)
                                                                                                                    

Session Presentation Slides


# LAB THREE EXERCISES

---

## 1. Create a login and database user

---

Grant others access to SQL Server by creating a login in the master database using the CREATE LOGIN statement.


sqlcmd -S localhost -U SA -P r3dh4t1!

1> CREATE LOGIN student WITH PASSWORD = 'r3dh4t1!'; 

2>  CREATE LOGIN Jerry WITH PASSWORD = 'r3dh4t1!'; 

3>  GO

---

To connect to a user-database, a login needs a corresponding identity at the database level, called a database user. Users are specific to each database and must be separately created in each database to grant them access. 

---


1>  USE AdventureWorks2014; 

2>  GO

---

1>  CREATE USER student; 

2>  CREATE USER Jerry; 

3>  GO

1>  QUIT

---

You can authorize other logins to create more logins by granting them the ALTER ANY LOGIN permission. Inside a database, you can authorize other users to create more users by granting them the ALTER ANY USER permission.

---


sqlcmd -S localhost -U SA -P r3dh4t1!

1> GRANT ALTER ANY LOGIN TO student;

2>  GO

---

1>  USE AdventureWorks2014;  

2>  GO

---

1>  GRANT ALTER ANY USER TO Jerry;

2>  GO

3>  QUIT


---

Now the login student can create more logins, and the user Jerry can create more users.

---


---

## 2. Granting Access with Least Priveleges

---

The first people to connect to a user-database will be the administrator and database owner accounts. However these users have all the permissions available on the database. This is more permission than most users should have.
When you are just getting started, you can assign some general categories of permissions by using the built-in fixed database roles. For example, the db_datareader fixed database role can read all tables in the database, but make no changes. Grant membership in a fixed database role by using the ALTER ROLE statement. The following example will add the user Jerry to the db_datareader fixed database role.



sqlcmd -S localhost -U SA -P r3dh4t1!

1>  USE AdventureWorks2014;

2>  GO

1>  ALTER ROLE db_datareader ADD MEMBER Jerry;

2>  GO


Later, when you are ready to configure more precise access to your data (highly recommended), create your own user-defined database roles using CREATE ROLE statement. Then assign specific granular permissions to you custom roles.

For example, the following statements create a database role named Sales, grants the Sales group the ability to see, update, and delete rows from the Orders table, and then adds the user Jerry to the Sales role.


1>  CREATE ROLE Sales

2>  GRANT SELECT ON Production.WorkOrder TO Sales

3>  GRANT UPDATE ON Production.WorkOrder TO Sales

4>  GRANT DELETE ON Production.WorkOrder TO Sales

5>  ALTER ROLE Sales ADD MEMBER Jerry

6>  GO


