# ELK Stack + Windows Filebeat + Sysmon Logging Setup

This document outlines the complete setup of an ELK stack (Elasticsearch, Logstash, Kibana) on Ubuntu, along with Windows Filebeat + Sysmon log shipping. Based on the LinuxTechi guide, enhanced with Windows logging and Sysmon support.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [1. Install ELK on Ubuntu](#1-install-elk-on-ubuntu)
  - [1.1 Install Java](#11-install-java)
  - [1.2 Install Elasticsearch](#12-install-elasticsearch)
  - [1.3 Install Logstash](#13-install-logstash)
  - [1.4 Install Kibana](#14-install-kibana)
  - [1.5 Start and enable services](#15-start-and-enable-services)
- [2. Optional: Install Filebeat on Ubuntu](#2-optional-install-filebeat-on-ubuntu)
- [3. Install Filebeat on Windows + Sysmon](#3-install-filebeat-on-windows--sysmon)
  - [3.1 Install Filebeat](#31-install-filebeat)
  - [3.2 Configure filebeat.yml](#32-configure-filebeatyml)
  - [3.3 Install Filebeat as a Windows service](#33-install-filebeat-as-a-windows-service)
- [4. Verify Logs in Kibana](#4-verify-logs-in-kibana)
- [5. Check Disk Usage](#5-check-disk-usage)
- [6. Optional Tweaks](#6-optional-tweaks)
- [References](#references)

---

## Prerequisites

- Ubuntu 22.04 server (ELK)
- Windows client for log forwarding
- Sysmon installed on Windows (optional but recommended)
- Network access from Windows → ELK (port 5044 open)

---

## 1. Install ELK on Ubuntu

These steps follow and adapt the LinuxTechi installation guide.

---

### 1.1 Install Java

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
```

### 1.2 Install Elasticsearch

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt install apt-transport-https ca-certificates -y
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update
sudo apt install elasticsearch -y
```

Edit Elasticsearch config:

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

```yaml
network.host: 0.0.0.0
```

```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```


