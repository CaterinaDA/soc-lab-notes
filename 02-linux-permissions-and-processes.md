# 02 — Linux Permissions and Processes

## Permissions

### Concepts
- Every file has three levels: owner, group, others
- Three types: r (read=4), w (write=2), x (execute=1)
- Numeric notation: rwxr-xr-x = 755, rw------- = 600

### Common permission values
- 600: owner read/write only (sensitive files, SSH keys)
- 644: owner write, everyone reads (normal files)
- 755: owner write, everyone reads/executes (folders, scripts)
- 777: everyone full access (avoid in production)

### Commands
- ls -l          → show file permissions
- ls -ld         → show directory permissions
- chmod 600 file → change permissions
- chown user:group file → change owner

### Lab exercise
Created file segreto.txt with chmod 600.
Verified testuser could not read it: Permission denied.
Key insight: Linux checks permissions on the entire folder chain,
not just the file itself.

## Processes

### Concepts
- Every running program is a process with a unique PID
- Zombie process (Z): finished but not removed from process table
- Background process: started with &, runs without blocking terminal

### Commands
- ps aux              → snapshot of all running processes
- ps aux | grep name  → filter specific process
- top                 → real-time process monitor
- kill PID            → terminate process (SIGTERM)
- kill -9 PID         → force terminate (SIGKILL)

### Lab exercise
Simulated a background process with: sleep 3600 &
Located it with: ps aux | grep sleep
Terminated it with: kill PID
Verified termination with: ps aux | grep sleep

### SOC red flags in process analysis
- Unknown process running as root
- High CPU/RAM usage from unrecognized process
- Process with suspicious or hidden name
- Zombie processes on a production server
