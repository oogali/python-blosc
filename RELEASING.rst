=======================
Releasing python-blosc
=======================

:Author: Francesc Alted
:Contact: faltet@gmail.com
:Date: 2012-09-16


Preliminaries
-------------

* Make sure that ``RELEASE_NOTES.rst`` and ``ANNOUNCE.rst`` are up to
  date with the latest news in the release.

* Check that ``VERSION`` file contains the correct number.

* Commit the changes::

  $ git commit -a -m"Getting ready for release X.Y.Z"


Testing
-------

* After compiling, run::

  $ PYTHONPATH=.   (or "set PYTHONPATH=." on Win)
  $ export PYTHONPATH=.  (not needed on Win)
  $ python -c "import blosc; blosc.test()"

* Run the test suite in different platforms (at least Linux and
  Windows) and make sure that all tests passes.


Updating the online documentation site
--------------------------------------

* Go to the doc directory::

  $ cd doc

* Make sure that the `version`/`release` variables are updated in
  'conf.py'.

* Make the html version of the docs::

  $ rm -rf _build/html
  $ make html

* Make a backup and upload the files in the doc site (xodo)::

  $ export UPSTREAM="/home/blosc/srv/www/python-blosc.blosc.org"
  $ ssh blosc@xodo.blosc.org "mv $UPSTREAM/docs/html $UPSTREAM/docs/html.bck"
  $ scp -r _build/html blosc@xodo.blosc.org:$UPSTREAM/docs

* Check that the new manual is accessible in http://python-blosc.blosc.org

* If everything is fine, remove the backup of the previous manual::

  $ ssh blosc@xodo.blosc.org "rm -r $UPSTREAM/docs/html.bck"

* Go up to the root directory for further proceeding with packging::

  $ cd ..


Packaging
---------

* Make the tarball with the command::

  $ python setup.py sdist

Do a quick check that the tarball is sane.


Uploading
---------

* Register and upload it also in the PyPi repository::

    $ python setup.py sdist upload


Tagging
-------

* Create a tag ``X.Y.Z`` from ``master``.  Use the next message::

    $ git tag -a vX.Y.Z -m "Tagging version X.Y.Z"

* Push the tag to the github repo::

    $ git push
    $ git push --tags


Announcing
----------

* Send an announcement to the blosc, numpy list and python-announce
  lists.  Use the ``ANNOUNCE.rst`` file as skeleton (or possibly as
  the definitive version).


Post-release actions
--------------------

* Edit ``VERSION`` in master to increment the version to the next
  minor one (i.e. X.Y.Z --> X.Y.(Z+1).dev0).

* Also, update the `version` and `release` variables in doc/conf.py.

* Create new headers for adding new features in ``RELEASE_NOTES.rst``
  and empty the release-specific information in ``ANNOUNCE.rst`` and
  add this place-holder instead:

  #XXX version-specific blurb XXX#

* Commit your changes with:

  $ git commit -a -m"Post X.Y.Z release actions done"


That's all folks!


.. Local Variables:
.. mode: rst
.. coding: utf-8
.. fill-column: 70
.. End:
