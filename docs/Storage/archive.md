# IBM Spectrum Archive

## Implementation

### Implementation Steps

1. Run this command on both nodes to enable the required repository:  

        ```
        subscription-manager repos --enable codeready-builder-for-rhel-8-ppc64le-rpms
        ```

1.  Install prereqs:  

```
dnf install attr boost-date-time boost-filesystem boost-program-options boost-serialization boost-thread boost-regex.ppc64le fuse fuse-libs  libxml2 libuuid libicu lsof nss-softokn-freebl openssl net-snmp  python3-pyxattr java
	3. Mount the iso
	mkdir /mnt/iso
mount -o loop /home/install/archive/ISAEEV-SME_1.3.3_MP_EN.iso /mnt/iso
	cd /mnt/iso
	cp ltfsee-1.3.3.0-53211-product.bin /home/install/archive
	4. Extract the installation files
	cd /home/install/archive/
	./ltfsee-1.3.3.0-53211-product.bin
		a. Run the LTFSEE installer check
	cd ~/LTFSEE/rpm.1330_53211
	./ltfsee_install --check
	
	[root@icas02 rpm.1330_53211]# ./ltfsee_install --check
	Checking the software prerequisites on icas02.esilabs.com.
	
	No error found during the prerequisite check.
	
	Checking if license rpm is installed on icas02.esilabs.com.
	
	ltfs-license module is not installed.
	
	5. Install
	[root@icas02 rpm.1330_53211]# ./ltfsee_install --install
	Checking the software prerequisites on icas02.esilabs.com.
	
	No error found during the prerequisite check.
	
	Checking if license rpm is installed on icas02.esilabs.com.
	
	ltfs-license module is not installed.
	Starting RPM installation.
	Created symlink /etc/systemd/system/multi-user.target.wants/ltfsle.service -> /etc/systemd/system/ltfsle.service.
	Installing HSM modules.
	Created symlink /etc/systemd/system/multi-user.target.wants/hsm.service -> /usr/lib/systemd/system/hsm.service.
	Installing EE modules.
	Starting HSM.
	Completed RPM installation.
	Running the post-installation steps.
	All rpm packages are installed successfully.
	The installed IBM Spectrum Archive EE version: 1.3.3.0-53211.
	
	Complete the configuration by using the /opt/ibm/ltfsee/bin/ltfsee_config command.
	
	** ATTENTION **
	For problem determination, it is strongly recommended that you disable log suppression and set up the abrtd daemon to capture the Spectrum Archive core dumps.
	Refer to the Troubleshooting section of the IBM Spectrum Archive Enterprise Edition (EE) documentation.
	6. Verify
	root@icas02 rpm.1330_53211]# ./ltfsee_install --verify
	An IBM Spectrum Archive EE package ltfs-license is installed.
	An IBM Spectrum Archive EE package ltfsle-library is installed.
	An IBM Spectrum Archive EE package ltfsle-library-plus is installed.
	An IBM Spectrum Archive EE package ltfsle is installed.
	An IBM Spectrum Archive EE package ltfs-admin-utils is installed.
	An IBM Spectrum Archive EE package gskcrypt64 is installed.
	An IBM Spectrum Archive EE package gskssl64 is installed.
	An IBM Spectrum Archive EE package TIVsm-API64 is installed.
	An IBM Spectrum Archive EE package TIVsm-BA is installed.
	An IBM Spectrum Archive EE package TIVsm-HSM is installed.
	An IBM Spectrum Archive EE package ltfs-mig is installed.
	All IBM Spectrum Archive EE packages are installed.
	
	The module installation check completed.
	The installed IBM Spectrum Archive EE version: 1.3.3.0-53211
	[root@icas02 rpm.1330_53211]#
	
	7. Ensure the GPFS Filesystem has DMAPI enabled.
	If DMAPI is not enabled - do the following
	Unmount the filesystem
	/usr/lpp/mmfs/bin/mmunmount /dev/gpfs0 -a
	Enable DMAPI
	/usr/lpp/mmfs/bin/mmchfs /dev/gpfs0 -z yes
	Remount the filesystem
	/usr/lpp/mmfs/bin/mmmount /dev/gpfs0 -a
	
	8. Configuration
cd /opt/ibm/ltfsee/bin
	[root@icas02 bin]# ./ltfsee_config -m CLUSTER
	The EE configuration script is starting: ./ltfsee_config -m CLUSTER
	CLUSTER mode starts.
	
	## Step-1. Check to see if the cluster is already created ##
	Successfully validated the prerequisites.
	
	## Step-2. Check prerequisite on cluster ##
	If you want to restore previous configuration that are
	saved by SOBAR Support for System Migration feature, run CLUSTER mode with -P option.
	Do you want to continue? [Y/n]: y
	
	## Step-3. List file systems in the cluster ##
	** Specify the location of IBM Spectrum Archive EE metadata directory.
	   Select the file system from the list by number.
	
	   File system
	   1. /dev/cesshared  Mount point(/ibm/cesshared)  DMAPI(no)
	   2. /dev/data  Mount point(/gpfs/data)  DMAPI(yes)
	   q. Quit
	
	Input number > 2
	
	** Specify the IBM Spectrum Scale file systems to be managed by IBM Spectrum Archive EE.
	   Select the file system from the list by number.
	   (Multiple file systems can be specified by comma-separated numbers.)
	
	   File system
	   1. /dev/data  Mount point(/gpfs/data)
	   a. Select all file systems above
	   q. Quit
	
	Input number > 1
	
	## Step-4. Configure HSM management ##
	Updating the configuration files (/opt/tivoli/tsm/client/ba/bin/dsm.opt, dsm.sys).
	Restarting the HSM component. (This will take a minute.)
	
	
	## Step-5. Add selected file systems to Space Management ##
	Adding file system '/gpfs/data' to Space Management.
	
	## Step-6. Store the file systems configuration and dispatch it to all nodes ##
	Copying ltfsee_config.filesystem file to other nodes.
	
	## Step-7. Create the IBM Spectrum Archive EE metadata directory. ##
	Created the files in IBM Spectrum Archive EE metadata directory (/gpfs/data).
	Copying ltfsee_config.filesystem file to "/gpfs/data/.ltfsee/config/" as a backup.
	Disabling runtime AFM file state checking.
	
	CLUSTER mode completed.
	
	Then do you want to perform the ADD_CTRL_NODE mode? [Y/n]: n
	
	9. Make sure you said "n" to the ADD_CTRL_NODE
	10. ltfsee_config -m ADD_CTRL_NODE -g
	
	
	Script to assign Tapes
	POOL=icas02_pool
	LIBRARY=icas02_ts3310
	eeadm tape list --no-header -l $LIBRARY | grep unassigned | while read -a tape ; do eeadm tape assign ${tape[0]} -p $POOL -l $LIBRARY -f ; done
	
	Removing Spectrum Archive
	cd ~/LTFSEE/rpm.1330_53211/
	./ltfsee_install --clean
	
	Start the cluster
	eeadm cluster start
	
	
	
	Create Export
	
	HSM Rule
	https://www.ibm.com/docs/en/spectrum-archive-ee/1.3.3?topic=migration-automated-spectrum-scale-policies
	
	mmaddcallback MIGRATION2 --command /usr/lpp/mmfs/bin/mmstartpolicy --event lowDiskSpace,noDiskSpace --parms "%eventName %fsName --single-instance -B 10000"
	
	
	define(
	    user_exclude_list,
	    (
	        FALSE
	        OR (PATH_NAME LIKE '/ibm/gpfs/%' AND NAME LIKE 'confidential.%')
	        OR PATH_NAME LIKE '%/.SpaceMan/%'
	        OR PATH_NAME LIKE '/ibm/gpfs/.ltfsee/%'
	        OR ((CURRENT_TIMESTAMP - MODIFICATION_TIME) < INTERVAL '121' SECONDS)
	        OR NLINK != 1
	    )
	)
	
	define(
	    user_include_list,
	    (
	    FALSE
	    )
	)
	
	define(
	    exclude_list,
	    (NAME LIKE 'dsmerror.log')
	)
	
	/* define is_premigrated uses Spectrum Scale inode attributes that mark a file 
	   as a premigrated file. Use the define to include or exclude premigrated 
	   files from the input file list */
	define(
	    is_premigrated, 
	    (MISC_ATTRIBUTES LIKE '%M%' AND MISC_ATTRIBUTES NOT LIKE '%V%') 
	) 
	
	/* define is_migrated uses Spectrum Scale inode attributes that mark a file 
	   as a migrated file. Use the define to include or exclude migrated 
	   files from the input file list */
	define(
	    is_migrated, 
	    (MISC_ATTRIBUTES LIKE '%V%') 
	) 
	
	RULE EXTERNAL POOL 'ltfs1'
	EXEC '/opt/ibm/ltfsee/bin/eeadm' /*The full path to the eeadm command must be specified */
	OPTS '-p ltfs-storage-pool1@library1,ltfs-storage-pool2@library2'
	SIZE 10485760
	
	
	RULE 'MigAndPremigToExt' MIGRATE FROM POOL 'system'
	THRESHOLD(90,80)
	WEIGHT( CURRENT_TIMESTAMP - ACCESS_TIME )
	TO POOL 'ltfs1'
	WHERE (
	    FILE_SIZE > 5242880 
	    AND NOT is_migrated
	    AND NOT exclude_list
	    AND (NOT user_exclude_list OR user_include_list)
	)
	
	
	
	
