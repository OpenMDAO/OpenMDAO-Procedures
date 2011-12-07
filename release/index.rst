Release Building and Publishing
===============================

After branch testing is complete, it may be time to create a new OpenMDAO
release. The tools and procedures described below make the process a little
easier.  The procedure to create and publish a release is as follows:

    - make_release -d <destdir> -v <version> --host=<win32_py26_host> --host=<win32_py27_host>
    - push_release <destdir> <releasedir>
    - test_release
    - push_release


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


Making an Official Release
~~~~~~~~~~~~~~~~~~~~~~~~~~

Once all of the distribution packages have been made and the release has 
been tested on all platforms of interest, it's time to make it official
by pushing it up to the distribution area on ``openmdao.org`` using the
``push_release`` script as follows:

::

    push_relase <release_directory> http://openmdao.org

where ``release_directory`` is the destination directory you supplied earlier
when you called ``make_release``.  The ``push_release`` script takes the files
in the release directory and places them in the proper locations on the
server, i.e., the docs and the ``go-openmdao.py`` file go in the *downloads* 
area and the distribution packages go in the *dists* area.  The second
argument to ``push_release`` can be the URL of a different server or even
a local directory path if you need to debug or test the process outside
of the production environment.  This is actually what ``test_release`` does
when you supply it with a release directory.

The last step is to update the repository on GitHub ...

