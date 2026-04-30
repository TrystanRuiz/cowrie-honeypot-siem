# Cowrie SSH Honeypot + SIEM Integration

SSH honeypot deployment and SIEM integration on Proxmox VE. Cowrie honeypot with Splunk log analysis, Hydra brute force simulation, and network hardening with UFW and fail2ban.

## Architecture

- **Proxmox Host:** Proxmox VE 9.1
- **Honeypot VM:** Ubuntu 22.04
- **Attacker VM:** Kali Linux
- **SIEM:** Splunk Enterprise

## Documentation

### Cowrie
- [Brute Force Attack — Cowrie](Cowrie/Brute-Force-Attack-Cowrie.md) — Hydra brute force attack against the honeypot
- [Hardening — UFW & Fail2Ban](Cowrie/Hardening-Cowrie-UFW-Fail2Ban.md) — UFW firewall rules, IP/subnet blocking, fail2ban setup

### Installation Steps
- [Cowrie Installation](Installation-Steps/Cowrie-Installation.md) — VM creation, Cowrie setup, iptables redirect
