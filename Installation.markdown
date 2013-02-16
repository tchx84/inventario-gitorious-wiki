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

Install Passenger (this is not packaged in Fedora, so the installation is a bit long winded)

    # yum install ruby-devel httpd-devel apr-devel gcc gcc-c++ make curl-devel openssl-devel zlib-devel
    # gem install passenger
    # passenger-install-apache2-module

> Follow the instructions to complete the Passenger installation. **Be sure to read the output of the final command, as you must make another change to the apache config (read the instructions)**.

Add the following to `/etc/httpd/conf.d/passenger.conf`
    PassengerDefaultUser apache
    PassengerDefaultGroup apache
> It's not exactly clear why this is needed. According to the <a href="http://www.modrails.com/documentation/Users%20guide%20Apache.html#PassengerUserSwitching">passenger docs</a>, PassengerUserSwitching (on by default) should cause yaas-web to run as the apache user (which is the owner of config/environment.rb). However, this seems to be broken at the time of writing: it runs as 'nobody' and is hence unable to write log files in /var/inventario/log. Adding these lines acts as a simple workaround.

If using SELinux, enable apache to run in permissive mode

    # semanage permissive -a httpd_t

>This command enables apache to run in "permissive" mode, meaning that your whole apache installation ignores all policies normally enforced by SELinux. I tried, and ran out of patience before being able to produce a leaner way to get passenger and SELinux working together without error.

Modify `/etc/httpd/conf/httpd.conf`, uncommenting the line <tt>NameVirtualHost *:80</tt> to enable name-based virtual hosting.

Now rename `/etc/httpd/conf.d/101-tracking.conf.example` (removing the .example extension) and customise file to change the ServerName to one that is appropriate for the system

Finally, (re)start apache.

    service httpd restart

You can now access the inventory system by the server name that you configured. The default username is admin and the default password is admin.