# TCP

Link (Data Link, Physical), Internet, Transport, Application (Session, Presentation, Application)

Establishes a connection;
Guarantees data delivery;
Preserves data order;
Uses acknowledgments and retransmission;
Works with ports

Examples:
* HTTP/HTTPS → usually TCP
* SSH → TCP
* FTP → TCP

Protocols (belonging to the Application layer)
* SMTP - email
* FTP - file transfer
* HTTP - web browser

<b>3-way handshake:</b>
```
Client        Server

SYN  ──────────→  
←────────── SYN-ACK  
ACK  ──────────→  

Connection established
```
Port range: 1 to 65535 (Transport layer)

Well-known ports: 1 to 1023 (HTTP - 80, HTTPS - 443, DNS - 53)

Registered ports: 1024 to 49151 (IANA)

Dynamic/Private ports: 49151 to 65535 (OS)
