Upgrading inventario can be done with "yum update" like any other package. This page lists any special considerations that must be kept in mind.

# Upgrading v0.5 to v0.6

When upgrading from v0.5 to v0.6 (e.g. updating to Ruby-1.9), we had to make some changes (improvements) to work with Ruby's improved unicode support. This exposed the fact that we were previously storing UTF-8 data in latin1 columns in the MySQL database. As this raw UTF8 data will now be encoded as latin1 under the new setup, you need to perform a one-off conversion to UTF-8 with:

    rake utf8_migration:run