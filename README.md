# Cowrie SSH Honeypot + SIEM Integration

SSH honeypot deployment and SIEM integration on Proxmox VE. Cowrie honeypot with Splunk log analysis, Hydra brute force simulation, and network hardening with UFW and fail2ban.

Video = https://youtu.be/uSohtNwQXuI?si=PXeezLlMXO5dqjA1

## Architecture

- **Proxmox Host (pve2):** 192.168.1.217
- **Honeypot VM (VM 100):** Ubuntu 22.04, 192.168.1.150
- **Attacker VM (Kali):** 192.168.1.54
- **Splunk Server:** 192.168.1.111

## Documentation

- [Installation Steps](Installation-Steps.md) — VM creation, Cowrie setup, iptables redirect
- [Brute Force Attack](Brute-Force-Attack.md) — Hydra brute force attack against the honeypot

**(in progress)** — Splunk integration and hardening documentation coming next.
