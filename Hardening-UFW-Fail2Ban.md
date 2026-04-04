# Hardening with UFW & Fail2Ban

## UFW

Enabled UFW on the honeypot VM with a default deny on all incoming traffic. Only ports 22 (real SSH for management) and 2222 (Cowrie) are open.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 2222/tcp
sudo ufw enable
```

Everything else gets dropped. If someone tries hitting any other port on this box, it's just gone.

![UFW status](screenshots/hardening/05-ufw-status.png)

## The Problem

Before fail2ban, Hydra could brute force the honeypot and find passwords without any consequences. It would just keep running through the wordlist until it found matches.

## Fail2Ban Setup

Installed fail2ban on the honeypot VM and created a custom jail for Cowrie. The filter watches Cowrie's log file and matches any login attempt (failed or succeeded) from an IP. If an IP hits 3 login attempts within 60 seconds, it gets banned for 1 hour.

The jail config (`/etc/fail2ban/jail.local`):

```
[cowrie]
enabled = true
filter = cowrie
logpath = /home/cowrie/cowrie/cowrie/var/log/cowrie/cowrie.log
maxretry = 3
findtime = 60
bantime = 3600
port = 2222
```

The filter regex (`/etc/fail2ban/filter.d/cowrie.conf`):

```
[Definition]
failregex = .*\[HoneyPotSSHTransport,\S+,<HOST>\] login attempt .* (failed|succeeded)
```

## Testing It

Ran Hydra again from Kali. It still found 3 passwords but fail2ban caught the login attempts and banned the IP right after.

![Hydra attack after fail2ban](screenshots/hardening/01-hydra-attack.png)

Checking the cowrie jail on the honeypot shows 192.168.1.194 is now banned. 4 total failed attempts detected, 1 IP currently banned.

![fail2ban showing the ban](screenshots/hardening/02-fail2ban-banned.png)

## Confirming the Ban

Tried to SSH back into the honeypot from Kali and got `Connection refused`. The IP is completely blocked on port 2222.

![Connection refused from Kali](screenshots/hardening/03-connection-refused.png)

## Fail2Ban Logs in Splunk

The fail2ban log is also forwarded to Splunk through the Universal Forwarder. You can search `sourcetype="fail2ban"` and see the ban event with the IP and timestamp.

![Splunk showing fail2ban ban event](screenshots/hardening/04-splunk-fail2ban.png)
