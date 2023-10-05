# IBM Storage Scale
## Documenation

- [Documentation Site](https://www.ibm.com/docs/en/storage-scale/5.1.7)
- [IBM Spectrum Scale 5.1.0: Problem Determination Guide](https://www.ibm.com/docs/it/STXKQY_5.1.0/com.ibm.spectrum.scale.v5r10.doc/pdf/scale_pdg.pdf?view=kc)

## Implementation
### Cheat Sheet

- Ansible Toolkit directory: `/usr/lpp/mmfs/5.1.x.0/ansible-toolkit`


### Linux Stretch Cluster Installation

1. Set adapter IP Address  
1. Set hostname  
1. Set DNS servers  

        vi /etc/resolv.conf

1. Attach Redhat Subscription  

        subscription-manager register
        subscription-manager attach --auto



1. Download the appropriate version of Spectrum Scale   
1. Copy the self-extracting product image to a local directory (We use: `/home/install/scale`)   
1. Make the file executible if needed   

        chmod Spectrum_Scale_Standard-5.1.7.x-x86_64-Linux-install

1. Extract    

        tar -xvf Scale_adv_5.1.7_ppc64le.tar


### Adding a Node to the Cluster

1. Download the appropriate version of Spectrum Scale   
1. Copy the self-extracting product image to a local directory (We use: `/home/install/scale`)   
1. Make the file executible if needed   

        chmod Spectrum_Scale_Standard-5.1.7.x-x86_64-Linux-install

1. Extract    

        tar -xvf Scale_adv_5.1.7_ppc64le.tar


### Setup File Access Authentication with Active Directory
1. 

        '''
        mmuserauth service create  --type ad --data-access-method file --netbios-name specscale --user-name adUser --idmap-role master --servers myAdserver --idmap-range-size 1000000 --idmap-range 10000000-299999999 --unixmap-domains 'DOMAIN(5000-20000:win)'
        '''
