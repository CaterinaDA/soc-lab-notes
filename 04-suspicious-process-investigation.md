# 04 — Suspicious Process Investigation

## Scenario
Identified and investigated a suspicious process running from /tmp
simulating malware behavior on a Linux server.

## Red flags that trigger investigation
- Process running from /tmp or /dev/shm
- Process name starting with . (hidden)
- Process running in infinite loop
- Unknown process running as root
- High CPU/RAM usage from unrecognized process

## Investigation workflow

### Step 1 — find suspicious process
ps aux | grep .update
Result: /tmp/.update running in infinite loop, PID 3359

### Step 2 — inspect the file
ls -la /tmp/.update
Result: created May 26 09:44, executable by all users

### Step 3 — check logs for traces
sudo grep ".update" /var/log/auth.log
Result: no sudo activity — process ran without privileges
Note: auth.log only records sudo commands and authentication events

### Step 4 — terminate the process
kill -9 3359
Result: process killed immediately (SIGKILL)

### Step 5 — remove the file
rm /tmp/.update

### Step 6 — verify cleanup
ps aux | grep .update  → only grep itself visible
ls /tmp/               → only legitimate systemd temp folders

## Key insight
auth.log does not record normal commands without sudo.
For complete process auditing, auditd is required.
Attackers that avoid sudo are harder to trace via auth.log alone.

## Commands used
- ps aux | grep name   → find process by name
- ls -la /path/file    → inspect file metadata
- kill -9 PID          → force terminate process
- rm file              → remove file
- ls /tmp/             → verify temp folder is clean
