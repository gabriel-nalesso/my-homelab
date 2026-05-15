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

## Key Troubleshooting & Insights
This lab provides real-world experience in identifying and resolving system anomalies:

* **Disk I/O Saturation**: Analysis via Netdata revealed that the original LVM device (`dm-0`) reached 100% utilization during standard background processes, justifying the migration to SSD storage.
* **DNS Interface Configuration**: Fixed a "non-local network" query drop by reconfiguring Pi-hole's listening behavior to `Permit all origins` to accommodate Docker's bridge networking.
* **Anomaly Correlation**: Correlated system read spikes ($>1,000~KiB/s$) with specific process restarts, such as `tc-qos-helper`, using Netdata's anomaly detection engine.

---

## Project Structure
* `docker-compose.yml`: Main orchestration file for all services.
* `docs/`: Network topology diagrams and performance reports.
* `.gitignore`: Security layer to prevent credential leaks.
