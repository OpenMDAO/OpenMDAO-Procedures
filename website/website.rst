Website Maintenance of OpenMDAO.org
====================================

All URLs, account names, and passwords are available internally at ``torpedo:/OpenMDAO/accounts_passwords.txt``.

WebFaction
-----------
	
The OpenMDAO website at ``openmdao.org`` is hosted by WebFaction.
	
**Connection**
		
`SSH:`
		 
     `server:`  ``web103.webfaction.com``

     `username and password` are in a secure file that is currently on our internal server, Torpedo.

`Control Panel:`  
			
     Control Panel login is at ``my.webfaction.com`` and uses the same login information as the SSH.

     `Domains:`  This is where we input ``openmdao.org`` and ``www.openmdao.org``.

     `Apps:` This is the place where all of the apps that run this website get routed to their part of the
     website and/or get their port numbers.

			
     .. image:: /images/website/apps.png
    
|
	 
   `Websites:` Makes sure that the site's apps have URLs that are directly reachable.  Here's the
   current mapping of site apps:
     
   .. image:: /images/website/urls.png
     
|   
 
     `Email addresses @openmdao.org:`  Under Emails, you can see how the ``@openmdao.org`` email addresses
     are set up.  Some are usernames, and others are aliases.  Currently all of these are forwarded to NASA
     addresses or gmail addresses as requested.  An email address can go to multiple people at once; for
     example, ``support@openmdao.org`` goes to both Keith and Justin.   


**Organization/Filestructure**

`Dists:`    

     The ``dists`` directory is for holding local versions of packages that might need to be installed
     (e.g., scipy) that can be gotten from our ``dists`` dir instead of having to go out to other
     places to find them.  To add a new package, you must FTP the new file into the
     ``/home/openmdao/dists/`` directory at ``web103.webfaction.com``.   

     Whenever a new package is added, the webpage at ``openmdao.org/dists`` needs to be updated to
     reflect that addition. To do that, you must run Python 2.6 on the ``mkegglistindex.py`` file from
     within the ``dists`` directory, like this:

     ::

       >> python2.6 /home/openmdao/dists/mkegglistindex.py

     Once you have run the script, refresh the ``openmdao.org/dists`` webpage to make sure that the
     update worked.  The newly added package should appear in the dists list.

`Downloads:`  

     This is the place where the releases of OpenMDAO sit.  This also has a script that creates the page
     at ``openmdao.org/downloads``, and each version directory has its own page that needs to be
     generated. From the instructions at ``/home/openmdao/downloads/README_TO_UPDATE``:

     ::
     
       TO UPDATE DOWNLOAD PAGES:

       First, cd into the most recent folder and then run "python2.6 dlversionindex.py".
       Then, cd back up to this level and run "python2.6 mkdownloadindex.py" in this directory.
       Finally, check openmdao.org/downloads/ to make sure that your efforts worked.


`Trac (NOW DEFUNCT)`

     The Trac environment lives in:  ``/home/openmdao/OpenMDAO/Trac``.  The ``plugins`` directory is where
     all the various plugins must be installed. The ``trac.ini`` file lives Inside the ``conf`` directory
     and runs the whole show.  Nearly everything on the website is affected by  ``trac.ini``.  To run the
     trac server after it has crashed, go to ``/home/openmdao/OpenMDAO/Trac`` and run: 

     ::

       ./RUN_ME_to_RUN_TRACD

     which contains:

     ::

       tracd --single-env --port 52359 	
       --basic-auth="Trac,/home/openmdao/OpenMDAO/Trac/trac.	
       htpasswd,OpenMDAO_Trac" /home/openmdao/OpenMDAO/Trac 
       --daemonize

     .. note:: This is no longer the main website, and the server no longer runs. However, for cases when we
	       need something from the old website, the  server can still be spun up, and it will appear
	       under the URL: ``openmdao.org/classic``

`webapps`

     `Custom app:` GitHub post-change hook.  This is the trigger app that gets called automatically by
     GitHub whenever a change to the ``dev``  branch of the repository is made.  This app parses out
     the XML from that commit and kicks off the testing process.

     `OSQA:` OSQA (Open Source Question & Answer) is an open source question-answer system written in Python
     with Django.

     `Procedures Doc:` The Procedure Doc is the document that you're reading now; it is kept on WebFaction under
     ``/home/openmdao/docs/procedure_docs`` and points to the URL ``openmdao.org/procedures``.  That
     WebFaction folder is a repository that watches
     ``git://github.com/OpenMDAO/OpenMDAO-Procedures.git``.  So when Procedures Doc repo is updated, 
     if the changes are to be reflected in the online version, then you must go to this folder, 
     do a ``git pull`` to update the repo, and then do ``make html`` to get the new doc built.

     `Stats:` This app populates a stats page up at ``openmdao.org/stats``.  It's a built-in WebFaction app, so you
     can't do much other than install it and give it a URL. There's nothing to configure here, although
     password protecting this page could be useful.

     `WordPress:` This app runs the bulk of the site. It's discussed in detail below.

WordPress 
--------- 

The main OpenMDAO website is done in WordPress.  The front page is a static HTML page. 
The News page is a blog app plugin.  Downloads leads to the downloads page that's generated by Justin's
script.  Support links all take users to either documentation, screencasts, or to the OSQA app mentioned
above.


Amazon EC2
-----------

The Amazon Electronic Cloud Compute is where we host our machines that are involved in the automated online
testing.  The login info will be available in the Torpedo doc.  The process of setting up the machines is
discussed in a separate chapter of this document. Click `here <http://openmdao.org/procedures/amazon.html>`_ to 
view this information.

YouTube
-------

OpenMDAO has a YouTube account that is used for posting screencasts of installations and various things.  A
document on how to shoot a standard OpenMDAO screencast is HERE (link to the doc once it exists).  The email
address ``screencasts@openmdao.org`` is tied to this account and currently goes only to Keith.  We have a
`channel` at http://www.youtube.com/openmdao.  The username and password for this account will be in the
password document on Torpedo.

Twitter
--------

OpenMDAO has a Twitter account that is used to announce new releases, new screencasts, or any other pertinent
news to our followers.  This is a simple one; simply use the login information to get into the account and
then post the pertinent information or reply to any direct mentions that may have happened.  Currently, the
Twitter account is tied to the ``support@openmdao.org`` email address, so if you want to be copied on Twitter
notifications, add yourself to that email address (see above section on email aliases). Our feed is available
at: ``http://twitter.com/#!/openmdao``.  The username and password for this account will be in the
password document on Torpedo.

Launchpad
----------

``launchpad.net/openmdao`` is no longer used, but has a re-direct to the current project site and to GitHub. 
The only way to control this stuff is through Keith's account.

GitHub
-------

`Service Hooks:`  GitHub is great for keeping code repositories, housing issues (formerly known as tickets in our Trac
world), and hosting wiki pages.  But for the Framework repository, we also have a post-commit hook
set.  Whenever a commit occurs on the dev branch, a blast of XML is sent to the custom app we have
running on WebFaction.  That app in turn kicks off the build and uses the XML to log info on the
commit that triggered the build.  

The place that this is wired together on GitHub is: https://github.com/OpenMDAO/OpenMDAO-Framework/admin 

Click "Service Hooks" in the left-hand menu.

Then click "Post-Receive URLs." 

At this point, you'll be able to edit the URL or turn off the service completely.

.. note:: The "Twitter" service hook is currently turned off because commit chatter is too high. Despite
	  being off, the hook is wired to work with just a simple activation of an "active"  check box.


Torpedo
-------

**Backups of WebFaction**

`Cron`

    In Keith's home directory is a script that backs up the ``web103.webfaction.com`` content
    every day. The cron job in the crontab looks like this:

    ::

      00 02 * * *  /home/kmarstel/bin/backup_website >> 	          
      /home/kmarstel/WEBSITE/website_backup.log 2>&1

`Script`

    The very simple script that does the actual backing up of the website lives in
    ``/home/kmarstel/bin/backup_website``, as noted in the cron entry above.  It looks like this:

    ::

      cd /home/kmarstel/webfaction_backup/
      #Perform the web backup using rsync
      rsync -arvzt -e ssh 	
      openmdao@web103.webfaction.com: .

GoDaddy.com
------------

``GoDaddy.com`` handles our domain names and forwards them to WebFaction.

`Names:` ``openmdao.org``  (``openmdao.net, openmdao.com,`` and ``openmdao.info`` are set up to redirect to ``www.openmdao.org``) 

`Renewal:` Domain names are held until 10/24/2018.

`Tying to WebFaction:` In the GoDaddy account, the nameservers ``NS1.WEBFACTION.COM`` (NS1 through NS4) are
used.
