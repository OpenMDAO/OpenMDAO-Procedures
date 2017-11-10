****************************
OpenMDAO 2.0 Release Process
****************************

as of 2017.10.25, by Keith G. Marsteller

Pivotal
-------

Make a release story on Pivotal for 2.x.y, and Start it.


Git
----

Make sure everything you want for the release is merged in on Github and has tested successfully.
Get your local git repo’s master clean and up to date with OpenMDAO/OpenMDAO@master; run tests locally.
If you pass, great, move on.

Create a branch named <release number>
git checkout -b 2.0.0

Now change version number in the following files:
openmdao/__init__.py
setup.py (version number AND download url)

Then it’s time to write some release notes.  Write release notes in the format that you see there, latest release notes go on top of the file, not at the end.
release-notes.txt

Once again, run all the tests and build the docs, and pass them before continuing.  Seriously. Actually do this.
Make any fixes that need to be done until it all passes.

Commit changes with a descriptive message.
git commit -am “Updating versions/notes for 2.0.0 release”

Once you pass, push to your own fork:
git push myfork 2.0.0

Using github.com, create a pull request from, for example, kmarsteller/OpenMDAO@2.0.0 to OpenMDAO/OpenMDAO@master
This would be a good time in slack to tell everyone to NOT ACCEPT any new PRs.
Upon creation of this pull request, continuous integration testing will run.  Wait for it to complete successfully.
If it works, MERGE IT. If it doesn’t work and there’s a problem, close this pull request, fix the problem with a different branch and issue a different pull request until that issue is fixed.  Then basically start this whole process over.

Once the pull request from your release branch has been accepted, and THOSE CI tests have passed, it’s time to “tag” the release.
This must happen before any other commits hit master.
git tag will show all the tags that exist up until now.

git tag 2.0.0 will locally create a new tag, 2.0.0.  You can git tag again to see that it exists now.
For minor changes to the code, we will increment the third digit, e.g. 2.0.1
For API changes to the code, we will increment the second digit, e.g. 2.1.0

Now we need to get that tag up to master:  (Assuming you’re an OpenMDAO owner…)
git push origin —-tags
When you do this, you should see the tags go up.

Now if you switch branches off of your 2.0.0 branch to your master branch:
git checkout master
Do git tags on this branch, and you’ll see that 2.0.0 is not there yet.  It only exists on your 2.0.0 branch and on OpenMDAO@master.

You can now pull back from master, to see if the tag made it and to update your master:
git pull origin master (should get a message that a new tag has been added.)

git tag  (should see the new tag in list)

You can also go to github.com website and look at OpenMDAO repo…in the same dropdown as branches, there’s a tab for tags also.  Make sure your new tag is there.

Go ahead and git checkout 2.0.0 again for the next section.

pypi
----

To be able to do anything on pypi, you need a  ~/.pypirc file that looks like this:

::

    [distutils]
    index-servers =
      pypi
      pypitest

    [pypi]
    repository: https://upload.pypi.org/legacy/
    username: openmdao
    password: xxxxxxxxxxxxxxxx

    [pypitest]
    repository: https://test.pypi.org/legacy/
    username: openmdao
    password: xxxxxxxxxxxxxxxxx


First, we need to build a source distribution, so from the top level, where setup.py lives.:
python setup.py sdist

This will create a dist directory, in which lies openmdao-2.0.0.tar.gz

Finally, we upload this file up to pypi using twine:
twine upload dist/openmdao-2.0.0.tar.gz

You should be able to see the dist upload.
Then go to our page at pypi and make sure it’s there.

Docs
----

Docs for a release will build on one particular travis machine, and then will be uploaded from there to our webfaction server.
See the procedure doc for docbuilding for more details.

News Post
---------

Update the news page on openmdao.org/news.  This usually involves some kind of summary of release notes and a link to our
stack overflow tag if there are problems.  http://openmdao.org/wp-login.php

Twitter
-------

Send out a Tweet from twitter.com/OpenMDAO touting this amazing release!

Pivotal
--------
Mark release story as “done” on Pivotal. try to make sure that before you do, any and all stories that are included in, e.g. 2.0.0 are approved and listed as done, so that the stories that lie between release tags will really have been completed between those releases.

Slack
-----

Tell the team that it’s done.  Wait for everything to fall apart.
