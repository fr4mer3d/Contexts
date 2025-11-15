# King's Assets: Kingdom Infrastructure Mapping

Command reference for authorized infrastructure reconnaissance.

---

## Workflow Overview

**Main Objective:** Map the target complete external infrastructure to identify exploitation vectors.

```
START: Identify Target Scope
    ↓
1. SURVEY → Know who we're targeting (parent companies, subsidiaries)
    ↓
2. MAP SUPPLY → Find network footprint (ASNs, IP ranges)
    ↓
3. ENUMERATE → Discover all digital properties (domains, subdomains)
    ↓
4. REVERSE → Link assets together (IP↔domain relationships)
    ↓
5. TRACK → Find related properties via shared trackers (expanded scope)
    ↓
6. BRUTE FORCE → Discover hidden/missed subdomains (comprehensive discovery)
    ↓
7. LOCATE SERVERS → Find active services (vulnerability targets)
    ↓
8. CLOUD ASSETS → Discover cloud storage (high-value exposure)
    ↓
9. SECRETS → Find exposed credentials/keys (immediate risk)
    ↓
10. GOOGLE DORKS → Expose publicly leaked information (final exposure layer)
    ↓
END: Complete Attack Surface Map + Prioritized Vulnerabilities
```
---

## Helper Functions

**Add these to `.bashrc` or source as needed:**

```bash
# Certificate Transparency enumeration
crt(){
  curl -s "https://crt.sh/?q=%25.$1" | grep -oE "[\.a-zA-Z0-9-]+\.$1" | sort -u
}

# RapidDNS enumeration
rapiddns(){
  curl -s "https://rapiddns.io/subdomain/$1?full=1" | grep -oE "[\.a-zA-Z0-9-]+\.$1" | sort -u
}

# Get all subdomains from multiple sources
allsubs(){
  (crt $1; rapiddns $1) | sort -u
}

# Resolve domain to IP
resolve(){
  dig +short $1 @8.8.8.8
}

# Screenshot web targets
screenshot(){
  eyewitness -f $1 -d screenshots
}
```

---

## 1. Survey the Kingdom (Companies & Subsidiaries)

**Manual:**
- https://www.crunchbase.com/ → Search company, identify acquisitions

---

## 2. Map Supply Lines (ASNs & IP Ranges)

**Get ASNs by domain:**

```bash
curl "https://bgp.he.net/?k=example.com&asn=1" | grep -oE "AS[0-9]+"
```

**Get IP ranges for ASN (free API):**

```bash
curl "https://asnlookup.com/api/v1/as/AS16509/" | jq '.data[].cidr'
```

**Domain to IP (historical):**

```bash
curl "https://ipv4info.com/?q=example.com"
# Or: securitytrails.com (free tier)
```

**Find domains on IP:**

```bash
hakip2host 1.2.3.4
```

---

## 3. Domain & Subdomain Enumeration

**Quick enumeration (uses helper functions):**

```bash
allsubs example.com | tee subdomains.txt
```

**Full enumeration with BBOT (automated + ASN aggregation):**

```bash
bbot -t example.com -f subdomain-enum
```

**Subdomain mutations & permutations:**

```bash
cat subdomains.txt | dnsgen -
```

---

## 4. Reverse Reconnaissance

**Reverse DNS on IP range:**

```bash
dnsrecon -r 1.2.3.0/24 -n 8.8.8.8
```

**Reverse Whois (manual):**
- https://viewdns.info/reversewhois/
- https://domaineye.com/reverse-whois
- https://www.reversewhois.io/
- https://www.whoxy.com/

**DomLink automation:**

```bash
domlink search -company "TargetName"
```

---

## 5. Track Royal Insignia (Trackers & Identifiers)

**Calculate favicon hash (Python function):**

```python
import mmh3, requests, codecs

def fav_hash(url):
    response = requests.get(url)
    favicon = codecs.encode(response.content, "base64")
    fhash = mmh3.hash(favicon)
    print(f"{url} : {fhash}")
    return fhash

fav_hash('https://example.com/favicon.ico')
```

**Search Shodan by favicon hash:**

```bash
shodan search org:"TargetName" http.favicon.hash:116323821 --fields ip_str,port --separator " " | awk '{print $1":"$2}'
```

**Shared tracker discovery (manual):**
- https://www.builtwith.com/ → Google Analytics ID, Adsense
- https://www.spyonweb.com/ → Search tracking codes
- https://www.sitesleuth.com/ → Related sites
- https://www.publicwww.com/ → Source code search

---

## 6. Hidden Settlements (DNS Brute Force)

**Download wordlists:**

```bash
curl -o subdomains.txt https://wordlists-cdn.assetnote.io/data/manual/best-dns-wordlist.txt
curl -o resolvers.txt https://raw.githubusercontent.com/trickest/resolvers/main/resolvers-trusted.txt
```

**shuffledns (GO async):**

```bash
shuffledns -d example.com -list subdomains.txt -r resolvers.txt -o found.txt
```

**aiodnsbrute (Python async):**

```bash
aiodnsbrute -r resolvers.txt -w subdomains.txt -t 1024 example.com
```

---

## 7. Locate Fortifications (Web Servers)

**Find active web servers:**

```bash
cat domains.txt | httprobe
cat domains.txt | httprobe -p http:8080 -p https:8443
```

**Screenshots:**

```bash
# EyeWitness
eyewitness -f domains.txt -d screenshots

# Aquatone
cat domains.txt | aquatone

# Gowitness
gowitness file -f domains.txt
```

---

## 8. Sky Fortresses (Cloud Assets)

**Download bucket wordlists:**

```bash
curl https://raw.githubusercontent.com/jordanpotti/AWSBucketDump/master/BucketNames.txt -o buckets.txt
```

**Create keyword list:**

```bash
echo "example examplecorp target backup prod db api" | tr ' ' '\n' > keywords.txt
```

**Enumerate buckets:**

```bash

# S3Scanner
s3scanner buckets keywords.txt

# cloud_enum
cloud_enum -k example -k examplecorp

# CloudScraper
cloudscraper -t example examplecorp
```

---

## 9. Hidden Treasures (Secrets & Credentials)

**GitHub secret search:**

```bash
# truffleHog
trufflehog github --org TargetCompany --only-verified

# gitleaks
gitleaks detect --source github --verbose --org TargetCompany

# noseyparker
noseyparker scan --datadir ./np-data https://github.com/TargetOrg

# GitDorker
python3 gitdorker.py -q "TargetCompany credentials api_key" -k github_token
```

**Local filesystem scan:**

```bash
# detect-secrets
detect-secrets scan > .secrets.baseline

# git-secrets
git secrets --register-aws && git secrets --scan
```

---

## 10. Cast the Net (Google Dorking)

**Manual dorks (one per line):**

```
site:example.com filetype:pdf
site:example.com "password" OR "api_key"
site:example.com inurl:admin
site:github.com example.com credentials
"example.com" inurl:backup
inurl:example.com/.git/config
```

**Automated Google dorking:**

```bash
gorks -target example.com
```

**Google checks via lynx (automated scraping):**

```bash
lynx -dump "https://www.google.com/search?q=site:example.com+inurl:admin" | grep -i "http"
```

**Shodan advanced:**

```bash
shodan search org:"TargetName" http.title:"admin"
shodan search org:"TargetName" product:Apache 2.4.49
shodan search org:"TargetName" "Server: nginx"
```

**External resources:**
- Google Hacking Database: https://www.exploit-db.com/google-hacking-database
- Shodan filters: https://github.com/jakejarvis/awesome-shodan-queries

---

## Quick Workflows

**Full reconnaissance pipeline:**

```bash
# 1. Get all subdomains
allsubs example.com | tee domains.txt

# 2. Resolve to IPs
cat domains.txt | while read d; do resolve $d; done | sort -u | tee ips.txt

# 3. Find active web servers
cat domains.txt | httprobe | tee active.txt

# 4. Screenshot targets
screenshot active.txt

# 5. Search Shodan
for ip in $(cat ips.txt); do shodan host $ip; done
```

**GitHub secret hunting + local scan:**

```bash
gitleaks detect --source github --verbose --org TargetOrg
detect-secrets scan > .secrets.baseline
```

**Full domain enumeration + mutations:**

```bash
allsubs example.com > subs.txt
cat subs.txt | dnsgen - | shuffledns -r resolvers.txt | tee all-domains.txt
```

---

**Authorization Required:** Authorized bug bounties, penetration tests, or defensive assessments only.
