# 🕳️ The Hole - Global SSH Threat Monitor

![Security](https://img.shields.io/badge/Security-Honeypot-red)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![AWS](https://img.shields.io/badge/AWS-EC2-orange)
![Python](https://img.shields.io/badge/Python-Data_Parsing-yellow)
![Bash](https://img.shields.io/badge/Bash-Automation-lightgrey)

An automated, containerized SSH honeypot deployed on AWS EC2, featuring a real-time SOC-style global threat monitoring dashboard.

## 🎯 Project Objective

This project was developed as a hands-on cybersecurity and DevSecOps portfolio piece. The goal was to design and deploy a secure, isolated environment capable of attracting, trapping, and analyzing real-world automated botnet attacks and brute-force attempts in real-time. It demonstrates practical application of **Network Security (iptables)**, **Containerization (Docker)**, **Cloud Infrastructure (AWS)**, and **Data Pipeline Automation**.

---

## 🌍 Live Dashboard

**[View the Live Global Threat Monitor Here](https://MorBarak23.github.io/TheHole-Honeypot/)**

_(Note: Data is updated dynamically based on real-time attacks hitting the AWS server)._

---

## 📸 Dashboard & Analytics

\*💡 **Screenshot Recommendation 1: The Global Map\***  
`[Insert a screenshot here showing the dark-themed CartoDB map filled with attack markers]`

\*💡 **Screenshot Recommendation 2: Top Passwords & Recent Attacks\***  
`[Insert a screenshot here focusing on the left/right sidebars showing the Top 10 Passwords and the live stream of recent IPs/Usernames]`

---

## 🛠️ Architecture & How It Works

The system operates as a continuous, automated pipeline:

1. **The Trap (AWS & iptables):**
   Attackers actively scan the internet for open SSH ports. When they hit the AWS EC2 instance on default port `22`, Linux `iptables` NAT rules seamlessly redirect the malicious traffic to port `2222`.
2. **The Isolation (Docker & Cowrie):**
   Port `2222` is bound to a lightweight **Cowrie SSH Honeypot** running securely inside an isolated Docker container. Attackers believe they are interacting with a real Linux server.
3. **Data Logging:**
   Cowrie records every interaction (IP address, guessed usernames, passwords, timestamps) into a local JSON log volume.
4. **Data Parsing & Geolocation (Python):**
   A custom Python script extracts the live container logs, filters out internal Docker SNAT noise (`172.17.0.x`), and queries the `ip-api.com` service to map attacker IP addresses to specific countries, cities, and ISPs.
5. **CI/CD Automation (Bash & Cron):**
   A scheduled Cron job runs a Bash script that merges historical logs, safely triggers the Python parser (with built-in API rate-limit protections), and pushes the updated dataset to GitHub.
6. **Frontend Visualization:**
   The web dashboard consumes the latest CSV data and renders it onto an interactive Leaflet.js map, applying a custom dark theme.

\*💡 **Screenshot Recommendation 3: Terminal Output\***  
`[Insert a screenshot here showing the terminal running the Python script, successfully extracting IPs and resolving locations (e.g., Exported to CSV: 218.13... (China))]`

---

## ✨ Key Features

- **Zero-Interference Logging:** Captures credentials and behavior without exposing the host OS to actual compromise.
- **Rate-Limit Safe:** The processing engine dynamically paces API requests to avoid geolocation service blocks.
- **Automated Data Sync:** The entire dataset is merged and published without human intervention.
- **Interactive Threat Map:** Visualizes attack origins globally using raw coordinates and CartoDB dark tiles.

---

## 🔍 Key Findings & Security Insights

By analyzing the gathered data, several cybersecurity trends become immediately clear:

1. **Automated Botnets:** The vast majority of attacks occur in rapid bursts (e.g., 30+ requests from a single Chinese IP within seconds), indicating scripted brute-force dictionary attacks rather than human hackers.
2. **Default Credential Stuffing:** The most frequently attempted usernames (`root`, `admin`, `support`) and passwords (`123456`, `admin`) highlight the critical danger of leaving default credentials on IoT devices and servers.
3. **Global Distribution:** Attacks originate from a wide array of global nodes, emphasizing that threats are highly distributed and borderless.

---

## 💻 Technology Stack

- **Infrastructure:** AWS EC2 (Ubuntu Linux)
- **Security & Networking:** `iptables` (Port Forwarding/NAT), SSH Configuration
- **Containerization:** Docker, Docker Volumes
- **Honeypot Engine:** Cowrie (Medium interaction SSH/Telnet honeypot)
- **Backend Automation:** Python 3 (JSON parsing, REST API requests), Bash scripting, Linux Cron
- **Frontend:** HTML5, CSS3, Vanilla JavaScript, Leaflet.js, PapaParse (CSV parsing)

---

## 👨‍💻 About the Author

**Mor Barak**  
_Engineering Student at Afeka College of Engineering_  
Passionate about software development, network security, and building scalable cloud architectures. This project was built to bridge the gap between theoretical data structures and algorithms, and real-world infrastructure security.

- **GitHub:** [@MorBarak23](https://github.com/MorBarak23)
