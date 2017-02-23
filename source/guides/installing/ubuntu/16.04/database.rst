.. _comparison: https://mariadb.com/kb/en/mariadb-versus-mysql-compatibility/

.. _settings: https://mariadb.com/kb/en/mariadb/system-variable-differences-between-mariadb-100-and-mysql-56/

Database
--------

**Installing an SQL Database server and client**

  You have multiple choices when it comes to a database server and client. You will need to do some research to figure out which is the best for you. The developers strongly recommend MariaDb 10.0 or later for all users. It has all the features of MySQL, plus additional stability and functionality (generally faster). MySQL has changed its default settings_ for a number of features/modes which cause issues with nZEDb, so we will no longer be supporting it directly.

  If you must use MySQL, you are expected to know what you are doing and maintain it yourself. Check the comparison_ docs.

 .. NOTE:: We only give configuration instructions for MariaDb below. If you want to use the alternatives you need to have enough experience to configure them yourself.


Install MariaDB
+++++++++++++++

 .. code:: bash

  sudo apt-get install mariadb-server mariadb-client libmysqlclient-dev


**Configuring Your Database Server:**

 Edit your server's configuration file. For MariaDb it is /etc/mysql/mariadb.conf.d/50-server.cnf. There are other configuration files that control other aspects of the database.

 .. code:: bash

  sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf


**Change or add (they go under the [mysqld] section) the following:**

 * max_allowed_packet = 16M
 * group_concat_max_len = 8192

Consider raising the key_buffer_size to 256M to start. Later on, you can raise this more as your database grows.


**Create a MySQL user.**

 Log in to the DBMS with the root user, to set permissions for your user.

  .. NOTE:: **Never** use the root user for your scripts.

 .. code:: bash

  sudo mysql -u root -p


**Add permissions for your user:**

 .. code:: mysql

  GRANT ALL ON nzedb.* TO 'YourMySQLUsername'@'YourMySQLServerHostName' IDENTIFIED BY 'SomePassword';


**Add the FILE permission to your Db user.**

The ALL permissions does not actually grant them all, you **must** add FILE as well:

 .. code:: mysql

  GRANT FILE ON *.* TO 'YourMySQLUsername'@'YourMySQLServerHostName';

Change YourMySQLServerHostName to the hostname of the server. If your database server is local, use localhost. If remote, try the domain name or IP address.

 .. warning:: It has been reported 127.0.0.1 does not work for the hostname.

Change YourMySQLUsername for the username you will use to connect to the DBMS in nZEDb.  Do not remove the quotes around the name / hostname / password.


**Exit from the MySQL console:**

 .. code:: mysql

  quit;
