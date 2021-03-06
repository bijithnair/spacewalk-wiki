# Spacewalk and SELinux



The notes below are development notes from the time when feature of confining Spacewalk with SELinux was being added to the project. Spacewalk as of 1.0+ is able to run on systems with SELinux in enforcing mode. As such this feature is now complete and we consider any issue with SELinux in Spacewalk as a normal bug. No active development around SELinux is currently active because it is finished.

So for Spacewalk users, things just work, and if they don't, it's a bug (or sometimes PEBKAC) and should be treated like that (mailing list, bugzilla).

For people interested in history, read on.

----
# Introduction



SELinux is different layer of access control, besides the owner / group / other. For a process to access file, directory, socket, etc., an explicit *allow* has to be specified in the SELinux policy. If there is no allow rule in the system, access is denied and logged (if SELinux is run in Enforcing mode) or the (so called) AVC denial is logged but the operation succeeds (in Permissive).

Red Hat Enterprise Linux 5 comes with *selinux-policy-targeted* package which defines the rules for the core system and many applications in the system, especially daemons. They don't however cover Spacewalk as Spacewalk is not part of EL 5, and even for applications that are covered by the generic policy, Spacewalk might need to allow access to additional resources. We add these via additional SELinux policy modules, distributed as additional rpm packages with Spacewalk.

There are also selinux-policy-strict and selinux-policy-mls packages but we only assume targeted for Spacewalk.
## Working with SELinux



SElinux logs problems to */var/log/audit/audit.log*. If you do


    # grep 'avc:  denied' /var/log/audit/audit.log      # note the double space after colon

you'll see any denials that happened on your system. If you report an issue, please make sure you only report new ones, not ancient ones. I prefer to do


    # tail -f /var/log/audit/audit.log | grep AVC

in an extra terminal to see what is going on (or that everything is calm).

Allows in SElinux are defined for types -- each process has a type and each file, directory, etc. has a type, and there has to be an allow for the process (source) to access the resource (target). In the audit.log, the type of the source is shown as the third part of the scontext value, and type of the target is shown in tcontext. If you see SELinux problem like


    type=AVC msg=audit(1231949469.659:664):
    avc:  denied  { read } for  pid=13479 comm="httpd" name="rhn_server_xmlrpc.log" dev=xvda1 ino=886725
    scontext=root:system_r:httpd_t:s0 tcontext=system_u:object_r:var_log_t:s0 tclass=file

in your audit.log (it will be on one line), you'll know that the problem is that process with type `httpd_t` tries to do something (`read` in this case) with something with type `var_log_t`, and there was not `allow` rule in the system which would allow that.

There are additional helpful fields in the message, and if you report a problem, always put the whole line to the report. In our example above, we see that it was `httpd` which tried to read file `rhn_server_xmlrpc.log`, and you can do


    # find /var -inum 886725

to check the file name via the inode number as well.

A type for something on a disk is stored on that disk, as a *label* attached to that something (file, directory, socket). Labels are attached using program *restorecon* which uses a filesystem path -> type mappings. You can see the mappings on your system in /etc/selinux/targeted/contexts/files/file_contexts*. Command


    # restorecon -nrv /some/path

will tell you if some of the files, directories, etc. under /some/path need their label on disk (that's what SELinux uses to allow or deny access) changed. Command


    # restorecon -rv /some/path

without that `-n` will do the relabeling.

To see what file context (SELinux type) is currently labeled to a file or directory, use `ls -Z`:


    # ls -Z /var/log/rhn

Both rpms in the core EL and additional SELinux rpms try to get the labels right behind the scenes. Therefore, ideally you should never need to explicitly `restorecon` anything, unless asked to do so by the documentation when you decide to have different layout on the disk or in similar situation. But if you get denial from SELinux, it is a good idea to check with `-n` whether labels on disk look sane.

If you see SELinux error and need to continue working with your Spacewalk server, switch from Enforcing to Permissive:


    # getenforce 
    Enforcing
    # setenforce 0
    # getenforce 
    Permissive
    #

Your Spacewalk server will no longer block any operations (and thus will be less secure but at least it will work). Please report the problem via mailing-list or bugzilla, and when rpms with fix are released, you can install them and turn Enforcing back on with


    # setenforce 1
## Spacewalk SELinux modules



For Spacewalk 0.4, the following rpms are available:
### spacewalk-selinux



Version 0.4.1-7.el5.noarch.

Contains *spacewalk* policy module, the core of the Spacewalk SELinux support. It defines a couple of new types, as well as adds new allow rules for existing types.

It is required by the spacewalk-0.4.2-3.el5.noarch package so you'll get it installed with Spacewalk 0.4 by default.

The policy module expects mount point to be */var/satellite*. If you point your `spacewalk-setup` to different path, you need to do


    # semanage fcontext -a -t spacewalk_data_t '<your_path_here>(/.*)?'

after you've installed `spacewalk-selinux` but before you run `spacewalk-setup` or `spacewalk-setup --upgrade`.
### oracle-nofcontext-selinux



Version 0.1-23.2.el5.noarch.

Contains policy module *oracle-nofcontext* which is basically *oracle* but without file contexts. It installs types and rules which Oracle XE and Oracle InstantClient SELinux rpms use.
### oracle-instantclient-selinux



Version 10.2-6.el5.i386.

It adds a couple of local file contexts and relabels a couple of files from oracle-instantclient-*. It also clears the execstack flag from libraries.

It is required by spacewalk-selinux, so it will be installed by default. It pulls in oracle-nofcontext-selinux.
### jabberd-selinux



Version 1.4.0-2.el5.noarch.

Contains *jabber* policy module for jabberd.

Note that the SELinux policy module is available in the reference policy but it is not compiled in to *selinux-policy-targeted* on EL 5. That's why we distribute it separate rpm with Spacewalk, and we also tune the file context to the version of jabberd we ship.

This package is optional in Spacewalk 0.4, we recommend you install it. This package will be installed in Spacewalk 0.5 by default.
### osa-dispatcher-selinux



Version 5.9.2-1.el5.noarch.

Contains *osa-dispatcher* policy module for osa-dispatcher.

This package is optional in Spacewalk 0.4, we recommend you install it. This package will be installed in Spacewalk 0.5 by default.
### oracle-xe-selinux



Version 10.2-7.el5.noarch.

Contains *oracle-xe* policy module.

If you run local Oracle XE, this module is needed. However, it is *necessary* to run


    # /usr/sbin/groupadd -r dba
    # /usr/sbin/useradd -r -M -g dba -d /usr/lib/oracle/xe -s /bin/bash oracle

before installing the oracle-xe-univ package, to create the oracle user as system user (with uid < 500).

Also, the policy module expects Oracle XE's web interface to be on port 9000. If you are about to configure different port in `service oracle-xe configure`, you need to do


    # semanage port -a -t oracle_port_t -p tcp <your_port_number>
### spacewalk-monitoring-selinux



This package is not in Spacewalk 0.4 but is available in nightly repo, and will be in Spacewalk 0.5.

Version 0.5.4-1.el5.noarch.

Contains *spacewalk-monitoring* policy module, for monitoring and monitoring scout.
## What a working Spacewalk 0.4 system looks like



If you've installed Spacewalk 0.4 correctly, you can use `ps axuwZ` (note the `Z`) to list processes with their SELinux types. The running processes should have the following types:
### Apache: httpd_t




    root:system_r:httpd_t           root     24047  0.0  1.6  38156 15168 ?        Rs   Jan14   0:01 /usr/sbin/httpd
    root:system_r:httpd_t           apache   27778  0.0  1.1  38156 10848 ?        S    Jan15   0:00 /usr/sbin/httpd
    root:system_r:httpd_t           apache   27779  0.0  1.1  38156 10848 ?        S    Jan15   0:00 /usr/sbin/httpd
    root:system_r:httpd_t           apache   27780  0.0  1.1  38156 10848 ?        S    Jan15   0:00 /usr/sbin/httpd
    root:system_r:httpd_t           apache   27781  0.0  1.1  38156 10848 ?        S    Jan15   0:00 /usr/sbin/httpd

Note that both the parent process owned by `root` and the processes owned by `apache` have the same SELinux type.
#### Apache CGI script (well, rewrite map): httpd_sys_script_t




    root:system_r:httpd_sys_script_t root    27777  0.0  0.3   5408  3456 ?        S    Jan15   0:00 /usr/bin/perl /etc/rhn/satellite-httpd/conf/satidmap.pl
### Java applications: java_t

#### Tomcat





    root:system_r:java_t            tomcat   23756  0.0 19.2 401388 180060 ?       Sl   Jan14   0:57 /usr/lib/jvm/java/bin/java -ea -Xms256m -Xmx256m -Djava.awt.headless=true -Dorg.xml.sax.driver=org.apache.xerces.parsers.SAXParser -XX:MaxNewSize=256 -XX:-UseConcMarkSweepGC -Dcatalina.ext.dirs=/usr/share/tomcat5/shared/lib:/usr/share/tomcat5/common/lib -Djava.endorsed.dirs=/usr/share/tomcat5/common/endorsed -classpath /usr/lib/jvm/java/lib/tools.jar:/usr/share/tomcat5/bin/bootstrap.jar:/usr/share/tomcat5/bin/commons-logging-api.jar:/usr/share/java/mx4j/mx4j-impl.jar:/usr/share/java/mx4j/mx4j-jmx.jar -Dcatalina.base=/usr/share/tomcat5 -Dcatalina.home=/usr/share/tomcat5 -Djava.io.tmpdir=/usr/share/tomcat5/temp org.apache.catalina.startup.Bootstrap start
#### Taskomatic




    root:system_r:initrc_t          root      8083  0.0  0.0  12216   436 ?        Sl   16:00   0:00 {{{ /etc/rhn/default/rhn_taskomatic_daemon.con
    root:system_r:java_t            root      8085  0.1 10.6 331324 56972 ?        Sl   16:00   0:40 /usr/bin/java -Dibm.dst.compatibility=true -Xms32m -Xmx192m -Dj
    root:system_r:java_t            tomcat    8643  0.3 13.8 411764 74120 ?        Sl   16:01   1:31 /usr/lib/jvm/java/bin/java -ea -Xms256m -Xmx256m -Djava.awt.hea

Note that the `/usr/bin/taskomaticd` process itself runs as `initrc_t`.
#### Search server




    root:system_r:initrc_t          root      8708  0.0  0.0  12216   432 ?        Sl   16:01   0:01 /usr/bin/rhnsearchd /etc/rhn/search/rhn_search_daemon.conf wrap
    root:system_r:java_t            root      8711  0.4  2.8 668408 15252 ?        Sl   16:01   1:50 /usr/bin/java -Djava.library.path=/usr/lib:/usr/lib64 -classpat

Note that the `/usr/bin/rhnsearchd` process itself runs as `initrc_t`.
### Jabberd: jabber_t




    root:system_r:jabberd_t         jabberd  23158  0.0  0.1   6304  1404 ?        S    Jan14   0:07 /usr/bin/router -c /etc/jabberd/router.xml
    root:system_r:jabberd_t         jabberd  23159  0.0  0.1   6160  1436 ?        S    Jan14   0:00 /usr/bin/resolver -c /etc/jabberd/resolver.xml
    root:system_r:jabberd_t         jabberd  23160  0.0  0.2   7340  2588 ?        S    Jan14   0:02 /usr/bin/sm -c /etc/jabberd/sm.xml
    root:system_r:jabberd_t         jabberd  23161  0.0  0.1   6176  1264 ?        S    Jan14   0:00 /usr/bin/s2s -c /etc/jabberd/s2s.xml
    root:system_r:jabberd_t         jabberd  23162  0.0  0.4   7956  3880 ?        S    Jan14   0:05 /usr/bin/c2s -c /etc/jabberd/c2s.xml
    root:system_r:jabberd_t         jabberd  23163  0.0  0.2   7576  2520 ?        Ss   Jan14   0:06 perl -w -x /usr/bin/jabberd -b
### OSA dispatcher: osa_dispatcher_t




    root:system_r:osa_dispatcher_t  root     23181  0.3  1.4 103008 13696 ?        S    Jan14  10:44 python /usr/sbin/osa-dispatcher --pid-file /var/run/osa-dispatc
### Oracle XE

#### Listener: oracle_tnslsnr_t




    root:system_r:oracle_tnslsnr_t  oracle   16158  0.0  0.6  21896  5668 ?        Ss   Jan14   0:12 /usr/lib/oracle/xe/app/oracle/product/10.2.0/server/bin/tnslsnr
#### Database processes: oracle_db_t




    root:system_r:oracle_db_t       oracle   16163  0.0  1.6 335464 15452 ?        Ss   Jan14   0:09 xe_pmon_XE
    root:system_r:oracle_db_t       oracle   16165  0.0  1.0 334848  9400 ?        Ss   Jan14   0:00 xe_psp0_XE
    root:system_r:oracle_db_t       oracle   16167  0.0  1.8 334852 17680 ?        Ss   Jan14   0:00 xe_mman_XE
    root:system_r:oracle_db_t       oracle   16169  0.0  6.4 336920 60296 ?        Ss   Jan14   0:06 xe_dbw0_XE
    root:system_r:oracle_db_t       oracle   16171  0.0  3.1 350404 29736 ?        Ss   Jan14   0:05 xe_lgwr_XE
    root:system_r:oracle_db_t       oracle   16173  0.0  2.0 335372 18800 ?        Ss   Jan14   0:27 xe_ckpt_XE
    root:system_r:oracle_db_t       oracle   16175  0.0  7.3 335380 68572 ?        Ss   Jan14   0:10 xe_smon_XE
    root:system_r:oracle_db_t       oracle   16177  0.0  1.8 334848 17292 ?        Ss   Jan14   0:00 xe_reco_XE
    root:system_r:oracle_db_t       oracle   16179  0.0  4.1 336452 39208 ?        Ss   Jan14   0:28 xe_cjq0_XE
    root:system_r:oracle_db_t       oracle   16181  0.0  6.6 337600 62296 ?        Ss   Jan14   0:13 xe_mmon_XE
### Monitoring



To add SELinux support for monitoring and monitoring scout, package spacewalk-monitoring-selinux version 0.5.* or higher needs to be installed. It is in the nightly repo and will be in Spacewalk 0.5.
#### Monitoring and monitoring scout: spacewalk_monitoring_t




    root:system_r:spacewalk_monitoring_t root 1861  0.0  0.1  14596  1500 pts/2    S    12:06   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=GenerateNotifConfig --us
    root:system_r:spacewalk_monitoring_t nocpulse 1862 0.0  0.2 14596 2460 pts/2   S    12:06   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=GenerateNotifConfig --us
    root:system_r:spacewalk_monitoring_t nocpulse 1863 0.0  2.1 107412 20240 pts/2 S    12:06   0:00 /usr/bin/perl /usr/bin/generate-config
    root:system_r:spacewalk_monitoring_t root 1885  0.0  0.1  14596  1468 pts/2    S    12:06   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=NotifEscalator --user=no
    root:system_r:spacewalk_monitoring_t nocpulse 1886 0.0  0.2 14596 2356 pts/2   S    12:06   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=NotifEscalator --user=no
    root:system_r:spacewalk_monitoring_t nocpulse 1887 0.4  1.5 33952 14476 pts/2  S    12:06   0:15 /usr/bin/perl /usr/bin/notif-escalator
    root:system_r:spacewalk_monitoring_t root 1903  0.0  0.1  14596  1480 pts/2    S    12:06   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=NotifLauncher --user=noc
    root:system_r:spacewalk_monitoring_t nocpulse 1904 0.0  0.2 14596 2368 pts/2   S    12:06   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=NotifLauncher --user=noc
    root:system_r:spacewalk_monitoring_t nocpulse 1905 0.9  1.5 34248 14768 pts/2  S    12:06   0:33 /usr/bin/perl /usr/bin/notif-launcher
    root:system_r:spacewalk_monitoring_t root 1951  0.0  0.1  14604  1496 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=Notifier --user=nocpulse
    root:system_r:spacewalk_monitoring_t nocpulse 1952 0.0  0.2 14604 2456 pts/2   S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=Notifier --user=nocpulse
    root:system_r:spacewalk_monitoring_t nocpulse 1953 0.0  2.0 106228 18980 pts/2 R    12:07   0:02 /usr/bin/perl /usr/bin/notifier
    root:system_r:spacewalk_monitoring_t root 2014  0.0  0.1  14596  1500 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=AckProcessor --user=nocp
    root:system_r:spacewalk_monitoring_t nocpulse 2015 0.0  0.2 14596 2460 pts/2   S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=AckProcessor --user=nocp
    root:system_r:spacewalk_monitoring_t nocpulse 2016 0.6  1.4 32568 13128 pts/2  S    12:07   0:23 /usr/bin/perl /usr/bin/ack-processor
    root:system_r:spacewalk_monitoring_t root 2036  0.0  0.1  14604  1408 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=TSDBLocalQueue --user=ap
    root:system_r:spacewalk_monitoring_t apache 2037 0.0  0.2 14604  2484 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=TSDBLocalQueue --user=ap
    root:system_r:spacewalk_monitoring_t apache 2040 0.0  1.2 30904 11440 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/TSDBLocalQueue.pl
    root:system_r:spacewalk_monitoring_t root 2082  0.0  0.1  14604  1484 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=SputLite --user=root --h
    root:system_r:spacewalk_monitoring_t root 2083  0.0  0.2  14604  2368 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=SputLite --user=root --h
    root:system_r:spacewalk_monitoring_t root 2084  0.0  1.5  21852 14808 pts/2    S    12:07   0:03 /usr/bin/perl /usr/bin/execute_commands
    root:system_r:spacewalk_monitoring_t root 2114  0.0  0.1  14604   948 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=Dequeuer --user=nocpulse
    root:system_r:spacewalk_monitoring_t nocpulse 2115 0.0  0.2 14604 1876 pts/2   S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=Dequeuer --user=nocpulse
    root:system_r:spacewalk_monitoring_t nocpulse 2116 0.1  1.2 18500 11916 pts/2  S    12:07   0:04 /usr/bin/perl /usr/bin/dequeue
    root:system_r:spacewalk_monitoring_t root 2165  0.0  0.1  14596  1496 pts/2    S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=Dispatcher --user=nocpul
    root:system_r:spacewalk_monitoring_t nocpulse 2166 0.0  0.2 14728 2444 pts/2   S    12:07   0:00 /usr/bin/perl /usr/bin/gogo.pl --fname=Dispatcher --user=nocpul
    root:system_r:spacewalk_monitoring_t nocpulse 2167 0.0  2.8 49924 26960 pts/2  S    12:07   0:02 /usr/bin/perl /usr/bin/kernel.pl --loglevel 1
## Parts that are missing in Spacewalk 0.4



 * Monitoring is not supported by any of the SELinux policy modules. You will likely see monitoring processes running as `initrc_t`, and you may see AVC denials in the log.
   * In nightly repo and in Spacewalk 0.5, monitoring is supported via spacewalk-monitoring-selinux package.
 * Spacewalk Proxy is not supported by any of the SELinux policy modules.
   * In nightly repo and in Spacewalk Proxy 0.5, the default Apache and squid, plus Spacewalk's spacewalk-proxy-selinux, osa-dispatcher-selinux, and spacewalk-monitoring-selinux packages are available.
## Other documents



 * Development notes: [[Features_SELinuxNotes]]
 * Current requirements: [[Features_SELinux_Requirements2]]
   * Original version of requirements: [[Features_SELinux_Requirements]]
 * Presentation about the experience: [SELinux: how we confined Spacewalk by Jan Pazdziora](http://www.adelton.com/docs/spacewalk/selinux-how-we-confined-spacewalk)
   * How to confine a large application, with examples. 
