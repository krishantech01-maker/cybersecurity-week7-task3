# SIEM (Security Information and Event Management) Tool Implementation (Week 7 - Task 3)

## Overview
This repository contains the deliverables for **Week 7 - Task 3** of the Cyber Security Internship. The project demonstrates the end-to-end deployment of an open-source **ELK Stack SIEM (Elasticsearch, Logstash, Kibana + Filebeat)** integrated with automated incident detection rules and active response workflows for Linux servers and web applications.

## Repository Structure
* `docs/SIEM_Implementation_Report.md` - Step-by-step installation, log ingestion pipeline setup, correlation logic, and Kibana dashboard architecture.
* `docs/Incident_Detection_and_Response_Demo.md` - Practical demonstration of simulated attacks (SSH Brute-Force & Web Directory Traversal), real-time SIEM alert detection, and incident response execution.
* `configs/logstash-siem-pipeline.conf` - Logstash parsing, Grok normalization, and security correlation filter configuration.
* `configs/filebeat.yml` - Endpoint log shipper configuration collecting system authentication and web server logs.

## Tech Stack Used
* **SIEM Core:** Elasticsearch, Logstash, Kibana (ELK Stack v8.x)
* **Log Shippers:** Filebeat, Syslog (UDP/514)
* **Response & Automation:** Linux `iptables` / `ufw` Active Response, Bash
