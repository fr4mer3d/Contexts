# Fortress Infrastructure: Internal Network Reconnaissance

Command reference for authorized internal network discovery and enumeration.

---

## Workflow Overview

**Main Objective:** Discover active hosts and open services within the target network to identify edge computers for lateral movement.

**Algorithm:**

```
START: Foothold Established in Network
    ↓
1. ICMP SWEEP → Fastest alive host discovery (if ICMP not filtered)
    ↓
2. TCP PORTS → Discover services when ICMP is blocked (common ports)
    ↓
3. UDP PORTS → Find UDP services (DNS, SNMP, DHCP, etc.)
    ↓
4. SCTP PORTS → Specialized protocol discovery (uncommon but fast)
    ↓
5. PASSIVE DISCOVERY → Learn network structure without sending packets
    ↓
6. ACTIVE DISCOVERY → Full port scan on discovered hosts (TCP + SYN)
    ↓
7. IPv6 DISCOVERY → Find dual-stack hosts (often forgotten/misconfigured)
    ↓
8. NETWORK SNIFFING → Capture unencrypted traffic for credentials/patterns
    ↓
END: Complete Internal Asset Map + Service Inventory + Traffic Analysis
```
---

## Helper Functions

**Add these to `.bashrc` or source as needed:**

```bash
# ICMP host sweep (fping quick check)
icmpsweep(){
  fping -g $1 2>/dev/null | grep "is alive"
}

# TCP port scan on single host
tcpscan(){
  nmap -p- --min-rate=1000 -T4 $1
}

# Quick HTTP/HTTPS discovery on network
httpscan(){
  nmap -p80,443,8000-8100,8443 -sV $1
}

# UDP port scan on single host
udpscan(){
  nmap -sU -sV --version-intensity 0 -F -n $1
}

# Passive ARP discovery
arpdiscover(){
  arp-scan -l
}

# TCPdump to file with rotation
startsniff(){
  sudo bash -c "sudo nohup tcpdump -i $1 -G 300 -w '/tmp/dump-%m-%d-%H-%M-%S-%s.pcap' -W 50 'tcp and (port 80 or port 443)' &"
}

# List captured pcap files
listcaps(){
  ls -lh /tmp/dump-*.pcap
}
```

---

## 1. ICMP Host Discovery

**Fastest method if ICMP not filtered:**

```bash
fping -g 199.66.11.0/24
fping -g 192.168.1.0/24 2>/dev/null | grep "is alive"
```

**Extended ICMP probes (echo + timestamp + subnet mask):**

```bash
nmap -PE -PM -PP -sn -n -vvv 199.66.11.0/24
```

**IPv4 CIDR notation examples:**
- `/24` = 256 hosts (255.255.255.0)
- `/25` = 128 hosts
- `/26` = 64 hosts

---

## 2. TCP Port Discovery

**Discover hosts via top 20 common ports (masscan, <5min on /24):**

```bash
masscan -p20,21-23,25,53,80,110,111,135,139,143,443,445,993,995,1723,3306,3389,5900,8080 199.66.11.0/24
```

**Focus on HTTP/HTTPS services:**

```bash
masscan -p80,443,8000-8100,8443 199.66.11.0/24
```

**Nmap alternative (slower but detailed):**

```bash
nmap -p- --min-rate=1000 -T4 -vvv 199.66.11.0/24
```

---

## 3. UDP Port Discovery

**Quick UDP scan (top 1000 ports, nmap):**

```bash
nmap -sU -sV --version-intensity 0 -F -n 199.66.11.0/24
```

**Common UDP services to target:**
- 53 (DNS)
- 67/68 (DHCP)
- 123 (NTP)
- 161 (SNMP)
- 162 (SNMP trap)
- 389 (LDAP)
- 514 (Syslog)

---

## 4. SCTP Port Discovery

**SCTP rarely filtered, often overlooked (telephony/signaling):**

```bash
nmap -T4 -sY -n --open -Pn 199.66.11.0/24
```

---

## 5. Passive Host Discovery (Stealth)

**ARP cache monitoring (no traffic sent):**

```bash
netdiscover -p
```

**P0f passive OS fingerprinting:**

```bash
p0f -i eth0 -p -o /tmp/p0f.log
```

**Bettercap network reconnaissance:**

```bash
net.recon on
net.show
set net.show.meta true
```

---

## 6. Active Host Discovery

**Full TCP port enumeration on single host:**

```bash
ports=$(nmap -p- --min-rate=1000 -T4 -vvv 192.168.1.50 | grep "^[0-9]" | cut -d '/' -f 1 | tr '\n' ',' | sed 's/,$//')
nmap -p$ports -sV -sC -vvv 192.168.1.50 | tee nmap.log
```

**Aggressive scan on network range:**

```bash
nmap -p- --min-rate=1000 -T4 -A 192.168.1.0/24
```

**Quick SYN scan (requires root):**

```bash
sudo nmap -sS -p80,443,22,3306,5432 192.168.1.0/24
```

---

## 7. IPv6 Discovery

**Ping IPv6 multicast (find dual-stack systems):**

```bash
alive6 eth0
```

**IPv6 network scan (if IPv6 enabled):**

```bash
nmap -6 fe80::/10 -p80,443,22
```

---

## 8. Network Sniffing & Packet Capture

**TCPdump DNS queries (discover hosts/domains in use):**

```bash
sudo tcpdump -i eth0 udp port 53
```

**Capture ICMP packets:**

```bash
tcpdump -i eth0 icmp
```

**Continuous HTTP/HTTPS capture with rotation (300sec, max 50 files):**

```bash
sudo bash -c "sudo nohup tcpdump -i eth0 -G 300 -w '/tmp/dump-%m-%d-%H-%M-%S-%s.pcap' -W 50 'tcp and (port 80 or port 443)' &"
```

**Wireshark live from remote host over SSH:**

```bash
ssh user@TARGET tcpdump -i ens160 -U -s0 -w - | sudo wireshark -k -i -
ssh user@TARGET tcpdump -i eth0 -U -s0 -w - 'port not 22' | sudo wireshark -k -i -
```

**Bettercap sniffing with filter:**

```bash
net.sniff on
net.sniff stats
set net.sniff.output sniffed.pcap
set net.sniff.filter "not arp"
set net.sniff.regexp "password|credential|token"
```

---

## Quick Workflows

**Fast internal reconnaissance (assumes foothold):**

```bash
# 1. Quick ICMP sweep
icmpsweep 192.168.1.0/24 | tee alive-hosts.txt

# 2. Masscan top ports on alive hosts
while read host; do masscan -p80,443,22,3306,5432 $host; done < alive-hosts.txt | tee services.txt

# 3. Full nmap on high-value targets
nmap -p- -sV -A 192.168.1.10 | tee target.nmap

# 4. Start passive sniffing in background
startsniff eth0
```

**Stealth-focused internal discovery:**

```bash
# 1. Passive ARP discovery (no probes sent)
arpdiscover | tee internal-arp.txt

# 2. P0f fingerprinting in background
p0f -i eth0 -p -o fingerprints.log &

# 3. Bettercap passive recon
net.recon on
net.show

# 4. TCPdump DNS snooping
sudo tcpdump -i eth0 udp port 53 -w dns-traffic.pcap
```

**Complete network mapping:**

```bash
# 1. ICMP sweep
nmap -PE -PM -PP -sn -n 192.168.1.0/24 | grep "Host is up" | awk '{print $2}' | tee hosts.txt

# 2. TCP ports on all hosts
nmap -p- --min-rate=1000 -iL hosts.txt | tee tcp-scan.log

# 3. UDP on discovered services
while read host; do nmap -sU -p53,67,123,161 $host; done < hosts.txt | tee udp-scan.log

# 4. Service fingerprinting
nmap -p80,443,22,3306,5432,139,445 -sV -sC -iL hosts.txt | tee services.nmap

# 5. Start traffic capture
startsniff eth0
```

---

