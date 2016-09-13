.. _predb:

PreDB
=====

FAQ
---

PreDB is a database that contains information about releases. Utilizing PreDB will assist nZEDb in renaming releases and picking up valid releases. It will also allow nZEDb to mark bad releases. Using a preDB will use up disk/database space however, so if you plan on utilizing it, be aware.

The majority of this guide comes from the hard work of forum user blindpet_

Setting up PreDB
----------------

**The initial setup of your preDB can take some time on lower end hardware. It is advisable to stop all indexing scripts while setting up preDB**

**Steps 1 and 2 are optional. They import preDB information that predates the daily nZEDb preDB dumps (started in 2014) that are imported in step 3 and 4.**

1. Download the initial preDB dump. 

    ``cd /tmp``

    ``wget https://www.dropbox.com/s/qkmgbvmdv9a5w8q/predb_dump_08172015.tar.gz``

    ``gunzip predb_dump_08172015.tar.gz``
    
2. Import initial preDB dump
    
    ``cd /var/www/nZEDb/misc/testing/PreDB``
    
    ``php dump_predb.php local /tmp/predb_dump_08172015.tar``
    
3. Import daily dumps from nZEDb since 2014.

    ``cd /var/www/nZEDb/cli``
    
    ``php data/predb_import_daily_batch.php 0 local true``
    
4. You have two options now. You can re-run step 3 daily or setup the :ref:`IRCScraper <ircscraper>` to fill the preDB.

.. _blindpet: http://forums.nzedb.com/index.php?topic=1860.0