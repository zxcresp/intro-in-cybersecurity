# DNS

DNS (Domain Name System) is a system that translates domain names into IP addresses

Humans remember:
google.com

Computers need an IP to connect:
142.250.x.x

For example:
```
google.com
↓
DNS
↓
142.250.x.x
```

Here is what happens when you enter a website domain:
```
Browser
↓
"I need the IP for example.com"
↓
DNS
↓
"IP = 93.184.216.34"
↓
Connect to 93.184.216.34
↓
TCP
↓
HTTPS
```
DNS uses UDP and TCP
```
DNS
├── UDP 53 - typically
└── TCP 53 - also used
```

DNS responses can also be cached to avoid asking the same thing every time.
