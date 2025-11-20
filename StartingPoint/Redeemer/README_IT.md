# HTB Starting Point – Redeemer

Difficoltà: Very Easy  
Data: 20/11/2025

---

## Connessione alla VPN

È stato utilizzato lo stesso file `.ovpn` presente nella directory `sf_shared` della VM Kali Linux.  
La connessione alla rete HTB è stata avviata con:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon

IP assegnato: 10.10.14.2  
Target: 10.129.136.187

---

## Verifica della Connettività

ping -c 3 10.129.136.187  
PING 10.129.136.187 (10.129.136.187) 56(84) bytes of data.  
64 bytes from 10.129.136.187: icmp_seq=1 ttl=63 time=61.2 ms  
64 bytes from 10.129.136.187: icmp_seq=2 ttl=63 time=60.9 ms  
64 bytes from 10.129.136.187: icmp_seq=3 ttl=63 time=62.1 ms

--- 10.129.136.187 ping statistics ---  
3 packets transmitted, 3 received, 0% packet loss  
rtt min/avg/max/mdev = 60.919/61.385/62.075/0.497 ms

Il target risponde correttamente.

---

## Scansione delle Porte

È stata eseguita una scansione Nmap con rilevamento versione:

nmap -sV 10.129.136.187  
Starting Nmap 7.95  
Host is up (0.062s latency).  
All 1000 scanned ports are in ignored states (all closed).

Nessuna porta aperta rilevata con scansione standard.

Successivamente, è stata effettuata una scansione aggressiva delle porte:

nmap -p- -min-rate 5000 10.129.136.187  
PORT STATE SERVICE  
6379/tcp open redis

È stata identificata una porta aperta: 6379/tcp (Redis).

---

## Task di Verifica

### Task 1 – Porta TCP aperta sulla macchina

Risposta: 6379  
Stato: Corretta

---

### Task 2 – Servizio in esecuzione sulla porta aperta

Risposta: Redis  
Stato: Corretta

---

### Task 3 – Tipologia di database Redis

Risposta: Database in memoria  
Stato: Corretta

---

### Task 4 – Utility a riga di comando per interagire con Redis

Risposta: redis-cli  
Stato: Corretta

---

### Task 5 – Flag per specificare l'host in redis-cli

Risposta: -h  
Stato: Corretta

---

### Task 6 – Comando per ottenere informazioni e statistiche del server

Risposta: info  
Stato: Corretta

Evidenza principale:

redis_version:5.0.7  
os:Linux 5.4.0-77-generic x86_64  
role:master  
db0:keys=4

---

### Task 7 – Versione Redis sul target

Risposta: 5.0.7  
Stato: Corretta

---

### Task 8 – Comando per selezionare un database

Risposta: select  
Stato: Corretta

---

### Task 9 – Numero di chiavi nel database 0

Risposta: 4  
Stato: Corretta

Evidenza:

keys *

numb
temp
stor
flag

---

### Task 10 – Comando per ottenere tutte le chiavi

Risposta: keys *  
Stato: Corretta

---

### Task 11 – Root Flag

Dopo aver selezionato il database 0, è stato possibile leggere la flag tramite:

get flag  
"03e1d2b376c37ab3f5319922053953eb"

Flag: 03e1d2b376c37ab3f5319922053953eb  
Stato: Corretta

---

## Note

La macchina mette in evidenza come un'istanza Redis esposta senza autenticazione possa consentire accesso completo al database, incluse eventuali chiavi sensibili.

---
## Author

Report by delerico
