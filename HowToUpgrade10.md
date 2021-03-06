# Spacewalk Upgrade Instructions



These are upgrade instruction for upgrading Spacewalk 0.8 to Spacewalk 1.0.

These upgrade instruction apply to Spacewalk installations meeting the following criteria:

  *  Spacewalk 0.8 running on Red Hat Enterprise Linux 5 Server or CentOS 5
  *  Your Spacewalk uses Oracle 10g / Oracle-XE as a database backend
# Archive of older upgrade instruction

Spacewalk 0.7 to 0.8 upgrade instructions, are available at

[HowToUpgrade08](HowToUpgrade08)
----


Spacewalk upgrade involves several basic steps:
  1.  Configuration files and Spacewalk database backup
  2.  Package upgrade
  3.  Schema upgrade
  4.  Upgrade of Spacewalk configuration
  5.  Restart Spacewalk
  6.  Update of monitoring setup
  7.  Restart Spacewalk
  8. Rebuild search indexes

----
# Assumptions



  * For RHEL or CentOS, you will need the Base, EPEL 5 repositories enabled for dependencies.
  * You had set up your yum repository configuration to point to spacewalk 1.0 repo. For the repo setup specifics, see HowToInstall#SettingupSpacewalkrepo.
# Database and configuration backup




  *  For existing configuration files, create a backup of everything under /etc/sysconfig/rhn /etc/rhn and /etc/jabberd
  *  Backup your ssl build directory, ordinarily /root/ssl-build
  *  For instructions on how to create a backup of your existing Spacewalk database consult either Oracle documentation or contact your DBA
# Package upgrade



Stop your Spacewalk by running:


    # /usr/sbin/rhn-satellite stop

Perform package upgrade using yum:


    # yum upgrade

During the upgrade, you may notice messages printed to the terminal when installing oracle-instantclient-selinux and spacewalk-selinux.
These messages are produced by restorecon and do not pose any harm.
On the contrary, these messages indicate relabeling of file system objects that is required for correct Spacewalk and SELinux symbiosis.
# Schema upgrade



Make sure your Spacewalk server is down:


    # /usr/sbin/rhn-satellite status

Make sure your oracle server is running (likely it isn't running since the earlier 'rhn-satellite stop' command stops the oracle-xe processes). For oracle-xe that would be:


    # service oracle-xe status

If Oracle isn't running, start it


    # service oracle-xe start
Run spacewalk-schema-upgrade script to upgrade database schema:


    # /usr/bin/spacewalk-schema-upgrade


Log files from schema upgrade are being put into /var/log/spacewalk/schema-upgrade
# Upgrade of Spacewalk configuration



For a successful upgrade, it is required to deploy configuration for cobbler and update configuration of apache SSL virtual host. Both steps can be accomplished by running:


    # spacewalk-setup --disconnected --upgrade

You'll be asked by spacewalk-setup to provide your database connection information.  This is the same information you provided to HowToInstall. Following this spacewalk-setup will ask you whether you wish to
have your apache ssl configuration (/etc/httpd/conf.d/ssl.conf) updated by spacewalk-setup. You may choose to setup your ssl.conf manually, in which
case you have to make sure you have the following directives present in your /etc/httpd/conf.d/ssl.conf:


    # cat /etc/httpd/conf.d/ssl.conf
    ...
    <VirtualHost _default_:443>
    ...
    SSLCertificateFile /etc/pki/tls/certs/spacewalk.crt
    SSLCertificateKeyFile /etc/pki/tls/private/spacewalk.key
    RewriteEngine on
    RewriteOptions inherit
    SSLProxyEngine on
    <IfModule mod_jk.c>
        JkMountCopy On
    </IfModule>
    ...
    </VirtualHost>
    ...

Restore some of the custom values you might have set previously in /etc/rhn/rhn.conf from the backup of your configuration files, such as:

  *  debug = 3
  *  pam_auth_service = rhn-satellite

Clean jabberd authentication database:


    # rm -f /var/lib/jabberd/db/authreg.db
# Restart Spacewalk
 



    # /usr/sbin/rhn-satellite start
# Update of monitoring setup



Recent Spacewalk installations may have set several important monitoring configuration values incorrectly. To correct these, run the following
as root (note that you should run this command regardless of your current monitoring status; even on installations that never had monitoring
enabled, running this script will have no ill effect):


    # /usr/share/spacewalk/setup/upgrade/rhn-update-monitoring.pl

You may now choose to activate monitoring or both monitoring & monitoring scout on your Spacewalk.

If you had monitoring enabled previously and wish to re-enable it now (without having to do so in the web ui), run as root:


    # /usr/share/spacewalk/setup/upgrade/rhn-enable-monitoring.pl

If you'd like to re-enable both monitoring and monitoring scout on your Spacewalk (without having to do so in the web ui), instead run as root


    # /usr/share/spacewalk/setup/upgrade/rhn-enable-monitoring.pl --enable-scout
# Restart Spacewalk



If all of the above steps completed successfully, you can start your upgraded Spacewalk server.


    # /usr/sbin/rhn-satellite restart
# Rebuild search indexes



After a successful upgrade, it is recommended to rebuild search indexes used by Spacewalk search engine (rhn-search service).
This can be accomplished by the following:


    # /etc/init.d/rhn-search cleanindex