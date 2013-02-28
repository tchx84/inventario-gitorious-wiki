# About

Inventario is a complete system for managing logistics of OLPC XO laptop deployments. It is used heavily in OLPC projects executed by [ParaguayEduca](http://www.paraguayeduca.org/) (10,000+ laptops in over 40 schools in Paraguay) and [Fundación Zamora Teran](http://www.fundacionzt.org/) (20,000+ laptops in over 80 schools in Nicaragua).

Inventario was orignally developed within ParaguayEduca, and the ParaguayEduca wiki continues to document installation instructions for their version of inventario, for which development was stopped for a long time. During that time, the project was adopted by the main OLPC project in Nicaragua and improvements were made in collaboration with Martin Abente, one of the original inventario architects from Paraguay. In the present day, the version of inventario found here is used by Nicaragua; the OLPC Paraguay project remains with an older version (documented on the ParaguayEduca wiki) but is expected to migrate to this version as time permits.

## Features
* Management of schools, teachers, students, laptops, school servers
* Tracks laptop movements, repairs
* Produces barcode stickers to help with the process of assigning laptops to students
* Supports varying levels of access for different users
* And much more

## Implementation details
* Web-application
* Client-side: uses Qooxdoo javascript-based UI, works in all major browsers
* Server-side: Ruby on Rails, MySQL, intended for use on Fedora Linux

# Server requirements

Fedora Linux is the target platform; the installation documentation and packages are specifically targetted at Fedora. The software should also work fine on other distributions, but with non-Fedora setups you will be on your own in terms of installation, configuration and support.

The software has been developed against Ruby 1.9.3 and Rails 3.2, intended for use on Fedora 18.

# Installation

See [[Installation]] and [[UpgradeNotes]]

# Documentation

* Managing laptop [[SoftwareVersions]]

Somewhat outdated/incomplete manuals:

* [English manual](http://wiki.paraguayeduca.org/index.php/Inventario_manual/en)
* [Spanish manual](http://wiki.paraguayeduca.org/index.php/Inventario_manual)

# Development

See [[Development]]

# Add-ons

* [[Leasegen]]