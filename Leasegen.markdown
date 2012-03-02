inventario-leasegen is a program which interacts with inventario, querying information about the schools and automatically creating activations (leases) for the XOs in each school.

# How it works

inventario-leasegen logs into a dedicated user account in inventario and requests data.

In response, inventario considers each school for which it has a school server registered.

For each of these schools, inventario examines the list of students and creates a list of all of the laptops in the school. More specifically, it looks at the list of people under the school's place (including sub-places such as each grade), and finds all the laptops that are either assigned to or in the hands of such people. Each list entry includes the serial number and the UUID of the laptop.

The resultant list of laptops is filtered: only laptops in "activated" or "buen estado" status which have a UUID recorded are included. These by-school laptop lists, the school server name, and the suggested expiry date for any new activations are all sent from inventario to inventario-leasegen.

Upon receiving the laptop lists and associated data, inventario-leasegen processes each laptop one-by-one, generating an activation lease. The resultant lease is stored twice: once in a one-file-per-school JSON format, and again in a split-out one-file-per-laptop flat file format.

inventario-leasegen is intelligent: it will avoid redundantly regenerating activations for a school providing all the following conditions are met:
 1. We have previously generated activations for this school, and
 2. The list of laptops in this school has not changed since last time, and
 3. The previously generated activations are sufficiently new (e.g. less than 1 week old).

Point 3 is important: as activations have a limited lifespan, they must be periodically regenerated (so that the laptop leases are renewed) even when the school's laptop list has not changed.

If the above conditions are not met, inventario-leasegen will regenerated leases for the entire school. This is a fast process: an Intel Core2duo E7500 CPU can generate over 30 activations per second (using only a single core).

inventario-leasegen only writes the generated leases to local storage; the challenge of delivering these leases to the laptop must then be handled by other processes. The on-disk format is designed to be compatible with existing tools in this area. Options include:

1. Place the school-wide JSON file on USB and plug it into each and every XO in a school to (re)activate them.
2. Place the school-wide JSON file on USB and plug it into a school server for automatic input to xs-activation: the laptops will then be activated automatically (via the wireless network) from the school server.
3. Automatically transport the school-wide JSON file to the school server over the internet (using e.g. puppet) every time it is updated, and feed it into xs-activation: this will achieve a fully-hands-off inventario-driven lease generation system.
4. Point oatslite to the one-file-per-laptop output in order to serve activation renewals over the internet before individual activations expire.

# Prerequisites

An operational inventario installation is obviously required, and as the data source, inventario must have:
 1. Each and every laptop registered in its database, with the laptop UUIDs
 2. Assignations or movements specifying the collections of laptops that are in each school
 3. School servers and activation durations registered for each school (see Technical support -> School servers -> List school servers)

A custom person and user must be created in inventario. This is similar to any administrative inventario account, but the person should have the role "extern system" under the root place, and a good password should be chosen for the corresponding user account.

OLPC's bios-crypto tools must be installed, compiled and available to the user that runs inventario-leasegen. Similarly, the keypair (including private key) needed to create and verify leases must be available.

The current version of inventario-leasegen is targetted to be run on a Fedora 16 host.

# Setup

Create /etc/yum.repos.d/inventario.repo with the following contents:

    [inventario]
    name=Inventario  
    baseurl=http://dev.laptop.org/~dsd/inventario-repo  
    enabled=1  
    gpgcheck=0

Install the package:

    # yum install inventario-leasegen

Adjust configuration as necessary:

    # cp /var/inventario-leasegen/etc/leasegen.conf{.example,}
    # nano /var/inventario-leasegen/etc/leasegen.conf

You will need to populate the required options and ensure that a writable path exists for the activation lease output.

Now run the program for the first time:

    # inventario-leasegen

Examine the output log to confirm that everything worked:

    # cat /var/inventaro-leasegen/var/leasegen.log

Finally, set up a crontab to run inventario-leasegen at regular intervals. leasegen will abort automatically if another instance is already running, so it is safe to run this at high frequency.

# Tips and tricks

The log file can be found at /var/inventaro-leasegen/var/leasegen.log

As noted above, leasegen will not re-generate leases if it thinks that the existing leases are new and match the current set of laptops that belong to the school. If you wish to force re-generation, remove the .checksum file that corresponds to the school in /var/inventario-leasegen/var/, or to force re-generation for all schools:

    # rm /var/inventario-leasegen/var/*.checksum
