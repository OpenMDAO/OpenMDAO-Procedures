Release Building, Testing, and Publishing
=========================================

After branch testing is complete, it may be time to create a new OpenMDAO
release. The ``release`` tool and its subcommands described below make the 
process a little easier.  The procedure to create and publish a release 
is as follows:

::

    release build -b -v <version>
    release test rel_<version>  --testargs=-x # tests release locally
    release test rel_<version>  --all  --testargs=-x  # tests on all testhosts having test_release=true
    release finalize -v <version>

The following sections will give more detailed explanations of the steps shown above.


Release Creation
----------------

The ``release build`` command is used to build the required distribution tar
files for all of the OpenMDAO packages. It also builds the HTML version
of the docs and the ``go-openmdao.py`` bootstrapping installer file.

Running ``release build`` with a ``-h`` option will display the following:

::

    usage: release build [-h] [-d DESTDIR] [-v VERSION] [-m COMMENT]
                         [--basebranch BASE] [-t] [-n] [-b] [--host HOST]
                         [-c CONFIG]

    create release versions of all OpenMDAO dists

    optional arguments:
      -h, --help            show this help message and exit
      -d DESTDIR, --dest DESTDIR
                            directory where all release distributions and docs
                            will be placed
      -v VERSION, --version VERSION
                            version string applied to all openmdao distributions
      -m COMMENT            optional comment for version tag
      --basebranch BASE     base branch for release. defaults to dev
      -t, --test            used for testing. A release branch will not be created
      -n, --nodocbuild      used for testing. The docs will not be rebuilt if they
                            already exist
      -b, --binaries        build binary distributions where necessary
      --host HOST           host from config file to build bdist_eggs on. Multiple
                            --host args are allowed.
      -c CONFIG, --config CONFIG
                            path of config file where info for hosts is located


The script places all of the tar files and docs in the destination directory specified
with the ``-d`` option. The default destination directory is ``rel_<version>``. The
version number is specified with ``-v``  and must be later than any version already
existing on ``openmdao.org``. OpenMDAO releases require binary distributions on Windows
for certain packages, so ``release build`` will fail if you don't run it with the
``-b`` option to specify that binaries should be built. If you are just doing testing
of the ``release build`` command, then the ``-b`` option may be omitted. The ``-t`` and
``-n`` options should  not be used except during testing or debugging of the ``release
build`` command.

When creating an *official* release, using all default values is recommended, which 
results in a command of the form:

::

    release build -b -v <version>
    
After executing the command, a ``rel_<version>`` directory containing all OpenMDAO
distribution packages and docs will exist in the current directory.  Also, a new
branch named ``release_<version>`` will exist in the local OpenMDAO-Framework repository.
At the end of the entire release building and testing process, this branch will be pushed
up to the official OpenMDAO-Framework repository by the ``release finalize`` command.


Release Testing
~~~~~~~~~~~~~~~

The ``release test`` command is used to test a release by running ``openmdao_test``
on a group of remote hosts.  It can also be used to test an existing 
production release on a specific host. Running it with a ``-h`` option 
will display the following:


::

    usage: release test [-h] [-c CONFIG] [--host HOST] [-o OUTDIR]
                        [--filter FILTERS] [--all] [-k] [-f FNAME]
                        [--testargs TESTARGS]
                        [fname]

    test an OpenMDAO release

    positional arguments:
      fname                 pathname of release directory or go-openmdao.py file

    optional arguments:
      -h, --help            show this help message and exit
      -c CONFIG, --config CONFIG
                            Path of config file where info for hosts is located
      --host HOST           Select host from config file to run on. To run on
                            multiple hosts, use multiple --host args
      -o OUTDIR, --outdir OUTDIR
                            Output directory for results (defaults to
                            ./host_results)
      --filter FILTERS      boolean expression to filter hosts
      --all                 Use all hosts found in testhosts.cfg file
      -k, --keep            Don't delete the temporary build directory. If testing
                            on EC2 stop the instance instead of terminating it.
      --testargs TESTARGS   args to be passed to openmdao_test


The positional argument *fname* is used to specify either the ``go-openmdao.py`` file that 
builds the release environment or the path to a directory that was built 
using the ``release build`` command.

If you run the ``release test`` command without supplying ``--all`` or ``--host=``, it will
test the release on localhost.  For example:

::

    release test rel_0.2.1 --testargs=-x
    
will test the release locally.  It's a good idea to do this before running ``release test`` 
with a ``--all`` because it can save the time and cost of starting up multiple EC2 instances,
only to find that they all have the same failure.  Also, the ``--testargs`` option can save
some time.  Setting ``--testargs=-x`` will cause the script to return immediately if any test
fails, rather than running the complete test suite before returning.

If ``release test`` succeeds locally, then the next step is to run it on the full set of 
test hosts.  This can be done as follows:

::

    release test rel_0.2.1 --all
    
    
.. note:: It's highly recommended that you add an OS X host to the hosts in your
          ``testhosts.cfg`` file, because by default no OS X machine will be tested.
          At the bottom of the ``config/testhosts.cfg`` file in the repository there is
          an example of an OS X host.

Assuming all of the ``release test`` commands succeeded, the final step is to run
``release finalize``, which will place the new release on the ``openmdao.org`` website
and update and tag the dev branch of the official OpenMDAO-Framework repository on GitHub.
Since this updates the dev branch, it will trigger automated branch tests.  

Running ``release finalize`` with ``-h`` will display the following help message:

::

    usage: release finalize [-h] [-v VERSION] [-d]

    push the release to the production area and tag the production repository

    optional arguments:
      -h, --help            show this help message and exit
      -v VERSION, --version VERSION
                            release version of OpenMDAO to be finalized
      -d, --dryrun          don't actually push any changes up to GitHub or
                            ``openmdao.org``


