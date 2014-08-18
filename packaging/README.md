tesora-dbaas-trove-packaging
============================

Current build instructions

Build Packages
--------------
##### Ubuntu 14.04 packages
	> cd tesora-dbaas-trove-packaging
	> vagrant up debbuilder
	> vagrant ssh debbuilder
	> sudo su -
	> cd /vagrant/deb
		optional, bump build # by editing build.sh and apt/changelog
	> ./build_enterprise.sh
	> exit

##### CentOS 6.x packages
> vagrant up rpmbuilder
> vagrant ssh rpmbuilder
> sudo su -
> cd /vagrant/rpm
	optional, bump build # by editing build.sh and tesora-dbaas.spec
> ./build_enterprise.sh
> cp /root/rpm_staging/enterprise/output/dbaas* /vagrant
> exit

Publish Packages
----------------
the dbaas[-enterprise]-transfer archives can uploaded in the normal fashion.
The rpm one exists in root of the packaging directory
The deb in apt/tmp/enterprise/tesora-trove/packages

upload these to the sftp server, kick the import-builds.bash script


Building Disk Images
--------------------

> cd packagingdir
> vagrant ssh debbuilder
> sudo su -
> cd /vagrant/diskimage
> ./build.sh

This builds only Juno right now
