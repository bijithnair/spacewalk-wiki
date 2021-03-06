
# **DEPRECATED, NO LONGER USED**

# Oracle 10g Express Edition Setup

## Limitations




 * will use maximum of 1GB RAM
 * 4GB user data (max)
 * may be installed on a multiple CPU server, but may only be executed on one processor in any server
 * one database (max)

The full license agreement is available via the download page linked below.
## Download

### Server




You can download Oracle 10g Express Edition (XE) Server from Oracle's [Oracle Database 10g Express Edition Downloads for Linux x86](http://www.oracle.com/technology/software/products/database/xe/htdocs/102xelinsoft.html) page. You will need an Oracle OTN account to download the rpms and you will need to accept the license agreements. Download the "Oracle Database 10g Express Edition (*Universal*)"

 * oracle-xe-univ-10.2.0.1-1.0.i386.rpm

which is the Multi-byte Unicode/UTF-8 database, not the "Western European" Single-byte LATIN1 database.
### Client



You will also need the Oracle Instant Client: [i386](http://www.oracle.com/technetwork/topics/linuxsoft-082809.html) or [x86_64](http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html) version. Download the following two rpms (where ARCH is either i386 or x86_64):

 * oracle-instantclient11.2-basic-11.2.0.2.0.ARCH.rpm
 * oracle-instantclient11.2-sqlplus-11.2.0.2.0.ARCH.rpm

Do *NOT* use oracle-xe-client.
## Before installing Oracle XE



If you want to run with SELinux enabled, run the following two commands to create the group and user for the oracle-xe-univ package, prior to installing said package:


    # /usr/sbin/groupadd -r dba
    # /usr/sbin/useradd -r -M -g dba -d /usr/lib/oracle/xe -s /bin/bash oracle

This is needed to give the oracle user uid below 500. See [Features/SELinux](Features/SELinux) for more details and limitations of SELinux support.

Check if you have swap > 1GB because Oracle XE RPM will need it. You can create temporary one (or grow existing) with


    # dd if=/dev/zero of=/var/swapfile bs=1M count=1100
    # mkswap /var/swapfile
    # swapon /var/swapfile

Ensure your hostname is resolvable either with by DNS or an entry in /etc/hosts. Otherwise the configuration of the Oracle Net Listener will fail.
## Install

### Various packages needed




The *bc* is a dependency of oracle-xe which is not resolved automatically causing the oracle install to be incomplete. We also make sure glibc and libaio are installed in 32bit variant, as Oracle XE is 32bit but it won't pull glibc.i686 automatically.


    # yum -y install bc libc.so.6 libaio.so.1
### Oracle XE itself




    # yum -y localinstall --nogpgcheck oracle-xe-univ-10.2.0.1-1.0.i386.rpm

The *--nogpgcheck* is required because the Oracle RPMs are not signed. If the installation of oracle-xe-univ fails on RHEL6 and Fedora 12+ with `error: "net.bridge.bridge-nf-call-ip6tables" is an unknown key` and similar errors, just rerun that step and it should work for the second time.
### Oracle InstantClient




    # yum -y localinstall --nogpgcheck oracle-instantclient11.2-basic*.rpm oracle-instantclient11.2-sqlplus*.rpm

  * *WARNING:* Don't run `oracle_env.sh` (or `oracle_env.csh`) in your root environment settings. Instantclient stuff works fine without it. It's usefull only for commandline XE server management under `oracle` user. 
### Oracle compatibility library



To install oracle-lib-compat, you'll need to have the Spacewalk channel configured, installation directions for that are in HowToInstall.


    # yum -y install oracle-lib-compat

This will likely pull in `compat-libstdc++-33`.
## Install SELinux Policy



If you are running with SELinux enabled then install the following packages from the Spacewalk repo before running `oracle-xe configure`:


    # yum -y install oracle-xe-selinux oracle-instantclient-selinux oracle-instantclient-sqlplus-selinux

This will likely pull in `oracle-nofcontext-selinux` as well.
## Server Setup



Configure the Oracle XE database by running


    # /etc/init.d/oracle-xe configure

Here are some sane values for the configure:


     HTTP port for Oracle Application Express: 9055
     Database listener port: 1521
     Password for SYS/SYSTEM: <password>
     Start at boot: y

Note that the port 8080 is used by tomcat so you cannot accept the default for the Oracle Application Express. Also, the SELinux policy module assumes that the Application Express port is 9055, if you want different value, you need to define it for `oracle_port_t` via `semanage`. (This 9055 value is new for Spacewalk 0.7, as we needed to give way to Fedora 12's default allocation of port 9000 for squid.)

Note that Oracle does not like certain characters in your password, but will not tell you until you can't login as sys later on.
### Rerunning the configure



If you find out you are not happy with the configuration, you can revert by doing the following steps:


    # service oracle-xe stop
    # rm -f /etc/sysconfig/oracle-xe /var/tmp/.oracle
    # rpm2cpio oracle-xe-univ-10.2.0.1-1.0.i386.rpm | \
        ( cd / && cpio -iud ./usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/scripts/DatabaseHomePage.sh \
                            ./usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/scripts/postDBCreation.sql \
                            ./usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/scripts/readonlinehelp.sh \
                            ./usr/lib/oracle/xe/app/oracle/product/10.2.0/server/network/admin/listener.ora \
                            ./usr/lib/oracle/xe/app/oracle/product/10.2.0/server/network/admin/tnsnames.ora \
                            ./usr/lib/oracle/xe/app/oracle/product/10.2.0/server/config/seeddb/xeseed.dfb )
    # /etc/init.d/oracle-xe configure
## Test your connection with sqlplus



Replace <password> with the password provided on /etc/init.d/oracle-xe configure

Run

    sqlplus 'sys/<password>@//localhost/XE as sysdba' 
 
You should see something like
 

    SQL*Plus: Release 10.2.0.4.0 - Production on Fri Apr 10 12:59:01 2009
    
    Copyright (c) 1982, 2007, Oracle.  All Rights Reserved.
    
    Enter password: 
    
    Connected to:
    Oracle Database 10g Express Edition Release 10.2.0.1.0 - Production
    
    SQL>  
## Create the spacewalk database user



Replace <password> with the password provided on /etc/init.d/oracle-xe configure


    # sqlplus 'sys/<password>@//localhost/XE as sysdba'
    SQL> create user spacewalk identified by spacewalk default tablespace users;
    SQL> grant dba to spacewalk;
    SQL> quit

Test that


    # sqlplus spacewalk/spacewalk@//localhost/XE

works.
## Additional Oracle configuration



Oracle XE is initially configured to grant 40 connections to its database.  Spacewalk needs more than this default configuration.  We recommend increasing this setting to 400. 

Oracle XE has a bug that causes an Internal Server Error (500) in Spacewalk when viewing Systems -> Pick a System -> Software -> Packages -> Install New Package
(URL: https://spacewalk.example.com/rhn/systems/details/packages/InstallPackages.do?sid=**********) or when running
`rhnreg_ks` to register a client.

In order to avoid these two problems we need to increase the max processes in Oracle and to 
 
{{{ 
# sqlplus spacewalk/spacewalk@//localhost/XE
SQL> alter system set processes = 400 scope=spfile; 
SQL> alter system set "_optimizer_filter_pred_pullup"=false scope=spfile; 
SQL> alter system set "_optimizer_cost_based_transformation"=off scope=spfile; 
SQL> quit  
# /sbin/service oracle-xe restart 
}}}

If you encounder 500s with messages like

    DATABASE CONNECTION TO 'xe' LOST

and /usr/lib/oracle/xe/app/oracle/admin/XE/bdump/alert_XE.log having messages like

    ORA-07445: exception encountered: core dump [08F93BDE] [SIGSEGV] [Address not mapped to object] [0x34979C] [] []

you might try to workaround it with
{{{ 
# sqlplus spacewalk/spacewalk@//localhost/XE
SQL>  alter system set query_rewrite_enabled=TRUE ;
SQL> quit
}}}

as suggested in http://stephensorablog.blogspot.com/2006/05/solution-to-dreaded-ora-07445-error.html

You might want to install 'rlwrap' to make your sqlplus more usable. `rlwrap` will retain 
a ['curses'](http://en.wikipedia.org/wiki/Curses_(programming_library)) like behavior with sqlplus such as maintaining sql call history, etc.


    yum install rlwrap
 
You can then do  

{{{ 
rlwrap sqlplus spacewalk/spacewalk@//localhost/XE
}}}  
## Setup development schema (optional)
 

 
This is only for use if running a development environment or development version of spacewalk.  See [Dev Station setup](DevelopmentWorkstationSetup) for this info.
## Troubleshooting



 * If you get errors such as: 


       ORA-01654: unable to extend index SPACEWALK.RHN_PACKAGE_FILE_CID_PID_IDX by 5 in tablespace SPACEWALK

 or anything from Oracle complaing that it can't *extend* an index, a table, etc.. you should run:


      SQL> ALTER TABLESPACE system ADD DATAFILE '/usr/lib/oracle/xe/oradata/XE/system_02.dbf' size 200m;
      SQL> ALTER TABLESPACE users ADD DATAFILE '/usr/lib/oracle/xe/oradata/XE/users_02.dbf' size 200m;
      SQL> ALTER TABLESPACE undo ADD DATAFILE '/usr/lib/oracle/xe/oradata/XE/undo_02.dbf' size 200m;

 Quit sqlplus and restart spacewalk and oracle-xe (may not require a restart)

 * If you run out of space in Oracle XE you can try and reclaim some space by shrinking your storage:

 Administration > Storage > Compact Storage in XE webUI (http://your.satellite.tld:9055/apex/) .

 * If you get `ORA-12514` using `oracle-xe` it could be `bc` is not installed, among many other things (ask google about ORA-##### errors)
## Misc links



[Installation Guide](http://www.oracle.com/technology/software/products/database/xe/files/install.102/b25144/toc.htm) 

[Installing Oracle XE on Debian](http://www.davidpashley.com/articles/oracle-install.html) 

[HowTo migrate from local Oracle XE to an external Oracle instance](http://www.mail-archive.com/spacewalk-list@redhat.com/msg03837.html)

Continue installing Spacewalk: [HowToInstall > InstallingSpacewalk](HowToInstall)
