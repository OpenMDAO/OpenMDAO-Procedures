Updating the Docs and Sphinx 
=============================

This section covers updating Sphinx in our Framework and Procedures repositories, checking
external links in our user docs, updating the Procedures doc, and updating text on our
``openmdao.org`` website. Information on updating the actual OpenMDAO user documents, can be
found in the `OpenMDAO Developer Guide` at http://openmdao.org/dev_docs/documenting/updating.html.

Updating Sphinx in the OpenMDAO-Framework Repository
-----------------------------------------------------

Sphinx is the program that that takes ReStructuredText files and generates the HTML files for
our OpenMDAO user documents and procedures document. To update Sphinx, typically you create a
new branch in your personal repo, grab the newest version of Sphinx, and build the documents
to make sure everything still looks okay and no warnings or errors show up during the build
process. If warnings or errors occur, tests will fail. 

To get the latest version of Sphinx, first activate your virtual environment using the ``. bin/activate`` command. 

Then type:

::

  easy_install -U sphinx
  
  
If you want to grab a specific version of Sphinx, type:

::

  easy_install Sphinx==<version_number>

For example::

  easy_install Sphinx==1.1.3
  
On one occasion when we upgraded Sphinx, the new version did not work until we upgraded our
version of Docutils. So if Sphinx does not work, check the version of Docutils to make sure
you are running the latest one.

Now, follow these steps:

1. Update the script ``go-openmdao-dev.py``, which is located in the root directory of your
   repo. (Change the version of Sphinx, and if necessary, Docutils.)

2. Deactivate your virtual environment::
  
     deactivate
     
3. Delete ``devenv/``::

     rm -rf devenv/
     
4. Now run the installation script to rebuild the environment (and check dependencies.)

   ::

     python go-openmdao-dev.py   
     
5. Run the test suite::

     openmdao test
             
6. Assuming there are no test failures, you can commit the changes on your branch and then issue a pull request
   from your personal OpenMDAO repo. 

.. note:: In December 2013, when Sphinx was upgraded and the test suite was run on havoc, the following error
   occurred:
   
   ::
   
     ERROR: runTest (openmdao.gui.test.js_unit_tests.test_js_unit_tests.ChromeJsUnitTestCase)

     ValueError: Failed to execute JsTestDriver tests for:
     /tmp/tmpcG519p (http://localhost:52484)
     Error: xvfb-run: error: Xvfb failed to start

   Ken had noticed this error earlier. Apparently it occurs only on havoc and is not
   significant, so the Sphinx update was pushed up anyway. 

After the branch is merged, one of the OpenMDAO maintainers will put a new distribution of Sphinx
(and if necessary, Docutils) in ``http://openmdao.org/dists``, which will cause the index to be
automatically updated.


Running Sphinx Linkcheck 
-------------------------

Linkckeck is a Sphinx builder you can run to check external links in the OpenMDAO user docs. Follow
the instructions below.


1. First, update the dev branch in your local OpenMDAO-Framework repo::

     git pull origin dev
   
2. Create and check out a new branch for running linkcheck::

     git checkout -b <branch_name>

2. Activate the virtual environment. From ``devenv/``, type::

     . bin/activate

3. Now go to the ``docs/`` directory and run linkcheck. (You must be in same directory as the ``conf.py.rst`` file.)

   ::
   
     cd ../docs/
     sphinx-build -b linkcheck . linkcheck-output


   This command creates a new directory (e.g., ``linkcheck-output``, although you can name it something else)
   and makes an ``output.txt`` file.

5. Go to ``linkcheck-output`` directory and open the ``output.txt`` file in your desired text
   editor. (NEdit is used in this example.)
   
   :: 
   
     cd ../linkcheck-output
     nedit output.txt &

   The ``output.txt`` file produced by linkcheck will list the document, line number, problem, and the
   URL. See the example below.

   ::

     licenses/index.rst:24: [redirected] http://somethingaboutorange.com/mrl/projects/nose/ to http://readthedocs.org/docs/nose/en/latest/
     licenses/index.rst:16: [redirected] http://fabfile.org to http://docs.fabfile.org/en/1.4.0/index.html
     licenses/index.rst:44: [redirected] http://www.pycrypto.org/ to https://www.dlitz.net/software/pycrypto/
     licenses/index.rst:62: [redirected] http://www.virtualenv.org to http://www.virtualenv.org/en/latest/index.html
     licenses/index.rst:50: [redirected] http://pyparsing.wikispaces.com/ to http://pyparsing.wikispaces.com/?responseToken=793c872cd5fdfc7394c68e7fd2a074a2
     
   The redirected links in  ``licenses/index.rst`` cannot be changed because the system is pulling metadata from each package's
   site. Such links will continue to appear until the metadata is updated.
     
   Occasionally a message will say that a link has timed out. The link still works, but it's a good
   idea to check it anyway. 
   
Updating Sphinx in the OpenMDAO-Procedures Repo
-----------------------------------------------

Unlike the OpenMDAO-Framework repo, the Procedures repo has no virtual environment to activate, so you
cannot use the ``easy_install`` command. Moreover, you do not have permission to install in the
directory you need. Therefore, when you want to update Sphinx, ask the system admin (currently J.
Below) to install the desired version at the system level. 

Remember to ask for the latest version of Docutils since it may be needed for Sphinx to run
properly.

Updating this Document (OpenMDAO Procedures)
--------------------------------------------

If you haven't done so, you need to make a personal fork of the OpenMDAO-Procedures repository and also clone the
repo. Additionally, define a remote branch in your local repo. You only do these steps once. (See the `Developer
Checklist <http://openmdao.org/dev_docs/code-contribution-example.html>`_ if you need help.) If you
have done all this, follow the steps below.

1. Your first step should always be to update the master branch in your local OpenMDAO-Procedures repo::
 
     git pull origin master
  
   If you have a problem, check to make sure your origin is correct::
   
     git remote -v
     
   The system should return something like this::
   
     myfork  git@github.com:pziegfeld/OpenMDAO-Procedures (fetch)
     myfork  git@github.com:pziegfeld/OpenMDAO-Procedures (push)
     origin  git@github.com:OpenMDAO/OpenMDAO-Procedures.git (fetch)
     origin  git@github.com:OpenMDAO/OpenMDAO-Procedures.git (push)   
        
2. From the updated master branch, create and check out a new working branch::
   
     git checkout -b <branch_name>

     
3. Update the text on your branch as you normally would. To build the docs, you must be in the
   branch's root directory. Type::

     make html
     
   This command not only builds the docs but also displays them in Firefox.
   
5. When ready, commit your changes and issue a pull request. (No tests are run in this repo as it is a
   private repo for the GRC team and is used by only two or three people.)
   
6. After you have issued the pull request, the maintainer of the repository must do the following before you
   can see your changes::

     1. Merge the branch on GitHub.
     
     2. Log in to WebFaction and change to the "docs/procedure_docs" directory.
     
     3. Do a "git pull origin master". 

     4. Type: "make html" in this location. 

Upon completion of these actions, your doc changes will be pushed up to our website at
``openmdao.org/procedures``, where you can view them.


Editing the openmdao.org Blog 
------------------------------

Before you can modify any text on the ``openmdao.org`` webpage, you need to log in to WordPress.

1. Go to this WordPress URL: http://openmdao.org/wp-admin 

2. Enter your WordPress Username and Password.
   
   The Dashboard page will appear and look similar to the following:
   
   .. figure:: WP.PNG
      :align: center
      :alt: Shows WordPress Dashboard; in far left column you can click on **Post, Media, Links, Pages, etc.**, to access and edit any of these items.
      
      WordPress Dashboard

3. Click on **Posts** in the left column to edit any of the ``openmdao.org`` posts. 

   The **Posts** page, with a list of entries to edit, will appear. 
   
4. Click on the title of the post you wish to edit. This brings up the the **Edit Post** page for that
   entry. 
   
5. Edit the desired file. When you are finished, you can preview changes or just click on the
   **Update** button to save changes.

If you want to edit the text on one of the website's pages, basically you follow the same steps
except you select **Pages** instead of **Posts** from the Dashboard list shown in the above
figure. 


