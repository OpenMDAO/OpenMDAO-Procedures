Maintaining the Documentation 
=============================

This section covers updating the user docs in the Framework repo, updating the Procedures doc, and
updating text on our website.

Updating Sphinx in the OpenMDAO-Framework Repository
-----------------------------------------------------

To update Sphinx, typically you create a new branch in your personal repo, grab the newest version of Sphinx,
and build the documents to make sure everything still looks okay and no warnings or errors show up during the
build process. If warnings or errors occur, tests will fail. 

To get the latest version of Sphinx, first activate your virtual environment using the ``. bin/activate`` command. 

Then type:

::

  easy_install -U sphinx
  
  
If you want to grab a specific version of Sphinx, type:

::

  easy_install Sphinx==<version_number>

For example::

  easy_install Sphinx==1.1.3
  
While Sphinx is being installed, notice which version of Docutils is being used since the new version of Sphinx
may require that Docutils be updated. For example, when we upgraded from Sphinx 1.0.6 to 1.1.3, for Sphinx
to work we had to move from Docutils 0.7 to 0.8.1.

Now, follow these steps:

1. Update the script ``go-openmdao-dev.py``, which is located in the root directory of your repo.
   (Change the version of Sphinx, and if necessary, Docutils.)

2. Deactivate your virtual environment::
  
     deactivate
     
3. Delete ``devenv/``::

     rm -rf devenv/
     
4. Now rebuild the environment by running the installation script::

     python go-openmdao-dev.py   
     
5. Run the test suite::

     openmdao test
             
6. Assuming there are no test failures, you can commit the changes on your branch and then issue a pull request
   from your personal OpenMDAO repo. 

After the branch is merged, one of the OpenMDAO maintainers will put a new distribution for Sphinx
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

Updating This Document (OpenMDAO Procedures)
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
     
     2. Log in to webfaction and change to the "docs/procedure_docs" directory.
     
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


Miscellaneous Notes
--------------------

- The ``conf.py`` file contains ``html_theme_options`` for customizing the look of our documents. In the
  OpenMDAO-Framework docs, we use Arial for the heading font, and this renders all the headings as well as the text
  on the sidebar and relation bars in Arial. However, in the OpenMDAO0-Procedures doc, if the ``"headfont"`` option
  is set to Arial, the headings in the document and links on the sidebar appear in Arial, but text on the sidebar
  becomes Times New Roman (at least when building during development). Therefore, in the Procedures ``conf.py``
  file, ``"headfont"`` is set to Helvetica, which renders all the text on the sidebar and relation bars and in the
  headings as Helvetica.

- We use the image file ``OpenMDAO_Logo_200w_padded.png`` in our Framework repo docs because the
  space between the logo and text was insufficient; however, in the Procedures repo, we use the
  unpadded version, ``OpenMDAO_Logo_200width.png``, because there is already enough space around it.

- In our Procedures repo, the favicon image just has to sit in the root directory to get built so it
  appears on the browser tab. In this repo if the file extension is not ``.ico``, Sphinx will
  generate a warning. Thus, the file extension was changed from ``.png`` to ``.ico``.

- Occasionally when building the docs, you may get a Sphinx warning for an ``undefined label``. The
  text of the warning will be similar to the following:

  ::

    /OpenMDAO/dev/pziegfel/OpenMDAO-Framework/docs/srcdocs/packages/openmdao.lib.rst:7:  WARNING: undefined label:
    enthought.traits.has_traits.py (if the link has no caption the label must precede a section header) 

  To get rid of the warning, go to the ``/OpenMDAO-Framework/docs/srcdocs`` directory and edit
  the ``index.rst`` file. Add the offending label to the file using the following format:

  ::
  
    .. _enthought.traits.has_traits.py:
    
  When you build again, the warning should not appear.
  

.. _`Using-NEdit`:

Using NEdit 
------------

NEdit is a text editor available for editing documentation or code. When you bring up a file in
NEdit, the file name is in the top left-hand corner of the window, above the menu. If the file has
been changed, it will say "(modified)" immediately to the right of the file name.

*Editing a File*
~~~~~~~~~~~~~~~~~

Go to the ``docs/`` directory on your branch, change to the desired directory, and bring up the file
that you want to edit:

::

  cd /OpenMDAO/dev/<your_working_directory>/<branch_name>
  cd <directory_name>
  nedit <file_name> &
  
This brings up your file; the ampersand allows NEdit to run in the background.

Using the **Fill Paragraph** Option on the **Edit** menu (or alternatively, **Ctrl+j**):  

- Select a range of text and then choose **Fill Paragraph** (or **Ctrl+j**). All of the text in
  the selection will be filled. (A paragraph is the space between blank lines.)
 
- Use **Fill Paragraph (Ctrl+j)** with a rectangular selection of text. NEdit interprets the right
  edge of the selection (text visible to the right boundary of the window) as the requested wrap
  margin. Text to the left of the selection is not disturbed, but text to the right of the
  selection is pulled in to the selected region. This method enables you to fill text to an
  arbitrary right margin, without going back and forth to the wrap-margin dialog. (In other words,
  you can make your XWindow the desired size and use this option so your text is visible.)
    
*Moving Text Right or Left*
~~~~~~~~~~~~~~~~~~~~~~~~~~~

To move text to the *right:* Highlight a block of text and type **Ctrl+0.** While holding down the **Ctrl** key, keep typing **0**
for every space you want the text to move to the right.
  
To move text to the *left:* Highlight the text and type **Ctrl+9.** While holding down the **Ctrl**
key, keep typing **9** for every space you want the text to move to the left.

An easy way to remember this is that the **0** is under the right parenthesis (for moving right), while
the **9** is under the left parenthesis (for moving left). If you hold down the **Ctrl** key and type a
right or left parens (requiring you to press the **shift** key simultaneously), the text moves one *tab*
instead of one *space.*


*Launching Spell Check from NEdit*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Open the file you want to spell-check and type: 

   ::
  
     nedit <file_name> & 

2. From inside the file, type: 

   ::
   
     Alt+b
     
   An XWindow named *ispell* will appear, and the first potentially misspelled word will be highlighted.
   
3. Select the letter or number of the desired option (e.g., Replace All, Ignore All, exit, etc.). You
   will automatically go to the next potentially misspelled word and so on until you come  to the end of
   the file.
   
4. When you are finished checking the file, save it, even in you made no changes. (Merely launching
   ispell is considered a modification to the file.)

*Using Line Numbers to Find Sphinx Errors*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you get a Sphinx build error when trying to build your documentation files, your build will fail.
Even if you get just a warning, you will want to correct it. Sphinx provides the file name and the line
number where the error or warning occurs. To find the error/warning, do the following:

1. Bring up the file with the error by typing:

   ::
   
     nedit <file_name> &
    
   
2. On the menu bar, click on **Preferences** and then on **Show Line Numbers.**

   You should be able to locate the line with the error and correct the problem. 

