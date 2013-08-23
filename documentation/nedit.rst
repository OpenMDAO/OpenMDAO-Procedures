Using NEdit 
===========

NEdit is a text editor available for editing documentation or code. When you bring up a file in
NEdit, the file name is in the top left-hand corner of the window, above the menu. If the file has
been changed, it will say "(modified)" immediately to the right of the file name.

Editing a File
----------------

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
    
Moving Text Right or Left
---------------------------

To move text to the *right:* Highlight a block of text and type **Ctrl+0.** While holding down the **Ctrl** key, keep typing **0**
for every space you want the text to move to the right.
  
To move text to the *left:* Highlight the text and type **Ctrl+9.** While holding down the **Ctrl**
key, keep typing **9** for every space you want the text to move to the left.

An easy way to remember this is that the **0** is under the right parenthesis (for moving right), while
the **9** is under the left parenthesis (for moving left). If you hold down the **Ctrl** key and type a
right or left parens (requiring you to press the **shift** key simultaneously), the text moves one *tab*
instead of one *space.*


Launching Spell Check from NEdit
---------------------------------

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

Using Line Numbers to Find Sphinx Errors
----------------------------------------

If you get a Sphinx build error when trying to build your documentation files, your build will fail.
Even if you get just a warning, you will want to correct it. Sphinx provides the file name and the line
number where the error or warning occurs. To find the error/warning, do the following:

1. Bring up the file with the error by typing:

   ::
   
     nedit <file_name> &
    
   
2. On the menu bar, click on **Preferences** and then on **Show Line Numbers.**

   You should be able to locate the line with the error and correct the problem. 

