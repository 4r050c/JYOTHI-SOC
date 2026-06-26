# Bulk Scanner — Domains, Hashes & IPs

A browser-based SOC automation tool that performs bulk reputation checks for **IPs**, **file hashes**, **domains**, and **URLs** using multiple threat intelligence sources through a secure Cloudflare Worker proxy. Designed for SOC analysts, threat hunters, DFIR teams, and incident responders who need fast IOC triage without exposing API keys to the browser. 

LINK: https://4r050c.github.io/JYOTHI-SOC/

---

## Features

### IP Reputation Scanning

* Bulk IPv4 reputation checks
* Powered by **AbuseIPDB**
* Automatic detection of private IP addresses
* Safe / Malicious / Highly Malicious classification
* Country flag display
* CSV export of scan results
* Filtering by verdict

### Hash Reputation Scanning

* Supports:

  * MD5
  * SHA1
  * SHA256
* Automatic extraction from pasted text
* Deduplication of hashes
* Multi-source intelligence correlation:

  * VirusTotal
  * MalwareBazaar
  * AlienVault OTX
* Expandable detailed results
* Bulk scan support with configurable delay

### Domain & URL Analysis

* Domain reputation checks via VirusTotal
* URL analysis support
* Automatic URL-to-domain correlation
* Embedded VirusTotal widgets
* Safe / Suspicious / Malicious verdicts
* Defanged domain display

### Security-Focused Architecture

* API keys never exposed to users
* Cloudflare Worker acts as a secure proxy
* Supports:

  * VirusTotal API
  * AbuseIPDB API
  * MalwareBazaar API
  * AlienVault OTX API

---

## Screenshots

<img width="1279" height="486" alt="image" src="https://github.com/user-attachments/assets/458c359b-8c54-4f51-a386-c18a49ed0733" />

<img width="821" height="580" alt="image" src="https://github.com/user-attachments/assets/b969841a-e24d-4278-848b-001179b4906d" />


```text
screenshots/
```
├── ip-scanner

<img width="812" height="602" alt="image" src="https://github.com/user-attachments/assets/44f77cd3-5fa9-422e-bda2-c26ca5240fc2" />

├── hash-scanner.png

<img width="781" height="804" alt="image" src="https://github.com/user-attachments/assets/e16bc781-0759-42cc-b13f-498b559b5fd0" />

├── domain-scanner.png

<img width="787" height="355" alt="image" src="https://github.com/user-attachments/assets/1cb25599-0f89-4bbd-9dc3-8ddff02acac0" />

└── vt-widget.png

<img width="1483" height="797" alt="image" src="https://github.com/user-attachments/assets/82ccc37e-37f2-4d28-8633-5ae9af9b0f56" />

```
---

## Supported Threat Intelligence Sources

| Source         | Purpose                         |
| -------------- | ------------------------------- |
| VirusTotal     | Domain, URL and Hash reputation |
| AbuseIPDB      | IP reputation                   |
| MalwareBazaar  | Malware hash intelligence       |
| AlienVault OTX | Community threat intelligence   |

---

## Architecture

```text
Browser
   │
   ▼
Cloudflare Worker
   │
   ├── VirusTotal API
   ├── AbuseIPDB API
   ├── MalwareBazaar API
   └── AlienVault OTX API
```

The browser communicates only with the Cloudflare Worker. The Worker injects the required API keys and forwards requests to the appropriate service.

---

## Cloudflare Worker Requirements

Create the following Worker secrets:

```bash
VT_API_KEY
ABUSEIPDB_KEY
```

The Worker should:

* Accept GET and POST requests
* Inject API keys server-side
* Remove client-supplied authentication headers
* Return API responses with CORS enabled

---

## Installation

### 1. Clone Repository

```bash
git clone https://github.com/yourusername/bulk-scanner.git
cd bulk-scanner
```

### 2. Deploy Cloudflare Worker

Configure:

```bash
VT_API_KEY=<your_vt_key>
ABUSEIPDB_KEY=<your_abuseipdb_key>
```

Deploy the Worker and copy its URL.

Example:

```text
https://your-worker.workers.dev/
```

### 3. Configure Worker URL

Open the application and enter:

```text
https://your-worker.workers.dev/
```

in the **Worker URL** field.

---

## Usage

### Bulk IP Scan

1. Paste IPv4 addresses
2. Click **Scan IPs**
3. Review verdicts
4. Export results as CSV if needed

Example:

```text
8.8.8.8
1.1.1.1
185.220.101.1
```

---

### Bulk Hash Scan

Paste hashes or raw text containing hashes:

```text
44d88612fea8a8f36de82e1278abb02f
275a021bbfb648f4b5baf13f2fcbfd0c8f9f7f3a
```

The scanner automatically:

* Extracts hashes
* Removes duplicates
* Queries multiple intelligence sources
* Displays consolidated verdicts

---

### Domain Scan

```text
example.com
evil-domain.com
```

---

### URL Scan

```text
https://example.com/file.exe
https://suspicious-site.com/login
```

---

## Verdict Logic

### IP Reputation

| Score | Verdict          |
| ----- | ---------------- |
| 0-4   | Safe             |
| 5-79  | Malicious        |
| 80+   | Highly Malicious |

### Hash Reputation

A hash is marked malicious if:

* VirusTotal reports malicious detections
* MalwareBazaar contains the sample
* AlienVault OTX reports significant malicious activity

Otherwise it is marked:

```text
Known / Clean
```

or

```text
Not Found
```

---

## SOC Use Cases

### Incident Response

* Validate suspicious IPs
* Check malware hashes
* Investigate domains from alerts

### Threat Hunting

* Bulk IOC enrichment
* Intelligence correlation
* Rapid triage

### DFIR

* Artifact validation
* Malware identification
* IOC enrichment during investigations

### Email Security

* URL reputation checks
* Domain validation
* Malicious attachment hash analysis

---

## Advantages

✅ Single interface for multiple IOC types

✅ No API keys exposed in browser

✅ Multi-source intelligence correlation

✅ Bulk processing support

✅ SOC-friendly workflow

✅ CSV export capability

✅ Lightweight and portable

---

## Future Enhancements

* VirusTotal relationship analysis
* WHOIS enrichment
* GeoIP visualization
* IOC timeline export
* YARA lookup integration
* Threat actor attribution
* IOC report generation (PDF)

---

## Disclaimer

This tool is intended for defensive security, threat hunting, incident response, DFIR, and SOC operations. Always verify findings with multiple intelligence sources before taking remediation actions.

---

## Author

**jyothi Raj V**

SOC Analyst | Threat Hunter | Security Automation Developer

*"Turning repetitive SOC tasks into one-click workflows."*
