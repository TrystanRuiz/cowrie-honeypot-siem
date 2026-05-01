# Cowrie SSH Honeypot + SIEM Integration

SSH honeypot deployment and SIEM integration on Proxmox VE. Cowrie honeypot with Splunk log analysis, Hydra brute force simulation, and network hardening with UFW and Fail2Ban.

## Architecture

- **Proxmox Host:** Proxmox VE 9.1
- **Honeypot VM:** Ubuntu 22.04
- **Attacker VM:** Kali Linux
- **SIEM:** Splunk Enterprise

## Documentation

### Cowrie
- [Brute Force Attack — Cowrie](Cowrie/Brute-Force-Attack-Cowrie.md) — Hydra brute force attack against the honeypot
- [Hardening — UFW and Fail2Ban](Cowrie/Hardening-Cowrie-UFW-Fail2Ban.md) — UFW firewall rules, IP/subnet blocking, Fail2Ban setup

### Installation Steps
- [Cowrie Installation](Installation-Steps/Cowrie-Installation.md) — VM creation, Cowrie setup, iptables redirect
- [Splunk Installation](Installation-Steps/Splunk-Installation.md) — Splunk Enterprise setup for Cowrie log ingestion
- [UFW Installation](Installation-Steps/UFW-Installation.md) — UFW firewall rules and subnet blocking
- [Fail2Ban Installation](Installation-Steps/Fail2Ban-Installation.md) — Fail2Ban setup for brute-force rate limiting
