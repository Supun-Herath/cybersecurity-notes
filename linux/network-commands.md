# Network Commands in Linux

> The terminal commands I use to inspect my network, check connectivity, look up DNS records, make web requests, and see open ports.

**Date:** 2026-10-22
**Source(s):** linuxjourney.com, hands-on practice in my terminal
**Tags:** `linux` `networking` `ping` `dig` `curl` `ss`

## What it is
These commands show how my computer connects to a network and how it talks to other machines. They are everyday tools for troubleshooting connections and investigating suspicious activity.

## How it works

| Command | What it does | Example |
| :-- | :-- | :-- |
| `ip a` | Shows interfaces, IP and MAC addresses | `ip a` |
| `ping` | Checks if a host replies and how fast | `ping -c 4 example.com` |
| `traceroute` | Shows each hop to a destination | `traceroute example.com` |
| `dig` | Looks up DNS records | `dig example.com` |
| `curl` | Fetches a web page or API | `curl https://example.com` |
| `curl -I` | Shows only the HTTP headers | `curl -I https://example.com` |
| `ss -tuln` | Lists ports my machine is listening on | `ss -tuln` |

### Reading the output
- **`ip a`:** `lo` is the loopback interface (`127.0.0.1`, my own machine). `inet` is the IPv4 address and `link/ether` is the MAC address.
- **`ping`:** `time=` is the round-trip time in milliseconds. "packet loss" shows how many replies were lost.
- **`traceroute`:** each line is a hop (a router) between me and the destination.
- **`dig`:** the ANSWER SECTION shows the IP address and TTL (how long the answer can be cached).
- **`curl -I`:** the first line is the status code (200 OK, 404 not found), followed by headers like `content-type` and `server`.
- **`ss -tuln`:** `t` TCP, `u` UDP, `l` listening, `n` numeric. The Local Address:Port column shows what is open.

## Example I ran
```bash
ping -c 4 example.com
dig +short example.com
curl -I https://example.com
ss -tuln
```

## Mistakes and surprises
- *traceroute was not installed and I had to install it.*
- *Some hops showed * * * because they do not reply to probes.*
- *ping to a made-up domain failed with "Name or service not known", which shows a DNS failure, not a network failure.*

## Security angle
- **Open ports are attack surface:** every listening port is a possible way in. `ss -tuln` helps me check that only needed services are open.
- **Privacy:** IP and MAC addresses identify my device and network, so I remove them from public notes.
- **DNS matters:** attackers abuse DNS (fake domains, phishing), and `dig` lets me check where a name points.
- **HTTP headers reveal information:** `curl -I` can show the server type and version, which attackers use to look for known weaknesses.
- **Ethics:** I only test my own machine and public test sites. Scanning others without permission can be illegal.

## Key takeaways
- `ip a` shows my addresses, and `ping` tests reachability.
- `dig` answers "what IP is this name?", and `traceroute` shows the path.
- `curl -I` shows how a web server responds without downloading the page.
- `ss -tuln` shows which ports my machine has open.

## Questions I still have
- What is the difference between a public and a private IP address?
- How does `nmap` find open ports on a machine?
- What do the different DNS record types (A, MX, TXT) mean?
