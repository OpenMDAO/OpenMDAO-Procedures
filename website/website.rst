
WebFaction
----------
	
The OpenMDAO website at ``openmdao.org`` is hosted by WebFaction.

.. note:: All URLs, usernames, and passwords for WebFaction and the other accounts listed below are available internally
          at ``havoc:/OpenMDAO/accounts_passwords.txt``.

	
Connection
==========
		
**SSH:**
		 
     `server:`  ``web39.webfaction.com``

     `username and password` are in a secure file that is currently on our internal server, Havoc.

**Control Panel:**  
			
     Control Panel login is at ``my.webfaction.com`` and uses the same login information as the SSH.

     `Domains:`  This is where we input ``openmdao.org`` and ``www.openmdao.org``.

     `Apps:` This is the place where all of the apps that run this website get routed to their part of the
     website and/or get their port numbers.

			
     .. image:: /images/website/apps.png

|    
 
  `Websites:` Makes sure that the site's apps have URLs that are directly reachable.  Here's the current
  mapping of site apps:

   .. image:: /images/website/urls.png
     
|   
 
     `Email addresses @openmdao.org:`  Under Emails, you can see how the ``@openmdao.org`` email addresses
     are set up.  Some are usernames, and others are aliases.  Currently all of these are forwarded to NASA
     addresses or gmail addresses as requested.  An email address can go to multiple people at once; for
     example, ``support@openmdao.org`` goes to both Keith and Justin.   


Organization/Filestructure
===========================

`Dists:`    
~~~~~~~~~

The ``dists`` directory is for holding local versions of packages that might need to be installed
(e.g., scipy) that can be gotten from our ``dists`` dir instead of having to go out to other
places to find them.  To add a new package, you must FTP the new file into the
``/home/openmdao/dists/`` directory at ``web39.webfaction.com``.   

Whenever a new package is added, the webpage at ``openmdao.org/dists`` needs to be updated to
reflect that addition. To do that, you must run Python 2.6 on the ``mkegglistindex.py`` file from
within the ``dists`` directory, like this:

::

  >> python2.6 /home/openmdao/dists/mkegglistindex.py

Once you have run the script, refresh the ``openmdao.org/dists`` webpage to make sure that the
update worked.  The newly added package should appear in the dists list.

`Downloads:`
~~~~~~~~~~~~  

This is the place where the releases of OpenMDAO sit.  This also has a script that creates the page
at ``openmdao.org/downloads``, and each version directory has its own page that needs to be
generated. From the instructions at ``/home/openmdao/downloads/README_TO_UPDATE``:

::

  TO UPDATE DOWNLOAD PAGES:

  First, cd into the most recent folder and then run "python2.6 dlversionindex.py".
  Then, cd back up to this level and run "python2.6 mkdownloadindex.py" in this directory.
  Finally, check openmdao.org/downloads/ to make sure that your efforts worked.

`Webapps`
~~~~~~~~~~

**Custom app:** GitHub post-change hook. This is the trigger app that gets called automatically by
GitHub whenever a change to the ``dev``  branch of the repository is made.  This app parses out
the XML from that commit and kicks off the testing process.

The following procedure will properly **update and restart the testserver:**

1.  Connect to ``web39.webfaction.com`` using the openmdao account.

2.  Change directories into the custom_app's repository with the command::

     cd webapps/custom_app/OpenMDAO-Framework


3.  Update the current repository by typing:: 

     git pull origin master

4.  Remove the old ``devenv`` with the command::

     rm -rf devenv

5.  Build a new ``devenv`` with the command::

     python2.6 go-openmdao-dev.py

6.  Activate that new environment with the command::

    . /devenv/bin/activate


7.  Change directories, going back up a level to the ``~/webapps/custom_app/openmdao_testapp`` directory 

8.  Type::

     python2.6 setup.py develop

9.  To restart the test server, type::

     start_openmdao_testapp  

    The script is located at: ``~/bin/start_openmdao_testapp`` 

10. Exit web39


**OSQA:** OSQA (Open Source Question & Answer) is an open source question-answer system written in Python with Django.
See the section below on removing :ref:`spam users from OSQA <OSQA>`.

**Procedures Doc:** The Procedure Doc is the document that you're reading now; it is kept on WebFaction under 
``/home/openmdao/docs/procedure_docs`` and points to the URL ``openmdao.org/procedures``.  That WebFaction folder is a
repository that watches ``git://github.com/OpenMDAO/OpenMDAO-Procedures.git``.  So when Procedures Doc repo is updated,  if
the changes are to be reflected in the online version, then you must go to this folder,  do a ``git pull`` to update the repo,
and then do ``make html`` to get the new doc built.

**Stats:** This app populates a stats page up at ``openmdao.org/stats``.  It's a built-in WebFaction app, so you
can't do much other than install it and give it a URL. There's nothing to configure here, although
password protecting this page could be useful.

**WordPress:** This app runs the bulk of the site. It's discussed in more detail below.

.. _`OSQA`:

OSQA: Removing Spam Users 
---------------------------

A script has been written to remove users from the OSQA database. It is located in ``~/bin`` and can be run
from anywhere with the command::

  osqaDBclean.py  

**Arguments:**

:: 
  
  -h, --help 
        Show help message and exit 
 
  -v, --verbose 
        Enable verbose output 
 
  --nolog 
        Disable writing of log file 
 
  -u USERNAME, --username=USERNAME 
        The username to delete from the database 
 
  -f FILENAME, --file=FILENAME, --usernamefile=FILENAME 
        A file of usernames (separated by newlines) to delete 
 
  --sql 
        Make an .sql file of the database commands but do not execute 
 
  -a 
        Remove all suspended users from the database 
 
 
**How to Use osqaDBclean.py:**

 
1. Create a backup of the database. Do this with the following command: 
   
   ::
   
     $ pg_dump -U database_name -f dump.sql 
    
  (The ``database_name`` is currently ``openmdao_osqa``.) 
    
2. Run ``osqaDBclean.py`` with required arguments.


   .. Note:: You can run ``osqaDBclean.py`` with any of the options listed above, but you MUST specify either ``-f, -u,`` or
             ``-a``. You may use ``-f, -u,`` and ``-a`` together to specify multiple users to delete.

  
3. Ensure the forums still work. If they do not, restore the database with the command:  
	
   ::
   
     $ psql -U database_name database_name < file  
 
 
**How to Change the Database that osqaDBclean.py Connects To:** 
 
You must edit the script in order to change the database that it connects to. Find the following line (near the top of
the file) and change the appropriate fields.  

::
  
  db = psycopg2.connect(host='127.0.0.1', 
    	      database='openmdao_osqa', 
    	      user='openmdao_osqa', 
    	      password=?supersecretpassword',) 

		      
.. Note:: On WebFaction, ``database`` and ``user`` are ALWAYS the same. ``Password`` is not necessarily the same as
	  the ssh password. It is unique to the database and should not be changed without changing the password field
	  in the ``osqalocal_settings.py`` file.)

WordPress 
--------- 

The main OpenMDAO website is done in WordPress.  The front page is a static HTML page. 
The News page is a blog app plugin.  Downloads leads to the downloads page that's generated by Justin's
script. All support links take users to either documentation, screencasts, or to the OSQA app mentioned
above.


Amazon EC2
-----------

The Amazon Electronic Cloud Compute is where we host our machines that are involved in the automated online
testing.  The login info will be available in the password doc on Havoc. The process of setting up the machines is
discussed in a separate chapter of this document. Click `here <http://openmdao.org/procedures/amazon.html>`_ to 
view this information.


YouTube
-------

OpenMDAO has a YouTube account that is used for posting screencasts of installations and various things.  A
document on how to shoot a standard OpenMDAO screencast is HERE (link to the doc once it exists).  The email
address ``screencasts@openmdao.org`` is tied to this account and currently goes only to Keith.  We have a
`channel` at http://www.youtube.com/openmdao.  The username and password for this account will be in the
password document on Havoc.

Twitter
--------

OpenMDAO has a Twitter account that is used to announce new releases, new screencasts, or any other pertinent
news to our followers.  This is a simple one; simply use the login information to get into the account and
then post the pertinent information or reply to any direct mentions that may have happened.  Currently, the
Twitter account is tied to the ``support@openmdao.org`` email address, so if you want to be copied on Twitter
notifications, add yourself to that email address (see above section on email aliases). Our feed is available
at: http://twitter.com/#!/openmdao.  The username and password for this account will be in the
password document on Havoc.


Launchpad
----------

``launchpad.net/openmdao`` is no longer used but has a re-direct to the current project site and to
GitHub.  The only way to control this stuff is through Keith's account.


GitHub
-------

**Service Hooks:** GitHub is great for keeping code repositories, housing issues (formerly known as tickets in our Trac
world), and hosting wiki pages.  But for the Framework repository, we also have a post-commit hook
set.  Whenever a commit occurs on the dev branch, a blast of XML is sent to the custom app we have
running on WebFaction.  That app in turn kicks off the build and uses the XML to log info on the
commit that triggered the build.  

The place that this is wired together on GitHub is: https://github.com/OpenMDAO/OpenMDAO-Framework/admin 

Click **Service Hooks** in the left-hand menu.

Then click **Post-Receive URLs.** 

At this point, you'll be able to edit the URL or turn off the service completely.

.. note:: The **Twitter** service hook is currently turned off because commit chatter is too high. Despite
	  being off, the hook is wired to work with just a simple activation of an "active"  check box.

GoDaddy.com
------------

``GoDaddy.com`` handles our domain names and forwards them to WebFaction.

**Names:** ``openmdao.org``  (``openmdao.net, openmdao.com,`` and ``openmdao.info`` are set up to redirect to ``www.openmdao.org``) 

**Renewal:** Domain names are held until 10/24/2018.

**Tying to WebFaction:** In the GoDaddy account, the nameservers ``NS1.WEBFACTION.COM`` (NS1 through NS4) are
used.
