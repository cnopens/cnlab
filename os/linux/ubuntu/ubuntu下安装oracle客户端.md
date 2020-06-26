# ubuntu下安装oracle客户端.md
---

Install RPMs

    Download the Oracle Instantclient RPM files from http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html. Everyone needs either "Basic" or "Basic lite", and most users will want "SQL*Plus" and the "SDK".

    Convert these .rpm files into .deb packages and install using "alien" ("sudo apt-get install alien" if you don't have it).

    For example, for version 12.1.0.2.0-1 for Linux x86_64 (64-bit):

    alien -i oracle-instantclient12.1-basic-12.1.0.2.0-1.x86_64.rpm
    alien -i oracle-instantclient12.1-sqlplus-12.1.0.2.0-1.x86_64.rpm
    alien -i oracle-instantclient12.1-devel-12.1.0.2.0-1.x86_64.rpm

    Test your Instantclient install by using "sqlplus" or "sqlplus64" to connect to your database:

sqlplus username/password@//dbhost:1521/SID

If sqlplus complains of a missing libaio.so.1 file, run  

sudo apt-get install libaio1

If sqlplus complains of a missing libsqlplus.so file, follow the steps in the section "Integrate Oracle Libraries" below.

If you execute sqlplus and get "sqlplus: command not found", see the section below about adding the ORACLE_HOME variable.  
Integrate Oracle Libraries

If oracle applications, such as sqlplus, are complaining about missing libraries, you can add the Oracle libraries to the LD_LIBRARY_PATH each time it is used:

export LD_LIBRARY_PATH=/usr/lib/oracle/<version>/client(64)/lib/:$LD_LIBRARY_PATH

For example, 12.1 version for Linux x86_64:

export LD_LIBRARY_PATH=/usr/lib/oracle/12.1/client64/lib/:$LD_LIBRARY_PATH

or to add it to the system library list create a new file as follows:

sudo vi /etc/ld.so.conf.d/oracle.conf

    and add the oracle library path as the first line.  For example,

/usr/lib/oracle/12.1/client64/lib/

    or

/usr/lib/oracle/11.2/client/lib/

    Then run ldconfig:

sudo ldconfig

ORACLE_HOME

Many Oracle database applications look for Oracle software in the location specified in the environment variable 'ORACLE_HOME'.

Typical workstations will only have one Oracle install, and will want to define this variable in a system-wide location.

sudo vi /etc/profile.d/oracle.sh

Add the following:

export ORACLE_HOME=/usr/lib/oracle/<version>/client(64)

For example

export ORACLE_HOME=/usr/lib/oracle/11.2/client

Alternatively, each user can define this in their ~/.bash_profile

Note: From Ubuntu 11.04 (confirmed in 11.04 and 14.04) sqlplus was not recognized as a command unless the following line was also included in the oracle.sh file:

export PATH=$PATH:$ORACLE_HOME/bin

SDK fix

Some packages may look for 'oci.h' in $ORACLE_HOME/include, or in $ORACLE_HOME/rdbms/public

The instant client sometimes places the include files, such as oci.h, in /usr/include/oracle/<version>/client.

Inspect your system by running the following commands

ls $ORACLE_HOMEls -d /usr/include/oracle/*/client*/*

If there is no 'include' directory under ORACLE_HOME, and it is located over in /usr/include/oracle/ , create a symbolic link to assist packages looking for these header files.  For example,

sudo ln -s /usr/include/oracle/11.2/client $ORACLE_HOME/include

or:

sudo ln -s /usr/include/oracle/12.1/client64 $ORACLE_HOME/include

And then check it is correct

ls $ORACLE_HOME