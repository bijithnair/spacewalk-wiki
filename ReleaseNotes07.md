Heya Spacewalkers,

Spacewalk 0.7 is finally here!


       http://spacewalk.redhat.com/yum/0.7/RHEL/5/<arch>/
       http://spacewalk.redhat.com/yum/0.7/Fedora/11/<arch>/
       http://spacewalk.redhat.com/yum/0.7/Fedora/12/<arch>/

For the first time we split server and client part. Client repo is:


       http://spacewalk.redhat.com/yum/0.7-client/RHEL/5/<arch>/
       http://spacewalk.redhat.com/yum/0.7-client/Fedora/11/<arch>/
       http://spacewalk.redhat.com/yum/0.7-client/Fedora/12/<arch>/

Make sure to read over the installation again:
   http://fedorahosted.org/spacewalk/wiki/HowToInstall

If you are upgrading from older release, please checkout:
   http://fedorahosted.org/spacewalk/wiki/HowToUpgrade
## Features & Enhancements

* new script "spacewalk-report" allows you to create reports with output to CSV file

    https://fedorahosted.org/spacewalk/wiki/Features/ScriptBasedReporting

* pages with erratas have column with links to CVE description, erratas can be filtered by its type
   https://fedorahosted.org/spacewalk/wiki/Features/WebuiErrataAndCvesEnhancements

* Spacewalk can be installed on Fedora 12

* Top level package spacewalk do not exists any more. It has been split into spacewalk-oracle and spacewalk-postgresql, which depends on oracle or postgresql library.
    Note: postgresql version is highly experimental.

* Spacewalk now tracks date and time of package installation

* Config channels now can handle symlinks.

* Support for WebUI based KVM guest management & provisioning.

* New script "monitoring-data-cleanup" allows you to delete old monitoring data.

* New script "NOCpulse-ini" allows you edit NOCpulse.ini configuration file.

* Base client packages are now in Fedora. That means you are now able to register Fedora machine to Spacewalk, without setting up additional repository.

* Spacewalk repositories have been split to server and client parts.

* Client tools now can report diff in selinux context.

* Satellite-sync improvements.

* API call system.listPackages now return additional field "installtime".

...and of course many bugfixes
## Known issues

* PostgreSQL support still does not work. We will need help with moving this forward.


* Fedora GPG key not recognized by rhnPackageKey table, packages will show up as unknown Provider.

* Documentation search does not work, other search are unaffected.

* The version of rhnsd in this release (rhnsd-4.5.16-1) does not work.
## Community

*  We greatly appreciate the contributions the community has made to this release. Thank you very much.

     - Colin Coe
     - David Nutter
     - Joshua Roys
     - Lukáš Ďurfina

   http://fedorahosted.org/spacewalk/wiki/ContributorList
## Installation Help

   http://fedorahosted.org/spacewalk/wiki/HowToInstall


----
Miroslav Suchy
Red Hat Satellite Engineering