
# **DEPRECATED, NO LONGER USED**

# Building Spacewalk virt image



[Greg's original email](https://www.redhat.com/archives/spacewalk-devel/2009-October/msg00003.html)

This is only for F11 users, and primarily for people who want to see/play with the Postgres code as it moves forward.

 1. Go get [thincrust](http://thincrust.org/), which is an appliance builder tool:

    `yum install thincrust`

 2. Go get the kickstart file I built:

    [http://fedorapeople.org/~gdk/SPACEWALK/spacewalk.ks]

 3. (Sadly still necessary) Build a local repo for Oracle, even if you don't intend to run Oracle at all (which, if you're playing with the PostgreSQL code, you won't):

    * Download the Oracle RPMs from Oracle
    * Drop them into a directory
    * From that directory, yum install createrepo
    * Make sure the Oracle repo in the ks file matches

   *NOTE:* Once the Oracle deps are completely dead, this step can be ripped out.

 4. Build your appliance!  It will pull the latest RPMs from the nightly build:

   `appliance-creator -n spacewalk --config spacewalk.ks`

 5. Go have a cup of coffee.  Better yet, run Step 4 via cron in the middle of the night.

 6. Run your new virtualized image!

   `virt-image spacewalk.xml`

Ultimately, I'd like to be able to see a tinderbox running this process to test things like deps issues, install time issues, and so forth.
