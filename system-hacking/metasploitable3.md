# Metasploitable 3 Walkthrough : Exploitation Guide


## Metasploitable 3 :
Metasploitable 3 is an intentionally vulnerable virtual machine designed for training, exploit testing, and general target practice. Unlike other vulnerable virtual machines, Metasploitable focuses on vulnerabilities at the operating system and network services layer instead of custom, vulnerable applications.

## Intelligence Gathering

### Host Disovery : 

```
nmap -sn 192.168.105.132 -oN host-discovery
```
```
# Nmap 7.94SVN scan initiated as: nmap -sn -oN host-discovery 192.168.105.132
Nmap scan report for 192.168.105.132
Host is up (0.00054s latency).
# Nmap done -- 1 IP address (1 host up) scanned in 0.32 seconds
```

### Port Scan : 

```
nmap -sV -O -v -p- -oN port-scan 192.168.105.132
```
```
# Nmap 7.94SVN scan initiated as: nmap -sV -O -v -p- -oN port-scan 192.168.105.132
Nmap scan report for 192.168.105.132
Host is up (0.00067s latency).
Not shown: 65521 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
21/tcp    open  ftp         ProFTPD 1.3.5
22/tcp    open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http        Apache httpd 2.4.7
111/tcp   open  rpcbind     2-4 (RPC #100000)
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
631/tcp   open  ipp         CUPS 1.7
3306/tcp  open  mysql       MySQL (unauthorized)
3500/tcp  open  http        WEBrick httpd 1.3.1 (Ruby 2.3.8 (2018-10-18))
6667/tcp  open  irc         UnrealIRCd
6697/tcp  open  irc         UnrealIRCd
8067/tcp  open  irc         UnrealIRCd
8080/tcp  open  http        Jetty 8.1.7.v20120910
38695/tcp open  status      1 (RPC #100024)
MAC Address:(VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Uptime guess: 0.003 days 
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=262 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: Hosts: 127.0.0.1, METASPLOITABLE3-UB1404, irc.TestIRC.net; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


```

### Service scan : 

```
nmap -A -PN -sU -sS -T2 -v -oN service-scan 192.168.67.132
```
```

```



## Vulnerability Analysis

### 21 ftp : ProFTPD 1.3.5

- **CVE-2015-3306** 
    - The mod_copy module in ProFTPD 1.3.5 allows remote attackers to read and write to arbitrary files via the site cpfr and site cpto commands. 
    - 

- metasploitable 
    - exploit/unix/ftp/proftpd_modcopy_exec





## Exploitation
