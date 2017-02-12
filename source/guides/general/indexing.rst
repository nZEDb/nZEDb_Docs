.. _indexing:

**Indexing**
++++++++++++

 .. note:: The instructions below assume the default install location and directory name on an Ubuntu install.  This is our **officially** supported platform.  If you are using a non Ubuntu distribution of Linux (or not Linux at all), or if you have deviated from our official instructions, you will need to adjust as appropriate.  If you deviate from our instructions and you have problems and ask for support, we will **assume** that your problems are self inflicted (your fault) because of those deviations unless you can demonstrate differently. This is because the majority of problems we get asked about are due to *not* following our instructions correctly.

 .. warning:: You **should** try the manual method before moving on to more advanced scripts, so that you understand the process.


  .. sectnum::

  **Manual indexing:**

  Go to the "view groups" admin section of the site. (http://AddressOfYourServer/admin/group-list.php)

  Turn on a group or two (alt.binaries.teevee is a good first choice).


   .. sectnum::

   **Move to the indexing script location in your terminal:**

   .. code:: bash

    cd /var/www/nZEDb/misc/update/


   .. sectnum::

   **Download binary headers from usenet:**

   .. code:: bash

    php update_binaries.php


   .. sectnum::

   **When that is complete, create releases and NZB files using those headers:**

   .. code:: bash

    php update_releases.php 1 true


  .. sectnum::

  **Automatic indexing:**


   .. sectnum::

   **Indexing using the screen sequential scripts:**

   First install screen, screen can let you run applications in the background while closing your terminal.

   .. code:: bash

    sudo apt-get install screen
   ..

    .. sectnum::

    **Move to the screen sequential folder:**

    .. code:: bash

     cd /var/www/nZEDb/misc/update/nix/screen/sequential/



    .. sectnum::

    **Run the script using screen:**

    .. code:: bash

     screen sh simple.sh

    Now everything will run automatically.


    .. sectnum::

    **You can detach from the script, allowing you to close your terminal:**


    .. code:: bash

     CTRL+a d  //Press control and a together, let go and press d :


    .. sectnum::

    **If you want to re-attach to screen to see what is going on, type:**


    .. code:: bash

     screen -x


   .. sectnum::

   **Indexing using the Tmux scripts:**

   Install tmux, tmux is similar to screen but allows to have multiple terminals visible and other features.

   .. warning:: Tmux versions 2.1 and 2.2 are known to **not** work with nZEDb, they cause a memory segmentation fault.


    .. code:: bash

     sudo apt-get install tmux time


    .. sectnum::

    **On your website, go to the admin tmux page** (http://AddressOfYourServer/admin/tmux-edit.php)

    Take your time and read through all the options attentively, I will however show the settings I used below.

     Set `Tmux Scripts Running` to `yes`.

     Set `Run Sequential` to `Basic Sequential`.

     Set `Update Binaries` to `Simple Threaded Update`.

     Set `Update Releases` to `Update Releases`.

     Set `Postprocess Additional` to `All`.

     Set `Postprocess Amazon` to `Yes`.

     Set `Postprocess Non-Amazon` to `Properly Renamed Releases`.

     Set `Decrypt Hash Based Release Names` to `All`.

     Set `Update TV and Theater Schedules` to `yes`.

    Click on `Save Tmux Settings` at the bottom of the page.


    .. sectnum::

    **In your terminal window (CLI), change current working directory to the tmux directory.**

    .. code:: bash

     cd /var/www/nZEDb/misc/update/nix/tmux/`


    .. sectnum::

    **Start the tmux script.**

    .. code:: bash

     php start.php`


    .. sectnum::

    **You can now detach from tmux using this keyboard combo**

    .. code:: bash

     control+a d // (press control and a, let go, press d)


    .. sectnum::

    **To re-attach to tmux, type:**

    .. code:: bash

     tmux attach
