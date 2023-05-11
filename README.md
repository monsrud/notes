

## Verifying that a Certificate is issued by a CA
How to use OpenSSL on the command line to verify that a certificate was issued by a specific CA, given that CA's certificate

$ openssl verify -verbose -CAfile cacert.pem  server.crt
server.crt: OK
If you get any other message, the certificate was not issued by that CA.

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

  ## Clean Old Data from Zabbix Database
  
  ```
  time mysql zabbix -e "
RENAME TABLE history_uint TO history_uint_old;
CREATE TABLE history_uint LIKE history_uint_old;
SET SESSION SQL_LOG_BIN=0;
INSERT INTO history_uint (
SELECT * FROM history_uint_old WHERE clock >= UNIX_TIMESTAMP(
\"$(date +%Y-%m-%d) 00:00:00\"
)
);
DROP TABLE history_uint_old;
"

time mysql zabbix -e "
RENAME TABLE history_str TO history_str_old;
CREATE TABLE history_str LIKE history_str_old;
SET SESSION SQL_LOG_BIN=0;
INSERT INTO history_str (
SELECT * FROM history_str_old WHERE clock >= UNIX_TIMESTAMP(
\"$(date +%Y-%m-%d) 00:00:00\"
)
);
DROP TABLE history_str_old;
"

time mysql zabbix -e "
RENAME TABLE history_log TO history_log_old;
CREATE TABLE history_log LIKE history_log_old;
SET SESSION SQL_LOG_BIN=0;
INSERT INTO history_log (
SELECT * FROM history_log_old WHERE clock >= UNIX_TIMESTAMP(
\"$(date +%Y-%m-%d) 00:00:00\"
)
);
DROP TABLE history_log_old;
"

time mysql zabbix -e "
RENAME TABLE history_text TO history_text_old;
CREATE TABLE history_text LIKE history_text_old;
SET SESSION SQL_LOG_BIN=0;
INSERT INTO history_text (
SELECT * FROM history_text_old WHERE clock >= UNIX_TIMESTAMP(
\"$(date +%Y-%m-%d) 00:00:00\"
)
);
DROP TABLE history_text_old;
"

time mysql zabbix -e "
RENAME TABLE history TO history_old;
CREATE TABLE history LIKE history_old;
SET SESSION SQL_LOG_BIN=0;
INSERT INTO history (
SELECT * FROM history_old WHERE clock >= UNIX_TIMESTAMP(
\"$(date +%Y-%m-%d) 00:00:00\"
)
);
DROP TABLE history_old;
"

time mysql zabbix -e "
RENAME TABLE trends_uint TO trends_uint_old;
CREATE TABLE trends_uint LIKE trends_uint_old;
SET SESSION SQL_LOG_BIN=0;
INSERT INTO trends_uint (
SELECT * FROM trends_uint_old WHERE clock >= UNIX_TIMESTAMP(
\"$(date +%Y-%m-%d) 00:00:00\"
)
);
DROP TABLE trends_uint_old;
"

time mysql zabbix -e "
RENAME TABLE trends TO trends_old;
CREATE TABLE trends LIKE trends_old;
SET SESSION SQL_LOG_BIN=0;
INSERT INTO trends (
SELECT * FROM trends_old WHERE clock >= UNIX_TIMESTAMP(
\"$(date +%Y-%m-%d) 00:00:00\"
)
);
DROP TABLE trends_old;
"

  ```

