.. _timezones: http://php.net/manual/en/timezones.php


.. sectnum::

**PHP**

 .. sectnum::

 **Installing PHP and its extensions.**

  .. code::

   sudo apt-get install php-pear php7.0 php7.0-cli php7.0-dev php7.0-common php7.0-curl php7.0-json php7.0-gd php7.0-mysql php7.0-mbstring php7.0-mcrypt php7.0-xml


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
