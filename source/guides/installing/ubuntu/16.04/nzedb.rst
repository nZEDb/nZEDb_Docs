.. sectnum::

**nZEDb (At Last!)**


 .. sectnum::

 **Acquiring nZEDb**


  .. sectnum::

  **Switch your active group to 'www-data'**

  .. code:: bash

   newgrp www-data


  .. sectnum::

  **Switch to the system's www location.**

  .. code:: bash

   cd /var/www/

  This directory should have been created when your web server was installed. If, for whatever reason, it did not do:

  .. code:: bash

   mkdir -p /var/www
   sudo chown www-data:www-data /var/www
   sudo chmod 775 /var/www



  .. include:: Guides:-composer.rest


  .. sectnum::

  **Set the permissions:**

  .. code:: bash

   sudo chmod -R 755 /var/www/nZEDb/app/libraries
   sudo chmod -R 755 /var/www/nZEDb/libraries
   sudo chmod -R 777 /var/www/nZEDb/resources
   sudo chmod -R 777 /var/www/nZEDb/www

 .. sectnum::

 **Setting up nZEDb:**

  Open up an internet browser, head to `http://AddressOfYourServer/install` changing `AddressOfYourServer` for the IP (or domain name) of your server.

  If this only shows you the web servers default page, you probably need to use a domain name of some kind.

  Open your local hosts file (on the computer which you are trying to browse from) and add an entry for the target server.

  .. code:: bash

   sudo nano /etc/hosts

  Add:

  .. code:: bash

   IpAddressOfYourServer   <server_name_used_in_configuration>

 .. sectnum::

 **Follow the on screen instructions to create the basic configuration of your indexer!**
