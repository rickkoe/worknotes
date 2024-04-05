# Cisco MDS SAN
##  Implementation
### Procedures
- [Procedure to collect show tech-support logs](/cisco-mds/mds-tech-support)
### Implementation Checklist
Enter configuration mode:  

```
config t
```

Rename a device-alias:

```
device-alias database
device-alias rename {old_name} {new_name}
device-alias commit
```

Save configuration changes

```
copy run start
```

Displaying power usage information

```
show environment power
```

Configuring the Power Supply Mode

```
configure terminal
power redundancy-mode {combined | insrc-redundant | ps-redundant |redundant}
(Optional) show environment power
(Optional) copy running-config startup-config
```

Power Cycling Modules
Step 1	
Identify the module that needs to be reset.

Step 2	
Issue the reload module command to reset the identified module. This command power cycles the selected module.
Caution 	
Reloading a module disrupts traffic through the module.

```
reload module 2
```
### Microcode Guidance

- [Download Microcode from IBM Fix Central](https://www.ibm.com/support/fixcentral)
### FCIP Configuration
https://www.cisco.com/c/en/us/td/docs/switches/datacenter/mds9000/sw/8_x/config/ip_services/ipsvc/cfcip.html#74650
https://thinksystem.lenovofiles.com/storage/help/index.jsp?topic=%2FMCC_Fabric-attached_MetroCluster_Installation_and_Configuration_Guide%2F5A8EEA1A-F3FA-4570-902C-2BF420654A60_.html

1. Enable the FCIP Feature

```
config t
feature fcip
```

1. Configure the IPStorage interface.

```
interface ipStorage 1/1-6
10g-speed-mode
interface ipStorage 1/1
ip address 10.1.1.100 255.255.255.0
no shut
interface ipStorage 1/2
ip address 10.1.1.101 255.255.255.0
no shut
```

1. Configure static routes

```
ip route 10.2.1.0 255.255.255.0 10.1.1.1
```
Display the routing table
```
show ips ip route interface ipStorage 1/1
```

1. Create an FCIP profile and then assign the IPStorage interface’s IP address to the profile.

```
fcip profile 10
ip address 10.22.9.20
sh fcip profile

```

1. Create an FCIP interface and then assign the profile to the interface.

```
interface fcip 51
use-profile 10
peer-info ipaddr 10.1.1.1
no shut
```
1. Configure the peer IP address for the FCIP interface.
1. Enable the interface.




## Implementation Checklist

- <input type="checkbox"/> Enter configuration mode

```
    config 
```

- <input type="checkbox"/> Set password to pre-defined secure password 

```
 username admin password <new password>   
```

- <input type="checkbox"/> Set the switch name 

```
 switchname <switch name> 
```

- <input type="checkbox"/> Set IP address and subnet mask

```
 int mgmt 0 ; ip address 10.0.0.10 255.255.255.0 
```

- <input type="checkbox"/> Set default gateway 

```
 ip default-gateway 10.0.0.1
```

- <input type="checkbox"/> Enable SSH 

```
 ssh key rsa 1024 force ; feature ssh 
```

- <input type="checkbox"/> Disable Telnet 

```
 no feature telnet 
```

- <input type="checkbox"/> Enable http-server 

```
 feature http-server 
```

- <input type="checkbox"/> Configure timezone 

```
 clock timezone CST -6 0 
```

- <input type="checkbox"/> Configure clock 

```
 clock set 15:30:00 29 September 2020 
```

- <input type="checkbox"/> Configure summertime

```
 clock summer-time CDT 2 Sunday March 02:00 1 Sunday November 02:00 60 
```

- <input type="checkbox"/> Enable NPIV feature 

```
 feature npiv 
```

- <input type="checkbox"/> Configure NTP server 

```
 ntp server ntp.server.com 
```

- <input type="checkbox"/> Configure default switchport interface state 

```
 system default switchport shutdown 
```

- <input type="checkbox"/> Configure default switchport trunk mode 

```
 system default switchport trunk mode on 
```

- <input type="checkbox"/> Configure switchport mode 

```
 interface fc1/1-96 ; switchport mode auto ; exit 
```

- <input type="checkbox"/> Configure default zone policy 

```
 no system default zone default-zone permit 
```

- <input type="checkbox"/> Enable full zoneset distribution 

```
 system default zone distribute full 
```

- <input type="checkbox"/> Create VSAN(s) 

```
 vsan database ; vsan 10 
```

- <input type="checkbox"/> Name VSAN 

```
 vsan 10 name fabricA 
```

- <input type="checkbox"/> Assign ports to VSAN(s) 

```
 vsan database ; vsan 10 interface fc1/1-96 
```

- <input type="checkbox"/> Configure default zone mode (basic/enhanced) 

```
 zone mode enhanced vsan 10 
```

- <input type="checkbox"/> Configure zoning confirm-commit 

```
 zone confirm-commit enable vsan 10 
```

- <input type="checkbox"/> Enable smart zoning 

```
 zone smart-zoning enable vsan 10 
```

- <input type="checkbox"/> Configure device-alias mode (basic/enhanced) 

```
 device-alias mode enhanced 
```

- <input type="checkbox"/> Configure device-alias confirm-commit 

```
 device-alias confirm-commit enable 
```

- <input type="checkbox"/> Configure domain name 

```
 ip domain-name lookup ; ip domain-name demo.com 
```

- <input type="checkbox"/> Configure DNS 

```
 ip name-server 1.1.1.1 2.2.2.2 
```

- <input type="checkbox"/> Verify licenses (POD, etc) 

```
     
```

- <input type="checkbox"/> Configure zoning 

```
 !Full Zone Database Section for vsan 
```

- <input type="checkbox"/> Import fcaliases to device-alias 

```
 device-alias import fcalias vsan 10 ; device-alias commit 
```

- <input type="checkbox"/> Update NX-OS 

```
     
```

- <input type="checkbox"/> Configure static domain-id 

```
 fcdomain domain 11 static vsan 10 
```

- <input type="checkbox"/> Configure contact name 

```
 snmp-server contact John Doe 
```

- <input type="checkbox"/> Configure contact email address 

```
 callhome ; email-contact johndoe@demo.com 
```

- <input type="checkbox"/> Configure contact phone 

```
 callhome ; phone-contact +1-123-456-7890 
```

- <input type="checkbox"/> Configure contact street address 

```
 callhome ; streetaddress 123 North Street, DemoVille, MN 55555 
```

- <input type="checkbox"/> Configure site-id 

```
 callhome ; site-id Site1 
```

- <input type="checkbox"/> Call home distribute 

```
 callhome ; distribute 
```

- <input type="checkbox"/> Call home destination profile 

```
 callhome ; destination-profile full-txt-destination email-addr DemoTeam@demo.com 
```

- <input type="checkbox"/> Email From 

```
 callhome ; transport email from DemoTeam@demo.com 
```

- <input type="checkbox"/> Email Reply To 

```
 callhome ; transport email reply-to switch1@demo.com 
```

- <input type="checkbox"/> SMTP Server 

```
 callhome ; transport email smtp-server smtprelay.demo.com port 25 
```

- <input type="checkbox"/> Enable call home 

```
 callhome ; enable 
```

- <input type="checkbox"/> Commit call home 

```
 callhome ; commit 
```

- <input type="checkbox"/> Label the switch ports 

```
interface fc1/1
switchport description <port label>
```

- <input type="checkbox"/> Copy run start

```
 copy run start 
```

## Port Channel Configuration

!!! info
    Ensure one or more ISL exists between the two switches.  Make sure they come up as “TE” ports with the correct VSANs.  ISLs can be in VSAN 1 or just in the VSAN that needs to be extended between two switches.


#### Create a Port Channel

```
interface port-channel 3
channel mode active
```

#### Add Ports to a Port Channel

```
interface fc1/1-2
channel-group 3 force
```

#### Activate a Port Channel

```
interface port-channel 3
no shut
```

#### Delete a Port channel

```
no interface port-channel 3
```

##  Misc Information

## Power Supply Modes
Cisco MDS 9000 Series Multilayer Switches support different number and capabilities of power supplies. This section describes the power modes that are available on Cisco MDS 9000 Series Multilayer Switches.

Cisco MDS 9710 Multilayer Switches can support up to four power supplies when they have only Cisco MDS 9700 48-Port 32-Gbps Fibre Channel Switching Modules installed on them. By default, the four power supplies are installed in the power supply bays 1 to 4.

You can configure one of the following power modes to use the combined power provided by the installed power supply units (no power redundancy) or to provide power redundancy when there is power loss. We recommend that you configure the full redundancy power mode on your switch for optimal performance.

**Combined mode**—This mode uses the combined capacity of all the power supplies. In case of power supply failure, the entire switch can be shut down (depending on the power used) causing traffic disruption. This mode is seldom used, except in cases when the switch requires more power.

**Input Source (grid) redundancy mode**—This mode allocates half of the power supplies to the available category and the other half to the reserve category. You must use different power supplies for the available and reserve categories so that if the power supplies used for the active power fails, the power supplies used for the reserve power can provide power to the switch. If the grid-redundancy mode is lost, the power mode reverts to combined mode.

**Power-supply (N+1) redundancy mode**—This mode allocates one power supply as reserve to provide power to the switch in case an active power supply fails. The remaining power supplies are allocated for the available category. The reserve power supply must be at least as powerful as each of the power supplies used for the active power.

**Full-redundancy mode**—This mode is a combination of input-source (grid) and power-supply (N+1) redundancy modes. Similar to the input-source redundancy mode, this mode allocates half of the power supplies to the available category and the remaining power supplies to reserve category. One of the reserve power supplies can alternatively be used to provide power if a power supply used for the active power fails.

For more information on the power supply modes supported on your switch, see the Hardware Installation Guide corresponding to your switch.

##  Links

- [Recommended Releases for Cisco MDS 9000 Series Switches](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/mds9000/sw/b_MDS_NX-OS_Recommended_Releases.html)


