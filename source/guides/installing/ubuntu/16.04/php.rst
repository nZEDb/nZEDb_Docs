.. _timezones: http://php.net/manual/en/timezones.php


.. sectnum::

**PHP**

 .. sectnum::

 **Installing PHP and its extensions.**

  .. code::

   sudo apt-get install php-pear php7.0 php7.0-cli php7.0-dev php7.0-common php7.0-curl php7.0-json php7.0-gd php7.0-mysql php7.0-mbstring php7.0-mcrypt php7.0-xml

  .. NOTE:: In the command above, we refer to php packages as php7.0-package.  These are the versions that Ubuntu 16.04 repositories contain (it also contains the same packages as 'php-package').

   Normally you will not need to change this, but there might be circumstances where you must use v5.6 or you need both v5.6 and v7.0 to coexist.  If this is the case for you, we recommend you use Ondřej Surý's PPA.

   Ondřej is the maintainer of the PHP package for Debian, the linux distribution from which Ubuntu is created, so you are unlikely to find a better place to get multiple versions of PHP.

   To add the PPA do this:

    .. code:: bash

     sudo add-apt-repository ppa:ondrej/php

     sudo apt-get update
sudo 
   Then follow the normal instructions for installing PHP as above, but using either php5.6-package or php7.0-package as appropriate for your desired version. If you are intending to install both, then you will have to add both versions of the packages.


 .. sectnum::

 **Configuring PHP:**


  .. sectnum::

  **Open php.ini for the CLI SAPI:**

    .. code:: bash

     sudo nano /etc/php/7.0/cli/php.ini


   .. sectnum::

   **Change the following settings:**

    .. code::

     max_execution_time = 120
     memory_limit = 1024M
     date.timezone = `YourLocalTimezone`

    `memory_limit` can be set to -1 if you have a large amount of system RAM (>=8GB).

    Change your timezone from a list of timezones_.


   .. sectnum::

   **Enable error logging** (Needed when reporting bugs)

    .. code::

     error_reporting = E_ALL
     log_errors = On
     error_log = php-errors.log

    Save and close this file.
