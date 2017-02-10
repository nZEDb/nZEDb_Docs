.. _composerguide:

.. _Composer: https://getcomposer.org/doc/00-intro.md#globally
.. _Contributing: https://github.com/nZEDb/nZEDb/wiki/Contributing-Code-For-Beginners-to-Git(hub)

.. sectnum::

**Install Composer**

Now it is time to install Composer_.  We **strongly** recommend the global method, as we do not provide the composer.phar file in our repo.

.. note:: Commands below assume you followed the getComposer site's instructions and renamed composer.phar to composer.

Do **one** of the following:

  If you will **not** be working on the code (most people), run this command:

 .. code:: bash

  git clone https://github.com/nZEDb/nZEDb.git

 This will download the latest **stable** branch of the project.

**or**

 If you **will** be working on the code, see the wiki page on Contributing_ Code for instructions on forking the repository and cloning locally.

.. sectnum::

**Install dependencies**

 Either way you will need to run:

  .. code:: bash

   composer install --prefer-source

 to have all the 3rd. party libraries installed.