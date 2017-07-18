.. _Apache2: http://httpd.apache.org/
.. _Nginx: http://nginx.org/


.. sectnum::

**Web Server**

 .. sectnum::

 **Installing and configuring a web server:**

  There are many options. We will however, only show you two options: Apache2_ or Nginx_.

  .. warning:: Only install one of them to avoid problems.


  .. sectnum::

  **Apache**

    .. code:: bash

     sudo apt-get install apache2 libapache2-mod-php


    .. sectnum::

    **Create a site configuration file:**

     .. code:: bash

      sudo nano /etc/apache2/sites-available/nZEDb.conf


     .. sectnum::

     **Paste the following into it (changing your paths as required):**

      .. code:: apache

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

      Save and exit nano.


    .. sectnum::

    **Edit apache to allow overrides in the /var/www directory (or the one you will put nZEDb into):**

      .. code:: bash

       sudo nano /etc/apache2/apache2.conf

      Under `<Directory /var/www/>`, change `AllowOverride None` to `AllowOverride All`

      Save and exit nano.


    .. sectnum::

    **Disable the default site, enable nZEDb, enable rewrite, restart apache:**

      .. code:: bash

       sudo a2dissite 00-default
       sudo a2dissite 000-default
       sudo a2ensite nZEDb.conf
       sudo a2enmod rewrite
       sudo systemctl restart apache2


   .. sectnum::

   **Open the Web SAPI php.ini**

    .. code:: bash

      sudo nano /etc/php/7.0/apache2/php.ini

    Change the settings (using the same settings), as described in the CLI SAPI of the PHP section above.


   .. sectnum::

  **Restart the web server**

   .. code:: bash

    sudo systemctl restart apache2



  .. sectnum::

  **Nginx**

   **[Optional]** You might want to remove apache2 first to avoid problems

    .. code:: bash

     sudo apt-get remove apache2


   .. sectnum::

   **Install Nginx_:**

    .. code:: bash

     sudo apt-get install -y nginx


   .. sectnum::

   **Install php fpm, which sends the PHP files to Nginx:**

     .. code:: bash

      sudo apt-get install -y php7.0-fpm


   .. sectnum::

   **Create an nginx configuration file for your nZEDb website:**

     .. code:: bash

       sudo nano /etc/nginx/sites-available/nZEDb


    .. sectnum::

    **Paste the following into the file, change the settings as needed:**

      .. code:: nginx

       server {
           # Change these settings to match your machine.
           listen 80 default_server;
           server_name localhost;

           # These are the log locations, you should not have to change these.
           access_log /var/log/nginx/access.log;
           error_log /var/log/nginx/error.log;

           # This is the root web folder for nZEDb, you shouldn't have to change this.
           root /var/www/nZEDb/www/;
           index index.php index.html index.htm;

           # Everything below this should not be changed unless noted.
           location ~* \.(?:css|eot|gif|gz|ico|inc|jpe?g|js|ogg|oga|ogv|mp4|m4a|mp3|png|svg|ttf|txt|woff|xml)$ {
               expires max;
               add_header Pragma public;
               add_header Cache-Control "public, must-revalidate, proxy-revalidate";
           }

           location / {
               try_files $uri $uri/ @rewrites;
           }

           location ^~ /covers/ {
               # This is where the nZEDb covers folder should be in.
               root /var/www/nZEDb/resources;
           }

           location @rewrites {
               rewrite ^/([^/\.]+)/([^/]+)/([^/]+)/? /index.php?page=$1&id=$2&subpage=$3 last;
               rewrite ^/([^/\.]+)/([^/]+)/?$ /index.php?page=$1&id=$2 last;
               rewrite ^/([^/\.]+)/?$ /index.php?page=$1 last;
           }

           location /admin {
           }

           location /install {
           }

           location ~ \.php$ {
               include /etc/nginx/fastcgi_params;

               # Uncomment the following line and comment the .sock line if you want to use TCP.
               #fastcgi_pass 127.0.0.1:9000;
               fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;

               # The next two lines should go in your fastcgi_params
               fastcgi_index index.php;
               fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
           }
       }

     The server_name must be changed if you want to use a different hostname than localhost.

      Example: "server_name localhost 192.168.1.29 mydomain.com;" would work on all those 3.

     The fastcgi_pass can be changed to TCP by uncommenting it, sockets are faster however.

     Save and exit nano.


     .. sectnum::

     **Change socket to TCP settings**

      If you have changed the fastcgi_pass to tcp (127.0.0.1:9000), you must edit www.conf to listen on it instead of the socket:

       .. code:: bash

        sudo nano /etc/php/7.0/fpm/pool.d/www.conf

      Change `listen = /var/run/php/php7.0-fpm.sock` to `listen = 127.0.0.1:9000`

      Save and exit nano.


   .. sectnum::

   **Open the Web SAPI php.ini**

    .. code:: bash

     sudo nano /etc/php/7.0/fpm/php.ini

    Change the settings (using the same settings), as described in the CLI SAPI of the PHP section above.


    .. sectnum::

    **Create a log folder if it does not exist:**

      .. code:: bash

       sudo mkdir -p /var/log/nginx
       sudo chmod 755 /var/log/nginx


    .. sectnum::

    **Delete the default site:**

      .. code:: bash

       sudo unlink /etc/nginx/sites-enabled/default


    .. sectnum::

    **Make nZEDb the default Nginx site:**

      .. code:: bash

       sudo ln -s /etc/nginx/sites-available/nZEDb /etc/nginx/sites-enabled/nZEDb


    .. sectnum::

    **Restart Nginx and php-fpm:**

      .. code:: bash

       sudo systemctl restart nginx
       sudo systemctl restart php7.0-fpm


 .. sectnum::

 **Add your user to the www-data group**

  Regardless of which web server you use, you should add your user to the www-data group so that you may create/edit files belonging to the group. Replace $USER for your user name, if you will not use the current user for nzedb.

   .. code:: bash

    sudo usermod -a -G www-data $USER

  This requires you to log back in before it takes effect. Do so now or before the Aquiring nZEDb section.

   .. code:: bash

    sudo reboot
