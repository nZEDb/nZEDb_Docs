.. _ubuntu_apache:

Apache 2.4 on Ubuntu
====================

1. Install Apache and required utilities

    ``sudo apt-get install apache2 libapache2-mod-php7.0``
    
    ``apache2 -v``
    
    This should say you're using Apache 2.4.x, if it doesn't something is messed up and outside the scope of this guide.
    
2. Create the configuration for your site

    Paste the following into ``/etc/apache2/sites-available/nZEDb.conf`` using your favorite editor and change paths as necessary::
    
        <VirtualHost *:80>
            ServerAdmin webmaster@localhost
            ServerName localhost
            DocumentRoot "/var/www/nZEDb/www"
            LogLevel warn
            ServerSignature Off
            ErrorLog /var/log/apache2/error.log
            <Directory "/var/www/nZEDb/www">
                Options FollowSymLinks
                AllowOverride All
                Require all granted
            </Directory>
            Alias /covers /var/www/nZEDb/resources/covers
        </VirtualHost>
    
3. Change some settings to make things work

    Overrides need to be allowed in ``/var/www``, change the following lines using your favorite editor::
    
        <Directory /var/www/>
            Options Indexes FollowSymLinks
            AllowOverride none
            Require all granted
        </Directory>
    
    to::
    
        <Directory /var/www/>
            Options Indexes FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>
    
4. Disable the default site, enable your site and the rewrite module

    ``sudo a2dissite 000-default``
    
    ``sudo a2ensite nZEDb.conf``
    
    ``sudo a2enmod rewrite``
    
    ``sudo service apache2 restart``
    
5. Verify everything
    
    At this point, if you fire up your browser and point it at your nZEDb, you should get a 404 not found. That's ok, and expected. Anything else and you've done something wrong. Go back and double check. :ref:`Help <help>` is available.
    
6. :ref:`Return whence you came <ubuntu>`