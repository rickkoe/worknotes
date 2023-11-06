# IBM Spectrum Control
## Administration
### Stopping the IBM Spectrum Control servers on Linux or AIX
Tip: The default installation_dir is /opt/IBM/TPC.  Custom directory we use is /SpectrumControl
To stop the servers on Linux or AIX operating systems, enter the following commands in the following order:
- Storage Resource Agent  
    /SRA_installation_dir/agent/bin/agent.sh stop
- Web server  
    /installation_dir/scripts/stopTPCWeb.sh
- Export server  
    /installation_dir/scripts/stopTPCExport.sh
- Data server
    /installation_dir/scripts/stopTPCData.sh.
- Device server
    /installation_dir/scripts/stopTPCDevice.sh
- Alert server
    /installation_dir/scripts/stopTPCAlert.sh



### Starting the IBM Spectrum Control servers on Linux or AIX  
!!! info
    Note: The default installation_dir is /opt/IBM/TPC.

To start the servers on the Linux or AIX operating systems, enter the following commands in the following order:
- Data server
    /installation_dir/scripts/startTPCData.sh
- Device server
    /installation_dir/scripts/startTPCDevice.sh
- Alert server
    /installation_dir/scripts/startTPCAlert.sh
- Export server
    /installation_dir/scripts/startTPCExport.sh
- Web server
    /installation_dir/scripts/startTPCWeb.sh
- Storage Resource Agent
    /installation_dir/agent/bin/agent.sh start