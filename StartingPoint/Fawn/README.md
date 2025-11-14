# HTB Starting Point – Fawn

Difficulty: Very Easy  
Date: 14/11/2025  
Start Time: 18:50  
Finish Time: 19:38

---

## VPN Connection

The same `.ovpn` file previously downloaded into the `sf_shared` directory of the Kali Linux VM was used again.  
The connection to the HTB VPN was started using:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon

Assigned IP: 10.10.15.9  
Target: 10.129.3.183

---

## Connectivity Check

ping -c 5 10.129.3.183  
PING 10.129.3.183 (10.129.3.183) 56(84) bytes of data.  
64 bytes from 10.129.3.183: icmp_seq=1 ttl=63 time=57.9 ms  
64 bytes from 10.129.3.183: icmp_seq=2 ttl=63 time=58.2 ms  
64 bytes from 10.129.3.183: icmp_seq=3 ttl=63 time=1358 ms  
64 bytes from 10.129.3.183: icmp_seq=4 ttl=63 time=345 ms  
64 bytes from 10.129.3.183: icmp_seq=5 ttl=63 time=58.4 ms

--- 10.129.3.183 ping statistics ---  
5 packets transmitted, 5 received, 0% packet loss, time 4023ms  
rtt min/avg/max/mdev = 57.893/375.562/1357.897/503.606 ms

The target responds correctly.

---

## Port Scanning

A basic Nmap scan was performed:

nmap 10.129.3.183  
Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org) ) at 2025-11-14 19:02 CET  
Nmap scan report for 10.129.3.183  
Host is up (0.065s latency).  
Not shown: 999 closed tcp ports (reset)  
PORT STATE SERVICE  
21/tcp open ftp

Nmap done: 1 IP address (1 host up) scanned in 1.22 seconds

Open port found:  
21/tcp – FTP

---

## Verification Tasks

### Task 1 – Meaning of the acronym FTP

Answer: File Transfer Protocol  
Status: Correct

---

### Task 2 – Default FTP port

Answer: Port 21  
Status: Correct

---

### Task 3 – Secure replacement for FTP (SSH extension)

Answer: SFTP  
Status: Correct

---

### Task 4 – Tool used to send an ICMP Echo request

Answer: ping  
Status: Correct

---

### Task 5 – FTP version running on the target

Answer: vsftpd 3.0.3  
Status: Correct

Service/version scan:

nmap -sV 10.129.3.183  
PORT STATE SERVICE VERSION  
21/tcp open ftp vsftpd 3.0.3  
Service Info: OS: Unix

---

### Task 6 – Operating System detected on the target

Answer: Unix  
Status: Correct

---

### Task 7 – Command to display the FTP client help menu

Answer: ftp -?  
Status: Correct

---

### Task 8 – Username used for unauthenticated FTP access

Answer: anonymous  
Status: Correct

---

### Task 9 – FTP response code for “Login successful”

Answer: 230  
Status: Correct

Evidence:

ftp -a 10.129.3.183  
Connected to 10.129.3.183.  
220 (vsFTPd 3.0.3)  
331 Please specify the password.  
230 Login successful.  
Remote system type is UNIX.

---

### Task 10 – Common Linux command to list files (alternative to dir)

Answer: ls  
Status: Correct

Evidence:

ftp> ls  
229 Entering Extended Passive Mode  
150 Here comes the directory listing.  
-rw-r--r-- 1 0 0 32 Jun 04 2021 flag.txt  
226 Directory send OK.

---

### Task 11 – Command to download a file from FTP

Answer: get  
Status: Correct

---

## Task 12 – Flag Retrieval

Attempting to download the flag via FTP returned a permission error:

ftp> get flag.txt  
ftp: Can't access `flag.txt': Permission denied

The flag was successfully retrieved using curl:

curl ftp://anonymous:@10.129.3.183/flag.txt

Flag:  
035db21c881520061c53e0536e44f815

Status: Correct

---

## Notes

This machine demonstrates the risks associated with misconfigured FTP services, particularly when anonymous access is enabled.  
It highlights how basic enumeration and simple FTP interactions can expose sensitive files.
