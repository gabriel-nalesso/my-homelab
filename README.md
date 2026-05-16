# my-homelab

## Project Overview
This repository documents the deployment and management of a persistent home server infrastructure. It serves as a hands-on laboratory for studying **Linux Administration (Debian 13)**, **Docker Orchestration**, and **Network Security**.

The primary goal is to optimize a low-power hardware setup for network-wide services and system monitoring while maintaining a secure remote management layer.

---

## Hardware Specs
The node is a repurposed laptop from 2013, selected for its high efficiency and low operating cost (~6W/R$ 3.89 per month in Itapetininga, SP).

* **CPU**: Intel® Celeron® N2807 (2 Cores, up to 2.17 GHz).
* **RAM**: 4GB DDR3L.
* **Storage**: 240GB SATA III SSD (Upgraded from 500GB HDD to eliminate I/O bottlenecks).
* **Operating System**: Debian GNU/Linux 13 (Trixie).

---

## Tech Stack & Services
* **Pi-hole**: Network-wide DNS sinkhole for ad and tracker blocking.
* **Netdata Cloud**: High-resolution monitoring and ML-based anomaly detection.
* **Tailscale**: Zero-config mesh VPN for secure remote SSH access.
* **Mysterium Network**: Decentralized VPN node deployment for passive resource monetization.
* **Docker & Docker Compose**: Container orchestration and environment isolation.

---

## Prerequisites
Before deploying the stack, ensure the host machine has the following installed:
* **Docker**: `v24.0+`
* **Docker Compose**: `v2.20+`
* **Git**: For version control and repository cloning.
* A configured static IP or a permanent MAC-to-IP binding on your local gateway.

## Getting Started
Follow these steps to deploy the infrastructure on your local node:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/my-homelab.git
   cd my-homelab
   ```

2. **Configure Environment Variables:**
   For security reasons, sensitive configurations are not committed to version control. Create a `.env` file in the root directory based on the provided template:
   ```bash
   cp .env.example .env
   ```
   *Edit the `.env` file with your specific credentials (e.g., Pi-hole Web Password, Tailscale Auth Key, Mysterium Node credentials).*

3. **Deploy the Stack:**
   Spin up all services in detached mode using Docker Compose:
   ```bash
   docker compose up -d
   ```

4. **Verify Deployment:**
   Check the status of your containers to ensure everything is running smoothly:
   ```bash
   docker ps
   ```

---

## Key Troubleshooting & Insights
This lab provides real-world experience in identifying and resolving system anomalies:

* **Disk I/O Saturation**: Analysis via Netdata revealed that the original LVM device (`dm-0`) reached 100% utilization during standard background processes, justifying the migration to SSD storage.
* **DNS Interface Configuration**: Fixed a "non-local network" query drop by reconfiguring Pi-hole's listening behavior to `Permit all origins` to accommodate Docker's bridge networking.
* **Anomaly Correlation**: Correlated system read spikes (>1,000KiB/s) with specific process restarts, such as `tc-qos-helper`, using Netdata's anomaly detection engine.
* **DHCP IP Persistence**: Fixed an issue where router reboots caused dynamic IP shifts (e.g., from `192.168.0.8` to `192.168.0.3`). While SSH remained accessible by scanning the network, this IP churn broke hardcoded infrastructure dependencies, including Pi-hole DNS forwarding and Tailscale subnet routes. Resolved by configuring a permanent MAC-to-IP binding on the Tp-Link EX141 gateway.

---

## Project Structure
* `docker-compose.yml`: Main orchestration file defining all services and networks.
* `docs/`: Contains network topology diagrams and system performance reports.
* `.env.example`: Template for required environment variables (passwords, API keys).
* `.gitignore`: Security layer to prevent accidental leaks of the `.env` file and other local data.

---

## Future Roadmap
As the homelab evolves, the following upgrades and deployments are planned:
* **Reverse Proxy Integration**: Implement Nginx Proxy Manager or Traefik to handle SSL termination and internal routing.
* **Automated Backups**: Set up a cron job or a dedicated container (like Duplicati) to back up persistent Docker volumes off-site.
* **Infrastructure as Code (IaC)**: Transition host provisioning to Ansible for fully automated bare-metal configuration.