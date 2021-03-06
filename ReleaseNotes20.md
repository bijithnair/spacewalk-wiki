# __Spacewalk 2.0 Release Notes__



Hello everyone,

We are proudly announcing the release 2.0 of Spacewalk, a systems management solution.

The download locations are

  * http://yum.spacewalkproject.org/2.0/RHEL/5/
  * http://yum.spacewalkproject.org/2.0/RHEL/6/
  * http://yum.spacewalkproject.org/2.0/Fedora/18/
  * http://yum.spacewalkproject.org/2.0/Fedora/19/

with client repositories under

  * http://yum.spacewalkproject.org/2.0-client/RHEL/5/
  * http://yum.spacewalkproject.org/2.0-client/RHEL/6/
  * http://yum.spacewalkproject.org/2.0-client/Fedora/18/
  * http://yum.spacewalkproject.org/2.0-client/Fedora/19/


SUSE Linux client packages can be found here

   * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.0/openSUSE_12.2
   * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.0/openSUSE_12.3
   * http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/2.0/openSUSE_Factory

For fresh installations, please use steps from

  * https://fedorahosted.org/spacewalk/wiki/HowToInstall

If you plan to upgrade from older release, search no more -- the following page will guide you:

  * http://fedorahosted.org/spacewalk/wiki/HowToUpgrade 
## Features & Enhancements in Spacewalk 2.0



  * Spacewalk runs on Fedora 19, support for Fedora 17 has been dropped
  * Auditing feature which enables tracking information like "Who created this user?" or "Who deleted this server?"
      * https://fedorahosted.org/spacewalk/wiki/AuditReporting
  * Managed external PosgreSQL database
      * https://fedorahosted.org/spacewalk/wiki/PostgreSQLServerSetup#pg-standalone
  * Further ABRT enhancements/improvements to make ABRT more functional
      * https://fedorahosted.org/spacewalk/wiki/HowToUseCrashReporting
  * SCAP improvements
      * The latest Spacewalk is able to aggregate full SCAP results, including the XCCDF Result file, OVAL Result Files and OpenSCAP HTML Report. These files are available for user download at the scan's details page.
      * This feature needs to be turned on in an organization's Configuration settings
  * ISS Features
      * Custom-channel-permissions and org-trusts synchronized
      * New WebUI for managing ISS configuration (see Admin\ISS Configuration)
      * https://fedorahosted.org/spacewalk/wiki/ISSSyncPermissions
  * WebUI is now smoother thanks to CSS3 (if you are using IE8 and lower you won't see this)
  * Plenty of small enhancements like overview page for Physical systems only
  * Modified API calls:
      * activationkey.addChildChannels
      * activationkey.setDetails
      * errata.setDetails
      * kickstart.createProfile
      * kickstart.profile.addScript
      * proxy.listAvailableProxyChannels
      * system.listSystemEvents
      * system.scheduleApplyErrata
      * system.schedulePackageInstall
      * system.scheduleHardwareRefresh
      * system.scheduleReboot
      * system.scheduleScriptRun
      * system.scheduleSyncPackagesWithSystem
      * system.crash.getCrashOverview
      * system.crash.listSystemCrashFiles
      * systemgroup.scheduleApplyErrataToActive
  * New API calls:
      * sync.master.addToMaster
      * sync.master.create
      * sync.master.delete
      * sync.master.getDefaultMaster
      * sync.master.getMaster
      * sync.master.getMasterByLabel
      * sync.master.getMasterOrgs
      * sync.master.getMasters
      * sync.master.makeDefault
      * sync.master.mapToLocal
      * sync.master.setCaCert
      * sync.master.setMasterOrgs
      * sync.master.unsetDefaultMaster
      * sync.master.update
      * sync.slave.create
      * sync.slave.delete
      * sync.slave.getAllowedOrgs
      * sync.slave.getSlave
      * sync.slave.getSlaveByName
      * sync.slave.getSlaves
      * sync.slave.setAllowedOrgs
      * sync.slave.update
      * system.crash.createCrashNote
      * system.crash.deleteCrashNote
      * system.crash.getCrashCountInfo
      * system.crash.getCrashNotesForCrash
      * system.crash.getCrashOverview
      * system.crash.getCrashesByUuid


The up-to-date API documentation can be found at http://www.spacewalkproject.org/documentation/api/
## Contributors



Our thanks go to the community members who contributed to this release: 

  * Aron Parsons
  * Avi Miller
  * Baptiste Agasse
  * Bram Mertens
  * Christopher Duryee
  * Dimitar Yordanov
  * Duncan Mac-Vicar P
  * Hubert Mantel
  * James Slagle
  * Jiri Mikulka
  * Johannes Renner
  * John Matthews
  * Lukas Pramuk
  * Marcelo Moreira de Mello
  * Matej Kollar
  * Matt Micene
  * Michael Calmer
  * Miroslav Suchý
  * Neha Rawat
  * Paresh Mutha
  * Pavel Studenik
  * Shannon Hughes
  * Silvio Moioli
  * Simon Lukasik
  * Trent Johnson

Special thanks to Jan Pazdziora.

https://fedorahosted.org/spacewalk/wiki/ContributorList
## Some statistics



In Spacewalk 2.0, we've seen

    * 140 bugs fixed 
    * 921 changesets committed 
    * 1552 commits done 
## User community, reporting issues



To reach the user community with questions and ideas, please use mailing list spacewalk-list@redhat.com. On this list, you can of course also discuss issues you might find when installing or using Spacewalk, but please do not be surprised if we ask you to file a bug at https://bugzilla.redhat.com/enter_bug.cgi?product=Spacewalk with more details or full logs.

Thank you for using Spacewalk.