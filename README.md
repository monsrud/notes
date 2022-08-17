
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

## show remaining life of eMMC

```
RES=`cat /sys/kernel/debug/mmc1/mmc1:0001/ext_csd`
typea="${RES:536:2}" ;
typeb="${RES:538:2}" ;
typead=`echo "ibase=16; $typea"|bc`
typebd=`echo "ibase=16; $typeb"|bc`
echo "Type A percent: $((typead * 10)) %"
echo "Type B percent: $((typebd * 10)) %"
```

## Check temperature of Cat 1k Cisco switch

```
Cisco1k24>show env temperature status
System Temperature Value: 62 Degree Celsius
PHY Temperature Value: 55 Degree Celsius
DDR Temperature Value: 58 Degree Celsius
System Temperature State: GREEN
Yellow Threshold : 85 Degree Celsius
Red Threshold    : 88 Degree Celsius

```

## Check MySQL Memory Settings

```
#!/bin/sh
# you might want to add some user authentication here
mysql -u root -p  -e "show variables; show status" | awk '
{
VAR[$1]=$2
}
END {
MAX_CONN = VAR["max_connections"]
MAX_USED_CONN = VAR["Max_used_connections"]
BASE_MEM=VAR["key_buffer_size"] + VAR["query_cache_size"] + VAR["innodb_buffer_pool_size"] + VAR["innodb_additional_mem_pool_size"] + VAR["innodb_log_buffer_size"]
MEM_PER_CONN=VAR["read_buffer_size"] + VAR["read_rnd_buffer_size"] + VAR["sort_buffer_size"] + VAR["join_buffer_size"] + VAR["binlog_cache_size"] + VAR["thread_stack"] + VAR["tmp_table_size"]
MEM_TOTAL_MIN=BASE_MEM + MEM_PER_CONN*MAX_USED_CONN
MEM_TOTAL_MAX=BASE_MEM + MEM_PER_CONN*MAX_CONN
printf "+------------------------------------------+--------------------+\n"
printf "| %40s | %15.3f MB |\n", "key_buffer_size", VAR["key_buffer_size"]/1048576
printf "| %40s | %15.3f MB |\n", "query_cache_size", VAR["query_cache_size"]/1048576
printf "| %40s | %15.3f MB |\n", "innodb_buffer_pool_size", VAR["innodb_buffer_pool_size"]/1048576
printf "| %40s | %15.3f MB |\n", "innodb_additional_mem_pool_size", VAR["innodb_additional_mem_pool_size"]/1048576
printf "| %40s | %15.3f MB |\n", "innodb_log_buffer_size", VAR["innodb_log_buffer_size"]/1048576
printf "+------------------------------------------+--------------------+\n"
printf "| %40s | %15.3f MB |\n", "BASE MEMORY", BASE_MEM/1048576
printf "+------------------------------------------+--------------------+\n"
printf "| %40s | %15.3f MB |\n", "sort_buffer_size", VAR["sort_buffer_size"]/1048576
printf "| %40s | %15.3f MB |\n", "read_buffer_size", VAR["read_buffer_size"]/1048576
printf "| %40s | %15.3f MB |\n", "read_rnd_buffer_size", VAR["read_rnd_buffer_size"]/1048576
printf "| %40s | %15.3f MB |\n", "join_buffer_size", VAR["join_buffer_size"]/1048576
printf "| %40s | %15.3f MB |\n", "thread_stack", VAR["thread_stack"]/1048576
printf "| %40s | %15.3f MB |\n", "binlog_cache_size", VAR["binlog_cache_size"]/1048576
printf "| %40s | %15.3f MB |\n", "tmp_table_size", VAR["tmp_table_size"]/1048576
printf "+------------------------------------------+--------------------+\n"
printf "| %40s | %15.3f MB |\n", "MEMORY PER CONNECTION", MEM_PER_CONN/1048576
printf "+------------------------------------------+--------------------+\n"
printf "| %40s | %18d |\n", "Max_used_connections", MAX_USED_CONN
printf "| %40s | %18d |\n", "max_connections", MAX_CONN
printf "+------------------------------------------+--------------------+\n"
printf "| %40s | %15.3f MB |\n", "TOTAL (MIN)", MEM_TOTAL_MIN/1048576
printf "| %40s | %15.3f MB |\n", "TOTAL (MAX)", MEM_TOTAL_MAX/1048576
printf "+------------------------------------------+--------------------+\n"
}'

```


