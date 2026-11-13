# NETWORKWALKS-MAHEK-B082-WK2-FOOTPRINTING-SCANNING

Week 2 submission — Networkwalks Cybersecurity & Ethical Hacking Internship (Batch B082)

This week's topic was **footprinting and reconnaissance**. I completed all four elective modules plus both required ones:

- ✅ W2-PM1 — Footprinting with multiple Kali tools
- ✅ W2-PM2 — Footprinting with GHDB (Google dorking)
- ✅ W2-PM3 — Footprinting with Maltego
- ✅ W2-PM4 — Footprinting with theHarvester
- ✅ W2-PM5 — Network scanning with Zenmap (required)
- ✅ W2-PM-FINAL — Penetration testing report (required)

📄 Full write-up with risk analysis and recommendations: [`W2-PM-FINAL-Report.pdf`](./W2-PM-FINAL-Report.pdf)

---

## Task 1 — Footprinting networkwalks.com with Kali Tools

Ran 6 built-in Kali tools against `networkwalks.com`.

| Tool | Command | What I found |
|---|---|---|
| whois | `whois networkwalks.com` | Registered with GoDaddy, created Nov 2019, HostGator name servers |
| whatweb | `whatweb networkwalks.com` | WordPress 7.1.4, WP Download Manager 3.3.76, IP 192.232.216.135 |
| nslookup | `nslookup networkwalks.com` | Confirmed IP 192.232.216.135 |
| curl | `curl -I https://networkwalks.com` | HTTP headers, WordPress REST API path (`/wp-json/`) |
| wafw00f | `wafw00f networkwalks.com` | Protected by ModSecurity (SpiderLabs) WAF |
| dnsrecon | `dnsrecon -d networkwalks.com` | MX, SPF and SRV (cPanel email) records |

WHOIS Command :-

<img width="1920" height="1200" alt="pm1-01-whois" src="https://github.com/user-attachments/assets/5d379db0-675a-4a77-9718-9130c25c679c" />

📁 [View all PM1 screenshots](./Pm1-Kali-Tools)

---

## Task 2 — Footprinting with GHDB

GHDB (exploit-db.com) isn't a search engine itself — it's a library of pre-written Google search queries ("dorks"). You copy a dork from GHDB, paste it into Google, and Google does the finding.

- Found 10 live, unauthenticated camera feeds using dorks like `intitle:"webcamXP" inurl:8080`.
- Found 10 open directories with downloadable maths PDFs using dorks like `intitle:index.of "parent directory" mathematics pdf` — e.g. `https://theswissbay.ch/pdf/Gentoomen%20Library/Maths/`.

**Note:** the live camera links aren't included here since they're real unsecured devices belonging to other people — only the dork technique and the harmless PDF-directory example are shown publicly. Full table (private): [`GHDB-Filled-Table.docx`](./GHDB-Filled-Table.docx).

Downloadable Mathematics ebook pdf :-

<img width="1920" height="1200" alt="pm2-02-math-pdf" src="https://github.com/user-attachments/assets/8a63d2a3-d5f6-4e27-8494-3f1d111d7453" />

📁 [View all PM2 screenshots](./PM2-GHDB)

---

## Task 3 — Footprinting with Maltego

Installed Maltego on Windows, ran the domain entity `networkwalks.com` through the **"To Emails [Search Engine]"** transform. Found `info@networkwalks.com` as a related email, shown as a graph instead of plain text output.

Maltego domain-to-email :-

<img width="1920" height="1200" alt="pm3-03-result" src="https://github.com/user-attachments/assets/a5f689fe-d628-47df-b153-07595cec4ecc" />

📁 [View all PM3 screenshots](./PM3-Maltego)

---

## Task 4 — Footprinting with theHarvester

Target for this module was `microsoft.com`.

| Run | Command | Result |
|---|---|---|
| 1 | `theHarvester -d microsoft.com -l 1000 -b baidu` | 27 sub-domains, 0 emails |
| 2 | `theHarvester -d microsoft.com -l 50 -b all` | 3 emails, 9978 hosts, 158 IPs, 9 ASNs |

theHarvester All source :-

<img width="1920" height="1200" alt="pm4-01-all-source" src="https://github.com/user-attachments/assets/39be14ac-0d33-42f7-9a4b-3ed458933e83" />

📁 [View all PM4 screenshots](./PM4-theHarvester)

---

## Task 5 — Network Scanning with Zenmap

Scanned my own home network instead of a company target.

**Problem I hit:** `ipconfig` showed 3 adapters (real Wi-Fi + VMware's vmnet1/vmnet8). A normal Ping Scan on any of them returned 0 hosts.

**Root cause:** vmnet1/vmnet8 are isolated virtual networks with nothing on them — and on the real Wi-Fi adapter, Windows Firewall was silently blocking ICMP Echo Request replies, so even a correct scan came back empty.

**Fix:** switched to ARP-based discovery, which works on a local network even when ICMP is blocked:
```
nmap -PR -sn 10.255.199.0/24
```
This found 2 live hosts: my PC (`10.255.199.116`, MAC `58-CD-C9-D6-0D-A3`) and the gateway (`10.255.199.242`, MAC `5A:B9:22:85:88:50`). Topology saved as PDF via the Topology tab.

Zenmap Topology :-

<img width="1920" height="1200" alt="pm5-04-topology" src="https://github.com/user-attachments/assets/477b75fc-efff-4012-a919-d289650ca256" />

📁 [View all PM5 screenshots](./PM5-Zenmap)

---

## What I learned

Reconnaissance doesn't need to touch a target to be powerful — whois, whatweb, GHDB, Maltego and theHarvester all pulled real, usable information from public sources alone. The most useful personal lesson was from the Zenmap troubleshooting: "no results" from a scan doesn't mean "no hosts," it can just mean a firewall or a virtual adapter is in the way.

---

**Author:** Mahek — Cybersecurity & Ethical Hacking Intern, Networkwalks (Batch B082)
