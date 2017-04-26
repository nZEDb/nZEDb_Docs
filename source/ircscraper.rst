.. _ircscraper:

Setting up the IRCScraper
=========================

Prior to setting up the IRCScraper, you may want to import previous preDB dumps. It's not necessary, but will help matching/renaming releases. Follow the :ref:`guide <predb>` to do this.

A majority of this guide was compiled by forum user hernando_

1. We need to add a PPA so we can install a better version of ZNC.

    ``sudo add-apt-repository ppa:teward/znc && sudo apt-get update``
    
    *If for some reason you get a command not found error, you don't have the pre-req install, run:*
    
        ``sudo apt-get install python-software-properties``
        
2. Install ZNC and the extras

    ``sudo apt-get install znc znc-dbg znc-dev znc-perl znc-python znc-tcl``
    
3. Run the configuration wizard
    
    ``znc --makeconf``
    
    Answer the wizard with the following answers, change to suit your needs.
    
    *We use SSL connections to the synIRC servers. If you run into issues with them, you can turn them off later.*
    
    *To avoid confusion, it is advisable to set both your username and nick as the same.* ::
    
        Listen on Port: **6664, you can change this if you want**
        Listen using SSL: **yes, but up to you (this is SSL for the scraper and ZNC, not to IRC**
        Listen on IPv4 and IPv6: **no, but up to you**
        Username: **Make up a unique one, will be used to connect to ZNC**
        Password: **Make up a unique one, will be used to connect to ZNC**
        Nick: **Make up a unique one, will be used to connect to IRC**
        Alt nick: **Make up a unique one, will be used to connect to IRC**
        Ident: **Leave as default**
        Real Name: **Leave as default**
        Bind host: **Leave as default**
        
        Setup Network: **yes**
        Name: **synIRC**
        Server host: irc.synirc.net
        Server SSL: **yes**
        Server port: **6697**
        Server password: **leave empty**
        Initial channels: **#nZEDbPRE**
        
        Launch ZNC: **yes**
        
4. Change some settings in ZNC

    Point your web browser to the IP and port of your ZNC machine, i.e. https://192.168.1.2:6664, and login.
    
    Under **Global Settings**, change **Max Buffer Size** to 1000 and click Save.
    
    Under **Your Settings -> Networks**, click Edit next to syncIRC.
    
        Paste the following into the textbox for Trusted SSL fingerprints::
        
            0b:35:ba:24:e7:1c:f6:9e:1f:82:1d:9a:4e:0d:9f:70:8e:91:74:26:57:13:9e:f9:c7:8e:9c:5c:a6:8e:30:62
            23:2d:7d:fd:79:09:d1:20:ad:6a:88:f1:fc:49:b5:34:cc:00:2a:7f:95:10:07:e7:b7:d7:90:af:7d:eb:7f:07
            54:86:50:b5:7e:08:cb:b4:95:d8:54:9e:fb:8d:f3:6b:97:8a:b7:25:95:d6:3e:38:4c:fb:42:b0:4e:2a:d8:de
            67:7b:f5:25:0d:c0:2d:06:b8:57:2a:ef:9f:5c:2f:c9:48:e9:17:f0:43:22:2e:67:0a:56:ca:f8:ee:98:79:71
            72:26:00:f7:f0:7f:1d:13:a6:20:88:73:ba:42:6c:8b:5e:ef:fd:04:b3:98:90:f7:23:63:bf:08:46:6d:2e:41
            8e:17:a3:cd:ea:5c:55:ff:06:14:91:23:3b:d8:26:d5:b0:d5:8f:69:88:5a:b7:60:dd:73:01:54:d0:b2:18:65
            b3:75:8a:5a:a7:ed:5e:ef:22:45:d4:07:bd:06:32:1e:b8:92:07:49:72:cc:7e:7a:63:fb:3f:e1:92:c3:9a:5a
            d9:10:d9:8a:96:0d:f2:89:d1:a0:87:d0:26:42:b8:51:f9:1d:72:fc:fb:ee:d4:32:14:e8:0c:0d:f3:7a:fa:63
            ef:da:d9:13:be:ad:97:aa:64:65:42:ed:77:24:89:65:97:44:81:da:3d:38:97:56:86:27:67:90:99:57:48:7c
            0b:35:ba:24:e7:1c:f6:9e:1f:82:1d:9a:4e:0d:9f:70:8e:91:74:26:57:13:9e:f9:c7:8e:9c:5c:a6:8e:30:62
            23:2d:7d:fd:79:09:d1:20:ad:6a:88:f1:fc:49:b5:34:cc:00:2a:7f:95:10:07:e7:b7:d7:90:af:7d:eb:7f:07
            67:7b:f5:25:0d:c0:2d:06:b8:57:2a:ef:9f:5c:2f:c9:48:e9:17:f0:43:22:2e:67:0a:56:ca:f8:ee:98:79:71
            72:26:00:f7:f0:7f:1d:13:a6:20:88:73:ba:42:6c:8b:5e:ef:fd:04:b3:98:90:f7:23:63:bf:08:46:6d:2e:41
            8e:17:a3:cd:ea:5c:55:ff:06:14:91:23:3b:d8:26:d5:b0:d5:8f:69:88:5a:b7:60:dd:73:01:54:d0:b2:18:65
            b3:75:8a:5a:a7:ed:5e:ef:22:45:d4:07:bd:06:32:1e:b8:92:07:49:72:cc:7e:7a:63:fb:3f:e1:92:c3:9a:5a
            d9:10:d9:8a:96:0d:f2:89:d1:a0:87:d0:26:42:b8:51:f9:1d:72:fc:fb:ee:d4:32:14:e8:0c:0d:f3:7a:fa:63
    
        Under Modules, check the following boxes: chansaver, keepnick, kickrejoin, nickserv, perform, simple_away
        
        Click "Save and Return"
    
    Under Modules, check the following boxes: chansaver, controlpanel, perform
    
    Under Default Settings, change Channel Modes to **+stn**
    
    Under Default Settings, change Buffer Size to 1000
    
    Click Save and Return
    
5. Configure IRCScraper

    *We're assuming you install nZEDb to /var/www/nZEDb/*
    
    ``cd /var/www/nZEDb/configuration/``

    ``cp ircscraper_settings_example.php ircscraper_settings.php``
    
    Open your favorite editor and change the following in ``ircscraper_settings.php``
    
        ``$username = '<put your username from your ZNC setup>';``
        
        ``define('SCRAPE_IRC_SERVER', '127.0.0.1');``
        
        ``define('SCRAPE_IRC_PORT', '6664 <or the port you set for the listener above>');``
        
        ``define('SCRAPE_IRC_TLS', true);``
        
        If you specified a different username and nick combo above, you'll want to also change the following::
        
            define('SCRAPE_IRC_NICKNAME', "<the nick you specified above>");
            
            define('SCRAPE_IRC_REALNAME', "<the nick you specified above>");
            
            define('SCRAPE_IRC_USERNAME', "<the nick you specified above>");
            
        The remainder of the conf file is editable at your discretion. It is commented, so read it all before making changes.
        
6. Verify everything is setup properly and works.

    ``cd /var/www/nZEDb/misc/IRCScraper/``
    
    ``php scrape.php true false true``
    
    You should get a bunch of text on your terminal, and eventually you should start seeing entries being added to the preDB. If you get any errors, feel free to ask for :ref:`help`.
    
7. Setup ZNC to start at boot.

    You have two options to do this. You can setup a cronjob or use a init script/systemd service/etc.
    
    For setting up the cron job use the following (via a non-root user preferably):
    
        *Note: Crontab syntax may differ slightly between distrobutions, but should work*
    
        ``@reboot znc``
    
    For setting up the init script/etc you can visit the ZNC documentation_.
        
.. _hernando: http://forums.nzedb.com/index.php?topic=2000.0
.. _documentation: http://wiki.znc.in/Running_ZNC_as_a_system_daemon
