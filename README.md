#Windows # #



**NMAP Scan**

```sh
  nmap -sV -sC -p- 10.10.3.65
  ```

- To see the versions of the services running (**-sV**)
- To perform a script scan using the default set of scripts (**-sC**)
- To scan all ports from 1 through 65535 (**-p-**)


 Open Ports

* ?
* ?

http://10.10.3.65 --> takes us to the following website. 

OSCommerce is an e-commerce and online store-management software program

(Ref: https://en.wikipedia.org/wiki/OsCommerce)

![image](https://user-images.githubusercontent.com/99097743/170889343-1ac2db1b-ee91-4ea9-8594-88bd6dda347b.png)

A quick **serchsploit** search on **osCommerce 2.3.4** shows the following vulnerabilities:

![image](https://user-images.githubusercontent.com/99097743/170889834-7bc4cd09-df46-41b0-ac76-48e7dc3e57ae.png)


**Exploit title: osCommerce 2.3.4.1 - Remote Code Execution (PATH: php/webapps/44374.py) **

* It is an interesting exploit as it does not require credentials.
* An unauthorized attacker can reinstall, ff installation directory has not been removed.  
* During the reinstallation process, OS commerce does not attempt to do any authentication, it simple executes the code. 



