# Linux Process Management

Linux doesn’t just manage files — it manages **processes**, which are running programs. Every service (like Docker, Nginx, or Jenkins) is a process. Understanding and controlling them is critical for DevOps engineers.

---

## The Process Tree

Linux processes form a hierarchy, starting from **PID 1 (systemd/init)**:

systemd (PID 1)
├── sshd (PID 1024)
├── nginx (PID 1100)
│     ├── nginx worker (PID 1101)
│     └── nginx worker (PID 1102)
└── docker (PID 1200)
└── containerd (PID 1201)


Every process has:
- **PID** → Process ID (unique number)
- **PPID** → Parent Process ID
- **UID** → User ID (who owns it)

---

## Key Commands

| Command | Purpose |
|---------|---------|
| `ps aux` | Snapshot of all processes |
| `ps -ef` | Tree view with parent/child relationships |
| `top` | Real-time CPU/memory monitoring |
| `htop` | Interactive process viewer (if installed) |
| `kill <PID>` | Terminate a process |
| `systemctl status <service>` | Check service health |
| `systemctl start/stop/restart <service>` | Control service lifecycle |
| `systemctl enable/disable <service>` | Manage auto-start at boot |

---

## Understanding `ps aux` Output

Example:

USER   PID  %CPU %MEM   VSZ   RSS TTY   STAT START   TIME COMMAND


- **USER** → Owner of process  
- **PID** → Process ID  
- **%CPU / %MEM** → Resource usage  
- **VSZ / RSS** → Virtual vs actual RAM usage  
- **TTY** → Terminal controlling process (`?` = daemon)  
- **STAT** → Process state  

### STAT Codes
- **R** → Running  
- **S** → Sleeping  
- **D** → Waiting for I/O  
- **T** → Stopped  
- **Z** → Zombie  
- Flags: `s` (session leader), `+` (foreground), `I` (idle kernel thread), `<` (high priority)

---

## Understanding `top` Output

### Header

top - 00:15:23 up 1 day, 3:42, 2 users, load average: 0.12, 0.08, 0.05

- **Load average** → workload over 1, 5, 15 minutes  
- Rule: load ≈ CPU cores → healthy; load > cores → overloaded

### Tasks

- **Load average** → workload over 1, 5, 15 minutes  
- Rule: load ≈ CPU cores → healthy; load > cores → overloaded

### Tasks

Tasks: 152 total, 1 running, 151 sleeping, 0 stopped, 0 zombie


### CPU Section
%Cpu(s): 1.2 us, 0.5 sy, 0.0 ni, 98.0 id, 0.3 wa

- **us** → User processes  
- **sy** → Kernel processes  
- **id** → Idle CPU  
- **wa** → I/O wait (disk/network bottleneck)

### Memory Section


MiB Mem : 7980 total, 1200 free, 4500 used, 2280 buff/cache

- **buff/cache** → Cached files (reclaimable)  
- **swap** → Disk used when RAM is full (bad sign if high)

### Process Table


PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

- Spot runaway processes here (e.g., `java` using 90% CPU)

---

## Managing Services with `systemctl`

- **Check status** → `systemctl status ssh`  
- **Start/Stop/Restart** → `sudo systemctl restart nginx`  
- **Enable at boot** → `sudo systemctl enable docker`  
- **Disable at boot** → `sudo systemctl disable docker`  
- **Logs** → `journalctl -u <service>`

---

## Practice Exercises

1. Run `ps aux` and identify your shell process (`bash` or `zsh`).  
2. Start a dummy process: `sleep 100 &` → kill it with `kill <PID>`.  
3. Use `top` to find the highest CPU process.  
4. Restart a service with `systemctl restart ssh`.  

---

## Challenge Exercise

- Run `yes > /dev/null &` → creates CPU hog.  
- Use `top` to identify it.  
- Kill it with `kill -9 <PID>`.  

---

## Common Mistakes

- Using `kill -9` immediately (too aggressive).  
- Forgetting `sudo` with `systemctl`.  
- Enabling a service but not starting it.  
- Confusing `restart` vs `reload`.  

---

## Troubleshooting

- Service fails → `systemctl status <service>` + `journalctl -u <service>`  
- Process respawns → managed by systemd, stop via `systemctl`  
- Zombie processes → usually harmless, but indicate parent didn’t clean up  

---

## Interview Questions

- What’s the difference between a process and a thread?  
- What does PID 1 represent?  
- How do you troubleshoot a high CPU process?  
- Difference between `systemctl stop` and `disable`?  

---

## Real-World Relevance

- DevOps engineers constantly monitor processes (`top`, `ps`) to debug performance.  
- Service lifecycle management (`systemctl`) is critical in AWS EC2, Docker hosts, Kubernetes nodes.  
- Recruiters want candidates who can explain **not just commands, but why they matter in production**.  

---

## Git Commit Suggestion

```bash
git add notes/process-management.md
git commit -m "Added process management notes with ps, top, kill, systemctl"
git push origin main

Screenshots to Include
Output of ps aux

top dashboard

Killing a process and verifying it’s gone

Restarting a service with systemctl

Mini Project
Create scripts/process_monitor.sh:


#!/bin/bash
echo "Top 5 CPU-consuming processes:" > logs/process.log
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -6 >> logs/process.log


Summary
Processes are the “living programs” of Linux. Managing them with ps, top, and systemctl is essential for keeping servers stable, debugging issues, and proving your DevOps readiness.

Quiz
What does the STAT column in ps aux represent?

How do you interpret load average in top?

What’s the difference between systemctl stop and systemctl disable?

Why is swap usage a red flag in production?


