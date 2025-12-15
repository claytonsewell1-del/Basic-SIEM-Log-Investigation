Basic SIEM Log Investigation (Elastic)

Overview
This project demonstrates a basic log investigation using Elastic (Kibana) and KQL queries. It simulates a real-world SOC Tier-1 scenario: identifying suspicious IP activity and correlating errors in unstructured logs.

Skills demonstrated:
- SIEM log discovery
- Field-level analysis
- KQL filtering
- Understanding keyword vs text field mappings
- IP-based investigation
- Error correlation

Logs Used
- Index: `sample_logs2`
- Fields of interest:
  - `client` → IP address
  - `alias_list` → error message (`[Errno 1] Unknown host`)
  - `hostname` → host info

Investigation Steps

1. Discover Logs
   - Open Kibana Discover
   - Select `sample_logs2` index pattern
   - Search for all documents:
     ```kql
     *
     ```
   - Screenshot: `05-discover-logs.png`

2. Identify Suspicious IP
   - Filter for IP `2.191.43.16`:
     ```kql
     client: "2.191.43.16"
     ```
   - Screenshot: `07-ip-filtered-view.png`

3. Correlate IP with Error
   - Filter for IP + DNS errors:
     ```kql
     client: "2.191.43.16" AND alias_list: "[Errno 1] Unknown host"
     ```
   - Screenshot: `08-ip-error-correlation.png`

Findings
- The IP `2.191.43.16` generated repeated DNS resolution failures.
- Correlation with `[Errno 1] Unknown host` suggests:
  - DNS misconfiguration  
  - Unreachable external resources  
  - Possible automated outbound connection attempts

Lessons Learned
- Keyword fields require **exact match queries**.  
- Unstructured logs often require **raw field filtering** for investigation.  
- SOC analysts must pivot between field-level queries and raw message inspection.

Next Steps
- Normalize logs for structured analysis
- Set alerts for repeated DNS failures
- Automate IP correlation for SOC dashboards

Screenshots
- `05-discover-logs.png` – All logs in Discover  
- `07-ip-filtered-view.png` – Filtered suspicious IP  
- `08-ip-error-correlation.png` – IP correlated with error

