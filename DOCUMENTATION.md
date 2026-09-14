# Technical Implementation & Pipeline Guide

## Data Transfer Workaround
To move scan output off the local VM without setting up extra tooling, Python's built-in HTTP server was used as a quick file-transfer method into the Splunk-accessible environment:

```bash
python3 -m http.server 8000
```

### Splunk Ingestion & Custom Parsing

### Sourcetype Configuration
Configured custom XML parsing logic (`nmap_xml`) to preserve nested XML event structures.

### SPL Regex Extraction Query
Engineered custom field extraction queries utilizing regular expressions (`rex`) with block-level isolation (`(?s)`) to resolve cross-matching and duplicate counts:

```spl
index="main" source="*nmap_scan_results*"
| rex max_match=0 field=_raw "(?s)(?<port_chunk><port\s+protocol=.*?</port>)"
| mvexpand port_chunk
| rex field=port_chunk "portid=\"(?<port>\d+)\""
| rex field=port_chunk "protocol=\"(?<protocol>[^\"]+)\""
| rex field=port_chunk "<state\s+state=\"(?<state>[^\"]+)\""
| rex field=port_chunk "<service\s+name=\"(?<service>[^\"]+)\""
| search state="open"
| table port, protocol, service
| dedup port, protocol, service
| sort port
```

### Security Insights & Findings
* **Discovered Services:** Identified active SSH (`22`), Web Services (`80`), Nping Echo (`9929`), and TCP-wrapped ports (`31337`).
* **Threat Mitigation:** Identified exposed administrative services; recommended implementing IP-restricted network access control lists (ACLs) and SSH public-key authentication.
