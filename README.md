# Purple Team Automation: Nmap Attack Surface & Splunk Cloud SIEM Pipeline

## Executive Summary
This project demonstrates an end-to-end Purple Team workflow combining offensive reconnaissance with SIEM log ingestion and analysis. An automated network scan was executed using **Nmap** on Kali Linux, structured as XML data, and ingested into **Splunk Cloud** to extract threat surface intelligence, exposed services, and potential attack vectors.

## Technical Architecture
* **Offensive Environment:** Kali Linux (Host / Container)
* **Target Domain:** `scanme.nmap.org` (Authorized Testing Target)
* **Ingestion Middleware:** Custom Python HTTP Server Transfer Pipeline
* **SIEM Platform:** Splunk Cloud Engine
* **Query Language:** Splunk Processing Language (SPL) & Custom Regex Field Extractions

---

# Workflow & Implementation

## 1. Attack Execution & XML Structured Output
Conducted target service version scanning using Nmap with raw XML output generation for structured SIEM parsing:
```bash
nmap -sV -Pn scanme.nmap.org -oX nmap_scan_results.xml
