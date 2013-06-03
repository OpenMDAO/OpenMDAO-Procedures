Managing the Official Plugins
=============================

The official plugin library is maintained at ``https://github.com/organizations/OpenMDAO-Plugins``.
Each plugin has its own repository in that organization.

Tagging Plugins
---------------
Users can install plugins into an activated OpenMDAO environment using the ``plugin install --github``
command. However, only versions that are tagged can be installed this way. You can create a
tag using the ``tag`` command in git as in this example:

::

  git tag -a 0.7 -m "Requires OpenMDAO 0.6.1"

Each tag requires a unique version number (0.7 in this example.) This number should match
the version number that you are using in setup.cfg. If a tag already exists, and you have
made changes to the plugin, then you must increment the version number in setup.cfg, and
rerun the ``plugin makedist`` command to generate a new setup.py file.

We also always provide an annotation for the tag ("requires OpenMDAO 0.6.1" in this example.)
We recommend that you use this space to indicate its compatibility with versions of OpenMDAO.
Otherwise, just summarize what is new about this version.

Once you have created the tag, you need to push it up to the repository. *Tags require
commit-access to the plugin repostiory.* You cannot pull request a tag on github, so in
most cases, someone on the development team will have to push any new tags that are needed.
The syntax for this is

::

  git push --tags origin master
  
assuming you have used the name "origin" for the originating repository. Note: you may also
want to pull new tags down from a repository:

::

  git pull --tags origin master

