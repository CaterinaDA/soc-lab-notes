# 01 — SSH Brute Force Attack Detection

## Scenario
Detected multiple failed SSH login attempts against a Linux server.
Goal: identify the attack, count attempts, and locate the source IP.

## Environment
- OS: Ubuntu Server 26.04 LTS
- Log file: /var/log/auth.log
- Tool: grep, awk, sort, uniq, wc

## Analysis

### Step 1 — identify failed login attempts
sudo grep -i "failed" /var/log/auth.log | grep -v "vboxadd"

### Step 2 — filter only SSH failures
sudo grep "Failed password" /var/log/auth.log

### Step 3 — count total attempts
sudo grep "Failed password" /var/log/auth.log | wc -l
Result: 8 failed attempts

### Step 4 — identify source IP
sudo grep "Failed password" /var/log/auth.log | awk '{print $9}' | sort | uniq -c | sort -rn
Result: 6 attempts from 10.0.2.2

## Findings
- Target user: caterina
- Source IP: 10.0.2.2
- Total failed attempts: 8
- Attack window: ~2 minutes
- Outcome: attack unsuccessful, no successful login detected

## Conclusion
Pattern consistent with SSH brute force attack.
In a real environment: block source IP via firewall, enable fail2ban.
