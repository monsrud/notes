
## Test Authenticated SSL Proxy with Curl

* marshall@basement:~$ http_proxy='http://tester:password@172.22.1.62:3128'
* marshall@basement:~$ https_proxy='http://tester:password@172.22.1.62:3128'
* marshall@basement:~$ curl -v -I https://www.google.com
