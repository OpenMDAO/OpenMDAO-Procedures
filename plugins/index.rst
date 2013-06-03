Managing the Official Plugins
=============================

The official plugin library is maintained at https://github.com/organizations/OpenMDAO-Plugins.
Each plugin has its own repository in that organization.

Tagging Plugins
---------------
Users can install plugins into an activated OpenMDAO environment using the ``plugin install --github``
command. However, only versions that are tagged can be installed this way. You can create a
tag using the ``tag`` command in Git as in this example:

::

  git tag -a 0.7 -m "Requires OpenMDAO 0.6.1"

Each tag requires a unique version number (0.7 in this example.) This number should match
the version number that you are using in ``setup.cfg``. If a tag already exists and you have
made changes to the plugin, then you must increment the version number in ``setup.cfg``, and
rerun the ``plugin makedist`` command to generate a new ``setup.py`` file.

You should always provide an annotation for the tag (as in this example, "Requires OpenMDAO 0.6.1").
Ideally, also use this space to indicate the plugin's compatibility with versions of OpenMDAO.
Otherwise, just summarize what is new about this version.

Once you have created the tag, you need to push it up to the repository. *Tags require commit-access to
the plugin repository.* Since most users cannot issue a pull request for a tag on GitHub, someone
on the development team will have to push any new tags that are needed. The syntax for this is:

::

  git push --tags origin master
  
where ``origin`` is the originating repository. 

Note: you may also want to pull down new tags from a repository using this command:

::

  git pull --tags origin master

