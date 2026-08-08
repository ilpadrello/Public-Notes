---
title: DNS Domain Name System
---
# DNS: Domain Name System
The DNS is a "phonebook" containing all domain names and attaching them to specific ip addresses (almost).

When someone ask for `example.com` to a web browser, the browser must first find out the IP address of the server that it need to call.
In order to do that there are numerous DNS services that tell you exactly that.


# Different kind of DNS Entry
You do not just have `example.com` to `127.0.0.1`(example), there are differents type of DNS entries: 
## A: IPv4
When Asking for this one you get the IPv4 version of the server

## AAAA : IPv6
When asking for the AAAA you get the IPv6 version of the server

## MX: The Mail Exchange server
When asking for the MX, is probably that you are a software that send mail, so you use different protocols, so you need the correct addresse listening to that kind of protocol.
So the MX points to the **Mail Transfer Agent (MTA)** that handles email delivery via **SMTP** (Simple Mail Transfer Protocol). When someone emails `user@example.com`, the sending mail server queries the MX record of `example.com` to find where to route the email.

## TXT: Get some text
This one give you back a simple plain text.
In the beginning this was just used add metadata to a domain, today we use it to verify that you are the owner of the domain (using DNS challeging to get [[tls-certifications|TLS]] certification), or for example to say to a service like google that you own the domain.

## CNAME (Canonical Name):
Maps an alias domain name to a canonical (true) domain name instead of an IP address. 
*Example:* Points `www.example.com` to `example.com`. The DNS lookups will then resolve the IP of `example.com`.
## NS (Name Server):
Specifies which DNS servers are authoritative for the domain—meaning they hold the actual DNS records for that zone.
*Example:* Tells the rest of the internet, "To get records for `example.com`, ask `ns1.provider.com`."

## TTL (Time to Live):
A numerical value (in seconds) that tells resolvers and clients how long to cache a DNS record before asking the authoritative name server for an update again.
*Example:* A TTL of `3600` means caches keep the record for 1 hour before re-querying. Lower TTLs are useful when preparing for server migrations; higher TTLs improve loading speeds and reduce DNS traffic.
# The Root/Apex Domain CNAME Limitation

*apex/root domain = example.com (www.example.com is not an apex domain)*
By DNS specification (RFC 1034 / RFC 1912), **a CNAME record cannot coexist with any other record type for the same name.** If a CNAME exists at `example.com`, you cannot have MX, TXT, SOA, or NS records at `example.com`.

Because the root domain (apex) **must** have SOA and NS records to define the DNS zone, placing a standard CNAME record directly on the apex (`example.com`) is invalid and breaks the zone.
## What is CNAME Flattening?

**CNAME Flattening** (also known as **ANAME** or **ALIAS** records, depending on the provider like Cloudflare, AWS Route 53, or Namecheap) is a server-side workaround for this limitation.

* **How it works:** You configure a CNAME-like target at the root apex in your DNS control panel. When a user requests `example.com`, your DNS provider internally follows the dynamic target hostname to its destination, gets the resulting IP address, and returns a standard **A** or **AAAA** record directly to the client.
* **Why it matters:** The outside world only sees a valid `A`/`AAAA` record at the root apex, keeping the domain fully RFC-compliant while allowing you to point your root domain to CDNs, PaaS hosts (like Vercel, Heroku, or Netlify), or load balancers.

### A BETTER EXPLANATION : The Mechanism: Server-Side Translation

Instead of telling the internet client _"Go resolve this other hostname yourself"_ (which is what a CNAME does and what breaks the root), **your DNS provider does the lookup work on its own servers first**, and then hands the final answer back as a normal **A** or **AAAA** record.

Because the response contains an **A record** instead of a **CNAME record**:

- `example.com` can have an **A record** (`192.0.2.45`).
- `example.com` can keep its required **NS** and **SOA** records.
- `example.com` can keep its **MX** records for email.

No DNS rules are violated because the CNAME redirect only existed temporarily inside your provider's internal control panel.

# GeoDNS (Geolocation / Latency-Based DNS)

**The same domain can return completely different IP addresses depending on where you are in the world.**
#### How it works:

When a request hits an authoritative DNS server, the server inspects the IP address of the requesting DNS resolver (or uses an extension called **EDNS Client Subnet (ECS)**, which includes a masked version of your actual user IP).

1. **User in Paris** requests `app.example.com` $\rightarrow$ Authoritative server detects a European IP $\rightarrow$ Returns IP `185.x.x.x` (Frankfurt datacenter).
2. **User in Tokyo** requests `app.example.com` $\rightarrow$ Authoritative server detects an Asian IP $\rightarrow$ Returns IP `45.x.x.x` (Tokyo datacenter).

#### Why use it?

- **Lower Latency:** Directs users to the physically closest server/CDN edge.
- **Compliance / Data Sovereignty:** Keeps European user traffic routed to European infrastructure.
- **Failover:** Reroutes traffic to another continent if an entire region goes offline.

# DNS Propagation (How updates spread)

Changes do **not** get "pushed" out to all DNS servers across the globe simultaneously. Instead, DNS propagation relies on a **pull-on-demand + caching system**.

#### The Step-by-Step Propagation Flow:

```plaintext
1. YOU UPDATE RECORD
   └─► Instant change on your Authoritative DNS Provider (e.g., Cloudflare, Route53).

2. USER REQUESTS DOMAIN
   └─► User's OS asks local Recursive Resolver (e.g., ISP DNS, Google 8.8.8.8, 1.1.1.1).

3. RESOLVER CHECKS ITS CACHE
   ├── IF CACHED (TTL remaining > 0):
   │   └─► Returns OLD record immediately. (Does NOT contact your provider).
   │
   └── IF EXPIRED OR NOT IN CACHE (TTL = 0):
       └─► Queries your Authoritative Provider ──► Fetches NEW record ──► Caches it for TTL seconds.
```

#### Why DNS Propagation Takes Time (and why people say "up to 48 hours"):

1. **TTL (Time To Live) Expiration:** Intermediate servers cache your old record until its TTL counter reaches `0`. If your old TTL was `86400` (24 hours), resolvers will keep serving the old record for up to 24 hours after you change it.
    
2. **Ignoring TTLs (Bad ISP Resolvers):** Some public or ISP resolvers ignore low TTL values and enforce a minimum cache duration (e.g., forcing a 2-hour cache even if you set TTL to 60 seconds).
    
3. **Multi-layer Caching:** Caching happens at multiple levels sequentially:
    - Browser cache
    - Operating System DNS cache (e.g., `systemd-resolved`, Windows DNS Cache)
    - Local Router cache
    - ISP / Public Resolver cache (1.1.1.1 / 8.8.8.8)