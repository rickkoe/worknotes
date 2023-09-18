# IBM FlashSystem
## Documentation Links
### Support Information

Find configuration limits and restrictions, release notes, and product documentation for IBM FlashSystem storage.

- [Support Information for FlashSystem 9500](https://www.ibm.com/support/pages/support-information-flashsystem-9500)
- [Support Information for FlashSystem 9200](https://www.ibm.com/support/pages/support-information-flashsystem-9200)
- [Support Information for FlashSystem 7300](https://www.ibm.com/support/pages/support-information-flashsystem-7300)
- [Support Information for FlashSystem 7200](https://www.ibm.com/support/pages/support-information-flashsystem-7200)
- [Support Information for FlashSystem 5000 and 5200](https://www.ibm.com/support/pages/node/6408990)


### Implementation Guide
*A great reference to use during initial implementation of a new FlashSystem*

-   [Implementation Guide for IBM Spectrum Virtualize Version 8.5](https://www.redbooks.ibm.com/abstracts/sg248520.html)


### Best Practices Guide
*Use these guides to ensure you are following best practices for zoning, copy services, host connectivity, etc.*

-   [IBM FlashSystem Best Practices and Performance Guidelines](https://www.redbooks.ibm.com/abstracts/sg248503.html)
-   [Performance and Best Practices Guide for IBM Spectrum Virtualize 8.5](https://www.redbooks.ibm.com/abstracts/sg248521.html)


*Use this next guide to ensure you are following best practices when connecting to VMware*

-   [IBM FlashSystem and VMware Implementation and Best Practices Guide](http://www.redbooks.ibm.com/abstracts/sg248505.html?Open)


### Microcode Guidance
*If you are upgrading microcode, use this link to determine the latest (LTS) recommended code level along with other levels that are available (non-LTS)*

-   [Recommended Code Levels](https://www.ibm.com/support/pages/spectrum-virtualize-family-products-upgrade-planning)


*This link will show if you are able to upgrade directly to another code level, or if you have to upgrade in two steps.*

-   [Concurrent Code Upgrade Paths](https://www.ibm.com/support/pages/node/5692850)


### Other Stuff
-   [CSM ESE Sizer](https://www.ibm.com/support/pages/node/6372180)
-	[IBM Storage Ansible Modules](https://galaxy.ansible.com/ibm/spectrum_virtualize)
-   [Enable the VMware iSER Adapter](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.storage.doc/GUID-4F2C10BB-3705-4040-BFDE-A190FE273060.html)
-   [Vdbench Downloads and User Guide](https://www.oracle.com/downloads/server-storage/vdbench-downloads.html)


## Implementation
- [Implementation Guide for IBM Spectrum Virtualize Version 8.5](https://www.redbooks.ibm.com/redpieces/pdfs/sg248520.pdf)
- [Host Configuration Best Practices](/ibm-flashsystem/flashsystem-host-configuration)
- [Spectrum Virtualize Support Center Addresses](/storage/ibm/flashsystem/support-addresses)


### Implementation Checklist
> This is not an official instruction manual from IBM.  Alway refer to the latest IBM docs if you are unsure about how to complete a step in the list below.{.is-info}

The following items are a summary of the most common steps that need to be completed on every implementation.  Some items may not pertain to specific hardware configs (e.g. IP replication), but verifying installs using this list will cover the most critical aspects of a thorough implementation.

-   <input type="checkbox"/> Power Up
-   <input type="checkbox"/> Configure cluster using the [technician port](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=system-initializing-technician-port-ssr-task)
-   <input type="checkbox"/> Use a web browser to open: *https://your\_management\_IP*
-   <input type="checkbox"/> Log in to the management GUI for the first time by using ID *superuser* and password *passw0rd*.
-   <input type="checkbox"/> After you log in, the initial setup wizard helps you get started.  Use the information on your [worksheets](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=planning-worksheets) to inform your inputs.
    -   <input type="checkbox"/> Welcome
    -   <input type="checkbox"/> License Agreement
    -   <input type="checkbox"/> Change Password
    -   <input type="checkbox"/> System Name
    -   <input type="checkbox"/> Licensed Functions
    -   <input type="checkbox"/> Date and Time
    -   <input type="checkbox"/> Encryption (license required)
    -   <input type="checkbox"/> Call Home
    -   <input type="checkbox"/> Storage Insights
    -   <input type="checkbox"/> Support Assistance
    -   <input type="checkbox"/> Automatic Configuration
    -   <input type="checkbox"/> Summary
-   <input type="checkbox"/> Add additional Control or Expansion Enclosures (if required)
-   <input type="checkbox"/> Configure the following:
    -   <input type="checkbox"/> [Set the Service IPs](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=problem-procedure-changing-service-ip-address-node-canister)
    -   <input type="checkbox"/> Ethernet IPs for ISCSI or IP replication
    -   <input type="checkbox"/> [Ethernet Portsets](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=overview-portsets)
    -   <input type="checkbox"/> [Volume Protection](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=volumes-volume-protection) (recommend leaving on)
    -   <input type="checkbox"/> Call Home / Email Notifications
    -   <input type="checkbox"/> SNMP
    -   <input type="checkbox"/> Syslog
    -   <input type="checkbox"/> LDAP (if desired)
-   <input type="checkbox"/> Enable Encryption ([USB](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=management-enabling-encryption-usb-flash-drives) or [SKLM](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=management-enabling-encryption-key-servers))
-   <input type="checkbox"/> Create encrypted pool with data reduction turned off and 1024 extent size (default in GUI)
-   <input type="checkbox"/> Add storage to the pool (DRAID6, typically take all defaults in GUI).
-   <input type="checkbox"/> Modify I/O Group bitmap space (CLI or now in GUI of newer code)
-   <input type="checkbox"/> Modify fibre channel port masking (if needed for replication)
-   <input type="checkbox"/> [Update System Software](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=updating-system-software)
-   <input type="checkbox"/> [Update Drive Firmware](https://www.ibm.com/docs/en/flashsystem-7x00/8.4.x?topic=software-updating-drive-firmware)
### Spectrum Virtualize Remote Support IP Addresses


#### **Content**

To continue to allow IBM to provide remote support for your Spectrum Virtualize system, you need to make some changes to your environment before the end of December 2022.

**Update:** Due to an unexpected infrastructure change there will only be a single, non-redundant, remote support server available to support clients between 1 November 2022 and 31 December 2022.  Clients are advised to update their environments to use the new servers before the end of October.

There are also changes to Fix Central IP addresses that require changes before the next Remote Code Upgrade is scheduled.

The new remote support IP addresses are active and ready to use.  For security reasons they do not respond to ping requests.

**_Spectrum Virtualize Configuration_**

The Spectrum Virtualize configuration must be updated to add the new IP addresses.

The simplest way to achieve this change is to download and run the software upgrade test utility v36.0 or higher, which updates the configuration automatically. 

Alternatively, use the mksystemsupportcenter -proxy no -ip X.X.X.X -port 22  command to add the additional IP addresses manually.

The list of currently configured remote support servers can be validated by using the lssystemsupportcentercommand.

**Note**:  It is not possible to delete the default system support center servers (id 0 and 1).

**_Remote Support Proxy_**

IBM provides an optional product known as a Remote Support proxy that can be installed in your environment.  This proxy coalesces the remote support traffic for multiple storage devices into a single server and tunnels the traffic through an HTTPS connection.

Any Remote Support Proxy installations must be upgraded to v1.3.2 or higher

**Note:**  The Remote Support proxy was deprecated in Spectrum Virtualize 8.4.2 and higher.  The newer releases of software can use an industry standard HTTP proxy instead of the Remote Support proxy.

**_Firewall Configuration_**

Any firewall holes that were created to allow connections to the current IP addresses must be updated with the new IP addresses.

|     | Source | Target | Port | Protocol | Direction |
| --- | --- | --- | --- | --- | --- |
| Existing Remote Support Servers<br><br>*These firewall holes can be removed once new servers are configured and running* | The service IP address of every node or node canister | 129.33.206.139 204.146.30.139 | 22  | ssh | Outbound only |
| New Remote Support Servers | The service IP address of every node or node canister | 170.225.126.11<br><br>170.225.126.12<br><br>170.225.127.11<br><br>170.225.127.12 | 22  | ssh | Outbound only |

  
 

> Port 22 is used for direct connections, but any traffic routed through either an HTTP proxy or the dedicated remote support proxy use port 443
{.is-info}


  
  
 

**_HTTP Proxy Configuration_**

  
 

Any HTTP proxies that are used for Remote Support connections might need to update configurations to allow connections to the new IP addresses.

  
  
 

**_IP address details_**

  
 

The current IP addresses used for remote support are:

-   129.33.206.139
-   204.146.30.139

  
 

The new IP addresses for remote support are:

-   170.225.126.11 - xrsc-front-srv-1.southdata.ibm.com
-   170.225.126.12 - xrsc-front-srv-2.southdata.ibm.com
-   170.225.127.11 - xrsc-front-srv-3.eastdata.ibm.com
-   170.225.127.12 - xrsc-front-srv-4.eastdata.ibm.com

  
 

**_Fix Central Code Download - Firewall Configuration_**

  
 

IBM Announced in [https://www.ibm.com/support/pages/node/6573219](https://www.ibm.com/support/pages/node/6573219) that there would be a number of changes to some central support infrastructure. 

  
 

The only impact of this announcement for Spectrum Virtualize customers is related to downloading code directly from Fix Central as part of the Remote Code Load process.

  
 

Systems running V8.4.1 or earlier that have configured their systems to use Remote Code Load might also need to update their firewall rules because the IP addresses of delivery04.dhe.ibm.com are changing. The only change required is to update firewall rules to permit connections to the replacement IP addresses

  
 

Direct connection to Fix Central on port 22 was deprecated in V8.4.2.  Systems running V8.4.2 or higher download code via esupport.ibm.com on port 443.

  
 

**The Fix Central DNS names were updated to point to the new IP addresses on 4 June 2022.  Spectrum Virtualize devices use DNS to connect to Fix Central, therefore all connections will automatically be connecting to the new IP addresses.**

  
 

|     | Source | Target | Port | Protocol | Direction |
| --- | --- | --- | --- | --- | --- |
| Existing Fix Central IP addresses<br><br>*These IP addresses are no longer usable, so the firewall holes should be removed.* | The service IP address of every node or node canister | 170.225.15.105<br><br>170.225.15.104<br><br>170.225.15.107<br><br>129.35.224.105<br><br>129.35.224.104<br><br>129.35.224.107 | 22  | sftp | Outbound only |
| --- | --- | --- | --- | --- | --- |
| New Fix Central IP addresses | The service IP address of every node or node canister | 170.225.126.44 | 22  | sftp | Outbound only |
| --- | --- | --- | --- | --- | --- |

  
 

**_Additional IBM Support IP address changes that do not affect Spectrum Virtualize products._**

The following notification was sent out relating to additional IP address changes.  These changes do not affect Spectrum Virtualize products

[https://www.ibm.com/support/pages/node/6587781](https://www.ibm.com/support/pages/node/6587781)

  
 

## Procedures
-   [Setup FlashSystem Remote Copy](/storage/ibm/flashsystem/remote-copy)
-   [Export Config XML File](/storage/ibm/flashsystem/config-export)


## Scripts
### Determine Remote Copy Status
#### Objective
This script will list the progress of all remote copy relationships defined on the system.  It will output the source volume name followed by the “progress” percentage. 


#### Script Syntax

```typescript
lsrcrelationship -nohdr | while read -a relationship
do echo "Volume Name:  "${relationship[5]}
lsrcrelationship ${relationship[0]}|grep progress
echo
done
```

#### Example output:

```plaintext
Volume Name:  Thor_0
progress 99	

Volume Name:  Thor_1
progress 99

Volume Name:  Thor_2
progress 99

Volume Name:  Thor_3
progress 99

Volume Name:  Thor_4
progress 99

Volume Name:  Thor_5
progress 99

Volume Name:  Thor_IASP1_0
progress 99

Volume Name:  Thor_IASP1_1
progress 99

Volume Name:  Thor_IASP1_2
progress 99

Volume Name:  Thor_IASP1_3
progress 99

Volume Name:  Thor_IASP1_4
progress 100
```

### Validate Fibre Channel Host Connectivity Script 
#### Objective
This script will display all of the SAN fabric connectivity for each defined Host and indicate what port the host is connected through.

#### Script Syntax

```typescript
svcinfo lshost -nohdr|while read -a host
do
  printf "\n%-20s\n" ${host[1]}
  svcinfo lsnode -nohdr |while read -a node
  do
  printf "\nnode %s " ${node[1]}
    for port in 1 2 3 4
    do
      printf "-"
      svcinfo lsfabric -nohdr -host ${host[1]} |while read -a fabric
      do
        [[ ${fabric[2]} == ${node[0]} && ${fabric[5]} == $port ]] && printf "\b\033[01;3%dm$port\033[0m" $((${#fabric[7]}/2-1))
      done
    done
  done
  printf "\n"
done
```

#### Example output:

```plaintext
esx20               

node node1 12--
node node2 12--

esx21               

node node1 12--
node node2 12--

esx22               

node node1 12--
node node2 12--

esx23               

node node1 12--
node node2 12--

esx24               

node node1 12--
node node2 12--

rick-w2019          

node node1 --3-
node node2 --3-

esx11               

node node1 12--
node node2 12--

test                

node node1 -2--
node node2 -2--
```
## Cool Links
- [Ansible and IBM Storage](https://www.ansible.com/integrations/infrastructure/ibm-storage)
