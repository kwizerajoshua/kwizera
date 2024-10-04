# kwizera

Connecting to the Database

SQL> Enter user-name: system
Enter password:

Purpose: Connect to the Oracle Database using the system account, which is a common administrative user with broad privileges.
Setting the Session Container

SQL> ALTER SESSION SET CONTAINER=CDB$ROOT;
Purpose: Switch the session context to the root container of the multitenant architecture. This allows you to perform operations that affect the entire database.
Creating a Pluggable Database (PDB)


SQL> CREATE PLUGGABLE DATABASE plsql_class2024db
      ADMIN USER <username> IDENTIFIED BY <password>
      FILE_NAME_CONVERT=('C:\USERS\ORADATA\ORCL\PDBSEED\',
                         'C:\USERS\ORADATA\ORCL\plsql_class2024db\');
Purpose: Attempt to create a new PDB named plsql_class2024db. The ADMIN USER clause specifies the administrative user for this PDB and the FILE_NAME_CONVERT clause specifies the source and target directories for the PDB files.
Fixing Administrative User Name Error


SQL> CREATE PLUGGABLE DATABASE plsql_class2024db
      ADMIN USER kw_plsqlauca IDENTIFIED BY 123
      FILE_NAME_CONVERT=('C:\USERS\ORADATA\ORCL\PDBSEED\',
                         'C:\USERS\ORADATA\ORCL\plsql_class2024db\');
Purpose: Create the PDB plsql_class2024db again, now with a valid administrative user kw_plsqlauca and password 123. The PDB is created successfully this time.
Opening the Pluggable Database


SQL> ALTER PLUGGABLE DATABASE plsql_class2024db OPEN;
Purpose: Attempt to open the newly created PDB. The operation fails due to insufficient privileges.
Connecting as SYSDBA


SQL> CONNECT sys/password@localhost AS SYSDBA;
Purpose: Attempt to connect as the SYS user with SYSDBA privileges. The connection fails due to a missing service name.
Successful Connection


SQL> CONNECT sys/password@localhost:1521/plsql_class2024db AS SYSDBA;
Purpose: Successfully connect to the database as SYSDBA using the appropriate service name.
Opening the Pluggable Database Again


SQL> ALTER PLUGGABLE DATABASE plsql_class2024db OPEN;
Purpose: Open the plsql_class2024db PDB. The operation is successful this time.
Switching to the New PDB


SQL> ALTER SESSION SET CONTAINER=plsql_class2024db;
Purpose: Change the session context to the plsql_class2024db PDB, allowing you to execute commands within this specific PDB.
Creating Another PDB


SQL> CREATE PLUGGABLE DATABASE kw_to_delete_pdb
      ADMIN USER kwizera_db IDENTIFIED BY 123
      FILE_NAME_CONVERT=('C:\USERS\ORADATA\ORCL\PDBSEED\',
                         'C:\USERS\ORADATA\ORCL\plsql_class2024db\');
Purpose: Attempt to create a second PDB named kw_to_delete_pdb. This fails because you cannot create a PDB while connected to another PDB.
Switching Back to CDB$ROOT


SQL> ALTER SESSION SET CONTAINER = CDB$ROOT;
Purpose: Switch back to the root container to perform operations that affect the entire CDB.
Attempting to Create the PDB Again


SQL> CREATE PLUGGABLE DATABASE kw_to_delete_pdb
      ADMIN USER kwizera_db IDENTIFIED BY 123
      FILE_NAME_CONVERT=('C:\USERS\ORADATA\ORCL\PDBSEED\',
                         'C:\USERS\ORADATA\ORCL\plsql_class2024db\');
Purpose: Attempt to create kw_to_delete_pdb again while still in the root container. This fails because the file already exists.
Creating the PDB with Correct Path


SQL> CREATE PLUGGABLE DATABASE kw_to_delete_pdb
      ADMIN USER kwizera_db IDENTIFIED BY 123
      FILE_NAME_CONVERT=('C:\USERS\ORADATA\ORCL\PDBSEED\',
                         'C:\USERS\ORADATA\ORCL\kw_to_delete_pdb\');
Purpose: Create kw_to_delete_pdb successfully with a new file path.
Opening the Newly Created PDB


SQL> ALTER PLUGGABLE DATABASE kw_to_delete_pdb OPEN;
Purpose: Open the kw_to_delete_pdb PDB. This operation is successful.
Dropping the PDB


SQL> DROP PLUGGABLE DATABASE kw_to_delete_pdb INCLUDING DATAFILES;
Purpose: Attempt to drop kw_to_delete_pdb including its datafiles. This fails because the PDB is not closed.
Closing the PDB


SQL> ALTER PLUGGABLE DATABASE kw_to_delete_pdb CLOSE IMMEDIATE;
Purpose: Close the PDB immediately, allowing you to drop it.
Dropping the PDB Again


SQL> DROP PLUGGABLE DATABASE kw_to_delete_pdb INCLUDING DATAFILES;
Purpose: Successfully drop kw_to_delete_pdb, including its datafiles.
Configuring HTTP and HTTPS Ports
Checking Current HTTP and HTTPS Ports


SQL> SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
       DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT
FROM DUAL;
Purpose: Retrieve the current HTTP and HTTPS ports configured in the Oracle database.
Setting HTTP and HTTPS Ports


SQL> BEGIN
       DBMS_XDB_CONFIG.SETHTTPPORT(8080);  -- Set HTTP port
       DBMS_XDB_CONFIG.SETHTTPSPORT(8443); -- Set HTTPS port (optional)
    END;
/
Purpose: Set the HTTP port to 8080 and the HTTPS port to 8443. This is done through a PL/SQL block, allowing for batch execution of multiple statements.
Verifying Port Configuration Again



SQL> SELECT DBMS_XDB_CONFIG.GETHTTPPORT() AS HTTP_PORT,
       DBMS_XDB_CONFIG.GETHTTPSPORT() AS HTTPS_PORT
FROM DUAL;
Purpose: Confirm that the HTTP and HTTPS ports have been updated to the new values.



