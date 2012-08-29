This page documents various procedures that are needed for inventario development. It assumes you are working in a git checkout of inventario.

# Install qooxdoo

To compile the GUI, you must [download the Qooxdoo Desktop SDK](http://qooxdoo.org/download)

Use the latest version. Extract it, rename the directory to `qooxdoo-sdk`, and place it within the `public` directory within the inventario git checkout.

# Compile the GUI

The first time you configure your development environment, and every time you change the set of classes inside `gui` (e.g. adding or removing a class, using a new part of Qooxdoo that was previously unused), you need to recompile and reinstall the GUI. 

    # rake gui:generate

# Initialise database

The first time you develop, you need to configure the MySQL server. In the `config` directory, copy `database.yml.example` to `database.yml` and change the username/password (if necessary).

To initialize or reset the database:

    # mysql -u root -e 'drop database if exists inventario; create database inventario character set utf8mb4;'
    # rake seed_data:install
    # rake db:migrate
    # rake seed_data:setup
    # rake seed_data:fix

# Execute local test server

You can run a HTTP server directly from the codebase, viewing the requests and errors in your terminal, without having to configure a webserver.

    # rails server

Now you can navigate to [http://127.0.0.1:3000](http://127.0.0.1:3000)

# Database changes

During development, if you want to make a change to the database structure (e.g. add a new table, change some columns), you have to use Rails' migration system. This builds a change history of the modifications we make to the database structure.

[This page](http://weblog.jamisbuck.org/2005/9/27/getting-started-with-activerecord-migrations) explains it well.

For example:

    # rails generate migration MiNuevaMigracion

Now you have a new file in `db/migrations` and you can write code to make the change. Use the existing migrations as examples. Then:

    # git add db/migrations/NEWFILE
    # rake db:migrate

# Update translations

The code and strings should be written in English. We use gettext to translate the user-visible strings to Spanish.

To update the translations:

    # rake gettext:find

Now you can update `translation/po/es/inventario.mo`

The GUI has its own translations. Generate the .mo files with:

    # rake gui:generate[translation]

Now you can update `public/gui/source/translation/es.po`.

In both cases, there is no need to compile the `.po` files into the `.mo` format. Finally, create a git commit with the updates.

# Working with ActiveRecord

Here are some useful links for working with ActiveRecord:

* [ActiveRecord README](http://api.rubyonrails.org/files/activerecord/README.html)
* [ActiveRecord Base class](http://api.rubyonrails.org/classes/ActiveRecord/Base.html)

# Working with Qooxdoo

* [Qooxdoo API](http://demo.qooxdoo.org/current/apiviewer/)

# Working with Ruby

* [To Ruby from Python](http://www.ruby-lang.org/en/documentation/ruby-from-other-languages/to-ruby-from-python/)
* [Ruby in 20 minutes](http://www.ruby-lang.org/en/documentation/quickstart/)

# Tips and tricks

* When you change something inside app/, there's no need to restart the session nor the rails server. Your changes take effect immediately.
* The system may feel slow to load and slow to use in your development environment. When you package it for installation on a real system, the final product is much much faster.

# Packaging

Place the qooxdoo SDK in `/usr/share/qooxdoo-sdk`

Configure your system for RPM building:

    # yum install rpmdevtools yum-utils fedora-packager 
    # rpmdev-setuptree

In your development environment, commit your changes.

Build an RPM package:

    # rake packaging:buildrpm

The output RPMs will be saved in ~/rpmbuild/RPMS/