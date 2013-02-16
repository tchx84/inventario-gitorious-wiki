Create `/etc/yum.repos.d/inventario.repo` with the following contents:

    [inventario]
    name=Inventario
    baseurl=http://dev.laptop.org/~dsd/inventario-repo/f$releasever
    enabled=1
    gpgcheck=0


Install and enable mysql
    # yum install mysql-server
    # chkconfig mysqld on
    # service mysqld start


The inventario installation expects the `root` mysql user to have no password (you can add one after), and requires mysqld to be running (as above). Install it now:

    # yum install inventario

Install Passenger:

    # yum install mod_passenger

If using SELinux, enable apache to run in permissive mode

    # yum install selinux-policy-devel policycoreutils-python
    # semanage permissive -a httpd_t
    # semanage permissive -a passenger_t

>This command enables apache to run in "permissive" mode, meaning that your whole apache installation ignores all policies normally enforced by SELinux. I tried, and ran out of patience before being able to produce a leaner way to get passenger and SELinux working together without error.

Copy the inventario apache config into place, and customise the file to change the ServerName to one that is appropriate for the system
    # cp /usr/share/doc/inventario-*/inventario.conf /etc/httpd/conf.d
    # $EDITOR /etc/httpd/conf.d/inventario.conf

Finally, (re)start apache.

    service httpd restart

You can now access the inventory system by the server name that you configured. The default username is admin and the default password is admin.