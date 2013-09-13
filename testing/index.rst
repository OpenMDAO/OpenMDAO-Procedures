Remote Testing Tools
====================

The ``openmdao.devtools`` package contains a number of console scripts that
allow openmdao to be tested and built on remote hosts. This section describes
how to set up and use the scripts.

General Setup
-------------

Information about remote hosts is contained in a config file.  An example of such a file
is ``config/testhosts.cfg`` in the  OpenMDAO-Framework repository.  This file can be
copied to ``~/.openmdao/testhosts.cfg`` and modified to contain the hosts or EC2 images
you intend to test on.  The scripts look for this file in ``config/testhosts.cfg`` by
default but will fall back to ``~/.openmdao/testhosts.cfg`` if the config directory's
version doesn't exist. The reason for using the config directory's version as the
default is so that developers will see changes to the image ID numbers (as images are
updated, their numbers change) rather than have to manually keep updating their own
versions.  You can specify a different config file (e.g., your ``~/.openmdao`` version)
on the command line using the ``-c`` argument.

Aside from the [DEFAULT] section, the file has one section per 
host or EC2 image.  The section name is used as a short alias for that host 
and is used with the ``--host=<section_name>`` arg in the testing and building scripts.

In all of the scripts described below, adding ``--host=<section_name>`` will cause 
the script to `do whatever it does` on that host.  Supplying multiple ``--host`` args is
allowed.  If ``--all`` is used instead of ``--host=``, then the script will do its thing
on all of the hosts specified in the ``testhosts.cfg`` file, subject to certain internal
filtering.  For example, when running ``release test`` with ``--all``, only those hosts
in the config file with the option ``test_release=true`` will be used.


EC2 Specific Setup
------------------

To run the scripts on EC2 images or non-running instances, you must create
a ``~/.boto``  config file with the appropriate id and secret key. You may
also specify other information in the ``.boto file``, e.g., debug level.  An
example of a ``.boto`` file is shown below.


::

    [Credentials]
    aws_access_key_id = <your id here>
    aws_secret_access_key = <your secret key here>
    
    [Boto]
    debug = 0
    num_retries = 5
    
    #proxy = myproxy.com
    #proxy_port = 8080
    #proxy_user = <your proxy userid>
    #proxy_pass = <your proxy password>
    

The AWS id and the secret key can be obtained from an OpenMDAO framework
developer.


SSH keys
~~~~~~~~

You'll need an identity file to execute operations like starting and
stopping instances on EC2 using the *boto* package. For OpenMDAO
we use an identity file called ``lovejoykey.pem`` for all of our EC2 images
and instances. You can obtain this file from an OpenMDAO framework developer.
This file should be placed in the ``~/.ssh`` directory and its permissions
should be set to prevent access to anyone but you. Any
host that you plan to ssh into should have the public key corresponding to
``lovejoykey.pem`` in its ``authorized_keys`` file.  If the host is a non-EC2 host
and your personal public key is already in its ``authorized_keys`` file, you
should be able to run the scripts there without any additional setup.


Test Scripts
------------

Two types of testing are performed.  One is branch testing, which
is run on some designated branch in a Git repository. The other is release testing, which 
is run on an OpenMDAO release (or the files that make up a release).

There are some differences in inputs and outputs for the two scripts, but several things
are common between them. This section talks about the common things.

The ``-h`` and the ``--help`` command-line options will display all 
of their allowed arguments.

Output is written to the output directory specified using the ``-o`` option.  The tests
run concurrently and write their outputs to  ``<outdir>/<host_config_name>/run.out``
where ``outdir`` defaults to ``host_results``, and ``host_config_name`` is the section
name for that host in the config file. So, for example, if the script were run with a
``--host=natty32_py27`` arg, the results for the ``natty32_py27`` host would be found in
``host_results/natty32_py27/run.out`` file.

The ``--testargs`` option can specify args that are passed to 
``openmdao_test`` on the remote host.  Adding a ``--testargs=-x``, for example, 
would cause the test to end as soon as any test on the remote host failed.
Adding the name of a specific module to test can also be a big time-saver
when debugging a specific test failure.

The ``--host`` arg specifies a remote host to run on and can be used multiple 
times to specify more than one host.

The ``--all`` arg will cause all hosts in the ``testhosts.cfg`` file to be used for the
test, subject to filtering based on the value of the ``test_release`` and
``test_branch`` options for that host in the file.


test_branch
~~~~~~~~~~~
When a developer has completed changes to the code, committed them and is ready to submit a pull request, that developer, before pushing the code up to his/her fork, should run a test_branch.  This is the best indicator as to how the new code will work and play with the various platforms.  Just running openmdao_test on a single machine is not enough.  To vastly improve the chances of not breaking the dev branch, all developers should run this script before even considering a pull request.

The ``test_branch`` script is used to test a branch running ``openmdao_test`` 
on a group of remote hosts. Running it with the ``-h`` option will display the following:

::
    
    usage: test_branch [-h] [-c CONFIG] [--host HOST] [-o OUTDIR]
                       [--filter FILTERS] [--all] [-k] [-f FNAME] [-b BRANCH]
                       [-t TESTARGS]

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
      -f FNAME, --file FNAME
                            Pathname of a tarfile or URL of a Git repo. Defaults
                            to the current repo.
      -b BRANCH, --branch BRANCH
                            If file is a Git repo, supply branch name here
      --testargs TESTARGS
                            args to be passed to openmdao_test



The script can test the current (committed) branch of a Git repository, 
a tarred repository, or a specific branch of a specified local or remote Git 
repository, depending upon the nature of the ``-f`` (or ``--file=``) arg.  
If a Git repository is specified rather than a tar file, then
the branch must also be specified. If no ``-f`` is supplied, the current
branch of the current repository is used.


release test
~~~~~~~~~~~~

Release testing is done using the ``release test`` command.  See the section on
:ref:`Release-Testing` for details.


