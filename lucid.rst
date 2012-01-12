==============================================
INSTALLING A KARL DEV BUILDOUT ON UBUNTU LUCID
==============================================


Before you begin
----------------

As root, Grant sudo permissions to your normal user

# adduser <yourusername> sudo

As root, Make sure you have a localhost alias

# [ -f /etc/hosts ] || echo '127.0.0.1    localhost' > /etc/hosts


Predependencies
---------------


As root, In /etc/apt/sources.list:

    deb http://archive.ubuntu.com/ubuntu lucid main universe multiverse restricted

Install some packages:

    sudo apt-get update

    # required
    sudo apt-get install -y python-setuptools gcc libc6-dev python2.6-dev zlib1g-dev libjpeg-dev git-core subversion

    # reccommended for binary file indexing
    sudo apt-get install -y aspell poppler-utils wv catdoc

    # optional personal tools
    sudo apt-get install -y jed ssh-client 

Install virtualenv:

    sudo easy_install virtualenv


KARL dev buildout
-----------------

Fetch the source

    git clone git://github.com/karlproject/dev-buildout.git karl

Later, when rebuilding, clean the buildout

    git clean -dfqx

Initialize the buildout

    virtualenv --clear -p python2.6 --distribute .
    bin/python bootstrap.py

Build using static lxml

    bin/buildout -c lxml.cfg


Relstorage
----------

Install PostgreSQL 9.1:

    sudo apt-get install -y python-software-properties
    sudo add-apt-repository ppa:pitti/postgresql
    sudo apt-get update
    sudo apt-get install -y postgresql libpq-dev

Switch to the postgres user to perform some database actions:

    sudo su - postgres

Add a testuser

    createuser -P karltest

(Enter 'test' for password.  Repeat.  Answer 'n' to next three questions.)

Create a test database

    createdb -O karltest karltest

Later, if you want to blow away the database and start over::

    dropdb karltest; createdb -O karltest karltest

Finally, log out of the postgres user

    logout

This drops you back into your normal user shell.


Running Karl
------------

Start the server in foreground mode

    bin/karserve serve

Open the site at http://localhost:6543/pg

    login: admin
    password: admin


Demo content
------------

You may want to add some demo content.

Stop the server

    ^C

Setup sample content

    bin/karlserve samplegen

Start the server

    bin/karlserve serve

Now add an intranet:
    - Goto http://localhost:6543/pg/add_community.html
    - Add the community
    - Goto the 'intranets' tab
    - Add an intranet
