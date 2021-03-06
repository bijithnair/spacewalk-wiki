
For up-to-date information about the PostgreSQL port, see [[PostgreSQL]]. This page is now obsolete.

----
# Postgresql Quick Start Guide



    #!html
    <div style="background-color:#2b299e; color:white; background-image:url('/spacewalk/attachment/wiki/WikiSnippetsAndTemplates/workinprogress.png?format=raw'); background-repeat:no-repeat; padding-left:7em;background-position:1em;" >
       <h2>Work in Progress</h2>
       <div style="padding-bottom:0.5em;" >
           <p>This is page in currently in a draft state.</p>
       </div>
    </div>

Want to help fix Spacewalk database queries so that they work in Postgres?  Getting started is easy.
## I. Getting everything installed



 * Install git, if you don't have it installed: (may require EPEL repositories for CentOS/RHEL 5)
  
    yum install git
      }}}
     * Clone spacewalk from git
      {{{
    git clone git://git.fedorahosted.org/git/spacewalk.git/
      }}}
     * Install postgres, if you haven't already.  You will need *8.4 beta* because the schema has not been back-ported to earlier versions. 
      The easiest way to in stall this version is using this public yum repo [http://yum.pgsqlrpms.org/].
      {{{
    yum install postgresql-server
      }}}
     * Install the python lex/yacc parser and PostgreSQL libraries:
      {{{
    yum install python-ply python-pgsql
      }}}
     * Install _chameleon_, the tool that builds both Oracle and Postgres schema from a single meta-schema
      {{{
    yum install chameleon
      }}}
      Or,
      {{{
    rpm -Uvh https://fedorahosted.org/releases/c/h/chameleon/chameleon-0.2-1.fc10.noarch.rpm?format=raw
      }}}
      * Formerly: dogchow 
     * Get the right branch, where the postgres porting work is going on
      {{{
    cd spacewalk
    git checkout origin/pgsql
      }}}
     * Configure PostgreSQL:
      * As root:
      {{{
    /sbin/service postgresql initdb
    # Setup PostgreSQL authentication
    # TODO: Probably need some careful thought here in the future:
    perl -p -i -e 's/ident sameuser/trust/g' /var/lib/pgsql/data/pg_hba.conf
    perl -p -i -e 's/ident/trust/g' /var/lib/pgsql/data/pg_hba.conf
    /sbin/service postgresql start
    createuser --user postgres spacewalk --superuser
    createdb --user spacewalk spacewalk && createlang --user spacewalk plpgsql spacewalk
    psql --user spacewalk spacewalk # to verify that the spacewalk db has been successfully created
      }}}
     * Build schema! We hope to improve this and make it simpler soon.
      {{{
    cd spacewalk/schema/spacewalk
    ../../rel-eng/bin/tito build --rpm --test
    
    OR
    
    cd schema/spacewalk/postgres
    make
    psql -U postgres -f main.sql
      }}}
      * Look at _chameleon_ go, creating both Oracle and Postgres schema on the fly!  Warnings are fine.
      * Also runs _blend_, which does depsolving and creates a properly ordered set of create scripts.
      * Examine output for the location of the noarch RPM that was built and run rpm -Uvh RPM.
     * Deploy PostgreSQL specific modifications not yet packaged:
      * Because we cannot properly tag and build rpms while working in a git branch, we've setup a short shell script that scp's all modified files into their correct locations.
      {{{
    SWHOST=root@localhost scripts/pgsql-deploy.sh el5
      }}}
      * Use "f10" instead of el5 if deploying to Fedora 10.
      * NOTE: Using scp, technically not required with these instructions as we're running the script on the host.
      * The script is setup for Fedora 10. If you need to install on EL5 then you'll need to modify some simple perl directories in the script.
     * Setup an answers file for PostgreSQL:
      {{{
    cat > ~/answers-postgresql.txt <<'EOF'
    admin-email = me@example.com
    db-backend = postgresql
    db-user = spacewalk
    db-host = localhost
    db-password = spacewalk
    db-sid = spacewalk
    db-port = 5432
    db-protocol = TCP
    ssl-set-org = My Org
    ssl-set-common-name = My Org Test
    ssl-set-org-unit = BLAH
    ssl-set-city = Raleigh
    ssl-set-state = NC
    ssl-set-country = US
    ssl-password = wer tyu
    ssl-set-email = me@example.com
    
    EOF
 * Install!
  {{{
spacewalk-setup --disconnected --answer-file=~/answers-postgresql.txt
  }}}
  * _THIS WILL FAIL_, but it goes a long way before it does. :) Schema will be populated.
  * If you want to try again, you must destroy your spacewalk database, we cannot "clear" yet.
  {{{
dropdb --user spacewalk spacewalk && createdb --user spacewalk spacewalk && createlang --user spacewalk plpgsql spacewalk
  }}}
## II. Fixing a query, an example



Your process may be different, but this is my process for porting a query, step-by-step.

 * Go find a file with some queries in it.  There's tons.  :)  At some point, we will have a wiki page that will allow contributors to claim a file.  For now, I'm picking a file at random: `spacewalk/backend/server/rhnAction.py`.  A good way to find queries in the Python codebase, for instance: `grep -r rhnSQL .`.
 * Hmm, looks like `grep -r "#pgport" .` does a better job.  Why are these tags invalid, again?  Is there any way to rescue any of this work?