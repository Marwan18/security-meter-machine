
## This is my writeup and walkthrough for CYNIX: 1

## `Enumeration`
   
#### 1-Nmap 
`nmap -p- -A 192.168.1.3`

```Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-13 14:03 EET
Nmap scan report for 192.168.1.3
Host is up (0.00013s latency).
Not shown: 65533 closed ports
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
6688/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 6d:df:0d:37:b1:3c:86:0e:e6:6f:84:b9:28:11:ee:68 (RSA)
|   256 8f:3e:c0:08:03:13:e8:64:89:f6:f9:63:b3:88:99:2a (ECDSA)
|_  256 fb:e3:40:e6:91:0b:3c:bc:b7:0e:c7:bd:ef:a2:93:fc (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
###### My focus was on Port 80 `http  Apache httpd 2.4.29 ((Ubuntu))`.
#### 2-website
first  i opened in my browser it redirect me to `http://192.168.1.3/lavalamp/` so lets see what we have 
its just single page with only contant form okey lets try to send data in this form and see the request in `burpsuite` 
![Screenshot from 2021-04-13 16-14-26](https://user-images.githubusercontent.com/34393428/114567382-7bf8b100-9c73-11eb-9098-e8adac347a57.png)
 
notic that this form action to ` /lavalamp/canyoubypassme.php` so lets discover this page 

![Screenshot from 2021-04-13 16-17-18](https://user-images.githubusercontent.com/34393428/114567759-d265ef80-9c73-11eb-9984-582015132664.png)

okey i see jusn an image so lets open inspect elemnets 
![Screenshot from 2021-04-13 16-18-45](https://user-images.githubusercontent.com/34393428/114567991-080ad880-9c74-11eb-8f04-3436b6de3723.png)
i notic here that i have in input so lets change `opacity` attribute to show the input 

![Screenshot from 2021-04-13 16-20-29](https://user-images.githubusercontent.com/34393428/114568265-4607fc80-9c74-11eb-92cf-46b76aea03fe.png)
 
okey lets enter value and  see what happen 
![Screenshot from 2021-04-13 16-22-00](https://user-images.githubusercontent.com/34393428/114568536-7ea7d600-9c74-11eb-9f1b-fda2354a687a.png)
## notic that i have `file` paramters 
at first time i think it maybe `LFI` & thats was a right expectation. 

![Screenshot from 2021-04-13 15-56-30](https://user-images.githubusercontent.com/34393428/114568725-ac8d1a80-9c74-11eb-8a55-5c5afd5f1e76.png) 
 but i need thing that to get an `RCE` OR something else .
 so lets have another see.

i noticed  `sshd:x:108:65534::/run/sshd:/usr/sbin/nologin` 
so we have ssh but forwich user !! 
if we look agian we will see that we have `ford` its user 
lets try to access his ssh directory  

![Screenshot from 2021-04-13 16-30-39](https://user-images.githubusercontent.com/34393428/114569950-b5cab700-9c75-11eb-8b01-aea718173c91.png)
this is ford privet key 
![Screenshot from 2021-04-13 16-35-29](https://user-images.githubusercontent.com/34393428/114570798-6933ab80-9c76-11eb-8057-1643ca5e2a98.png)
 i copied the id_rsa key in a text on my machine and finally get logged in.
## user flag
![Screenshot from 2021-04-13 16-40-07](https://user-images.githubusercontent.com/34393428/114571867-45bd3080-9c77-11eb-9207-933c4701ba6c.png)
