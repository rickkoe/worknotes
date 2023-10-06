# IBM Spectrum Archive

## Implementation

### Installation Steps

1. Run this command on both nodes to enable the required repository:  

        subscription-manager repos --enable codeready-builder-for-rhel-8-ppc64le-rpms

1. Install prereqs:  

        dnf install attr boost-date-time boost-filesystem boost-program-options boost-serialization boost-thread boost-regex.ppc64le fuse fuse-libs  libxml2 libuuid libicu lsof nss-softokn-freebl openssl net-snmp  python3-pyxattr java

1. Mount the iso

        mkdir /mnt/iso
        mount -o loop /home/install/archive/ISAEEV-SME_1.3.3_MP_EN.iso /mnt/iso
	    cd /mnt/iso
	    cp ltfsee-1.3.3.0-53211-product.bin /home/install/archive

1. Extract the installation files

    	cd /home/install/archive/
	    ./ltfsee-1.3.3.0-53211-product.bin

1. Run the LTFSEE installer check
	
        cd ~/LTFSEE/rpm.1330_53211
	    ./ltfsee_install --check

1. Install the software

        ./ltfsee_install --install
	[root@icas02 rpm.1330_53211]#
	
1. Ensure the GPFS Filesystem has DMAPI enabled

        mmlsfs gpfs0 | grep DMAPI
    
    1. If DMAPI is not enabled - do the following
	    1. Unmount the filesystem
	
                /usr/lpp/mmfs/bin/mmunmount /dev/gpfs0 -a

	    1. Enable DMAPI

	            /usr/lpp/mmfs/bin/mmchfs /dev/gpfs0 -z yes

	    1. Remount the filesystem

	            /usr/lpp/mmfs/bin/mmmount /dev/gpfs0 -a
	
1. Configuration 

        cd /opt/ibm/ltfsee/bin
        ./ltfsee_config -m CLUSTER

    !!! alert
        During the configuration, you will be prompted to choose a filesystem.  Choose **gpfs0** or whatever the main GPFS filesystem name is.
        Make sure you say "n" to this question:  `Then do you want to perform the ADD_CTRL_NODE mode? [Y/n]:`  



	
1. Log into each node where there is a tape library zoned/configured and add it as a control node:

        ltfsee_config -m ADD_CTRL_NODE -g

1. Start the cluster

        eeadm cluster start


### Uninstall Steps
1. CD to the rpm directory

    	cd ~/LTFSEE/rpm.1330_53211/

1. Uninstall the software

	./ltfsee_install --clean

## Script
### Script to assign Tapes
```
POOL=icas02_pool
LIBRARY=icas02_ts3310
eeadm tape list --no-header -l $LIBRARY | grep unassigned | while read -a tape ; do eeadm tape assign ${tape[0]} -p $POOL -l $LIBRARY -f ; done
```

	


### Lifecycle Policy
Create Export

HSM Rule
https://www.ibm.com/docs/en/spectrum-archive-ee/1.3.3?topic=migration-automated-spectrum-scale-policies

```
mmaddcallback MIGRATION2 --command /usr/lpp/mmfs/bin/mmstartpolicy --event lowDiskSpace,noDiskSpace --parms "%eventName %fsName --single-instance -B 10000"
```


Here's an example of the policy

```
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
```

	
	
