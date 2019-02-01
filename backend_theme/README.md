# Odoo 12.0 Tutorial

This tutorial is aimed towards new and starting Odoo developer, Odoo is structured like MVC it has Model,View and a Controller


# Getting Started

Hold on there cowboy, lets start by setting Odoo first, this tutorial will focus on Linux particularly Debian (it should work as well with Ubuntu), for Windows or Mac user you need to use VM for this one i recommend [Virtualbox](https://www.virtualbox.org/wiki/Downloads). This installation  is specific to version 12.0 **ONLY** it may or may not work on other version. So you have been warned!

### Installation
Odoo 12.0 'deb' package currently supports `Debian Stretch`_, `Ubuntu 18.04`_ or above.

#### Prepare

Odoo needs a `PostgreSQL`_ server to run properly. The default configuration for the Odoo 'deb' package is to use the PostgreSQL server on the same host as your Odoo instance. Execute the following command as root in order to install PostgreSQL server :

    # apt-get install postgresql -y

In order to print PDF reports, you must install wkhtmltopdf_ yourself:
the version of wkhtmltopdf_ available in Debian repositories does
not support headers and footers so it is not used as a direct dependency.

    wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.4/wkhtmltox-0.12.4_linux-generic-amd64.tar.xz
    tar xf  wkhtmltox-0.12.4_linux-generic-amd64.tar.xz    
    cd ./wkhtmltox/bin
    sudo mv wkhtmltopdf /usr/local/bin/wkhtmltopdf
    sudo mv wkhtmltoimage /usr/local/bin/wkhtmltoimage

#### Repository

Odoo S.A. provides a repository that can be used with  Debian and Ubuntu
distributions. It can be used to install Odoo Community Edition by executing the following commands as root:

    # wget -O - https://nightly.odoo.com/odoo.key | apt-key add -
    # echo "deb http://nightly.odoo.com/12.0/nightly/deb/ ./" >> /etc/apt/sources.list.d/odoo.list
    # apt-get update && apt-get install odoo

You can then use the usual ``apt-get upgrade`` command to keep your installation up-to-date.

#### Deb Package

Instead of using the repository as described above, the 'deb' package can be
downloaded [here](https://nightly.odoo.com/12.0/nightly/).

You can then use ``gdebi``:

    # gdebi <path_to_installation_package>

Or ``dpkg``:

    # dpkg -i <path_to_installation_package> # this probably fails with missing dependencies
    # apt-get install -f # should install the missing dependencies
    # dpkg -i <path_to_installation_package>

This will install Odoo as a service, create the necessary PostgreSQL_ user
and automatically start the server.

> **WARNING:** The 3 following python packages are only suggested by the Debian package. Those packages are not available in Ubuntu Xenial (16.04).

* python3-vobject: Used in calendars to produce ical files.
* python3-pyldap: Used to authenticat users with LDAP.
* python3-qrcode: Used by the hardware driver for ESC/POS

If you need one or all of the packages mentioned in the above warning, you can install them manually. One way to do it, is simply using pip3 like this:

    $ sudo pip3 install vobject qrcode
    $ sudo apt install libldap2-dev libsasl2-dev
    $ sudo pip3 install pyldap

> **WARNING:** Debian 9 and Ubuntu do not provide a package for the python module num2words. Textual amounts will not be rendered by Odoo
> and this could causeproblems with the "l10n_mx_edi" module.

If you need this feature, you can install the python module like this:

    $ sudo pip3 install num2words

#### Firewall
Sometimes  odoo service may not be accessible due tcp `8069` port being blocked by `iptables`. To to allow it has to be added into `iptables` and allow it, we will focus on `firewall-cmd`command since it is much easy than `iptables`, firewall-cmd is available once you install `firewalld`.

	# apt-get install firewalld
	# firewall-cmd --zone=public --add-port=8069/tcp --permanent
	# firewall-cmd --reload
	
#### Multicast DNS (Optional)
If you want to access you odoo installation via ZeroConf you need to setup first the domain name and `avahi-daemon` and its dependencies
##### Setup domain
You need to edit hostsname file and add your own domain name

	# nano /etc/hostsname 
You may need to update hosts file as well 

	# nano /etc/hosts

and replaced localhost with your own domain name
##### ZeroConf
Once you domain has been configured you can now proceed with the installation of avahi.

    # apt-get install avahi-daemon libnss-mdns
	# firewall-cmd --add-service=mdns; firewall-cmd --permanent --add-service=mdns

> **NOTE:** For windows user you need to install bonjour service, you can
> get it by installing itunes.
