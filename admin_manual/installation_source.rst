Manual Installation
-------------------

If you do not want to use packages, here is how you setup ownCloud on from scratch
using a classic :abbr:`LAMP (Linux, Apache, MySQL, PHP)` setup:

Prerequisites
~~~~~~~~~~~~~

To run ownCloud, your webserver must have the following installed:

* php5 (>= 5.3)
* php5-gd
* php-xml-parser
* php5-intl

And as *optional* dependencies:

* php5-sqlite (>= 3)
* php5-mysql
* smbclient
* curl
* libcurl3
* php5-curl

You have to install at least one of php5-sqlite or php5-mysql, depending
on which of the two database systems you want to use.

smbclient is only used if you want to mount SMB shares to your owncloud.
The curl packages are needed for some apps (e.g. http user authentication)


Commands for Ubuntu and Debian (run as root):

::

  apt-get install apache2 php5 php5-gd php-xml-parser php5-intl
  apt-get install php5-sqlite php5-mysql smbclient curl libcurl3 php5-curl

.. todo:: Document other distributions.

You don’t need any WebDAV support of your webserver (i.e. apache’s mod_webdav)
to access your ownCloud data via WebDAV, ownCloud has a WebDAV server built in.
In fact, you should make sure that any built-in WebDAV module of your webserver
is disabled (at least for the owncloud directory), as it can interfere with
owncloud's built-in WebDAV support.

Extract ownCloud and Copy to Your Webserver
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  tar -xjf path/to/downloaded/owncloud-x.x.x.tar.bz2
  cp -r owncloud /path/to/your/webserver

Set the Directory Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The owner of your webserver must own the apps/, data/ and config/ directories
in your ownCloud install. You can do this by running the following command for
the apps, data and config directories:

::

  chown -R www-data:www-data /path/to/your/owncloud/install/data

Replace ``www-data:www-data`` with the user and group of the owner of your
webserver.

.. note:: The ``data/`` directory will only be created after setup has run
          (see below) and is not present by default in the tarballs.

Enable .htaccess and mod_rewrite if Running Apache
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you are running the apache webserver, it is recommended that you enable
``.htaccess`` files as ownCloud uses them to enhance security and allows you to
use webfinger. To enable .htaccess files you need to ensure that
``AllowOverride`` is set to ``All`` in the ``Directory /var/www/`` section of
your virtual host file. This is usually in
``/etc/apache2/sites-enabled/000-default``.  You should also run ``a2enmod
rewrite`` and ``a2enmod headers``. Then restart apache: service apache2 restart
(for Ubuntu systems). In order for the maximum upload size to be configurable,
the .htaccess file in the owncloud folder needs to be made writable by the
server.

Follow the Install Wizard
~~~~~~~~~~~~~~~~~~~~~~~~~

Open your web browser and navigate to your ownCloud instance. If you are
installing ownCloud on the same machine as you will access the install wizard
from, the url will be: http://localhost/ (or http://localhost/owncloud).  For
basic installs we recommend SQLite as it is easy to setup (ownCloud will do it
for you). For larger installs you should use MySQL or PostgreSQL. Click on the
Advanced options to show the configuration options. You may enter admin
credentials and let ownCloud create its own database user, or enter a
preconfigured user.  If you are not using apache as the webserver, please set
the data directory to a location outside of the document root. See the advanced
install settings.

Test your Installation
~~~~~~~~~~~~~~~~~~~~~~

Login and start using ownCloud. Check your web servers errror log. If it shows
error, you might have missed a dependency or hit a bug with your particular
configuration.

If you plan on using the Webfinger app and
your ownCloud installation is not in the webroot then you’ll have to manually
link ``/var/www/.well-known`` to ``/path/to/your/owncloud/.well-known``.
