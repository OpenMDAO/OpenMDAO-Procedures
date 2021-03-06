Release Building, Testing, and Publishing
=========================================

After branch testing is complete, it may be time to create a new OpenMDAO
release. The ``release`` tool and its subcommands described below make the
process a little easier.  The basic commands to create and publish a release
are as follows:

::

    release build -b -v <version>
    release test rel_<version>  --testargs=-x # tests release locally
    release test rel_<version>  --all  --testargs=-x  # tests on all testhosts having test_release=true
    release finalize -v <version>

The following sections will give more detailed explanations of the steps shown above.

Preparation
----------------

In order to build a release, go to your OpenMDAO-Framework repo.  Make sure that you have no outstanding code changes on branches "dev" or "master," as you'll be working on them and want them to be clean.  The idea here is that the most recent release is sitting on branch "master," and the most recent development code is of course sitting on branch "dev." We obviously want the most recent code to get in to the release and end up on master.  If you don't have a local master branch, you'll need to create one, and to make sure your remotes are set up properly, with "origin" pointing to git://github.com/OpenMDAO/OpenMDAO-Framework.git and "myfork" (for example) pointing to git://github.com/<username>/OpenMDAO-Framework.git. When you're done with setup, you should be able to do this:

::

  kmarstel$ git remote -v
   myfork	https://github.com/kmarsteller/OpenMDAO-Framework.git (fetch)
   myfork	https://github.com/kmarsteller/OpenMDAO-Framework.git (push)
   origin	https://github.com/OpenMDAO/OpenMDAO-Framework.git (fetch)
   origin	https://github.com/OpenMDAO/OpenMDAO-Framework.git (push)

Once you have your dev and master branches set up properly, let's go to your dev branch, and make sure you have the latest code there:

::

   git checkout dev
   git pull origin dev

Once dev is updated, switch to your dev branch.  Then make sure you have the latest master.

::

   git checkout master
   git pull origin master

It's at this point that we will merge the changes from dev into master:

::

   git merge dev

Occasionally, manual help is needed to completely resolve the merge.  Be cautious.

Now, completely remove the devenv directory, and rebuild from scratch. Then run an "openmdao test" suite locally.  While that runs, perform the manual tests that are enumerated in OpenMDAO-Framework/openmdao.gui/src/openmdao/gui/test/functional/manual. Once you have successfully locally built and tested your updated master, create the release.

.. NOTE:: While the release is being built and tested, you should request that no new merges be made to the dev branch.


Release Creation
----------------

The ``release build`` command is used to build the required distribution tar
files for all of the OpenMDAO packages. It also builds the HTML version
of the docs and the ``go-openmdao.py`` bootstrapping installer file.

Running ``release build`` with the ``-h`` option will display the following:

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
      --basebranch BASE     base branch for release. defaults to master
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

.. _`Release-Testing`:

Release Testing
~~~~~~~~~~~~~~~

The ``release test`` command is used to test a release by running ``openmdao_test``
on a group of remote hosts.  It can also be used to test an existing
production release on a specific host. Running it with the ``-h`` option
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
test the release on localhost.  For example,

::

    release test rel_0.10.1 --testargs=-x

will test the release locally.  It's a good idea to do this before running ``release test``
with ``--all`` because it can save the time and cost of starting up multiple EC2 instances,
only to find that they all have the same failure.  Also, the ``--testargs`` option can save
some time.  Setting ``--testargs=-x`` will cause the script to return immediately if any test
fails, rather than running the complete test suite before returning.

If ``release test`` succeeds locally, then the next step is to run it on the full set of
test hosts.  This can be done as follows:

::

    release test rel_0.10.1 --all


.. note:: It's highly recommended that you add an OS X host to the hosts in your
          ``testhosts.cfg`` file because by default no OS X machine will be tested.
          At the bottom of the ``config/testhosts.cfg`` file in the repository is
          an example of an OS X host.

Assuming all of the ``release test`` commands succeeded, the final step is to run
``release finalize``, which will place the new release on the ``openmdao.org`` website
and update and tag the master branch of the official OpenMDAO-Framework repository on GitHub.
Since this updates the master branch, it will not trigger automated branch tests.  Only a push from master to dev will do that, which we will do after the finalize.

Before running release finalize, it helps to make sure you have git set up to run without needing a GitHub login, as this can interrupt the finalize process midway.  There are several pages on the web that show how to do this.

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

IMPORTANT!  Once the release has been finalized, you will need to then push master back to the dev branch, which will kick off a round of automated testing, and get the version number updated on the dev branch.  If you forget this, dev's version number will be off!


Release Notes
--------------

Once the release is finalized, there will be a directory created up on webfaction at: ``/home/openmdao/downloads/0.10.x``
You will need to go in there and create a file called "release_notes.html."  Check the format of other release notes for help, and use Pivotal Tracker to figure out what belongs in the release notes.


Wordpress Blog
--------------
A release is usually accompanied by a news piece on the openmdao.org blog that links to the downloadable go file and a link to the release notes.


Twitter Account
---------------
A new release usually warrants a Tweet from the OpenMDAO account, usually with a link to the blog story.


Plugin Tagging
--------------

Once a release has been completed, the OpenMDAO-Plugins need to be inspected and tagged. Follow these steps.

1. Have an activated env of the latest OpenMDAO ready that contains the newest release tags.

2. See if a plugin has changed since the last OpenMDAO release. You can get a date from the website's downloads page.
   GitHub's OpenMDAO-Plugins page lists the date of the most recent changes. Usually only a few will have changed since the previous OpenMDAO release.

3. If a plugin has changed, pull those changes to your local repo.  If you don't have a local repo, it's time to make one.

4. Increment the version number in the ``setup.cfg`` file. Save the file.

5. From within that plugin's directory (activated), run ``plugin makedist``

6. Commit changes with ``git commit -a -m "Comment"``

7. Update the actual Git tags as such:

   ``git tag -a 0.x.x -m "Tagging for OpenMDAO release 0.10.x"``

   where ``0.x.x`` is the newly-incremented version number of the plugin, NOT the OpenMDAO version.

8. Push the tags directly back up to their repository. **DANGER, don't screw this up!**

   ``git push origin master --tags``

.. note::

   An advanced user might decide that a documentation change doesn't necessitate an increment in the version. In such a case, the user might
   skip Step 4 and instead move the current tag to the latest commit by following the steps below.


1. Deleting the current version's tag:

   ``git tag -d 0.x.x``

2. Pushing that deletion up to the server:

   ``git push origin :refs/tags/0.x.x``

3. Re-doing the same tag on the new code. Resume at Step 5 above and re-tag with the same number ``0.x.x`` as in Step 7.
