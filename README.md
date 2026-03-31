# Cowrie SSH Honeypot + SIEM Integration

SSH honeypot deployment and SIEM integration on Proxmox VE. Cowrie honeypot with Splunk log analysis, Hydra brute force simulation, and network hardening with UFW and fail2ban. 

Video = https://youtu.be/uSohtNwQXuI?si=PXeezLlMXO5dqjA1

## Architecture

- **Proxmox Host (pve2):** 192.168.1.217
- **Honeypot VM (VM 100):** Ubuntu 22.04, 192.168.1.150
- **Attacker VM (Kali):** 192.168.1.54
- **Splunk Server:** 192.168.1.111

## Installation

### 1. Create the VM in Proxmox

Creating a new Ubuntu 22.04 VM (ID 100) on pve2 using a cloud image. 2 cores, 2GB RAM, 32GB disk with cloud-init for automated provisioning.

![Create VM](screenshots/installation/01-create-vm.png)

### 2. Ubuntu cloud-init provisioning

Ubuntu 22.04 installing automatically via cloud-init (curtin). The cloud image handles OS setup without a manual installer.

![Ubuntu Install](screenshots/installation/02-ubuntu-install.png)

### 3. Fresh shell on the honeypot VM

Logged in as `cowrie@cowrie-honeypot` on VM 100. Fresh Ubuntu 22.04 system ready for Cowrie installation.

![Fresh Shell](screenshots/installation/03-fresh-shell.png)

### 4. Clone the Cowrie repository

Creating the cowrie directory, cloning the official Cowrie repo from GitHub.

```bash
mkdir cowrie && cd cowrie
git clone https://github.com/cowrie/cowrie
```

![Clone Cowrie](screenshots/installation/04-clone-cowrie.png)

### 5. Set up the Python virtual environment

Creating and activating a Python virtual environment for Cowrie's dependencies.

```bash
python3 -m venv cowrie-env
source cowrie-env/bin/activate
```

![Python Venv](screenshots/installation/05-python-venv.png)

### 6. Configure iptables port redirect

Setting up an iptables NAT PREROUTING rule to redirect incoming SSH traffic on port 22 to Cowrie's listening port 2222. This makes the honeypot transparent to attackers.

```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
```

![iptables Redirect](screenshots/installation/06-iptables-redirect.png)

### 7. Verify iptables rules

Confirming the NAT PREROUTING rules are active with `sudo iptables-save`. The redirect from port 22 to 2222 is in place.

![iptables Save](screenshots/installation/07-iptables-save.png)

### 8. Start Cowrie

Starting the Cowrie honeypot with `cowrie start`. TripleDES deprecation warnings are expected and non-breaking.

```bash
cowrie start
```

![Cowrie Start](screenshots/installation/08-cowrie-start.png)

### 9. Cowrie listening for SSH connections

Cowrie is running and listening on port 2222. The log confirms `CowrieSSHFactory starting on 2222` and `Ready to accept SSH connections`.

![Cowrie Listening](screenshots/installation/09-cowrie-listening.png)

---

**(in progress)** — Brute force attack simulation, Splunk integration, and hardening documentation coming next.
