# DHCP (Dynamic Host Configuration Protocol)

**DHCP** is a network protocol that automatically assigns IP configurations to devices.

## What DHCP assigns

* IP address
* Subnet mask
* Default gateway
* DNS servers
* IP address lease time

## How DHCP works

DHCP uses the four-step **DORA** process:

1. **Discover** - the client searches for a DHCP server.
2. **Offer** - the server offers an IP address.
3. **Request** - the client requests the offered address.
4. **ACK (Acknowledgment)** - the server confirms the assignment of the address.

```
Client → Discover → DHCP Server
Client ← Offer    ← DHCP Server
Client → Request  → DHCP Server
Client ← ACK      ← DHCP Server
```

Initial messages are typically transmitted via broadcast, as the client does not yet know its network configuration.

## DHCP ports

DHCP for IPv4 uses UDP:

* **UDP 67** - server.
* **UDP 68** - client.

## DHCP Lease

A **lease** is the assignment of an IP address for a specific period. Upon expiration, the client attempts to renew the lease. The address may be assigned to another device after the lease is released or expires.

## DHCP vs. Static IP

* **DHCP** - configuration is assigned automatically.
* **Static IP** - configuration is set manually or via a permanent DHCP reservation. ## Example

The router acts as a DHCP server and assigns the following to the computer:

```
IP address:      192.168.1.100
Subnet mask:     255.255.255.0
Default gateway: 192.168.1.1
DNS server:      192.168.1.1
```

The computer receives these settings automatically and can communicate with other devices on the network.

## DHCP and Security

* **Rogue DHCP Server** - an unauthorized DHCP server that may issue incorrect network settings.
* **DHCP Starvation** - the exhaustion of the available IP address pool caused by a large number of requests.
* To ensure security, managed switches use **DHCP Snooping** and restrict trusted DHCP ports.

## DHCP Server
A device or program that automatically assigns network settings to connected devices, enabling them to communicate with each other and access the Internet.
