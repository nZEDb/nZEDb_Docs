Pre-requisites
--------------

Updating your operating system
++++++++++++++++++++++++++++++

Update all of your sources:

 .. code:: bash

  sudo apt-get update


Upgrade your applications:

 .. code:: bash

  sudo apt-get upgrade


Upgrade some other stuff (like the kernel) **Optional:**

 .. code:: bash

  sudo apt-get dist-upgrade


You must reboot your server now.

 .. code:: bash

  sudo reboot


Installing software dependencies
++++++++++++++++++++++++++++++++

  These programs will be needed later for additional software.  They might already be installed on your operating system.

  .. code:: bash

   sudo apt-get install software-properties-common git

   sudo apt-get install imagick
