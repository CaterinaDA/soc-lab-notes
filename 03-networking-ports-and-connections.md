# 03 — Networking: Ports and Connections

## Key ports to know
- 22   SSH
- 80   HTTP
- 443  HTTPS
- 21   FTP
- 25   SMTP
- 53   DNS
- 3306 MySQL
- 3389 RDP (Windows Remote Desktop)

## Commands
- ss -tlnp   → listening ports (TCP)
- ss -tnp    → active established connections

## Local Address meaning
- 127.0.0.1 → local only, not reachable from outside
- 0.0.0.0   → open to any IP (exposed to network)
- [::]      → same as 0.0.0.0 but IPv6

## Lab exercise — unexpected open port

### Step 1 — baseline scan
ss -tlnp
Result: only port 22 (SSH) and 53 (DNS local)

### Step 2 — installed nginx
sudo apt install nginx -y
sudo systemctl status nginx

### Step 3 — rescan
ss -tlnp
Result: port 80 now open on 0.0.0.0 (unexpected)

### Step 4 — investigate who opened it
sudo grep "nginx" /var/log/auth.log
Result: caterina installed nginx via apt at 08:39:15

### Step 5 — check active connections
ss -tnp
Result: one ESTAB connection on port 22 (SSH session)

## SOC red flags in network analysis
- Unknown service listening on 0.0.0.0
- Unexpected port open on production server
- ESTAB connection to unknown external IP
- Connection on unusual ports (4444, 8080, 1337)
- Process with open connection not matching expected software
