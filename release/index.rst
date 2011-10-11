Release Building and Publishing
===============================

After branch testing is complete, it may be time to create a new OpenMDAO
release. The tools and procedures described below make the process a little
easier.


Release Creation
----------------

The ``make_release`` script is used to build the required distribution tar
files for all of the OpenMDAO packages. It also builds the html version
of the docs and the ``go-openmdao.py`` bootstrapping installer file.  
Running ``make_release`` with a ``-h`` option will display the following:

::

    Usage: make_release [options]

    Options:
      -h, --help        show this help message and exit
      -d DESTDIR, --destination=DESTDIR
                        directory where distributions and docs will be placed
      -v VERSION, --version=VERSION
                        version string applied to all openmdao distributions
      -m COMMENT        optional comment for version tag
      -b BASE, --basebranch=BASE
                        base branch for release. defaults to master
      -t, --test        used for testing. A release branch will not be created
      -n, --nodocbuild  used for testing. The docs will not be rebuilt if they
                        already exist
      --host=HOST       host from config file to build bdist_eggs on. Multiple
                        --host args are allowed.
      -c CONFIG, --config=CONFIG
                        path of config file where info for hosts is located


The script places all of the tar files and docs in the destination directory
specified with the ``-d`` option. The version number is specified with ``-v``
and must be later than any version already existing on ``openmdao.org``. OpenMDAO
releases require binary distributions on Windows for certain packages, so
``make_release`` will fail if you don't specify a Windows host using the
``--host`` option. The ``-t`` and ``-n`` options should be used for
testing purposes only.


Release Testing
~~~~~~~~~~~~~~~

See the previous description of ``test_release``.


The next step is to update the repository on GitHub.  If ``make_release`` was
run earlier without the ``-t`` arg, then it will have created a branch in the
local repository named ``release_<version_num>``, where <version_num> would be
something like **0.2.1** for example.  Check out that directory as follows:

::

    git checkout release_0.2.1


Now push the release commit up to the dev branch on GitHub.

::

    git push origin release_0.2.1:dev --tags


This assumes that the remote called ``origin`` points to the official repo for
OpenMDAO-Framework on GitHub.  The ``--tags`` arg pushes the version tag, **0.2.1**
in this case, up to the official repository.  The version tag was created when
we ran ``make_release``.

Pushing a new commit up to the dev branch will trigger the automated branch tests.
Assuming they all pass, all that's left to do is to update the ``master`` branch
by issuing a pull request from ``dev``, and pushing the distrubution files up to
the production server.


Updating Distribution Files
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once all of the distribution packages have been made and the release has 
been tested on all platforms of interest, it's time to make it official
by pushing it up to the distribution area on ``openmdao.org`` using the
``push_release`` script as follows:

::

    push_release <release_directory> openmdao@web103.webfaction.com

where ``release_directory`` is the destination directory you supplied earlier
when you called ``make_release``.  The ``push_release`` script takes the files
in the release directory and places them in the proper locations on the
server, i.e., the docs and the ``go-openmdao.py`` file go in the *downloads* 
area and the distribution packages go in the *dists* area.  The second
argument to ``push_release`` can be the ssh login to the server or even
a local directory path if you need to debug or test the process outside
of the production environment.  This is actually what ``test_release`` does
when you supply it with a release directory.




