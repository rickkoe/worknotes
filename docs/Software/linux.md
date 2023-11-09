# Linux
## RedHat Enterprise Server
### Procedures
#### Changing the default kernel in Red Hat Enterprise Linux 8 & 9

Simple way common to RHEL8 and RHEL9

1. Confirmation before the change

        grubby --default-kernel
        /boot/vmlinuz-5.14.0-70.30.1.el9_0.x86_64

        uname -r
        5.14.0-70.30.1.el9_0.x86_64

1. Show installed kernel list ( You can count 0,1,.. from the top of the title line)

        grubby --info=ALL | grep title
        title="Red Hat Enterprise Linux (5.14.0-70.30.1.el9_0.x86_64) 9.0 (Plow)"    <---0
        title="Red Hat Enterprise Linux (5.14.0-70.13.1.el9_0.x86_64) 9.0 (Plow)"    <---1

1. Set the default kernel

        grub2-set-default 1  

1. Confirmation after the change

        grubby --default-kernel
        /boot/vmlinuz-5.14.0-70.13.1.el9_0.x86_64

1. reboot the system

        reboot

1. Confirmation after system reboot

        uname -r 
        5.14.0-70.13.1.el9_0.x86_64

        grubby --default-kernel
        /boot/vmlinuz-5.14.0-70.13.1.el9_0.x86_64