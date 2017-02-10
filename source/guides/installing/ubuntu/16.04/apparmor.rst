.. _Apparmor: http://en.wikipedia.org/wiki/AppArmor

.. _tutorial: http://www.cyberciti.biz/faq/ubuntu-linux-howto-disable-apparmor-commands/



Apparmor [Mandatory]
--------------------

 Apparmor_ restricts certain programs. For nZEDb, it stops us from using the SQL's LOAD DATA commands.  You can read more on Apparmor_.


 **You have two options:**



**Disabling Apparmor**

 .. code:: bash

  sudo update-rc.d apparmor disable
  sudo apt-get purge apparmor


**Making Apparmor ignore MySQL**

 .. code:: bash

  sudo apt-get install apparmor-utils
  sudo aa-complain /usr/sbin/mysqld

 If this does not work, try this AppArmor tutorial_.


 **You MUST reboot your server after doing this!**

 .. code:: bash

  sudo reboot
