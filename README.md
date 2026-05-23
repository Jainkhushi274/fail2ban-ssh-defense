# fail2ban-ssh-defense
#  SSH Intrusion Detection & Prevention with Fail2ban

![Built with Kali Linux](https://img.shields.io/badge/Built%20with-Kali%20Linux-blue)
![Secured with Fail2ban](https://img.shields.io/badge/Secured%20with-Fail2ban-green)
![Cybersecurity Project](https://img.shields.io/badge/Category-Cybersecurity-orange)

##  Project Overview
This project demonstrates how to secure SSH access on **Kali Linux** using **Fail2ban**.  
It detects brute‑force login attempts and automatically bans malicious IPs.

##  Features
- Real‑time monitoring of SSH logs via `systemd-journald`
- Fail2ban jail configuration for SSH protection
- Tuned parameters (`maxretry`, `bantime`, `findtime`) for testing and production
- Verified bans/unbans with proof screenshots
- Optional reporting with `awk` and cron

##  Repository Contents
- `jail.local` → Fail2ban jail configuration
- `screenshots/` → Proof of failed attempts, bans, and status checks

##  How to Run
1. Copy `jail.local` to `/etc/fail2ban/`
2. Restart Fail2ban:
   ```bash
   sudo systemctl restart fail2ban
3. Test with wrong SSH logins.
4. Check status:
   ```
   sudo fail2ban-client status sshd

   
## jail.local configuration
- File content:
  ```
  [sshd]
  enabled = true          # Turn on protection for SSH
  port = ssh              # Service port (default SSH)
  filter = sshd           # Use the default sshd filter
  backend = systemd       # Use systemd-journald for logs
  maxretry = 3            # Number of failed attempts before ban
  bantime = 600           # Ban duration in seconds (10 minutes)
  findtime = 300          # Time window in seconds (5 minutes)
  ignoreself = true       # Don’t ban your own IP
  
- Explanation of Each Setting :
  - [sshd] → Jail section for SSH service.
  - enabled = true → Activates this jail.
  - port = ssh → Protects the SSH port (default 22).
  - filter = sshd → Uses Fail2ban’s built‑in SSH filter (no need for custom regex unless you want).
  - backend = systemd → Reads logs from systemd-journald (important for Kali).
  - maxretry = 3 → After 3 failed login attempts, the IP is banned.
  - bantime = 600 → IP stays banned for 10 minutes.
  - findtime = 300 → Fail2ban looks at failed attempts within a 5‑minute window.
  - ignoreself = true → Prevents banning your own machine.
 
    

##  Screenshots
- Failed SSH Attempts:
  <img width="1920" height="945" alt="failedloginattempts" src="https://github.com/user-attachments/assets/d83ed32b-fb03-484c-af82-43dbd6ac7127" />
- Fail2ban Status (Active Ban):
  <img width="1920" height="945" alt="fail2banstatus" src="https://github.com/user-attachments/assets/9a70d94a-aeac-4286-8482-cce2ecc7c850" />
- Log Output:
  <img width="1920" height="945" alt="fail2ban log output" src="https://github.com/user-attachments/assets/67cbbb5e-7401-4323-865f-9ae72734e129" />
- Fail2ban Status (Expired Ban/Unban):
  <img width="1920" height="945" alt="bannediplistexpires" src="https://github.com/user-attachments/assets/ab4c1907-5ff4-47e0-b127-c25c9a15c1b3" />
- awk Reporting:
  <img width="1920" height="945" alt="awk" src="https://github.com/user-attachments/assets/74dc7520-6420-40ea-bb7a-045a4d360176" />
  
## Reports
By default, Fail2ban logs bans/unbans into /var/log/fail2ban.log.
I tested reporting manually using awk (see screenshot above).

If you want daily automated reports, you must set up a cron job to run a script.
- Example one‑liner:
  ```
  journalctl -u ssh --since "24 hours ago" | grep "Failed password" | awk '{print $13}' | sort | uniq -c
- To automate: 
  ```
  0 0 * * * /path/to/ssh_report.sh
This will generate a daily report file (e.g., /var/log/ssh_report.txt) with failed login counts.

## Skills Demonstrated
- Linux Security
- Fail2ban
- Intrusion Detection
- Cybersecurity
- System Hardening

## Outcome
- Reduced risk of unauthorized SSH access
- Demonstrated ability to implement intrusion prevention systems (IPS)
- Strengthened portfolio in defensive cybersecurity and system hardening
