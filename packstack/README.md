Packstack
=========

This vagrantfile will install and configure Icehouse release on a CentOS 6.5
host using the packstack installer.
This environment is then used to install/test tesora-dbaas.

##### Bring up packstack vm
from the root of the openstack-utils git repo

	pushd vagrant/boxes
	vagrant box add centos-6.5-amd64.json --provider vmware_desktop
	popd

	pushd vagrant/packtack
  	vagrant up

(note, vagrant up will often return failure, yet actually succeed.)

##### Install Tesora-DBaaS

	vagrant ssh
	sudo su -
	cd /vagrant
	./1-install-tesora-dbaas.sh

run setup.sh (2nd part of installation)

	./2-run-setup.sh
 
These are the questions that have non-default answers:
...
MySQL host/port?  [192.168.2.129:3306] 127.0.0.1:3306
...
Trove admin tenant?  [service] services
...
Nova password?  [admin] admin_password
...
MySQL admin password?  [password] mysql_password
...
Keystone password?  [admin] admin_password
...
Trove Region?  [Default] RegionOne

see  setup-answers-packstack.txt for a complete log


##### Add MySQL datastore

run:
	3-add-datastore.sh 

##### Create MySQL database:

run:
	4-create-database.sh

if everything goes well, the instance will go from "BUILD" to "ACTIVE", and the new database can be used.
