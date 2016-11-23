.. _ubuntu:

Ubuntu
======

***This guide is designed for Ubuntu 16.04 LTS Server. Using it for other versions of Ubuntu, Debian, or other Linux distributions may or may not work.***

This guide is assuming you're starting with a completely minimal Ubuntu installation. It can be accomplished via local terminal, or over ssh. It is also assuming you're not using the root account. If you're not starting with a fresh minimal install, you might run into problems. Google and the :ref:`help page <help>` might be able to offer guidance.


1. Ensure you're using the most up to date packages:
----------------------------------------------------
    
    ``sudo apt-get update``
    
    ``sudo apt-get upgrade``
    
    ``sudo apt-get dist-upgrade``
    
    ``sudo reboot``


2. Ensure some essential system utilities are installed
-------------------------------------------------------

   ``sudo apt-get install software-properties-common python-software-properties git``


3. Install PHP
--------------

    PHP 7 is used as it is the default::
    
        sudo apt-get install php-pear php7.0 php7.0-cli php7.0-dev php7.0-common \
        php7.0-curl php7.0-json php7.0-gd php7.0-mysql php7.0-mbstring php7.0-mcrypt php7.0-xml


4. Install MySQL
----------------
   
   There are a few flavors of MySQL you can install. They're all drop-in ready. That said, we'll show you how to install them all, and let you decide. 
    
    Do not leave your MySQL root account's password empty_.
    
    Oracle MySQL
    
        ``sudo apt-get install mysql-server mysql-client libmysqlclient-dev``
        
    MariaDB
    
        ``sudo apt-get install mariadb-server mariadb-client libmysqlclient-dev``
        
    Percona
        
        ``sudo apt-get install percona-server-server-5.6``


5. Decide on what to do with Apparmor
-------------------------------------
    
    Apparmor_ causes issues with loading data into MySQL. You have two options, and we're going to assume you choose to configure it vice remove it for the rest of this guide.
    
        A. Completely remove/disable it
        
            ``sudo apt-get purge apparmor``
            
            ``sudo update-rc.d apparmor disable``
    
    
        B. Configure it to ignore MySQL
        
            ``sudo apt-get install apparmor-utils``
            
            ``sudo aa-complain /usr/sbin/mysqld``
    
    
6. Configure MySQL
------------------

    A. Add/change settings in my.cnf. They all need to be in the [mysqld] section.
    
    Locations for this vary depending on which MySQL flavor you installed.
    
        ``innodb_file_per_table = 1`` - This will not affect you unless you switch to InnoDB later during :ref:`tuning`.
    
        ``max_allowed_packet = 16M`` - You might consider raising this as your database grows.
    
        ``group_concat_max_len = 8192``
    
    
    B. Create MySQL user
    
    You will want to substitute in your database name, username, hostname, and password you want to use. **Ensure you leave the single quotes in place**.
    
        ``GRANT ALL ON SomeDatabaseName.* TO 'SomeUserName'@'SomeHostName' IDENTIFIED BY 'SomePassword';``
        
        i.e.
        
        ``GRANT ALL ON nzedb.* TO 'nzedb'@'localhost' IDENTIFIED BY '5fgGYS$';``
    
    
    C. Grant extra user permissions
    
    This adds needed permissions to the account you just created.
    
        ``GRANT FILE ON *.* TO 'SomeUserName'@'SomeHostName';``
        
        i.e.
    
        ``GRANT FILE ON *.* TO 'nzedb'@'localhost';``
    
    
7. Install and configure the webserver
--------------------------------------

    You have numerous options here. It's far beyond the scope of this guide to cover them all.
    
        Apache - :ref:`Install and configure <ubuntu_apache>`
    
        nginx - :ref:`Install and configure <ubuntu_nginx>`
    
    
8. Fix user permissions for later
---------------------------------

    You need to add a user to the ``www-data`` group so you have access to put nZEDb where it needs to go. Once the following command is complete, you'll need to log out and back in prior to continuing. If you're not using the account you plan to use later, change ``$USER`` to the username.
    
    ``sudo usermod -a -G www-data $USER``
    
    
9. Configure the PHP settings
-----------------------------
    
    A. Configure the CLI
        
        Open ``/etc/php/7.0/cli/php.ini`` in your favorite editor and change the following settings
    
            ``max_execution_time = 120``
        
            ``memory_limit = 1024M`` - This can be change to -1 if you've got >8GB ram, or less if you have less ram.
        
            ``date.timezone = YourLocalTimezone`` - Change to your local timezone_ and make sure to uncomment it.
        
            These next ones are needed for filing bug reports. You don't have to enable them. If you don't, helping you if problems come up is going to be near impossible. We recommend enabling them. You're call.
        
            ``error_reporting = E_ALL``
        
            ``log_errors = On``
        
            ``error_log = php-errors.log``
        
        
    B. Configure the SAPI
        
        Open one of the following and change the settings as per step A above.
        
            Apache - ``/etc/php/7.0/apache2/php.ini``
            
            nginx - ``/etc/php/7.0/fpm/php.ini``
            
            
    C. Reload the php.ini changes
    
        Use the one that applies to you.
    
            Apache - ``sudo service apache2 restart``
        
            nginx - ``sudo service php7.0-fpm restart``
    
    
10. Install more system extras.
-------------------------------

    Be aware that that the versions in the Ubuntu repository aren't always the most up to date. You should, in theory, be able to use other versions, if you can get them installed. Doing so is outside the scope of this guide however. These tools are not required, however you will loose certain :ref:`features <ubuntu_extra_features>`.
    
    
    A. Media extras

        ``sudo apt-get install unrar p7zip-full mediainfo lame ffmpeg libav-tools``

    B. yEnc - speeds up header and message processing during indexing::
    
        cd ~
        mkdir yenc
        cd yenc
        wget http://heanet.dl.sourceforge.net/project/yydecode/yydecode/0.2.10/yydecode-0.2.10.tar.gz
        tar xzf yydecode-0.2.10.tar.gz
        cd yydecode-0.2.10
        ./configure
        make
        sudo make install
        cd ../..
        rm -rf ~/yenc

    C. php-yenc extension - even faster header and message processing::
    
        https://github.com/niel/php-yenc/releases
        
      Make sure you download the php7.0 version of the package.

11. Obtain nZEDb
----------------

    A. To obtain nZEDb we need to obtain composer_, a PHP dependency manager. You can install it locally or globally. We recommend globally. This guide details a global install::
    
        php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
        php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
        sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
        php -r "unlink('composer-setup.php');"
        composer -V


    B. You should now have composer installed and it should be displaying which version. If not, something went wrong. :ref:`help` is available.


    C. Grab a fresh copy of nZEDb and set the correct permissions.
    
        **You can install the development branch by changing ``--no-dev`` to ``--stability dev``** ::
    
            cd /var
            sudo chown www-data:www-data -R www
            cd www
            newgrp www-data
            composer create-project --no-dev --keep-vcs --prefer-source nzedb/nzedb nZEDb
            sudo chmod -R 755 /var/www/nZEDb/app/libraries
            sudo chmod -R 755 /var/www/nZEDb/libraries
            sudo chmod -R 777 /var/www/nZEDb/resources
            sudo chmod -R 777 /var/www/nZEDb/www
            sudo chown -R www-data:www-data /var/lib/php/sessions/


12. Configure nZEDb
-------------------

    Open a web browser and head to http://<host/ip of your machine>/install
    
    Follow the instructions on the wizard, and everything should go smoothly.
    
    Once the wizard is complete, more detailed setup instructions are located :ref:`here <config_nzedb>`.


13. Set up indexing
-------------------
    
    You can manually index, or set it up to be done automatically. Both are detailed :ref:`here <indexing>`. Once you've got indexing setup, you have a fully functioning nZEDb installation.


14. Optional extras
-------------------

    There are a few extras that can be setup to make your nZEDb installation more useable.
    
        - :ref:`PreDB <predb>`: PreDB is a database that contains information about releases.
    
        - :ref:`IRCScaper <ircscraper>`: This is a IRC bot that will populate your PreDB. This greatly improves renaming of releases (i.e. you'll want this one)

        - :ref:`Comment sharing <comment_share>`: It is possible to share release comments between all nZEDb installations.
        
.. _empty: https://technet.microsoft.com/en-us/library/cc722488
.. _Apparmor: https://en.wikipedia.org/wiki/AppArmor
.. _timezone: http://php.net/manual/en/timezones.php
.. _composer: https://getcomposer.org/doc/00-intro.md#downloading-the-composer-executable
