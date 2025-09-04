# BOTS V2 Playbooks

## Overview
This repository contains my 21-day learning journey to master Splunk and security investigations using the **Boss of the SOC (BOTS) V2 dataset**. I’m a beginner SOC analyst training to become proficient in Splunk Processing Language (SPL), incident response (IR), and creating playbooks. Each day focuses on a real-world incident scenario from BOTS V2, where I investigate logs, correlate data, and prepare reports, mimicking an L1 SOC analyst role.

The project follows a **scenario-based approach**, treating each day as a mini case study. I use the BOTS V2 dataset (a pre-indexed, historical dataset from ~August 2017) to practice searching, analyzing, and reporting on security incidents. All work is done in Splunk’s `botsv2` index, with historical time ranges (e.g., `earliest=0` or `All Time`).

## Repository Structure
- **/Day1/**: Investigation artifacts for Day 1 (Unusual Web Activity).
- **/Day2/**, **/Day3/**, etc.: Future folders for daily case studies.
- Each folder contains:
  - **Report (DayX_Report.md)**: Incident summary, evidence, and SPL queries.
  - **SPL Queries (DayX_Queries.txt)**: Saved search commands.
  - **Screenshots (optional)**: Visuals of Splunk results or dashboards.
  - **Other Assets**: Playbooks, dashboards, or lookups as I progress.

## Day 1: Unusual Web Activity Investigation
**Date**: September 04, 2025  
**Scenario**: An alert flagged employee Amber Turing (username: `amber`, email: `aturing@froth.ly`) for browsing external websites after a failed acquisition at Frothly Beer Company. This could indicate insider risk or phishing. The goal was to identify any competitor websites she visited using web logs (`sourcetype=stream:http`) in the `botsv2` index.

### Tasks Completed
1. Verified the `botsv2` index and sourcetypes using `tstats` commands.
2. Searched for Amber’s activity to find her IP address.
3. Correlated web traffic logs to identify visited domains.
4. Confirmed data time spans (~August 2017).
5. Saved key search as "Amber_Web_Traffic" and exported results.

### Key Findings
- Amber’s IP: [e.g., `10.0.1.51`—replace with your actual IP from results].
- Suspicious domain visited: [e.g., `taedonggang.com`—replace with your finding, a competitor site].
- Evidence: Web logs (`stream:http`) showed visits to non-internal domains, correlating to acquisition context.
- No immediate escalation needed; flagged for monitoring.

### Artifacts in /Day1/
- **Day1_Report.md**: Detailed IR report with environment, SPL, evidence, and decisions.
- **Day1_Queries.txt**: SPL queries used (e.g., `index=botsv2 earliest=0 amber`).
- **Day1_Results.csv** (optional): Exported Splunk results for web traffic.

### SPL Queries Used
```spl
| tstats count where index=* by index
| tstats count where index=botsv2 by sourcetype
index=botsv2 earliest=0 amber
index=botsv2 earliest=0 src_ip=[Amber Lill in your IP] sourcetype=stream:http
| tstats earliest(_time) as earliest, latest(_time) as latest, count where index=botsv2 sourcetype=stream:http | eval earliest=strftime(earliest, "%Y-%m-%d"), latest=strftime(latest, "%Y-%m-%d")
