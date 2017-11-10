
WebFaction
----------

The OpenMDAO website at ``openmdao.org`` is hosted by WebFaction.

.. note:: All URLs, usernames, and passwords for WebFaction and the other accounts listed below are available internally
          at ``havoc:/OpenMDAO/accounts_passwords.txt``.


Connection
==========

**SSH:**

     `server:`  ``web39.webfaction.com``

     `Username and password` are in a secure file that is currently on our internal server, Havoc.

**Control Panel:**

     Control Panel login is at ``my.webfaction.com`` and uses the same login information as the SSH.

     `Domains:`  This is where we input ``openmdao.org`` and ``www.openmdao.org``.

     `Apps:` This is where all of the apps that run this website get routed to their part of the
     website and/or get their port numbers.


       .. image:: /images/website/apps.png

       Note that the above image shows Python2.6, which we no longer support.

     `Websites:` Makes sure that the site's apps have URLs that are directly reachable.  Here's the current mapping of site apps:

       .. image:: /images/website/urls.png


     `Email addresses @openmdao.org:`  Under Emails, you can see how the ``@openmdao.org`` email addresses
     are set up.  Some are usernames, and others are aliases.  Currently all of these are forwarded to NASA
     addresses or gmail addresses as requested.  An email address can go to multiple people at once; for
     example, ``support@openmdao.org`` goes to both Keith and Justin.


Organization/Filestructure
===========================

`Dists`
~~~~~~~~

The ``dists`` directory is for holding local versions of packages that might need to be installed
(e.g., scipy) that can be gotten from our ``dists`` dir instead of having to go out to other
places to find them.  To add a new package, you must FTP the new file into the
``/home/openmdao/dists/`` directory at ``web39.webfaction.com``.

Whenever a new package is added, the webpage at ``openmdao.org/dists`` needs to be updated to
reflect that addition. To do that, you must run Python 2.7 on the ``mkegglistindex.py`` file from
within the ``dists`` directory, like this:

::

  >> python2.7 /home/openmdao/dists/mkegglistindex.py

Once you have run the script, refresh the ``openmdao.org/dists`` webpage to make sure that the
update worked.  The newly added package should appear in the dists list.

`Downloads`
~~~~~~~~~~~

The releases of OpenMDAO sit in this directory.  *Downloads* also has a script that creates the page
at ``openmdao.org/downloads``, and each version directory has its own page that needs to be
generated. From the instructions at ``/home/openmdao/downloads/README_TO_UPDATE``:

::

  TO UPDATE DOWNLOAD PAGES:

  First, cd into the most recent folder and then run "python2.7 dlversionindex.py".
  Then, cd back up to this level and run "python2.7 mkdownloadindex.py" in this directory.
  Finally, check openmdao.org/downloads/ to make sure that your efforts worked.

`Webapps`
~~~~~~~~~~

This section lists the webapps that ``openmdao.org`` uses. You will most likely need to refer to the information
on ``custom_app`` and :ref:`OSQA`.


OpenMDAO Test Server (custom_app)
+++++++++++++++++++++++++++++++++

This server is called by the GitHub post-receive hook. It's the trigger app that gets called automatically by GitHub whenever a change to
the ``dev``  branch of the repository is made.  This app parses out the XML from that commit and kicks off the
testing process.  The OpenMDAO test server is kept in a separate repository on Github:  https://github.com/OpenMDAO/openmdao_testapp.


When a pull request is approved on GitHub, it should trigger GitHub's ``post_receive`` hook.  The ``post_receive`` hook
should in turn contact the OpenMDAO testing application, which then turns on EC2 machines (specified by the test_branch qualifier) to test the most recent commit
against our test suite.  The results are then tabulated, posted to openmdao.org/p_r, and sent off through another hook to the OpenMDAO Slack channel #github-pivotal.

Sometimes, however, due to problems like the test server crashing, the ``post_receive`` fails to
start the testing.  In these cases, you'll need another event to trigger the ``post_receive`` hook, once the
underlying problem that stopped the server has been resolved (e.g., rebooting the test server.)  You don't want to have to do another commit to
trigger testing.  The event you need is the ``send_payload`` command, which works as follows:

**send_payload Command**

::

  -h, --help
        show this help message and exit

  -c COMMIT_ID, --commit COMMIT_ID
        id of commit to test

  -r REPO, --repo REPO
        repo url

  -b BRANCH, --branch BRANCH
        branch name

  -s SERVER, --server SERVER
        url:port of testapp server

The most common usage example of ``send_payload`` would look like this::

  send_payload -c [commit number] -s http://openmdao.org

If the ``send_payload`` usage is successful, an automated test will get kicked off and results will be posted to
http://openmdao.org/p_r.


**Updating and Restarting the Testserver**

The following procedure will properly update and restart the testserver:

1.  Connect to ``web39.webfaction.com`` using the openmdao account.

2.  Change directories into ``~/webapps/custom_app/openmdao_testapp/openmdao_testapp`` directory.

3.  Make sure that the previously-running testserver is no longer running.

    From this directory, use the ``./killserver`` command.

    If for some reason this isn't working, do a process listing using the command::

     ps -u openmdao

    Get the testserver's PID from that listing and then kill testserver by typing::

     kill -9 XXXX

    where XXXX is the PID.

4.  Change directories into the ``custom_app`` repository with the command::

     cd ~/webapps/custom_app/OpenMDAO-Framework

5.  Update the current repository by typing::

     git pull origin dev

6.  Remove the old ``devenv`` with the command::

     rm -rf devenv

7.  Build a new ``devenv`` with the command::

     python2.7 go-openmdao-dev.py

8.  Activate that new environment with the command::

    . /devenv/bin/activate

9.  Change directories into ``~/webapps/custom_app/openmdao_testapp`` directory.

10. Type, from the activated prompt::

     python setup.py develop

11. If changes were made to which platforms are going to be used, for example in testhosts.cfg, then a change needs to be made to the /home/openmdao/webapps/custom_app/openmdao_testapp/openmdao_testapp/testing.cfg file.  This must be done before server restart, as this file is read in when the server starts.  In other words, any time the testing hosts change, the server needs to be rebooted.

12. To restart the test server, type::

     start_openmdao_testapp

13. Exit web39


.. _`OSQA`:

OSQA
+++++

OSQA (Open Source Question & Answer) is an open source question-answer system written in Python with Django.

**Removing Spam Users**

A script has been written to remove spam users from the OSQA database. It is located in ``~/bin`` and can be run
from anywhere with the command::

  osqaDBclean.py

+ *Arguments*

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

		-e    Remove all unverified users from the database

	  -i    Remove all inactive users from the database, who have
                        gotten past recaptcha


- *How to Use osqaDBclean.py*

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


- *How to Change the Database that osqaDBclean.py Connects to*

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

- *Periodic user cleanup*

	There are scripts that run osqaDBclean via cron jobs.  One script runs each night at midnight, and deletes users with
	unverified email addresses.  The other script runs every Saturday at noon, and removes users who have less than 12
	karma and zero posts.  In order to change the running of these two scripts, login to webfaction and run "crontab -e"

Procedures Doc
+++++++++++++++

The Procedure Doc is the document that you're reading now; it is kept on WebFaction under
``/home/openmdao/docs/procedure_docs`` and points to the URL http://openmdao.org/procedures. That WebFaction folder is a
repository that watches ``git://github.com/OpenMDAO/OpenMDAO-Procedures.git``.  So when Procedures Doc repo is updated,  if
the changes are to be reflected in the online version, then you must go to this folder,  do a ``git pull`` to update the
repo, and then do ``make html`` to get the new doc built.

Stats
+++++++

This app populates a stats page up at ``openmdao.org/stats``.  It's a built-in WebFaction app, so you
can't do much other than install it and give it a URL. There's nothing to configure here, although
password-protecting this page could be useful.


WordPress
+++++++++

This app runs the bulk of the OpenMDAO website.  For details on WordPress, please see the following section.


WordPress
----------

This tool is used to manage the information on the ``openmdao.org`` website.

Content
=======

Most of the pages on the site are created as a `page` through the WordPress editor. The `front` page is a static HTML page.


**News** - The `News` page is a blog app plugin. Any `post` created in the WordPress editor shows up here. As the name implies, it should be used for news.

**Downloads** - This is a family of pages. (`Downloads` leads to the downloads page that's generated by Justin's script.)

- **Recent Releases** and **Archives** pages are automatically generated. To add a release to the  downloads
  page, see the ``README_TO_UPDATE`` file in the ``downloads`` folder on the server.

- **Plugins** is simply a link to the GitHub repo.

- **Supported Operating Systems** is also automatically generated. This plugin (OpenMDAO Supported Systems
  Provider) grabs data from the Amazon EC2 machines to determine what OS, architecture, and Python version is
  being tested. To manually add a supported system, please see the ``README`` file in the plugin's directory.

**Support** - This is also a family of pages that take users to either documentation, screencasts, or to the OSQA app mentioned previously.

- **Docs** and **Dev Docs** point to Sphinx documentation.

- **Forum** points to the OSQA forum.

- **Screencasts** points to our YouTube page.

**Publications** - This is automatically generated from the ``publications`` folder on the server's home directory. Any file in that folder
will show up on the `Publications` page -- EXCEPT files that start with ``!``. File names must `not` contain spaces, and any underscores in the name will display as a space. See ``!README_TO_UPDATE.txt`` in the ``publications`` folder for more details.

Changing the WordPress URL
=============================

1. Change the "app" path on ``my.webfaction.com``

 a) Go to ``my.webfaction.com`` and log in

 b) Navigate to ``Domains/Websites``

 c) Go to ``Websites``

 d) Click **edit** on the WordPress site

 e) Change the URL path of the ``wp_test`` app

2) In the ``functions.php`` of the current theme of the WordPress site (found in ``/wp-content/themes/'NAME-OF-THEME'/functions.php``), add two lines of code.
   These should be the FIRST THING IN THE FILE, after ``<?php`` of course.

   ::

     define('WP_HOME','http://example.com');
     define('WP_SITEURL','http://example.com');

   If there is no ``functions.php`` file, create one with only those two lines.

   Next, load the WordPress admin page until it works.
   Log in and check to see that this is your site.

   .. note:: Once your site is working, REMOVE THE LINES OF CODE FROM the ``function.php`` file.


3. Update the database (The image gallery will not work correctly until you do this.)

 a) Log in to the site's ``phpMyAdmin`` page, accessible from ``my.webfaction.com``. The password to the
    WordPress  database can be found under "Extra info" when clicking on the ``wp_test`` app from the
    **Applications** tab.

 b) Click on the WordPress database, and then click on the **SQL** tab on the top. Run the following code (replacing NEWURL with your new
    url, and OLDURL with your old url):

   ::

     update wp_posts set post_content = replace(post_content, "http://OLDURL.org", "http://NEWURL.org");
     update wp_options set option_value = replace(option_value, "http://OLDURL.org", "http://NEWURL.org");

   .. note:: Depending on your install, ``wp_posts`` and ``wp_options`` could have different prefixes. Adjust accordingly!


Updating the CSS or Header Art
================================

The website's CSS is defined by the current theme of the WordPress site. As of this writing, our theme is ``Yoko-OpenMDAO``
customization. Simply edit ``style.css`` as defined in the theme files to change our website's style.

To change the header art, modify ``header.php`` in the current theme. The header art is loaded in the ``custom_banner`` div.



Checking For/Recovering From a Code Injection Attack Against WordPress
======================================================================

The website has recently been the victim of a code injection attack--with malicious code inserted into the actual php header tags of all the .php files
that make up the site's file structure.  Recovery was tedious, as all affected files had to be manually edited.  To combat this in the future, several measures have been taken, including the disabling of comments and
tracebacks for any of the WordPress pages, and registering with Akismet spam cleanup service.  Most importantly, the entire directory has also been backed up
as a Git repo in the form of a private repository at  ``https://github.com/OpenMDAO/wordpress``.  This way, if another attack occurs, the hundreds of .php
files that make up our Wordpress site can be restored with one ``git reset --hard HEAD`` (a dangerous command, as it discards all uncommitted changes.)

The administrator should periodically check to make sure that no code injection has happened.  How will you know?  Well, if the website is offline with a 503 error, that's a big clue. If the website is up, you should still check once a week.

1. Login to WebFaction via ssh to web39 and change directories to the ~/webapps/wp_test dir, where the repo lives.

2. Do a ``git status`` to see if hundreds of files have changed--this will be obvious--take a look at a changed file to be sure.

3. If it has happened, we must do a ``git reset hard --HEAD`` to get things back to where they belong. This discards any changes that have been made since the last commit.

4. After the reset, view the file that you just verified in step 2 had a code injection in it.  Make sure that it now doesn't. Make sure that `git status` no longer shows hundreds of changed files.  The website should still be up, or if it was down, it should come back up now.


Keeping WordPress Up to Date
=============================
Since we are now tracking all of the .php files in the repository, every time that WordPress or any of its plugins are updated,  those changes must be committed
and pushed up to the main repository as the new normal.  That way, if we need to reset, we reset without losing our updates.  Here's the procedure:


1. First, login via ssh to web39 and change directories to ~/webapps/wp_test, where the repo lives.

  a. If you don't have a fork of the OpenMDAO/wordpress repo, go make one at the github site.

  b. If you don't have a remote hooked up to your fork, do so:  ``git remote add myfork https://github.com/username/wordpress``

2. Make sure that no code injection has happened. See the section above titled "Checking For/Recovering From a Code Injection Attack Against WordPress"

3. Make sure the site is still up, then login via browser to the site at ``http://openmdao.org/wp-admin/``, but also keep the ssh to webfaction open.

*For updates of WordPress or its plugins, repeat steps 4 - 7 for each update desired:*

4. Use the graphical menu in the wp-admin page (Dashboard -> Update) to update to the latest WordPress version, or to update a plugin.  After the update, make sure the site is still up.

5. Each update should itemize for you which files were modified.  Now, in your command prompt login, do a ``git status``, and you should see the same files listed by the update as having just been changed.  If you see additional files changed, something is wrong.  Assuming all is well, this is when you should do a ``git commit -am "Updating to WordPress X.X.X"`` or ``git commit -am "Updating Akismet plugin to version X.X"``

6. Push the new commit up to your fork.  ``git push myfork master``

7. Using the github website, initiate a pull request back to the origin repository.  Once the pull request is approved, repeat steps 4-7 to install additional updates.

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

GitHub
-------

**Service Hooks:** GitHub is great for keeping code repositories.  But for the Framework repository, we also have
a post-commit hook set.  Whenever a commit occurs on the dev branch, a blast of XML is sent to the
``custom_app`` we have running on WebFaction.  That app in turn kicks off the build and uses the XML
to log info on the commit that triggered the build.

This process is wired together on GitHub at: https://github.com/OpenMDAO/OpenMDAO-Framework/admin.
(This link works only if you have admin privileges.)

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
