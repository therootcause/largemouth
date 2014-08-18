Packstack
=========

This vagrantfile will install and configure Icehouse release on a CentOS 6.5
host using the packstack installer.

This environment is then used to install/test tesora-dbaas.

Please note that this tooling is not entirely up-to-date for Community vs. Enterprise.   Some extra steps exist for installing enterprise.

##### Bring up packstack vm
from the root of the openstack-utils git repo

	pushd vagrant/boxes
	vagrant box add centos-6.5-amd64.json --provider vmware_desktop
	popd

	pushd vagrant/packstack
  	vagrant up
	popd

(note, vagrant up will often return failure, yet actually succeed.)

##### Install Tesora-DBaaS

To get consumer edition

	vagrant ssh
	sudo su -
	cd /vagrant
	./1-install-tesora-dbaas.sh

To get enterprise edition

	vagrant ssh
	sudo su -
	cd /vagrant
	./1-install-tesora-dbaas-enterprise.sh



        
run setup.sh (2nd part of installation)

	./2-run-setup.sh
 
Press enter to accept defaults for all questions except the following:

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

This will download the standard Ubuntu datastore for Consumer branch

Alternate instructions to add Enterprise data stores (via cache hackery):

        source ~/keystonerc_admin
	mkdir /var/cache/tesora/images
	cd /var/cache/tesora/images
	wget https://s3.amazonaws.com/tesora-bhunter/trove-ubuntu-trusty-mysql-5.5.qcow2
        /opt/tesora/dbaas/bin/add-datastore.sh mysql 5.5

if you want to, repeat for others

	wget https://s3.amazonaws.com/tesora-bhunter/trove-ubuntu-trusty-mongodb-2.4.qcow2
        /opt/tesora/dbaas/bin/add-datastore.sh mongodb 2.4

	wget https://s3.amazonaws.com/tesora-bhunter/trove-ubuntu-trusty-cassandra-2.0.qcow2
        /opt/tesora/dbaas/bin/add-datastore.sh cassandra 2.0


##### Create MySQL database:

run:

	4-create-database.sh

If everything goes well, a mysql instance will be created, and the output will transition from "BUILD" to "ACTIVE". The database should be usable.

