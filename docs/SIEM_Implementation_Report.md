# SIEM Implementation & Architecture Report

**Date:** 25-11-2026  
**Engineer:** Krishan Kumar  
**Task:** Week 7 - Task 3 (SIEM Tool Implementation)  
**SIEM Stack:** ELK Stack (Elasticsearch, Logstash, Kibana) + Filebeat  

---

## 1. Executive Summary
A centralized **Security Information and Event Management (SIEM)** solution was implemented using the open-source **ELK Stack** on an Ubuntu Linux server (`192.168.56.10`). The system ingests real-time authentication, web server, and firewall logs, normalizes raw log data using Logstash Grok filters, correlates suspicious patterns into high-severity security alerts, and visualizes threat telemetry through custom Kibana SOC dashboards.

---

## 2. Architecture & Data Flow
1. **Log Collection (Endpoints & Network):**
   * Installed **Filebeat** on monitored Linux and web servers (`192.168.56.20`) to stream `/var/log/auth.log` and `/var/log/nginx/access.log` over TCP port `5044`.
   * Configured perimeter firewall devices to forward Syslog messages over UDP port `514`.
2. **Log Parsing & Correlation (Logstash):**
   * Configured `logstash-siem-pipeline.conf` to extract key fields (`attacker_ip`, `target_user`, HTTP status codes) and tag malicious patterns (`ssh_failed_login`, `web_attack_detected`) mapped to MITRE ATT&CK IDs.
3. **Indexing & Storage (Elasticsearch):**
   * Stored structured JSON security events in daily time-series indices (`siem-security-events-YYYY.MM.DD`) with 30-day retention policies.
4. **Visualization & Alerting (Kibana):**
   * Built a **SOC Security Operations Dashboard** displaying alert trends, geographic source IP maps, top targeted user accounts, and threshold-based Kibana Alert rules (triggering when `ssh_failed_login >= 5` within 2 minutes).
