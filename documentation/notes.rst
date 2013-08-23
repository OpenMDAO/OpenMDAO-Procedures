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

- In our Procedures repo, if the file extension is not ``.ico``, Sphinx will generate a warning. Thus,
  the file extension was changed from ``.png`` to ``.ico``.  Also, as long as the favicon file is in the
  root directory of this repo, it will show up on the browser tab.  

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
  
- In the unlikely event that you need to change the colors of the documentation again (updated Sept.
  2012), you can easily update them in the ``conf.py`` file. If the colors change, you also need to update
  the doc colors for the OpenMDAO plugins. To change the colors for the plugin docs, you must be on a
  branch in your OpenMDAO-Framework repo. Assuming you are at the root, do the following: 
  
  ::
  
    cd /openmdao.main/src/openmdao/main/plugin_templates
    nedit conf_py_template &
  
  Update ``html_theme_options`` in ``conf_py_template`` with the new colors. The change might not be implemented in
  the plugins until ``makedist`` is run in each plugin's repo.

- The sidebar: The sidebar in our OpenMDAO docs is on the left side of the page because it did not display correctly
  in IE when it was on the right side of the page (at least when the project began and IE was widely used). IE would
  display the sidebar on the right, but the text would be omitted. Also, when the sticky sidebar was tried,
  the Table of Contents was omitted. Thus, the sidebar is on the left.


.. _`Using-NEdit`:

