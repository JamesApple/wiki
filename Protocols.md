# NAT: Network Address Translation

# OSI Layers

## Layers and their PDU _Protocol Data Unit_
7. _Application Layer_
  - HTTP Defines how data is encoded for communication between server and browser

4. _Transport Layer_
  - TCP _Transmission Control Protocol_ Builds a virtual connection between clients that ensures lost/interrupted packets are always received in order and retried. Applications are assigned TCP Ports to allow multiple applications to send and receive data on the same host

3. _Internetwork Layer_ 
  - IP _Internet Protocol_ Delivers packets from an origin host to a destination host

# Layer 7 Load Balancing

EG:
* NGINX
* Reverse proxies
* Service mesh routing

Combines layers 5, 6 and 7

Determines request target by examining the requests content such as headers, URL or data instead of a static address

* Slower than L4LB for standard requests
* More efficient if an instance or region has cacheable state so that the data doesn't have to be retransmitted

# Layer 3/4 Load Balancing

Uses a L3 IP address and an L4 Port to determine the correct host to send load to.

# Reverse Proxy

A pass-through service that sits behind the firewall

- Compression
- Load balancing
- Caching
- Limit access to hosts
                                          Host A
                                         / 
                                        /
Client --> [Internet] -> Reverse Proxy -
                                        \
                                         \
                                          Host B

# Forward Proxy

Client A
          \
           \
            --Forward Proxy--> [Internet] -> Target
           /
          /
Client B


# Level 7 Protocols
## FTP
## SMTP
## HTTP

# EFS
Hybrid encryption filesystem
1. Files are encrypted with a symmetric key
2. Symmetric key is encrypted with public key of each user. Users auth to the system in order to receive symmetric key for decryption

# LDAP Lightweight Directory Access Protocol
Open LDAP
Cons:
1. Server is unauthenticated to the client so a user could be redirected
2. Traffic could be intercepted
Initially communicated over 636 with SSL but now uses STARTTLS to communicate over standard port

