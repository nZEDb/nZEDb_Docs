.. _ubuntu_nginx:

nginx on Ubuntu
===============

1. Install nginx

    A. Use Ubuntu's version (not always up-to-date)
    
        ``sudo apt-get install nginx``

    B. Use nginx's up-to-date version
    
        ``sudo add-apt-repository ppa:nginx/stable``
        
        ``sudo apt-get update``
        
        ``sudo apt-get install nginx``
        
2. Verify PHP-FPM installed

    ``sudo apt-get install php7.0-fpm``
    
3. Create the config for your installation

    Paste the following into ``/etc/nginx/sites-available/nZEDb`` using your favorite editor::

        server {
            # Change these settings to match your machine.
            listen 80 default_server;
            server_name localhost;

            # These are the log locations, you should not have to change these.
            access_log /var/log/nginx/access.log;
            error_log /var/log/nginx/error.log;

            # This is the root web folder for nZEDb, you shouldn't have to change this.
            root /var/www/nZEDb/www/;
            index index.html index.htm index.php;

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
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;

                # The next two lines should go in your fastcgi_params
                fastcgi_index index.php;
            }
        }

4. Enable your site

    A. Disable the default site
        
        ``sudo unlink /etc/nginx/sites-enabled/default``
        
    B. Enable your nZEDb site
    
        ``sudo ln -s /etc/nginx/sites-available/nZEDb /etc/nginx/sites-enabled/nZEDb``
        
    C. Restart nginx and PHP
    
        ``sudo service nginx configtest`` - This will let you know if you messed up on step 3, if you didn't carry on
        
        ``sudo service nginx reload``
        
        ``sudo service php7.0-fpm restart``
        

5. Changes to fastcgi_params are needed. Using your favorite editor, ensure ``/etc/nginx/fastcgi_params`` looks like below::

    fastcgi_param  QUERY_STRING       $query_string;
    fastcgi_param  REQUEST_METHOD     $request_method;
    fastcgi_param  CONTENT_TYPE       $content_type;
    fastcgi_param  CONTENT_LENGTH     $content_length;

    fastcgi_param  SCRIPT_FILENAME    $request_filename;
    fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
    fastcgi_param  REQUEST_URI        $request_uri;
    fastcgi_param  DOCUMENT_URI       $document_uri;
    fastcgi_param  DOCUMENT_ROOT      $document_root;
    fastcgi_param  SERVER_PROTOCOL    $server_protocol;
    fastcgi_param  REQUEST_SCHEME     $scheme;
    fastcgi_param  HTTPS              $https if_not_empty;

    fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
    fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

    fastcgi_param  REMOTE_ADDR        $remote_addr;
    fastcgi_param  REMOTE_PORT        $remote_port;
    fastcgi_param  SERVER_ADDR        $server_addr;
    fastcgi_param  SERVER_PORT        $server_port;
    fastcgi_param  SERVER_NAME        $server_name;

    # PHP only, required if PHP was built with --enable-force-cgi-redirect
    # fastcgi_param  REDIRECT_STATUS    200;

6. Verify everything
    
    At this point, if you fire up your browser and point it at your nZEDb, you should get a 404 not found. That's ok, and expected. Anything else and you've done something wrong. Go back and double check. :ref:`Help <help>` is available.

7. :ref:`Return whence you came <ubuntu>`