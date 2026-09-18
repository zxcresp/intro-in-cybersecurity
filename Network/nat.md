# NAT

NAT – Network Address Translation
It allows devices with private addresses to access the Internet using the router's public address

Example:
```
	Your PC
192.168.1.100:52000
      ↓
	Router
  192.168.1.1
      ↓
   Public IP
   85.x.x.x
      ↓
   Internet
```
The router tracks connection mappings and translates addresses/ports

This is one of the reasons why a vast number of devices can use private IPv4 addresses
