
## Test Authenticated Squid SSL Proxy with Curl

* marshall@basement:~$ http_proxy='http://tester:password@172.22.1.62:3128'
* marshall@basement:~$ https_proxy='http://tester:password@172.22.1.62:3128'
* marshall@basement:~$ curl -v -I https://www.google.com

## Convert NetApp Volume Serial to NAA
lun serial -x /vol/test/lun
Serial (hex)#: 0x486e542d4f5a2f47694d684b
Take the example of a LUN with the following NAA number:
naa.60a98000486e542d4f5a2f47694d684b

