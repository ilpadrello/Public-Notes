---
title: Understanding TLS and Certifications
aliases:
  - tls
---
# The Complete Guide to TLS: How It Works, Certificates, and Handshakes

## Introduction to TLS

### What is TLS?
**TLS** stands for **Transport Layer Security**. It is the modern, secure successor to **SSL** (Secure Sockets Layer). It is a cryptographic protocol that sits between the transport layer (TCP) and the application layer (http or other) to provide three core guarantees over untrusted networks:

1. **Authentication:** Proves that the server you are talking to is actually who it claims to be (not an imposter).
2. **Confidentiality (Encryption):** Encrypts all transmitted data so eavesdroppers cannot read it.
3. **Integrity:** Ensures data cannot be modified or tampered with in transit without detection.
### What Protocols Can TLS Be Used On?
TLS is completely **application-agnostic**. It runs on top of TCP and can secure virtually any application protocol:
* **HTTP:** Transformed into HTTPS.
* **Email:** SMTP, IMAP, and POP3 (via dedicated TLS ports or upgraded via `STARTTLS`).
* **Databases:** PostgreSQL, MySQL, MongoDB, Redis.
* **Infrastructure & Directory Services:** DNS over TLS (DoT), LDAPS (Active Directory).
* **Real-time & IoT:** WebSockets (`wss://`), MQTT.
* **UDP Traffic:** Secured using **DTLS** (Datagram TLS) for video calls (WebRTC) and low-latency gaming.

---

## Step 0: Obtaining a Certificate

Before a server can serve TLS traffic, it must obtain a digital certificate signed by a trusted **Certificate Authority (CA)** like Let's Encrypt, DigiCert, or Sectigo.

### 1. Generate a Private Key and CSR (Certificate Signing Request)
You generate a Private Key locally and use `certbot` (or `openssl`) to create a **CSR (Certificate Signing Request)** file.

#### What is inside a CSR file?
A CSR contains the essential information that the CA needs to issue your certificate:
* **Server's Public Key:** Derived directly from your generated Private Key early.
* **Domain Name(s):** The Common Name (CN) and Subject Alternative Names (SAN) (e.g., `example.com`, `*.example.com`).
* **Organizational Info:** Country, State, Organization Name (optional for DV certificates).
* **Digital Signature:** Signed with your local **Private Key** to prove you control that key pair.

#### Example Command & CSR Structure
```bash
# Generate a private key and a CSR using OpenSSL
openssl req -new -newkey rsa:2048 -nodes \
  -keyout server.key \
  -out server.csr \
  -subj "/C=FR/ST=Paris/O=MyCompany/CN=example.com"
```
A raw CSR file looks like a base64-encoded block of text:

```plaintext
-----BEGIN CERTIFICATE REQUEST-----
MIICvDCCAaQCAQAwdzELMAkGA1UEBhMCRlIxDjAMBgNVBAgMUVBhcmlzMRIwEAYD
VQQKDAlNeUNvbXBhbnkxEjAQBgNVBAMMCWF1em91LmNvbTCCASIwDQYJKoZIhvcN
AQEBBQADggEPADCCAQoCggEBAO3m...
-----END CERTIFICATE REQUEST-----
```

### 2. Send the CSR to the CA
You submit the `.csr` file to the Certificate Authority (either manually via a web form or automatically via an ACME client like `certbot`). **Important:** Your Private Key (`server.key`) NEVER leaves your server.

### 3. Getting Challenged
Now is the turn of the Certificate Autority issuer to challenge your request, and make sure that you own the domain!
What is a challenge ? How you get challenged ?

#### Option A: HTTP-01 Challenge (Challenge by File)

- **How it works:** The CA says, _"Place a specific text file containing a random string at `http://example.com/.well-known/acme-challenge/Y`"
- **Verification:** The CA sends an HTTP request on Port 80 to your web server. If it fetches file `Y` and reads token (random string), ownership is proven.
- **Use case:** Standard domain certificates (e.g., `example.com` or `www.example.com`).

#### Option B: DNS-01 Challenge (Challenge by DNS)
*This method is required for wildcard certifications.*

- **How it works:** The CA says, _"Create a DNS TXT record named `_acme-challenge.example.com` with value `random_string`."
- **Verification:** The CA queries your domain's authoritative DNS servers over Port 53. If the TXT record matches the given random string, ownership is proven.
- **Why it is REQUIRED for Wildcard Certificates (`*.example.com`):**
    A wildcard certificate grants authority over _all_ current and future subdomains (e.g., `app.example.com`, `billing.example.com`, `dev.example.com`). Because these subdomains can live on completely separate physical servers across the world, proving control over a single web server (HTTP-01) is insufficient. Modifying DNS TXT records proves control over the entire domain zone.
### 4. Receiving the Certificate File
Once the challenge passes, the CA signs your certificate using their private key and returns it. The file typically ends with `.crt`, `.pem`, or `.cer`.
#### What is inside the Certificate?
A `.crt` / `.pem` file contains clear-text data accompanied by a digital signature:

- **Domain(s) Name(s):** Domain name(s) covered by the certificate. 
- **Server's Public Key:** The one that you have generated in your server.
- **Validity Period:** "Not Before" and "Not After" expiration dates.
- **CA Metadata:** Issuing authority details (e.g., Let's Encrypt).
- **CA Digital Signature:** A cryptographic hash of all the above data, **encrypted using the CA's Private Key**.
## Step 1: The Handshake & Connection Flow

When a browser (or client) opens a TLS connection to your domain, identity verification and session key negotiation occur step-by-step:

- **Server Sends `.cert`:** The web server sends its signed `.crt` / `.pem` file to the browser.
- **Browser Verifies the Signature:**
	- The browser already is pre-loaded with a list of trusted Certificate Authorities's  **Public Keys**.
    - It uses the **CA's Public Key** to verofy the CA Signature attached to the certificate.
    - If the it matches, the certificate is authentic, untampered, and valid.
- **Integrity Guaranteed:** Because changing even a single byte (like swapping the server's public key) alters the hash, a Man-In-The-Middle cannot tamper with the certificate without breaking the CA's signature.
- **Symmetric Session Key Exchange:**
    - Now that the server's identity and Public Key are trusted, both parties negotiate a shared **Secret Key** using an algorithm like **Diffie-Hellman (ECDHE)**.
    - **Why Diffie-Hellman?** Asymmetric encryption (Public/Private Keys) is computationally slow. Diffie-Hellman allows the client and server to securely establish a shared key over an untrusted network without ever transmitting the secret key itself.
- **Secure Communication:** Once the shared secret key is established, all subsequent HTTP traffic is encrypted using fast, symmetric encryption (AES-GCM or ChaCha20)

# Multi-Domain Certificate (SAN):
Is is possible to create a single certificate for multiple domains.
Those are called SAN (Subject Alternative Name) or Multi-Domain certificates.

A single certificate can cover completely different domain names, distinct TLDs, and subdomains all in one file:

- `example.com`
- `example.fr`
- `example-publishing.co.uk`
- `api.exaple.com`

#### How it works:

When generating the CSR, you define the **Subject Alternative Names (SAN)** extension. When you submit the CSR to the CA, the CA will issue separate challenges for **every single domain** listed in the request. Once you prove ownership of all of them, the CA issues one certificate listing all approved domains in its metadata.

### 2. How do multiple servers share the same private key and certificate?

A TLS certificate and private key are cryptographic files—they are tied to the **domain name's identity**, not to a physical machine, MAC address, or IP address.

If your domain points to 10 different servers, there are two primary ways this is handled in production:

#### Method A: TLS Termination at the Entry Point (Most Common)
In modern cloud architectures and Kubernetes setups, individual backend application servers don't handle TLS at all.

```plaintext
											  ┌─► Backend Server 1 (HTTP)
[ User ] ──( HTTPS )──► [ Load Balancer / ] ──┼─► Backend Server 2 (HTTP)
                        [ Ingress Proxy   ]   └─► Backend Server 3 (HTTP)
                        (Holds Key & Cert)
```

1. A **Load Balancer**, **Ingress Controller** (like NGINX or Traefik), or **Reverse Proxy** sits in front of your infrastructure.
2. The Private Key and Certificate live **only on the Load Balancer**.
3. The Load Balancer completes the TLS handshake with the user's browser (TLS Termination) and passes unencrypted (or internally encrypted) HTTP traffic to your backend servers over a secure private network.

#### Method B: Distributing the Private Key Across All Servers
If all 10 servers are directly exposed to the internet (e.g., via Round-Robin DNS), you simply copy the exact same `server.key` and `server.crt` files to all 10 servers.

```plaintext
[ User Request ] ──► Hits Server 3 ──► Uses shared server.key to complete handshake
```

- **Deployment:** Configuration management tools (Ansible, Puppet, Terraform) or Kubernetes Secrets automatically synchronize the certificate and private key across all nodes.
- **Why it works:** When a browser connects to Server 3, Server 3 uses its local copy of `server.key` to decrypt/sign the Diffie-Hellman handshake. Because `server.key` matches the public key in the `.crt` file, the browser's verification succeeds seamlessly—it doesn't care _which_ physical server responded, as long as the server possesses the matching private key.

### Is it safe to share a private key across servers?

Yes, provided all servers reside within the same trust boundary (e.g., your company's private network or cluster).

However, security best practices dictate:

- Restricting read permissions on `server.key` so only the web server process (e.g., `www-data` or `nginx`) can read it.
- Using centralized secret storage (like HashiCorp Vault, Kubernetes Secrets, or AWS Secrets Manager) to securely inject the key into servers at runtime rather than hardcoding it into disk images.

