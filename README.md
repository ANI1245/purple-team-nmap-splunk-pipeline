# Purple Team Automation: Nmap Attack Surface & Splunk Cloud SIEM Pipeline

## Executive Summary
This project demonstrates a Purple Team workflow combining offensive reconnaissance with SIEM log ingestion and analysis. A network scan was run using **Nmap** on Kali Linux, exported as structured XML, and ingested into **Splunk Cloud** to extract exposed services and potential attack vectors using custom SPL field extraction.

## Technical Architecture
* **Offensive Environment:** Kali Linux (Host / Container)
* **Target Domain:** `scanme.nmap.org` (Authorized Testing Target)
* **Data Transfer:** Python's built-in HTTP server (`http.server`)
* **SIEM Platform:** Splunk Cloud
* **Query Language:** Splunk Processing Language (SPL) & Custom Regex Field Extractions

---

# Workflow & Implementation

## 1. Attack Execution & XML Structured Output
Conducted target service version scanning using Nmap with raw XML output generation for structured SIEM parsing:

```bash
nmap -sV -Pn scanme.nmap.org -oX nmap_scan_results.xml
```

## 2. Splunk Ingestion Result

![Splunk field extraction output](Screenshot%202026-09-14%20133422.png)
