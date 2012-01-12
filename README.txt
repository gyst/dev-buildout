=============================
Get Started Quickly With Karl
=============================

If you're on Ubuntu 10.4 Lucid, see lucid.rst for installation instructions.


PostgreSQL
----------

Karl requires PostgreSQL be installed on your system.

If you are on OSX, this is reported to work::

  $ sudo port install postgresql90
  $ sudo port install postgresql90-server

Link pg_config to a place that is in the path:

  $ sudo ln -s /opt/local/lib/postgresql90/bin/pg_config /usr/local/bin/

Alternately, add /opt/local/lib/postgresql90/bin/ to your path.

Relstorage
----------

As the 'postgres' user, create the user and database for the PostgreSQL/Relstrage based instance of
Karl::

  $ createuser -P karltest
    (Enter 'test' for password.  Repeat.  Answer 'n' to next three questions.)
  $ createdb -O karltest karltest

Later, if you want to blow away the database and start over::

  $ dropdb karltest; createdb -O karltest karltest


Buildout
--------
Check out the buildout from github::

  $ git clone git://github.com/karlproject/dev-buildout.git karl
  $ cd karl

Create a virtual environment and run the buildout::

  $ virtualenv -p python2.6 --no-site-packages .
  $ bin/python bootstrap.py
  $ bin/buildout

Karl is now built and ready to run.  Run Karl using Paste HTTP server in the
foreground::

  $ bin/karlserve serve

Alternatively, you can use Paster::

  $ bin/paster serve etc/karlserve.ini

Visit the Relstorage based test instance at::

  http://localhost:6543/pg

Default login and password are admin/admin.


Customization Packages
----------------------

Both instances are 'vanilla' instances of Karl which do not use any
customization package. Most customers that are not OSI, going forward, will
not use any customization package. To make the pg instance use the 'osi'
customization package::

  $ bin/karlserve settings set pg package osi
  $ bin/karlserve serve (restart if already running)

To revert back to vanilla::

  $ bin/karlserve settings remove pg package

Hacking
-------

To hack on some source code::

  $ bin/develop co karl
  $ bin/buildout -No

Source code will now be in src/karl and src/karlserve.

When playing with the code it's usually very useful to have some sample
content added to the site, so that it looks a bit closer to a real site.
The karlserve command can be used for that::

  $ bin/karlserve samplegen

Using this command 10 sample communities will be added to the site, each
with their own wikis, blogs, calendars and files.

The samplegen command does not create intranets, so they need to be added
manually if they are required. To do that visit your instance at:

  http://localhost:6543/pg/add_community.html

Fill the form to add a community, making sure the 'intranets' checkbox is
selected. An 'intranets' tab will be visible on the community pages after
that, from which new intranets can be added.

If you need to work with versioning, you need to initialize the repository
before the versioning UI will show up. This is done with::

  $ bin/karlserve init_repozitory pg

Enjoy!

