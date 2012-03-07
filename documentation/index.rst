Maintaining the Documentation 
=============================

Updating Sphinx
---------------

Generally, the tech writer creates a new branch, grabs the newest version of Sphinx, and checks the documents to make
sure everything still looks okay -- that nothing weird came in with the new  Sphinx. The branch has to be merged, and the
SCM has to do certain things to incorporate a new version. 

To get the latest version of Sphinx, first activate your virtual environment using the ``. bin/activate`` command. 

Then type:

::

  easy_install -U sphinx
  
  
If you want to grab a specific version of Sphinx, type:

::

  easy_install Sphinx==<version_number>

For example::

  easy_install Sphinx==1.0.6

Running Sphinx Linkcheck 
-------------------------

Linkckeck is one of the Sphinx builders you can run to check external links. Follow the instructions
below.


1. You'll need to create a branch from the latest truck.

2. Activate the virtual environment (``/devenv``) on your branch.

3. Go to the ``/docs`` directory.  (You must be in same directory as the ``conf.py.rst`` file.)

4. Run the linkcheck builder as follows:

::

  sphinx-build -b linkcheck . linkcheck-output


This command creates a directory (e.g., ``linkcheck-ouput``, although you can name it something else)
and makes an ``output.txt`` file.

When you go to ``linkcheck-output`` (or whatever you name the directory), you can bring up the
``output.txt`` file in your desired text editor. (NEdit is an option.)

The ``output.rst`` file produced by linkcheck will list the document, line number, problem, and the URL.
See the example below.

::

  dev-guide/accessing.rst:18: [broken] https://launchpad.net: <urlopen error unknown url type: https>
  dev-guide/accessing.rst:51: [broken] https://launchpad.net/people/+me/+editsshkeys: <urlopen error unknown url type: https>
  dev-guide/working.rst:288: [broken] https://launchpad.net/openmdao: <urlopen error unknown url type: https>
  licenses/index.rst:42: [redirected] http://www.pycrypto.org/ to http://www.dlitz.net/software/pycrypto/
  licenses/index.rst:53: [broken] http://pyparsing.wikispaces.com/: <urlopen error unknown url type: https>
  
  
In the first line of the example, the redirected link in the Architecture Document can be updated in the ``concepts.rst`` file. 

The next three "broken" links are not really broken. They refer to secure sites that require a Launchpad userid and password. The
links cannot be changed, so they should always appear in the output file. 

In line 5, the redirected link in the ``licenses.rst`` cannot be changed because the system is pulling metadata from the package's
site. Thus this line will show up until the metadata is updated.


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
  
This brings up an empty file; the ampersand allows NEdit to run in the background.

Using the **Fill Paragraph** Option on the **Edit** menu (or alternatively, **Ctrl+j**):  

-  Select a range of text and then choose **Fill Paragraph** (or **Ctrl+j**). All of the text in
   the selection will be filled. (A paragraph is the space between blank lines.)
 
-  Use **Fill Paragraph (Ctrl+j)** with a rectangular selection of text. NEdit interprets the right
   edge of the selection (text visible to the right boundary of the window) as the requested wrap
   margin. Text to the left of the selection is not disturbed, but text to the right of the
   selection is pulled in to the selected region. This method enables you to fill text to an
   arbitrary right margin, without going back and forth to the wrap-margin dialog. (In other words,
   you can make your XWindow the desired size and use this option so your text is visible.)
    
*Moving Text Right or Left*
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- To move text to the *right:*
  Highlight a block of text and type **Ctrl+0.** While holding down the **Ctrl** key, keep typing **0**
  for every space you want the text to move to the right.
  
- To move text to the *left:*
  Highlight the text and type **Ctrl+9.** While holding down the **Ctrl** key, keep typing **9**
  for every space you want the text to move to the left.

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

Updating this Document
----------------------

Once this is moved to a Git repository, update the instructions. 
