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

##1. Create a login and database user

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

