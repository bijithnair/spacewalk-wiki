# Spacewalk installation with users & groups defined in LDAP



This page contains useful information in case you are deploying Spacewalk in an environment, where
it is common for user and group information to be pulled from an LDAP server.

Note, that here we mean unix user accounts and groups, that are normally created during package installation (for example:
apache, tomcat, nocpulse, ...). 

Instructions contained here do not describe how to setup your Spacewalk
server to authenticate users accessing Spacewalk web ui and API against a centralized identity service (LDAP, Kerberos). This can be accomplished using [PAM](https://access.redhat.com/knowledge/docs/en-US/Red_Hat_Network_Satellite/5.5/html/Installation_Guide/sect-Installation_Guide-Maintenance-Implementing_PAM_Authentication.html)

Setting up an environment for Spacewalk to play nice with users & groups defined in LDAP consists of two basic steps:

 * Creating user and group records in LDAP

 * Setting up the client -- supposedly the system that Spacewalk is about to be deployed on


----
## Creating user and groups in LDAP



Let's assume the following:

 * you have your LDAP server configured, up and running

 * your database superuser is "admin"

 * base of your organization is "dc=organization,dc=org"

These values were chosen for the simplicity of the whole description, values for your environment may be different.

Spacewalk installation requires the following:

 * users: _apache_, _oracle_, _nocpulse_, _tomcat_, _jabber_

 * groups: _apache_, _oracle_, _tomcat_, _nocpulse_, _dba_, _jabber_

 * all _apache_, _oracle_, _nocpulse_, _tomcat_, _jabber_ need to be created as system accounts (that is uid & gid < 500)

 * _oracle_, _nocpulse_, _tomcat_ have _apache_ as their secondary group

 * _oracle_ has _dba_ as its secondary group

For convenience and more details, see file users-groups.ldif attached to this page. Edit this file to reflect the reality of your environment:

 * edit the _dc_ values (in the example set to "dc=organization,dc=org")

 * edit the _userPassword_ values (in the example set to MD5 encoded string 'password') 

Deploy the ldif file:


    # ldapadd -x -D "cn=admin,dc=organization,dc=org" -W -f users-groups.ldif

----
## Client setup (i.e. the system your Spacewalk will be deployed to)



Make sure you have nss_ldap & authconfig packages installed:


    # yum install nss_ldap authconfig

Run authconfig-tui: 

    # authconfig-tui

and do:

* check "Use LDAP" in User information section

* check "Use LDAP Authentication" in Authentication section

* optionally, you may choose to use nscd to cache retrieved information (User Information section)


          ┌────────────────┤ Authentication Configuration ├─────────────────┐       
          │                                                                 │       
          │  User Information        Authentication                         │       
          │  [ ] Cache Information   [*] Use MD5 Passwords                  │       
          │  [ ] Use Hesiod          [*] Use Shadow Passwords               │       
          │  [*] Use LDAP            [*] Use LDAP Authentication            │       
          │  [ ] Use NIS             [ ] Use Kerberos                       │       
          │  [ ] Use Winbind         [ ] Use SMB Authentication             │       
          │                          [ ] Use Winbind Authentication         │       
          │                          [ ] Local authorization is sufficient  │       
          │                                                                 │       
          │            ┌────────┐                      ┌──────┐             │       
          │            │ Cancel │                      │ Next │             │       
          │            └────────┘                      └──────┘             │       
          │                                                                 │       
          │                                                                 │       
          └─────────────────────────────────────────────────────────────────┘       

Proceed with next configuration window and put in appropriate values for your environment:


                 ┌─────────────────┤ LDAP Settings ├─────────────────┐              
                 │                                                   │              
                 │          [ ] Use TLS                              │              
                 │  Server: ldap://10.34.32.131/____________________ │              
                 │ Base DN: dc=organization,dc=org__________________ │              
                 │                                                   │              
                 │         ┌──────┐                  ┌────┐          │              
                 │         │ Back │                  │ Ok │          │              
                 │         └──────┘                  └────┘          │              
                 │                                                   │              
                 │                                                   │              
                 └───────────────────────────────────────────────────┘              

Now open file /etc/ldap.conf and look for this directive:


    nss_initgroups_ignoreusers

If you see tomcat in the list following it, remove it.

Now you should be able to retrieve user & group information via LDAP:


    # getent passwd oracle
    oracle:x:101:103:oracle:/usr/lib/oracle/xe:/bin/bash
    # getent group apache
    apache:*:48:oracle,nocpulse,tomcat

If your LDAP server & client setup was successful, you may proceed with Spacewalk installation now.


----
## Troubleshooting



Use ldapsearch utility to retrieve information from your LDAP server:


    # ldapsearch -x -b 'dc=organization,dc=org' 'objectclass=*'


If you are running nscd and are getting weird results, try to invalidate nscd cache:


    # nscd -i hosts
    # nscd -i group
    # nscd -i passwd