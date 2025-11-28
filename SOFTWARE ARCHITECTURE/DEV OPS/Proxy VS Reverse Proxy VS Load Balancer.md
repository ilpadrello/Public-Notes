
## Proxy (or Foward Proxy)
It Forward all request from one place to another. Also protect from unwanted inbound connections. This is useful when you have a big company and you want to centralise internet routing. So that you can : 
- **Access Control** : Block access to certain websites or restrict internet usage within a company
- **Security** : Scan for any viruses and block them in a centralized way.
- **Monitoring** : You also could log employees' web activity

There is also another advantage, **caching** ! 

## Reverse Proxy
If a proxy act as a middleman that receive all the requests from inside and forward them to the outside, the reverse proxy does the exact opposite.
The reverse proxy is like a waiter that sit you at the correct table.
So based on the request, it now internally where to redirect the request.

So is a one "entry point" for all requests that are distributed based on différents factors : 

- **Sub-Domains** : is it possible to redistribute content based on the sub domain of a particular domain : sub1.domain.com, sub2.domain.com
- **Load Balancing** : distributing requests to différents servers based on load.
- **Security** : Block from unwanted or repeated access (brute forcing)
- **Security** : Let you hide servers from behind a single entry point (server are not directly exposed to attacks)
- **Centralized Security** :  Let you centralize security in one place (SSL, etc.)

Some Software : 
- Nginx

## Load Balancer
A load balancing work can be done by a reverse proxy but can also be done by a specific piece of software (or cloud service). So why  are there necessary ? 

