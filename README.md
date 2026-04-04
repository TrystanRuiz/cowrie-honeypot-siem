# Cowrie SSH Honeypot + SIEM Integration

SSH honeypot deployment and SIEM integration on Proxmox VE. Cowrie honeypot with Splunk log analysis, Hydra brute force simulation, and network hardening with UFW and fail2ban.

Video = https://youtu.be/uSohtNwQXuI?si=PXeezLlMXO5dqjA1

## Architecture

- **Proxmox Host (pve2):** 192.168.1.217
- **Honeypot VM (VM 100):** Ubuntu 22.04, 192.168.1.150
- **Attacker VM (Kali):** 192.168.1.54
- **Splunk Server:** 192.168.1.111

## Documentation

### Attack Simulation & Hardening
- [Brute Force Attack — Cowrie](Brute-Force-Attack-Cowrie.md) — Hydra brute force attack against the honeypot
- [Hardening — UFW & Fail2Ban](Hardening-Cowrie-UFW-Fail2Ban.md) — UFW firewall rules, IP/subnet blocking, fail2ban setup

### T-Pot
- [Brute Force Attack — T-Pot](Brute-Force-Attack-TPot.md) — *(in progress)*
- [Hardening T-Pot — UFW & Fail2Ban](Hardening-TPot-UFW-Fail2Ban.md) — *(in progress)*

### Installation Steps
- [Cowrie Installation](Installation-Steps/Cowrie-Installation.md) — VM creation, Cowrie setup, iptables redirect
