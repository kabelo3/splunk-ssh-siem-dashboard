# Splunk SSH SIEM Dashboard

A Splunk dashboard for monitoring SSH authentication activity, detecting brute-force login attempts, and visualizing attacker geo-location — built as a hands-on SIEM/log analysis project.

![Splunk](https://img.shields.io/badge/Splunk-000000?style=flat&logo=splunk&logoColor=white)
![SIEM](https://img.shields.io/badge/Category-SIEM%20%2F%20Log%20Analysis-blue)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

## Overview

This project ingests SSH authentication logs into Splunk and builds a single dashboard that answers three questions analysts care about during an SSH-focused investigation:

1. **How much SSH activity is there, and how much of it is failing?** (Authentication Overview)
2. **Who/what is being targeted, and is there a pattern that looks like brute forcing?** (Login Activity Trends)
3. **Where in the world are the attack attempts coming from?** (Geo-location Visualization)

A shared time range token (`time_range`) drives every panel, so the whole dashboard can be re-scoped to any time window from one control.

## Dashboard Preview

### Task 1 — Authentication Overview

![Authentication Overview](screenshots/task1_full_authentication_overview.png)

Total SSH Events, Successful Logins, Failed Logins, and Connection without Authentication, all sharing the same time picker.

### Task 2 — Login Activity Trends

**Failed Logins by Username**

![Failed Logins by Username](screenshots/task2_1_failed_logins_by_username_result.png)

**Possible Brute Force by IP Address**

![Possible Brute Force by IP Address](screenshots/task2_2_possible_brute_force_by_ip_result.png)

### Task 3 — Brute Force Geo-location

![Brute Force Geo-location Map](screenshots/task3_1_geolocation_map_result.png)

The US, China, and Brazil stand out as the top sources of failed-authentication attempts in this dataset, with the US in the highest bucket (80–100 attempts).

## Project Structure
