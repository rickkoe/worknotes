# Linux
## RedHat Enterprise Server
### Procedures
#### Changing the default kernel in Red Hat Enterprise Linux 8 & 9

Simple way common to RHEL8 and RHEL9

1. Confirmation before the change

Raw
# grubby --default-kernel
/boot/vmlinuz-5.14.0-70.30.1.el9_0.x86_64

# uname -r
5.14.0-70.30.1.el9_0.x86_64
2. Show installed kernel list ( You can count 0,1,.. from the top of the title line)

Raw
# grubby --info=ALL | grep title
title="Red Hat Enterprise Linux (5.14.0-70.30.1.el9_0.x86_64) 9.0 (Plow)"    <---0
title="Red Hat Enterprise Linux (5.14.0-70.13.1.el9_0.x86_64) 9.0 (Plow)"    <---1
3. Set the default kernel

Raw
# grub2-set-default 1  
4. Confirmation after the change

Raw
# grubby --default-kernel
/boot/vmlinuz-5.14.0-70.13.1.el9_0.x86_64
5. reboot the system

Raw
# reboot
6. Confirmation after system reboot

Raw
# uname -r 
5.14.0-70.13.1.el9_0.x86_64

# grubby --default-kernel
/boot/vmlinuz-5.14.0-70.13.1.el9_0.x86_64