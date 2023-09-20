# IBM Storage Scale
## Linux Stretch Cluster Installation
From [https://www.ibm.com/docs/en/spectrum-scale/5.1.7?topic=toolkit-preparing-use-installation](https://www.ibm.com/docs/en/spectrum-scale/5.1.7?topic=toolkit-preparing-use-installation)


Spectrum Scale
Documentation:  [https://www.ibm.com/docs/en/spectrum-scale/5.1.7](https://www.ibm.com/docs/en/spectrum-scale/5.1.7)
Ansible Toolkit directory:  `/usr/lpp/mmfs/5.1.7.0/ansible-toolkit`
Cluster definition file:  `/usr/lpp/mmfs/5.1.7.0/ansible-toolkit/ansible/vars`

	1. Copy install files to /home/install
		a. /home/install/scale
		b. /home/install/archive
	2. Extract files
	3. Install the following RPMs on RHEL
		a. kernel-devel
		b. cpp
		c. gcc
		d. gcc-c++
		e. binutils
		f. elfutils-libelf-devel (Required for Red Hat Enterprise Linux 8.x)
		yum install kernel-devel cpp gcc gcc-c++ binutils elfutils-libelf-devel
	4. Verify python3 is installed (which python, which python3)
		Install depndencies for cryptogoraphy:  dnf install rust.ppc64le redhat-rpm-config gcc libffi-devel python3-devel openssl-devel cargo pkg-config
		yum install python3
		python3 -m pip install --upgrade pip
		pip3 install ansible==2.9.15
		Verify python version (3.6.8 or higher) python --version
	5. Verify locale
		locale
	if not correct:
		export LC_ALL=en_US.UTF-8 
		export LANGUAGE=en_US.UTF-8
	6. Set up passwordless SSH between all nodes in the cluster and to the nodes themselves using FQDN, IP address, and host name.
		○ ssh-keygen -t rsa
		○ Issue the command from the $HOME/.ssh directory.
		○ cat ~/.id_rsa.pub and copy the contents
		○ vi a file called authorized_keys in ~/.ssh and paste contents of id_rsa.pub
	7. From each node, ssh to self and all other nodes using FQDN, IP, and Hostname
	8. disable firewall
		systemctl status firewalld
		systemctl stop firewalld
		systemctl disable firewalld
		systemctl status firewalld
	9. Extract Spectrum Scale Media
	/home/install/scale/Spectrum_Scale_Data_Management-5.1.7.0-ppc64LE-Linux-install
	10. Install RHEL packages
		dnf install net-tools kernel-devel cpp gcc gcc-c++ binutils elfutils-libelf-devel nfs-utils ethtool nfs-utils rpcbind psmisc iputils  boost-regex postgresql.ppc64le librdkafka openssl-devel cyrus-sasl-devel numactl tcpdump strace openssl 
	11. Install more stuff:  yum install kernel-devel-4.18.0-305.el8.ppc64le kernel-headers-4.18.0-305.el8.ppc64le elfutils elfutils-devel make
	12. Toolkit Deployment
	13. Setup Installer Node
		cd /usr/lpp/mmfs/5.1.7.0/ansible-toolkit
		./spectrumscale setup -s 172.22.18.20
			[ INFO  ] Installing prerequisites for install node
			[ INFO  ] Found existing Ansible installation on system.
			/usr/local/lib/python3.6/site-packages/ansible/parsing/vault/__init__.py:44: CryptographyDeprecationWarning: Python 3.6 is no longer supported by the Python core team. Therefore, support for it is deprecated in cryptography. The next release of cryptography (40.0) will be the last to support Python 3.6.
			  from cryptography.exceptions import InvalidSignature
			[ INFO  ] Install Toolkit setup type is set to Spectrum Scale (default). If an ESS is in the cluster, run this command to set ESS mode: ./spectrumscale setup -s server_ip -st ess
			[ INFO  ] Your ansible controller node has been configured to use the IP 172.22.18.21 to communicate with other nodes.
			[ INFO  ] Port 10080 will be used for package distribution.
			[ INFO  ] SUCCESS
			[ INFO  ] Tip : Designate protocol, nsd and admin nodes in your environment to use during install:./spectrumscale -v node add <node> -p  -a -n
	14. Define the cluster topology for the installation toolkit
		./spectrumscale node add ctcicas01 -q -m -a -n -p -g -c
		./spectrumscale node add elricas01 -q -m -a -n -p -g -c
		./spectrumscale node add labicas01 -q -n
	15. Disable call home (temporary for install)
		./spectrumscale callhome disable
	16. Create CES root filesystem
	provision 100GB LUN from storage  (This link shows size of 4GB or larger)
	rescan-scsi-bus.sh to discover new LUNs
	multipath -ll to get dm-x name
	
	The device you reference must be in /proc/partitions; therefore use the /dev/dm-x name for multipath devices.  This can be found in multipath -ll
	
	./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs ces -fg 1 /dev/dm-10
	./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs ces -fg 2 /dev/dm-1
	./spectrumscale nsd add -p labicas01 -u descOnly -fs ces -fg 3 /dev/dm-7
	
	Verify filesystem will be created
	./spectrumscale filesystem list
	
	Configure filesystem to be replicated
	./spectrumscale filesystem modify -r 2 -mr 2 ces
		[ INFO  ] Data for the ces filesystem will be replicated 2 times across your nodes. A value of 3 has been automatically set for the maximum number of data replicas.
		[ INFO  ] Metadata for the ces filesystem will be replicated 2 times across your nodes. A value of 3 has been automatically set for the maximum number of metadata replicas.
	
	Designate file system paths for protocols and for object by issuing the following commands:
	./spectrumscale config protocols -f ces -m /ibm/ces
	
	Name the cluster:
./spectrumscale config gpfs -c optumicas
	
	Install and deploy the stretch cluster by issuing the following command:
	./spectrumscale install --precheck
	./spectrumscale install
		# Install failed waiting for GUI to start.  We discovered that the GUI did eventually start, so we just ran the install again and it worked!
	 
	./spectrumscale deploy --precheck
		
		Warninings from precheck
		[ WARN  ] Ephemeral port range is not set. Please set valid ephemeral port range using the command ./spectrumscale config gpfs --ephemeral_port_range . You may set the default values as 60000-61000
	./spectrumscale config gpfs --ephemeral_port_range 60000-61000
	
		[ WARN  ] On the nodes: [['ctcicas01.esilabs.com with OS RHEL8'], ['elricas01.esilabs.com with OS RHEL8'], ['labicas01.esilabs.com with OS RHEL8']], the Portmapper service (rpcbind) is found running and it is advised to disable it or verify with the operating system's administrator.
	
	All nodes in cluster:
	systemctl status rpcbind
	systemctl stop rpcbind
	systemctl stop rpcbind.socket
	systemctl disable rpcbind
	systemctl disable rpcbind.socket
	
./spectrumscale enable object
 
./spectrumscale config protocols -e 172.22.18.28,172.22.18.29
 
./mmchnode --ces-group CTC -N ctcicas01
./mmchnode --ces-group ELR -N elricas01
 
./mmces address change --ces-ip 172.22.18.28 --ces-group elr
./mmces address change --ces-ip 172.22.18.29 --ces-group ctc
 
./mmces interface add --nic ctcexpif1 --ces-node ctcicas01
./mmces interface add --nic elrexpif1 --ces-node elricas01
	
Create gpfs0 filesystem
Ctcicas01:
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-12
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-13
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-14
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-15
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-16
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-17
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-18
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-19
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-20
./spectrumscale nsd add -p ctcicas01 -u dataAndMetadata -fs gpfs0 -fg 1 /dev/dm-21

Elricas01:
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-12
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-13
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-14
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-15
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-16
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-17
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-18
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-19
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-20
./spectrumscale nsd add -p elricas01 -u dataAndMetadata -fs gpfs0 -fg 2 /dev/dm-21


Verify filesystem will be created
./spectrumscale filesystem list

Configure filesystem to be replicated
./spectrumscale filesystem modify -r 2 -mr 2 gpfs0

./spectrumscale install --precheck

./spectrumscale install

Create Admin user for GUI:
/usr/lpp/mmfs/gui/cli/mkuser admin -g SecurityAdmin


Configuring Object
Unknown if this is needed.  Maybe try without first.
	We kept getting an error during deploy, so we updated pyopenssl: to v23.0.0 (was at v19.0.0)
		pip3 install pyOpenSSL --upgrade
		pip3 list
		
	
	
	./spectrumscale config object -f gpfs0 -m /ibm/gpfs0
	./spectrumscale enable object
	./spectrumscale config object -o object
	./spectrumscale config object --adminpassword
	./spectrumscale config object --databasepassword
	./spectrumscale config object -e ctcicas01exp.esilabs.com
	./spectrumscale config object -I 2000000
	./spectrumscale config object -su swadmin
	./spectrumscale config object -sp abc123
	./spectrumscale config -s3 on
	
Changed DNS to have entry for both sides pseudo load balanceer and then updated the config
	./spectrumscale config object -e optumicasobj.esilabs.com
	
Ran these commands to configure credentials to be accessible via S3:
	source $HOME/openrc
	openstack credential create --type ec2 --project admin admin '{"access": "admintest", "secret": "abc123!!"}'
	openstack credential list
