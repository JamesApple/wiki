Cryptography
Asymmetric Encryption

Enciphering
Deciphering


mostly about transit


Symmetric Encryption:
  Same key locks and unlocks
  - DES
  - 3DES
  - AES
  - Blowfish

Asymmetric Encryption:
  The locker is different to the opener


Diffie-Hellman
- Creates a private key by combining two public keys

RSA
  - Ron Rivest
  - Adi Shamr
  - Leonard Adleman
Has public key and private key


PKI
Not a tech, a framework
Certificates to be issued by trusted authority


Certificate Authority
- Issues keys
- Distributes keys
- Manages keys
- Revokes keys - CRO

Sending Encrypted Email;
---
Sender acquires subjects public key
send encrypts with subjects key
Subject decrypts with their private key


_Web server SSL requests are large scale PKI infrastructures_

CA Certificate Authority -> RA Registration Authority -> Server
RA:
  Verifies the source for the servers request. Expects payment
CA:
  Generates a signed certificate for the server

Unencrypted Protocols:
- HTTP
- FTP
- SMTP

Encrypted Protocols:
- ssh Secure SHell
  * Secures a connection by sharing a public key through other means such as sending a public key to a SYSADMIN
  * All SSH protocols must communicate using port 22 as it operates at the application layer L7

SSL and TLS are frequently used interchangeably but TLS is almost always used instead of SSL.
USE TLS
- TLS Transport Layer Security ( SSL Secure Sockey Layer )
  * Operates at the Transport Layer  in the OSI model L4
  * Wraps the communication withough changing the payload
  * TLS can be used on any port as it operates at the transport layer

Hybrid encryption
- Symmetric and Asymmetric Encryption combined
Symmetric encryption requires exchanging keys in open air
Asymmetric requires relatively massive amounts of computing power

# Perform standard asymmetric encrpytion to share a symmetric encryption key
1. Asymmetric key exchange
2. Symmetric data exchange

# PKE Public Key Exchange -- TLS Handshake
1. Client sends hello
2. Server sends hello that includes servers public key
  * Client optionally verifies certificate with CA and that it hasn't been added to a CRL _Cert Revocation List_
3. Client creates symmetric session key and encrypts it with the servers public key
4. Server decrypts symmetric key with private key
  * If the shared key is in an unknown format it will request a format it understands
5. Success! Server and client communication will now be encrypted.


SSL/TLS Timeline

- SSL 1.0
  * Never publicly released
- SSL 2.0
  * 1995 First publicly available version
- SSL 3.0
  * 1996 Total redesign.
- TLS 1.x
  * 1999 TLS is not compatible but contained a fallback to allow SSL3 to continue to be used

SSL Proprietary. TLS open standard based on SSL

SSL required use of an explicit port (443) and did not perform an initial hello?. The ports on the machines themselves handle negotiating the PKE

TLS is a protocol that allows implicit port selection. When a connection is made to the port the server is responsible for handling the handshake


Building a certificate
---
```sh
sudo su
yum install nginx 

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/test.key -out /etc/nginx/ssl/test.crt

nano /etc/nginx/sites-enabled/default

Under where you find HTTP traffic, or listening on port 80, add the line:

return 301 https://$server_name$request_uri;

server {
    listen 443 ssl;

    ssl_certificate /etc/nginx/ssl/test.crt;
    ssl_certificate_key /etc/nginx/ssl/test.key;
}
```

```sh
# Add server IP to OpenSSL config file before creating certs:

vim /etc/pki/tls/openssl.cnf
# Add line:
# subjectAltName=IP:serverIPaddress


openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /certs/test.key -out /certs/test.crt

# Create the registry

docker run -d -p 5000:5000 --restart=always --name registry -v /certs:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/test.crt -e REGISTRY_HTTP_TLS_KEY=/certs/test.key registry:2
# Add certificates to Docker's trusted certs, and then reload Docker:

mkdir -p /etc/docker/certs.d/<serverIP>:5000
cd /certs
cp /certs/test* /etc/docker/certs.d/<serverIP>:5000/
cd /etc/docker/certs.d/<serverIP>:5000/
mv test.crt ca.crt
```


Getting a web server TLS cert
1. Generate a CSR on the host machine
2. Choose a CA
3. Give the CA your CSR
4. Validate your domain to the RA/CA by placing a file on your server
