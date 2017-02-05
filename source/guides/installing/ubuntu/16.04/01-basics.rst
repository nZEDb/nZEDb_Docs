.. sectnum::

**Pre-requisites**


 .. sectnum::

 **Updating your operating system**


  .. sectnum::

  Update all of your sources:

   .. code:: bash

    sudo apt-get update


  .. sectnum::

  Upgrade your applications:

   .. code:: bash

    sudo apt-get upgrade


  .. sectnum::

  **Optional:** Upgrade some other stuff (like the kernel).

   .. code:: bash

    sudo apt-get dist-upgrade


  .. sectnum::

  You must reboot your server now.

   .. code:: bash

    sudo reboot


 .. sectnum::


 **Installing software dependencies**

  These programs will be used later to install additional software.  They might already be installed on your operating system.

  .. code:: bash

   sudo apt-get install software-properties-common git
