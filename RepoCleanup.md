
# **DEPRECATED, NO LONGER USED**


Cleaning up Repository and getting the free items into the Fedora Space

Note: This page is now obsolete -- we now have everything built from sources.

----

A new page now has information on GettingPackagesIntoFedora
# RPMS missing SRPMS

The following binary RPMS are found in http://spacewalk.redhat.com/yum/rhel/5Server/i386/ and not in the SRPM tree http://www.redhat.com/spacewalk/source/

This needs to be fixed :)


 * oracle-lib-compat-10.2-3.el5.i386.rpm
 * osad-5.2.0-1.noarch.rpm
 * osa-dispatcher-5.2.0-1.noarch.rpm
 * PyXML-0.8.4-4.i386.rpm
 * rhncfg-5.2.0-0.noarch.rpm
 * rhncfg-actions-5.2.0-0.noarch.rpm
 * rhncfg-client-5.2.0-0.noarch.rpm
 * rhncfg-management-5.2.0-0.noarch.rpm
 * rhn-custom-info-5.2.0-1.noarch.rpm
 * rhn-kickstart-5.2.0-5.noarch.rpm
 * rhn-kickstart-common-5.2.0-5.noarch.rpm
 * rhn-kickstart-virtualization-5.2.0-5.noarch.rpm
 * rhnlib-2.2.5-1.noarch.rpm
 * rhn-oracle-jdbc-1.0-14.el5.noarch.rpm
 * rhn-oracle-jdbc-tomcat5-1.0-14.el5.noarch.rpm
 * rhnpush-5.2.0-5.noarch.rpm
 * rhns-0.1-4.el5.noarch.rpm
 * rhns-app-0.1-4.el5.noarch.rpm
 * rhns-applet-0.1-4.el5.noarch.rpm
 * rhns-config-files-0.1-4.el5.noarch.rpm
 * rhns-config-files-common-0.1-4.el5.noarch.rpm
 * rhns-config-files-tool-0.1-4.el5.noarch.rpm
 * rhns-package-push-server-0.1-4.el5.noarch.rpm
 * rhns-proxy-broker-5.2.0-4.el5.noarch.rpm
 * rhns-proxy-common-5.2.0-4.el5.noarch.rpm
 * rhns-proxy-docs-5.2.0-4.el5.noarch.rpm
 * rhns-proxy-html-5.2.0-5.el5.noarch.rpm
 * rhns-proxy-management-5.2.0-4.el5.noarch.rpm
 * rhns-proxy-package-manager-5.2.0-4.el5.noarch.rpm
 * rhns-proxy-redirect-5.2.0-4.el5.noarch.rpm
 * rhns-proxy-tools-5.2.0-4.el5.noarch.rpm
 * rhns-satellite-tools-0.1-4.el5.noarch.rpm
 * rhns-server-0.1-4.el5.noarch.rpm
 * rhn-ssl-cert-check-1.4-10.6.noarch.rpm
 * rhns-sql-0.1-4.el5.noarch.rpm
 * rhns-upload-server-0.1-4.el5.noarch.rpm
 * rhns-xml-export-libs-0.1-4.el5.noarch.rpm
 * rhns-xmlrpc-0.1-4.el5.noarch.rpm
 * rhns-xp-0.1-4.el5.noarch.rpm
 * rhn-virtualization-common-5.2.0-5.noarch.rpm
 * rhn-virtualization-guest-5.2.0-5.noarch.rpm
 * rhn-virtualization-host-5.2.0-5.noarch.rpm
 * spacewalk-setup-0.01-3.noarch.rpm
## Hack Script to find this



    #!/bin/bash
    
    
    # Quick hack to figure out if binary rpms have matching source rpms
    
    # This assumes you have a directory of the binary rpms called rpms and a directory
    #   of the source rpms called srpms.  Also assumes that you run from a level up.
    #  Like ./rpms and ./srpms and then repo-diff.sh script is in .
    #  YMMV
    
    for rpm in `ls -1 rpms/*rpm`
    do
       echo -n `basename $rpm`
       echo -n " | "
       srpm=`rpm -qpi $rpm  | grep "Source RPM" | awk '{print $NF}'`
       ls -1 srpms/$srpm &> /dev/null && echo "Good" || echo "Source RPM not found"
    done
# SRPMS in Spacewalk Repo already found in Fedora Proper



 * asm-1.5.3-1jpp.ep1.1.el5.2.src.rpm
 * bea-stax-1.2.0-0.rc1.2jpp.ep1.1.el5.src.rpm
 * bouncycastle-1.37-1jpp.1.el5.src.rpm
 * concurrent-1.3.4-6jpp.ep1.2.el5.1.src.rpm
 * dom4j-1.6.1-2jpp.ep1.5.el5.2.src.rpm
 * icu4j-3.4.5-2jpp.ep1.2.el5.src.rpm
 * isorelax-0.1-0.20041111.2jpp.ep1.2.el5.4.src.rpm
 * jabberd-2.0s10-3.42.el5.src.rpm
 * jakarta-commons-cli-1.0-6jpp.ep1.1.el5.1.src.rpm
 * jakarta-taglibs-standard-1.1.1-4jpp_1rh.src.rpm
 * jaxen-1.1-1jpp.ep1.4.el5.2.src.rpm
 * jcommon-0.9.7-1jpp.ep1.1.el5.src.rpm
 * jython-2.2-0.a0.2jpp_1rh.src.rpm
 * libapreq2-2.09-6.el5.src.rpm
 * mod_perl-2.0.3-3.el5s2.src.rpm
 * msv-1.2-0.20050722.5jpp.ep1.1.el5.2.src.rpm
 * oro-2.0.8-1jpp_2rh.src.rpm
 * perl-Algorithm-Diff-1.1901-11.el5.src.rpm
 * perl-Apache-DBI-0.94-12.el5.src.rpm
 * perl-Authen-PAM-0.14-14.el5.src.rpm
 * perl-BerkeleyDB-0.33-1.el5.src.rpm
 * perl-Cache-Cache-1.03-7.el5.src.rpm
 * perl-Class-Factory-Util-1.6-6.el5.src.rpm
 * perl-Class-Loader-2.02-11.el5.src.rpm
 * perl-Class-MethodMaker-1.12-12.el5.src.rpm
 * perl-Class-Singleton-1.03-6.el5.src.rpm
 * perl-Config-IniFiles-2.38-6.el5.src.rpm
 * perl-Convert-ASCII-Armour-1.4-11.el5.src.rpm
 * perl-Convert-PEM-0.06-7.el5.src.rpm
 * perl-Crypt-Blowfish-2.09-4.8.el5.src.rpm
 * perl-Crypt-CBC-2.12-6.el5.src.rpm
 * perl-Crypt-DES-2.03-13.el5.src.rpm
 * perl-Crypt-DES_EDE3-0.01-6.el5.src.rpm
 * perl-Crypt-DSA-0.12-10.el5.src.rpm
 * perl-Crypt-Primes-0.49-11.el5.src.rpm
 * perl-Crypt-Random-1.11-11.el5.src.rpm
 * perl-Crypt-RSA-1.50-7.el5.src.rpm
 * perl-Data-Buffer-0.04-8.el5.src.rpm
 * perl-DateTime-0.27-10.el5.src.rpm
 * perl-DBI-1.54-2.el5s2.src.rpm
 * perl-Devel-Symdump-2.03-21.el5.src.rpm
 * perl-Digest-MD2-2.03-10.el5.src.rpm
 * perl-Error-0.15-7.el5.src.rpm
 * perl-FreezeThaw-0.43-14.el5.src.rpm
 * perl-Frontier-RPC-0.07-7.el5.src.rpm
 * perl-IO-Capture-0.03-8.el5.src.rpm
 * perl-IO-stringy-2.109-11.el5.src.rpm
 * perl-IPC-ShareLite-0.13-1.el5.src.rpm
 * perl-Mail-RFC822-Address-0.3-10.el5.src.rpm
 * perl-MailTools-1.66-6.el5.src.rpm
 * perl-Math-Pari-2.010603-7.el5.src.rpm
 * perl-MIME-Lite-3.01-6.el5.src.rpm
 * perl-Net-SNMP-4.0.3-10.el5.src.rpm
 * perl-Params-Validate-0.74-10.el5.src.rpm
 * perl-Proc-Daemon-0.03-6.el5.src.rpm
 * perl-RPM2-0.68-34.el5.src.rpm
 * perl-RPM-Specfile-1.17-6.el5.src.rpm
 * perl-SOAP-Lite-0.60a-11.el5.src.rpm
 * perl-Sort-Versions-1.4-11.el5.src.rpm
 * perl-TermReadKey-2.30-11.el5.src.rpm
 * perl-Text-Diff-0.35-11.el5.src.rpm
 * perl-Tie-EncryptedHash-1.21-4.4.el5.src.rpm
 * perl-Unix-Syslog-0.100-10.el5.src.rpm
 * perl-XML-RSS-1.05-6.el5.src.rpm
 * perl-XML-Writer-0.4-12.el5.src.rpm
 * relaxngDatatype-1.0-2jpp.ep1.2.el5.2.src.rpm
 * tanukiwrapper-3.2.1-2jpp.ep1.1.el5.src.rpm
 * ws-jaxme-0.5.1-2jpp.ep1.1.el5.1.src.rpm
 * xom-1.0-2jpp.ep1.3.el5.1.src.rpm
 * xpp2-2.1.10-4jpp.ep1.2.el5.1.src.rpm
 * xpp3-1.1.3.4.O-2jpp.ep1.1.el5.1.src.rpm
# SRPMs NOT found in Fedora Proper

 * c3p0-0.9.0-2jpp.ep1.1.el5.src.rpm

 * cglib-2.1.3-2jpp.ep1.3.el5.1.src.rpm
 * cx_Oracle-4.2.1-4.el5.src.rpm
 * freemarker-2.3.6-2jpp_1rh.src.rpm
 * hibernate3-3.2.4-1.SP1_CP01.0jpp.ep1.1.el5.1.src.rpm
 * jabberpy-0.5-0.13.el5.src.rpm
 * jakarta-commons-configuration-1.2-1jpp_1rh.src.rpm
 * jfreechart-0.9.21-2jpp.ep1.1.el5.2.src.rpm
 * jpam-0.4-16.el5.src.rpm
 * MessageQueue-3.26.0-5.el5.src.rpm
 * NPalert-1.125.17-19.el5.src.rpm
 * np-config-2.110.3-6.el5.src.rpm
 * NPusers-1.17.11-6.el5.src.rpm
 * oscache-2.2-2jpp.ep1.1.el5.src.rpm
 * perl-Business-CreditCard-0.28-6.el5.src.rpm
 * perl-Crypt-CAST5_PP-1.02-6.el5.src.rpm
 * perl-Crypt-GeneratePassword-0.03-11.el5.src.rpm
 * perl-Crypt-OpenPGP-1.03-14.el5.src.rpm
 * perl-Crypt-Rijndael-1.05-2.el5.src.rpm
 * perl-Crypt-RIPEMD160-0.04-13.el5.src.rpm
 * perl-DateTime-Locale-0.09-6.el5.src.rpm
 * perl-DateTime-TimeZone-0.59-5.el5.src.rpm
 * perl-DBD-Oracle-1.19-8.el5.src.rpm
 * perl-Filesys-Statvfs-Df-0.72-9.el5.src.rpm
 * perl-GD2-2.17-10.el5.src.rpm
 * perl-Math-FFT-0.26-12.el5.src.rpm
 * perl-NOCpulse-CLAC-1.9.4-10.el5.src.rpm
 * perl-NOCpulse-Debug-1.23.4-6.el5.src.rpm
 * perl-NOCpulse-Gritch-1.16.1-5.el5.src.rpm
 * perl-NOCpulse-Object-1.26.4-7.el5.src.rpm
 * perl-NOCpulse-OracleDB-1.28.2-12.el5.src.rpm
 * perl-NOCpulse-Probe-1.183.1-21.el5.src.rpm
 * perl-NOCpulse-SetID-1.5.2-5.el5.src.rpm
 * perl-NOCpulse-Utils-1.14.2-8.el5.src.rpm
 * perl-Satcon-1.3-9.el5.src.rpm
 * perl-Schedule-Cron-Events-1.8-11.el5.src.rpm
 * perl-Set-Crontab-1.00-12.el5.src.rpm
 * ProgAGoGo-1.11.0-5.el5.src.rpm
 * PyPAM-0.4.2-20.el5.src.rpm
 * python-gzipstream-1.4.0-15.el5.src.rpm
 * python-sgmlop-1.1.1-20040207.15.el5.src.rpm
 * quartz-1.5.2-1jpp.ep1.5.el5.src.rpm
 * redstone-xmlrpc-1.1_20071120-6.el5.src.rpm
 * rhn-java-sat-0.1-3.el5.src.rpm
 * rhn-oracle-jdbc-1.0-13.el5.src.rpm
 * rhns-0.1-3.src.rpm
 * rhn-satellite-admin-5.2.0-3.el5.src.rpm
 * rhn-satellite-config-5.2.0-1.el5.src.rpm
 * rhn-satellite-schema-5.1.0-27.src.rpm
 * rhns-certs-tools-5.2.0-2.el5.src.rpm
 * rhn-web-0.1-3.el5.src.rpm
 * SatConfig-cluster-1.54.2-5.el5.src.rpm
 * servletapi4-4.0.4-3jpp_5rh.src.rpm
 * sitemesh-2.1-1jpp_1rh.src.rpm
 * spacewalk-0.1-7.src.rpm
 * spacewalk-setup-0.01-3.el5.src.rpm
 * velocity-dvsl-0.45-5jpp_1rh.src.rpm
 * velocity-tools-1.2-1jpp_1rh.src.rpm
## The hack script to generate the above list
 


    #!/bin/bash
    
    # This is a hack to do to figure out what packages in the spacewalk repo
    #  are available in Fedora Proper.  Setup needs to be like this:
    #  ./$0 needs to be in the dir with all the srpms.
    #  Remember you can only look up srpms in fedora-pkgdb (I think)
    #
    
    echo "Downloading meta-data from pkgdg"
    wget -q -O /tmp/pkgdb https://admin.fedoraproject.org/pkgdb/acls/bugzilla?tg_format=plain
    grep ^"Fedora|" /tmp/pkgdb | awk -F"|" '{print $2}' > /tmp/foo
    mv -f /tmp/foo /tmp/pkgdb
    
    for rpm in `ls -1 *rpm`
    do
       rrr=`rpm -qp --qf  "%{NAME}\n" $rpm`
       echo -n " * $rpm |"
       grep $rrr /tmp/pkgdb &> /dev/null  &&  echo " In Fedora" || echo " NIF"
    done