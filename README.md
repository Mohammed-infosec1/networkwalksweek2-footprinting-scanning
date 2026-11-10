# Networkwalks Week 2 - Footprinting and Network Scanning

**Cybersecurity Internship - Week 2**  
**Instructor:** Waqas Karim (CCIE)  
**Organization:** Networkwalks IT Solutions & Training  
**Student:** Mohammed Khasro

## About this project

For Week 2 of my cybersecurity internship, I worked on gathering public information about a domain and scanning a virtual machine in my own lab. I used **theHarvester**, **WHOIS**, and **Zenmap**. I also checked the Windows IP configuration before scanning it.

At first I was learning how to use the tools, so I wanted to keep the results and explain what I actually did. I did the **W2-PM4 theHarvester** elective and the required **W2-PM5 Zenmap** project. This README documents my work, and I prepared a separate Week 2 report as well.

## Scope of the work

- **Passive footprinting:** `networkwalks.com` (approved by my instructor for this exercise).
- **Active network scanning:** only my own **Windows virtual machine** at `10.0.0.11` on the VirtualBox NAT Network.
- I did **not** scan Networkwalks' public servers with Zenmap or try to exploit any system.

## Tools I used

- Kali Linux and **theHarvester 4.10.1**
- `whois` on Kali Linux
- Zenmap (graphical version of Nmap)
- Oracle VirtualBox
- Windows 10 Command Prompt (`ipconfig`)

## 1. Footprinting with theHarvester

First, I used theHarvester to search for public information about Networkwalks. I tried the `crtsh` source earlier, but it didn't return hosts. Then I changed to the **HackerTarget** source, and this time I found two hosts.

```bash
theHarvester -d networkwalks.com -b hackertarget -l 100
```

### My result

| Hostname | IP address returned by HackerTarget |
| --- | --- |
| `autodiscover.networkwalks.com` | `192.232.216.135` |
| `mail.networkwalks.com` | `192.232.216.135` |

The tool reported **2 hosts found**. It also showed **no direct IP entries, emails, or people** in its other result categories. The hostnames were associated with the same IP address in that result. This is information returned by the source at the time I ran it, not proof that the related services are currently running.

![My theHarvester HackerTarget results](images/01.png)

## 2. Extra footprinting - WHOIS lookup

I also tried WHOIS to learn more about the domain registration.

```bash
whois networkwalks.com
```

The output showed **GoDaddy.com, LLC** as the registrar, a **creation date of November 6, 2019**, and a **registry expiry date of November 6, 2027**. The screenshot also showed the nameservers `NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM`.

This helped me understand that WHOIS can show registration details, registrar information, nameservers, and domain status. These are the values visible in my screenshot; WHOIS records can change.

![My WHOIS lookup results](images/02.png)

## 3. Checking my Windows virtual machine

Before using Zenmap, I started my Windows VM in VirtualBox and checked its network information.

```cmd
ipconfig
```

These were the settings shown:

| Setting | Value |
| --- | --- |
| IPv4 address | `10.0.0.11` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `10.0.0.1` |

I checked this so I would know the correct IP address for the Zenmap target. This was my **own lab machine**, not a public website.

![Windows VM IP configuration](images/03.png)

## 4. Zenmap network scanning

Next, I opened Zenmap inside Kali Linux and selected my Windows VM as the target. I used the **Intense scan** profile.

```bash
nmap -T4 -A -v 10.0.0.11
```

### My scan results

- **Target:** `10.0.0.11`
- **Host status:** up
- **TCP ports checked:** 1,000
- **Port result:** all 1,000 were reported as **filtered (no-response)**
- **MAC address:** identified as an Oracle VirtualBox virtual NIC
- **Network distance:** 1 hop
- **Operating system:** Zenmap couldn't determine one specific OS from the fingerprints
- **Scan time:** 28.22 seconds

So Zenmap was able to identify that the host was up, but it couldn't confirm any open TCP ports among those 1,000 checks. This does **not** mean Windows has no services, and it doesn't prove that it's secure. A firewall or another filtering mechanism can make ports show as filtered.

![My Zenmap Intense Scan output](images/04.png)

## What I learned

I learned how passive footprinting is different from active scanning. With theHarvester and WHOIS, I collected information that was already publicly available. With Zenmap, I scanned my own Windows machine to learn about host discovery and TCP port states.

I also learned why checking the target IP address matters, what an Nmap **filtered** result means, and why I shouldn't assume that a port is closed just because it didn't respond.

## Result

I completed my theHarvester footprinting exercise and my Zenmap Intense Scan in my own virtual lab. I also tried WHOIS as extra practice. I saved these four screenshots as evidence of what I did, and I included the observations in my Week 2 report.

Thanks to **Networkwalks IT Solutions & Training** and our instructor **Waqas Karim (CCIE)** for the guidance.

---
*Educational internship documentation. Public-domain information was gathered passively, and the active scan targeted only my own local Windows virtual machine. No exploitation or vulnerability validation was performed.*
