# Demonstration of Incident Detection & Response Capabilities

**Task:** Week 7 - Task 3 (SIEM Tool Implementation)  

---

## Scenario 1: Simulated SSH Brute-Force Attack & Containment
1. **Attack Simulation:**
   * From an external test machine (`192.168.56.99`), executed an automated SSH login brute-force test against the monitored server (`192.168.56.20`):
     ```bash
     hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://192.168.56.20 -t 4
     ```
2. **SIEM Detection:**
   * Within 5 seconds, Filebeat shipped the `/var/log/auth.log` failure entries to Logstash.
   * Logstash extracted `attacker_ip: 192.168.56.99` and tagged the events with `ssh_failed_login` (`MITRE T1110`).
   * Kibana's threshold rule fired a **High Severity Brute-Force Alert** after 5 failed attempts in 60 seconds.
3. **Incident Response Execution:**
   * **Containment:** Blocked the attacking IP immediately on the target host firewall:
     ```bash
     sudo iptables -A INPUT -s 192.168.56.99 -j DROP
     ```
   * **Hardening:** Disabled root SSH login (`PermitRootLogin no` in `/etc/ssh/sshd_config`) and enforced SSH key-based authentication.

---

## Scenario 2: Web Application SQLi / Directory Traversal Detection
1. **Attack Simulation:**
   * Sent crafted HTTP requests containing directory traversal (`../../../../etc/passwd`) and `UNION SELECT` payloads to the monitored Nginx web server.
2. **SIEM Detection:**
   * Logstash matched the malicious URI pattern, tagged the event as `web_attack_detected` (`MITRE T1190`, Severity: **Critical**), and highlighted the source IP on the Kibana **Top Web Attackers** panel.
3. **Incident Response Execution:**
   * **Containment:** Added the offending IP to the Nginx `deny` blocklist and reloaded the service (`sudo systemctl reload nginx`).
   * **Eradication & Recovery:** Verified via web access logs that no `HTTP 200` data exfiltration occurred and enabled ModSecurity WAF rules to block future injection payloads automatically.
