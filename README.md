# Networking Commands Every DevOps / SRE Must Know (Production Focus)

This repository is built from **real production incidents**, not tutorials.

Every command here has been used while:
- Applications were down
- APIs were timing out
- Kubernetes pods were unreachable
- Load tests were failing
- Teams were waiting for answers

If you want to survive in DevOps or SRE,  
you must be **calm, methodical, and accurate with networking**.

---

## 1. ping – Basic Connectivity Check

~~~bash
ping google.com
~~~

### What this command does  
Sends ICMP echo requests to check whether a host is reachable.

### When I use it in production  
- First signal check when an outage is reported  
- Confirming DNS resolution and basic network reachability  
- Verifying whether a server is alive at all

### Common parameters

~~~bash
ping -c 4 google.com
~~~
- `-c 4` → Sends only 4 packets and exits (recommended for scripts)

~~~bash
ping -i 2 google.com
~~~
- `-i 2` → Sends packets every 2 seconds instead of default 1

### Production reality  
Many production servers block ICMP.  
A failed ping does **not automatically mean downtime**.

---

## 2. traceroute – Identify Network Hop Issues

~~~bash
traceroute google.com
~~~

### What this command does  
Shows the path packets take across routers to reach a destination.

### When I use it in production  
- Application works from one network but not another  
- Suspected firewall or routing misconfiguration  
- Identifying where traffic is being dropped

### How to read the output  
- Each line is one network hop  
- `* * *` usually means timeout or blocked traffic

---

## 3. curl – The Most Used DevOps Networking Tool

~~~bash
curl https://example.com
~~~

### What this command does  
Sends HTTP requests and prints the response.

### When I use it in production  
- API health checks  
- Backend validation without a browser  
- SSL and header debugging

### Common parameters

~~~bash
curl -I https://example.com
~~~
- `-I` → Fetch only response headers

~~~bash
curl -v https://example.com
~~~
- `-v` → Verbose output (TLS, headers, handshake details)

~~~bash
curl -X POST https://api.example.com
~~~
- `-X` → Specify HTTP method

### Production insight  
If `curl` hangs, suspect:
- Firewall rules  
- Security groups  
- Proxy configuration  
- Load balancer health

---

## 4. ss – Check Listening Ports (Preferred)

~~~bash
ss -tulnp
~~~

### What this command does  
Displays listening ports and the processes using them.

### When I use it in production  
- Service is running but unreachable  
- Verifying port bindings  
- Identifying port conflicts

### Parameter breakdown  
- `-t` → TCP  
- `-u` → UDP  
- `-l` → Listening sockets  
- `-n` → Numeric output  
- `-p` → Process information

---

## 5. netstat – Legacy but Still Seen

~~~bash
netstat -tulnp
~~~

### What this command does  
Shows network connections and listening ports.

### When I use it in production  
- Older Linux systems  
- Quick validation when `ss` is unavailable

### Note  
Prefer `ss` whenever possible. It is faster and more accurate.

---

## 6. telnet – Verify Port Connectivity

~~~bash
telnet example.com 443
~~~

### What this command does  
Attempts a raw TCP connection to a specific port.

### When I use it in production  
- App cannot connect to database  
- Testing firewall and security group rules  
- Verifying basic TCP reachability

### Interpretation  
If telnet connects, networking is **not the problem**.

---

## 7. nc (netcat) – Lightweight Connectivity Testing

~~~bash
nc -zv example.com 443
~~~

### What this command does  
Checks TCP or UDP port availability without opening a session.

### When I use it in production  
- Faster alternative to telnet  
- Script-friendly connectivity checks  
- CI/CD validation steps

### Parameters  
- `-z` → Scan mode  
- `-v` → Verbose output

---

## 8. nslookup – Basic DNS Resolution

~~~bash
nslookup google.com
~~~

### What this command does  
Queries DNS servers to resolve domain names.

### When I use it in production  
- DNS-related application failures  
- Verifying internal and external DNS records

---

## 9. dig – Advanced DNS Debugging

~~~bash
dig google.com
~~~

### What this command does  
Provides detailed DNS response information.

### When I use it in production  
- DNS propagation issues  
- TTL verification  
- Multi-region DNS validation

### Clean output option

~~~bash
dig google.com +short
~~~
- Displays only resolved IPs

---

## 10. tcpdump – Packet-Level Investigation

~~~bash
tcpdump -i eth0
~~~

### What this command does  
Captures network packets directly from an interface.

### When I use it in production  
- Deep network issues  
- Load balancer debugging  
- Verifying request flow between services

### Warning  
Use carefully on high-traffic systems.  
Packet capture can generate massive output.

---

## Final Production Advice

- Never assume. Always validate.  
- Test from the **same host** where the app runs.  
- Correlate multiple commands before concluding.

---

Follow **devopsvibez** on Instagram  
& subscribe it on YouTube
