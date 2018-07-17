!SLIDE center subsection blue

# ACM and Refreshes

!SLIDE bullets incremental

# ACM and Refreshes

* Will the ACM replace my refresh SQL scripts? 
* Not completely
* ACM can't handle any DB-level refresh steps
* ACM can perform any PeopleTools or App refresh scripts

~~~SECTION:notes~~~
* ACM can't do db tasks like dropping/restoring databases
* We also leave the SYSADM password change, encryption in our SQL refresh scripts. This prevents us from having to run psae under two different passwords during the refresh.

But, once the database is online, you can use the ACM to perform your refresh steps.
~~~ENDSECTION~~~

!SLIDE bullets 

# Refresh Templates

* Organize your templates
   1. DBNAME_REFRESH
   1. DBNAME_CONFIG_PRE
   1. DBNAME_CONFIG_POST
* Remember to separate Pre/Post Boot activities

!SLIDE bullets

# ACM on the Command Line

* Call `psae` with `PTEM_CONFIG` as the Program ID
* Use `psrunACM.bat` under `PS_HOME\utilities`
* `$SERVER ORACLE $DATABASE $USER $PASS $TEMPLATE $OPTION`
* OPTION is:
  1. EXEC - run the ACM Template
  1. IMP - Import Template
  1. EXP - Export Template

!SLIDE center subsection grey

# Demo

~~~SECTION:notes~~~
Create the _REFRESH template (clean up Search Deployment)
Run the `c:\vagrant\856-psadmin-delta-scripts\acm\refresh_db.sql` script to show a sample refresh processing for Search
~~~ENDSECTION~~~

