
## Test Authenticated Squid SSL Proxy with Curl

* marshall@basement:~$ http_proxy='http://tester:password@172.22.1.62:3128'
* marshall@basement:~$ https_proxy='http://tester:password@172.22.1.62:3128'
* marshall@basement:~$ curl -v -I https://www.google.com

## Convert NetApp Volume Serial to NAA
lun serial -x /vol/test/lun

Serial (hex)#: 0x486e542d4f5a2f47694d684b

Take the example of a LUN with the following NAA number:

naa.60a98000486e542d4f5a2f47694d684b

## Log into an ISCSI Target
iscsiadm --mode discovery -t sendtargets --portal <ipaddress>
  
iscsiadm --mode node --targetname  <iqn output from command above>  --portal <ipaddress> --login
  
  
## Create the Unit/Service /usr/lib/systemd/system/myservice.service file with the following content:
```
[Unit]
Description=A service that does something

[Service]
Type=simple
ExecStart=/my/path/myservice.sh

[Install]
WantedBy=multi-user.target
```


## Create the /usr/lib/systemd/system/myservice.timer file with the following content:
```
[Unit]
Description=Execute backup every day at midnight

[Timer]
OnCalendar=*-*-* 00:00:00
Unit=myservice.service

[Install]
WantedBy=multi-user.target
```

## Enable and start the timer service:
```
chmod ugo+x /my/path/myservice.sh
systemctl enable myservice.timer
systemctl start myservice.timer
```



## Install grub to a second disk drive

If you are trying to make a copy of an entire system like /dev/sda to /dev/sdb, these
are the steps you may run afterward to install grub to the new disk and also copy the 
mbr from one disk to the other.

```
dd if=/dev/sda of=/dev/sdb bs=512 count=1
mount --bind /dev /mnt/dev 
mount --bind /proc /mnt/proc 
mount --bind /sys /mnt/sys
cd /mnt
chroot .
update-grub2
grub-install /dev/sdb
```


