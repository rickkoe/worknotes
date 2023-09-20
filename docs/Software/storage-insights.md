# IBM Storage Insights

## Documentation Links
- [Storage Insights Documentation](https://www.ibm.com/docs/en/storage-insights)  
- [Quick Start Guide](https://www.ibm.com/docs/en/SSQRB8/pdf/IBM_Storage_Insights_Getting_Started_Guide.pdf)  
## Implementation
### Data Collector Requirements
#### Operating Systems  

- Windows Server 2012 and later.  
- POWER6® or later systems that use AIX® 7.x or later. The AIX data collector can run on a physical AIX installation or a logical partition (LPAR).  
- The Linux® data collector runs on 64-bit Linux operating systems on x86-64 and PPC64LE systems only.  
    - The supported Linux operating systems for x86-64 are Red Hat® Enterprise Linux 7 or later and CentOS 7 and later versions.  
    - The supported Linux operating system for PPC64LE is Red Hat Enterprise Linux 7.x on POWER8® and Red Hat Enterprise Linux 8.x. on POWER9™® and POWER10®. The data collector on Linux PPC64LE has the additional limitation that you cannot monitor IBM FlashSystem A9000, XIV®, IBM Storage Accelerate, and non-IBM devices.  
#### Hardware

- Deploy data collectors on servers or virtual machines that have access to the devices that you want to monitor and access to the internet to communicate with IBM Cloud. You can also deploy data collectors in a LAN where routing is configured to allow access across different subnets and on a WAN where devices are located in different geographical locations.
- On the server or virtual machine, you must provide at least 1 GB of RAM and 3 GB of disk space.
- Ensure that the operating system where you install the data collector has general or extended support for maintenance and security fixes.
- The location where you install a data collector must be available 24X7:
Don't install a data collector on a laptop or personal workstation. Shutting down a laptop or personal workstation or putting it into sleep mode will interrupt data collection.
- Don't install the data collector to a file system that was mounted or mapped under a user login session. These file systems can be temporary and might not persist after the user logs out of the network. Note that you can install a data collector to a system-mounted or mapped file system because those file systems are typically more permanent.