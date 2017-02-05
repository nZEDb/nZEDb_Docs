.. _MariaDB: https://mariadb.com/kb/en/mariadb-versus-mysql-compatibility/

.. _MySQL: http://dev.mysql.com/doc/refman/5.6/en/features.html

.. _Percona: http://www.percona.com/software/percona-server/feature-comparison
.. sectnum::

**Database**


 .. sectnum::

 **Installing an SQL Database server and client**

  You have multiple choices when it comes to a database server and client. You will need to do some research to figure out which is the best for you. Our current minimum requirement is for a DBMS compatible with MySQL 5.5 (the instructions below will all meet this requirement by default), however newer (stable) implementations are generally considered better and more efficient.

  If you do not understand the choices, or are just not sure which is best for you, we recommend MariaDB_ as the simplest one to set up for the performance it provides.

 .. WARNING:: Only install one of the following three to avoid issues.

 .. NOTE:: We only give configuration instructions for MariaDb below. If you want to use the alternatives you need to have enough experience to configure them yourself.


 * MariaDB_ (recommended):

  .. code:: bash

   sudo apt-get install mariadb-server mariadb-client libmysqlclient-dev

 * MySQL_:

  .. code:: bash

   sudo apt-get install mysql-server mysql-client libmysqlclient-dev

 * Percona_:


   .. sectnum::

   **Add the repo key:**

    .. code:: bash

     sudo apt-key adv --keyserver keys.gnupg.net --recv-keys 1C4CBDCDCD2EFD2A


   .. sectnum::

   **Create a text file to put in the repo source:**

    .. code:: bash

     sudo nano /etc/apt/sources.list.d/percona.list


   .. sectnum::

   **Paste the deb and deb-src lines, replacing VERSION with the name of your ubuntu (16.04: Xenial):**

    .. code:: bash

     deb http://repo.percona.com/apt VERSION main
     deb-src http://repo.percona.com/apt VERSION main

    Exit and save nano (press control+x, then type y and press Enter).


   .. sectnum::

   **Update your packages and install percona.**

   .. code:: bash

     sudo apt-get update
     sudo apt-get install percona-server-server-5.6 percona-server-client-5.6 libmysqlclient-dev


 .. sectnum::

 **[Mandatory] Configuring Your Database Server:**

  Edit your server's configuration file. For MariaDb it is /etc/mysql/mariadb.conf.d/50-server.cnf. There are other configuration files that control other aspects of the database.

  .. code:: bash

   sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf


  .. sectnum::

  **Change or add (they go under the [mysqld] section) the following:**

  * max_allowed_packet = 16M
  * group_concat_max_len = 8192

   Consider raising the key_buffer_size to 256M to start. Later on, you can raise this more as your database grows.


  .. sectnum::

  **Create a MySQL user.**

  .. NOTE:: **Never** use the root user for your scripts.

  Log in to the DBMS with the root user

   .. code:: bash

    sudo mysql -u root -p


  .. sectnum::

  **Add permissions for your user:**

   .. code:: mysql

    GRANT ALL ON nzedb.* TO 'YourMySQLUsername'@'YourMySQLServerHostName' IDENTIFIED BY 'SomePassword';


  .. sectnum::

  **Add the FILE permission to your Db user.**

   The ALL permissions does not actually grant them all, you **must** add FILE as well:

   .. code:: mysql

    GRANT FILE ON *.* TO 'YourMySQLUsername'@'YourMySQLServerHostName';

  Change YourMySQLServerHostName to the hostname of the server. If your database server is local, use localhost. If remote, try the domain name or IP address.

  .. warning:: It has been reported 127.0.0.1 does not work for the hostname.

  Change YourMySQLUsername for the username you will use to connect to the DBMS in nZEDb.  Do not remove the quotes on the name / hostname / password.


  .. sectnum::

  **Exit from the MySQL console:**

  .. code:: mysql

   quit;
