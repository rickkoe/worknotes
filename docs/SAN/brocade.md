# Brocade SAN
## Documentation Links
### Support Information
- [Broadcom Support Portal](https://support.broadcom.com)

### Reference Guides
- [Brocade FOS Administration Guide, 9.0.x](https://docs.broadcom.com/doc/FOS-90x-Admin-AG)
- [Brocade FOS Command Reference Manual, 9.0.x](https://docs.broadcom.com/doc/FOS-90x-Command-RM)

### Microcode Guidance
- [Brocade Target Path Selection Guide](https://docs.broadcom.com/doc/Brocade-SW-Support-RM)
- [Download Microcode from IBM Fix Central](https://www.ibm.com/support/fixcentral)


## Implementation

### Health Check
```
switchshow
```
```
psshow
```


### Load Firmware From USB
usbstorage -e
usbstorage -l
firmwaredownload -U v9.2.1_EXT
### Implementation Checklist

These are the typical items that should be reviewed and/or configured when setting up a new Brocade SAN switch/director. While many of these settings can be set in WebTools, the CLI commands are listed here for a simlper checklist.

#### User Settings

- <input type="checkbox"/> Enable root access *optional*

```
userconfig --change root -e yes ; rootaccess --set all
```

- <input type="checkbox"/> Set root password to pre-defined password (Login as root/fibranne and it will prompt you to change)

- <input type="checkbox"/> Set admin password to pre-defined password

```plaintext
passwd
```

#### Name and Address

- <input type="checkbox"/> Set the switch name

```plaintext
switchname new_name
```

- <input type="checkbox"/> Set IP address, subnet mask, gateway

```plaintext
ipaddrset
```

- <input type="checkbox"/> Set the Registered Organization Name **(Directors Only)**

```
ron --set <customer_name>
```

#### Data Center Settings

- <input type="checkbox"/> Verify Management Port Speed (Default is 1000-auto, but some customers need this set to 100-full)

```plaintext

## If 100-full is required
ethif --set eth0 -an off -speed 100 -duplex full

## Validate
ethif --show eth0
```

- <input type="checkbox"/> Configure timezone

```plaintext
tstimezone America/New_York
tstimezone America/Chicago
```

- <input type="checkbox"/> Reboot to make new timezone effective (do this before configurating the clock)
```
reboot
```

- <input type="checkbox"/> Configure clock

```plaintext
date mmddHHMMyy
```

- <input type="checkbox"/> Configure DNS

```plaintext
dnsconfig --add -domain <domain_name> -serverip1 <ip_address_1> -serverip2 <ip_address_2>
```

- <input type="checkbox"/> Configure NTP server

```plaintext
tsclockserver "ntp_server_name"
```
- <input type="checkbox"/> Configure SMTP server

```plaintext
relayConfig --config -rla_ip <relay IP> -rla_dname <domain name>
```

- <input type="checkbox"/> [Configure LDAP](/san/brocade/ldap-switch) (optional)

#### Licenses

- <input type="checkbox"/> [Apply new licenses](/san/brocade/license) (POD, etc.)

- <input type="checkbox"/> Verify licenses (POD, etc.)

```plaintext
licenseshow
```

- <input type="checkbox"/> [Assign static port licenses](/san/brocade/license#static-port-assignment)


#### Firmware
- <input type="checkbox"/> Update FOS
```plaintext
firmwaredownload <ftp_server_ip>,<ftp_userid>,<path_to_firmware>
```
- <input type="checkbox"/> Configure domain id and set insistent domain ID

```plaintext
switchdisable ; configure
```

#### Zone Config

- <input type="checkbox"/> Configure default zone policy to No Access

```plaintext
defzone --noaccess ; cfgsave
```

- <input type="checkbox"/> Configure zoning

#### Optional
- <input type="checkbox"/> Persistent disable all ports
```
portcfgpersistentdisable 3/0-47
portcfgpersistentdisable 4/0-47
portcfgpersistentdisable 5/0-47
portcfgpersistentdisable 6/0-47
portcfgpersistentdisable 9/0-47
portcfgpersistentdisable 10/0-47
portcfgpersistentdisable 11/0-47
portcfgpersistentdisable 12/0-47
```

#### MAPS
- <input type="checkbox"/> Enable a policy
```
mapspolicy --show -summary
mapspolicy --enable dflt_conservative_policy
```
- <input type="checkbox"/> Verify policy is active
```
mapsconfig --show
mapsconfig --actions raslog,email
mapsConfig --emailcfg -address rick.k@evolvingsol.com
mapsconfig --show
mapsconfig --testmail -subject "this is a test" -message "test email body"
```
### Configuring Security
- [Brocade Security Templates](https://techdocs.broadcom.com/us/en/fibre-channel-networking/fabric-os/fabric-os-administration/9-2-x/v26755091/v26754426.html)
This command will show security setup.
```
seccryptocfg --show
```
```
br6510-fabA-1:FID128:admin> seccryptocfg --show
SSH Crypto:
SSH Cipher               : aes128-ctr,aes256-ctr,aes128-gcm@openssh.com,aes256-gcm@openssh.com
SSH Kex                  : diffie-hellman-group14-sha1,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521
SSH MAC                  : hmac-sha2-256,hmac-sha2-512
TLS Ciphers:
HTTPS                    : !ECDH:!DH:HIGH:-MD5:!CAMELLIA:!SRP:!PSK:!AESGCM:!3DES:!aNULL
RADIUS                   : !ECDH:!DH:HIGH:-MD5:!CAMELLIA:!SRP:!PSK:!AESGCM:!3DES:!aNULL
LDAP                     : !ECDH:!DH:HIGH:-MD5:!CAMELLIA:!SRP:!PSK:!AESGCM:!3DES:!aNULL
SYSLOG                   : !ECDH:!DH:HIGH:-MD5:!CAMELLIA:!SRP:!PSK:!AESGCM:!3DES:!aNULL
TLS Protocol:
HTTPS                    : TLSv1.2
RADIUS                   : TLSv1.2
LDAP                     : TLSv1.2
SYSLOG                   : TLSv1.2
X509v3:
Validation               : Strict
```

To list security templates that are available:
```
seccryptocfg --lstemplates
```
To list the contents of a particular template:
```
seccryptocfg --show template_file
```
To apply the configurations in a template file:
```
seccryptocfg --apply template_file
```

### FCIP Configuration
#### Resources
- [Brocade® Fabric OS® Extension User Guide](https://techdocs.broadcom.com/us/en/fibre-channel-networking/fabric-os/fabric-os-extension/9-2-x.html)
#### FCIP Configuration Steps

!!! note

    The steps shown below are the steps followed in a real world implementation of Brocade 7810s with two 10G links. 

##### Configure GE Interfaces
1. Show the GE Interfaces and make note of speed and WAN or LAN indication.  A flag of **L** means it is set for LAN (don't want this for FCIP).  No **L** indicates it is set for WAN (this is correct).  
        ```
        portcfgge --show
        ```
1. If needed, set the port for WAN
        ```
        portcfgge ge6 --set -wan
        ```
1. Set the GE port to 10G
        ```
        portcfgge ge2 --set -speed 10G
        portcfgge ge3 --set -speed 10G        
        ```
1. Assign IP addresses to the IP interfaces
        ```
        portcfg ipif ge2 create 10.1.1.10/24 mtu 1500
        portcfg ipif ge3 create 10.1.1.11/24 mtu 1500
        ```
1. If needed, set PMTU for auto MTU size:

        portcfg ipif ge2.dp0 modify 10.1.1.10/24 mtu auto
        portcfg ipif ge3.dp0 modify 10.1.1.11/24 mtu auto

1. Show the IP interfaces
        ```
        portshow ipif
        ```
##### Configure FCIP Tunnels
1. Create FCIP tunnel
        ```
        portcfg fciptunnel 12 create
        ```
1. Enable compression on the tunnel
        ```
        portcfg fciptunnel 12 modify -c deflate
        ```
##### Configure FCIP Circuits
1. Create circuits (Do on both sides)   
        ```
        portcfg fcipcircuit 12 create 0
        portcfg fcipcircuit 12 create 1
        ```
1. Add circuit IP addresses (Do on both sides)
        ```
        portcfg fcipcircuit 12 modify 0 --local-ip 10.0.0.5 --remote-ip 192.168.0.5
        portcfg fcipcircuit 12 modify 1 --local-ip 10.0.0.6 --remote-ip 192.168.0.6
        ```
1. Set min and max bandwidth (Do on both sides)
        ```
        portcfg fcipcircuit 12 modify 0 -b 615000 -B 1230000
        portcfg fcipcircuit 12 modify 1 -b 615000 -B 1230000
        ```
##### Configure IPsec Encryption
1. Create an IPsec Policy
        ```
        portcfg ipsec-policy MyPolicyName create --preshared-key Li6a28@yfH@sgT28PNUj
        ```
1. Verify the IPsec policy is correct on all switches  
        ```
        portshow ipsec-policy --password
        ```
1. Apply the IPSec Policy to the tunnel
        ```
        portcfg fciptunnel 12 modify --ipsec MyPolicyName
        ```
1. Verify the IPsec configuration was applied to all circuits correctly  
        ```
        portshow ipsec-policy --ike
        ```
##### Validation Commands
- Show the IP Interfaces
        ```
        portshow ipif
        ```
- Show the FCIP Tunnel status along with circuits
        ```
        portshow fciptunnel -c
        ```

## Brocade SAN Switch Licenses
In order to activate a license, you must have a **Transaction Key** for each license to be activated along with the **License ID** (LID) of the appropriate switch.
### Obtaining the Transaction Key
A Transaction key is unique key, along with the LID, used to generate a software license from the Broadcom licensing portal. The transaction key is issued when a license is purchased. The transaction key is delivered in one of two methods:

- **Paperpack** – The transaction key is printed and delivered within a POD Optic Hardware Kit. 
- **E-license** – The transaction key is contained in an email that is sent instantly to the customer after the sales order is created. The customer is sent the email message within a few minutes after the sales order is submitted, although the timing will vary depending on the network, internet connection, and so on. 
### Obtaining the License ID
You must have a license ID to generate the license file in the Broadcom licensing portal.
Obtain the license ID of the switch or chassis using the CLI method:

1.	Connect to the switch, and log in using an account with admin permissions. 
2.	Enter the following command to display the license ID of the switch:
```
license --show -lid
```
### Generating a License Key 
Use the following procedure to generate and obtain a FOS license key. 

1.	Go to [https://www.broadcom.com](https://www.brocade.com), and then select the LOGIN drop-down at the top-right of the web page. 
2.	Click **LOGIN** or **REGISTER**. Once logged in, you will be redirected to the Broadcom support portal. 
3.	Click **Brocade Products**. You will be redirected to the Brocade Products page. 
4.	Click **Licensing**. You will be redirected to the Broadcom Licensing Portal page. 
5.	Enter the transaction key. Click Next to continue. 
> Re-host keys are generated only by the SANnav application and are used only on SANnav. 
{.is-info}
6.	Enter the license ID (LID) that you obtained earlier in the Unit Information field. Click **Next** to continue. 
7.	Read the Broadcom End User License Agreement, and if you agree to the terms, select the **I have read and accept** checkbox. 
8.	Click **Generate** to generate the license. 
9.	The license key will be sent by email and can also be downloaded by selecting the blue license hyperlink from the Broadcom licensing portal UI. 
10.	If a license string is generated, save it to your local folder for future reference. 
11.	If an XML certificate file is generated, save it to the remote server, where it will be retrieved for installing the license. 
### Installing the License Using a License String 
For Brocade Gen 6 switches and directors, you must pass the license key in the license --install command to install the licenses.

1.	Connect to the switch, and log in using an account with admin permissions. 
2.	Activate the license using the license --install -key <lic_key> command. 
```
license --install -key HP9ttZNSgmB4MCD3NmNWgQDWtAKBFtXtBSFJF
```
3.	Verify that the license was installed by entering the license --show command. The licensed features that are currently installed on the switch are listed. If the feature is not listed, use the license --install command to install the license. 
```
license --show
```
### Displaying Port License Assignments 
When you display the available licenses, you can also view the current port assignment of those licenses. 
Use the following procedure to display the port license assignments.

1.	Connect to the switch, and log in using an account with admin permissions. 
2.	Enter the license --show -port command to view how port licenses are currently assigned.
```
license --show -port
```

### Static Port Assignment
When Dynamic Ports On Demand (DPOD) is enabled, ports are licensed from a pool of available licenses based on the server blade or switch installation.  Each port that is enabled will secure a license on a first-come, first-served basis.  If you would like to statically assign port licenses, use the following command.  Example below would reserve licenses on the first 16 ports in a switch.
```
license --reserve -port 0-15
```

Use the command in the previous section to display the port licenses after making the static assignment to ensure they were properly assigned.


## SAN Discovery
### Brocade SAN Health
*This tool is free of charge to anyone with a Broadcom.com account.*

1. Download and Install the Free Tool
Go to this URL and follow the steps to download and install Brocade SAN Health Diagnostic Capture:
[Download Brocade SAN Health Diagnostics Capture](https://www.broadcom.com/support/fibre-channel-networking/tools/san-health/diagnostics-capture)
1. Before running, turn on automatic upload by clicking the **Options** button.
![Click Options](assets/images/san-health-1.png)
1. Check the box to **Automatically send the BSH file to the report generation queue on audit completion**.
![Automatic Upload](assets/images/san-health-2.png)
1. Click **Done**.
1. Click the **New** button to generate a new audit file.  This only needs to be done the first time setting up SAN Health.  Future runs can be done by opening a saved audit file.
![Click New](assets/images/san-health-3.png)
1. Complete all 5 tabs in the SAN Health Audit.  Include *Rick Koetter - rick.k@evolvingsol.com* or other partners as desired in the **Optional Additional Recipients** fields.
![Additional Recipients](assets/images/san-health-4.png)
1. Be sure to include all fabrics.  There is no good reason to run this multiple times for different fabrics.  All fabrics can be included in a single SAN Health report.
1. Before starting the capture, click on the Save button to save the audit so you can run this report easily with a few clicks in the future.
1. Start the Audit on the Capture tab after all other tabs are complete and all fabrics have been discovered successfully.  The file will upload automatically after all data is completed.
![Start The Audit](assets/images/san-health-5.png)
1. It may take up to several hours for the report to be generated.  When it is ready for download, you will receive notification by email.  You will be instructed to download the report from [https://support.broadcom.com](https://support.broadcom.com). Your consultant will also receive a copy of the report if you entered their email in the Optional Additional Recipients field.

### Generate CSR

```
seccertmgmt generate -csr https  -type rsa -keysize 2048 -hash sha256 -years 1 -f
```
```
seccertmgmt show -csr https -hexdump
```