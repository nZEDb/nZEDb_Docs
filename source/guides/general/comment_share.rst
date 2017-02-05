.. _comment_share:

Comment Sharing
===============

Comment sharing is just what it sounds like. It's a way to share comments between nZEDb sites. This is a community based feature.

It may take a few runs before comments start showing up. This is normal. If you enable the backfill option, it may take quite some time to grab all comments as there are a lot.

1. Toggle sharing in the database.

    ``cd /var/www/nZEDb/misc/update/``
    
    ``php postprocess.php sharing true``
    
2. Configure sharing.

    A. Head to your Site Admin -> Misc -> Sharing Settings
    
        Next to "Enabled" click until it reads "Disable"
        
        Changes to the rest of the settings are optional.
        
3. Comments are grabbed during post-processing.
    
    If you're using Tmux scripts to index, you have enable comment processing there as well.
        
        1. Site Admin -> Site Settings -> Tmux Settings -> Comment Sharing: On
        
    If you're using screen scripts to index, as long as it's running post-processing or update_releases, you're good.
        
    If you're indexing manually, you can grab comments via
        
        ``php /var/www/nZEDb/misc/update/postprocess.php sharing true``
            
        or
        
        ``php /var/www/nZEDb/misc/update/update_releases.php 1 true``