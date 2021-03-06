# Jabber Database



Spacewalk utilizes Jabber to facilitate communications between the server and the clients for osa-dispatcher/osad. The Jabber program uses the Berkeley DB to store active transactions and these transaction log files can get out of control very fast and take up a lot of disk space. Each log file is 10MB in size and there's no built-in mechanism to clean them up automatically. 


    # ls -l /var/lib/jabberd/db/ | grep log
    
    -rw-r-----. 1 jabber jabber 10485760 Sep 10 08:38 log.0000000001
    -rw-r-----. 1 jabber jabber 10485760 Sep 10 08:43 log.0000000002
    -rw-r-----. 1 jabber jabber 10485760 Sep 10 08:48 log.0000000003
## Setting a checkpoint and cleaning up log files



WARNING! Using this method for a long period of time seems to cause Jabber to become overwhelmed. A SYN flood can occur that apparently requires the DB files be wiped out anyway. 

 * [SYN Flooding with OSAD](https://www.redhat.com/archives/spacewalk-list/2014-April/msg00030.html)

In order to clean up the Berkeley DB log files, you must draw a line in the sand and then use a command to remove the unnecessary log files. The following commands can likely be used on any modern version of Spacewalk, e.g. Spacewalk 2.0, 2.1, 2.2. and 2.3 - however they have only been tested on Spacewalk 2.3. 

First, ensure you have the db4-utils package installed and install it if you don't. Second, note that the db_checkpoint command must be used first in order for the db_archive command to work. Also note that on CentOS/RHEL 7, the following commands are db4_checkpoint and db4_archive; if you don't use the right command you'll end up with an error like "BDB1538 Program version 5.3 doesn't match environment version 4.8"

As root execute the following command to set a checkpoint within Berkeley DB, which will flush all of the active transactions into the DB:


    # sudo -u jabber db_checkpoint -1 -h /var/lib/jabberd/db/

Then, (if you're interested or to verify) run the db_archive command to view which files aren't needed:


    # db_archive -a -h /var/lib/jabberd/db/

Now, you can actually remove the log files that are no longer in use:


    # db_archive -d -h /var/lib/jabberd/db/

For good measure, restart jabberd and you should be all set. 


    # /etc/init.d/jabberd restart
## A warning about removing all Jabber DB files



While it is possible to clean up these log files by nuking all files under the /var/lib/jabberd/db/ directory and restarting jabberd, I have found that this actually interrupts the communication between the server and osad on the clients. From what I can tell, nuking all of the jabber db files and restarting jabber will force clients to have to re-establish communication with the Spacewalk server. Most of the time, this requires removing the osad-auth.conf file on the client and restarting osad. Using the method in the section above, you can cleanly perform maintenance on Jabber's Berkeley DB without having to do anything on the clients. 
## External References



 * [Berkeley DB Command Line Utilities - db_checkpoint](http://docs.oracle.com/cd/E17275_01/html/api_reference/C/db_checkpoint.html)
 * [Berkeley DB Command Line Utilities - db_archive](https://docs.oracle.com/cd/E17275_01/html/api_reference/C/db_archive.html)
 * [jabberd2](http://jabberd2.org/)
 * [Berkeley DB](https://en.wikipedia.org/wiki/Berkeley_DB)