# Splunk SSH SIEM Dashboard

A Splunk dashboard for monitoring SSH authentication activity, detecting brute-force login attempts, and visualizing attacker geo-location — built as a hands-on SIEM/log analysis project.

!\[Splunk](https://img.shields.io/badge/Splunk-000000?style=flat\&logo=splunk\&logoColor=white)
!\[SIEM](https://img.shields.io/badge/Category-SIEM%20%2F%20Log%20Analysis-blue)
!\[Status](https://img.shields.io/badge/Status-Complete-brightgreen)

## Overview

This project ingests SSH authentication logs into Splunk and builds a single dashboard that answers three questions analysts care about during an SSH-focused investigation:

1. **How much SSH activity is there, and how much of it is failing?** (Authentication Overview)
2. **Who/what is being targeted, and is there a pattern that looks like brute forcing?** (Login Activity Trends)
3. **Where in the world are the attack attempts coming from?** (Geo-location Visualization)

A shared time range token (`time\_range`) drives every panel, so the whole dashboard can be re-scoped to any time window from one control.

## Dashboard Preview

### Task 1 — Authentication Overview

!\[Authentication Overview](screenshots/task1\_full\_authentication\_overview.png)

Total SSH Events, Successful Logins, Failed Logins, and Connection without Authentication, all sharing the same time picker.

### Task 2 — Login Activity Trends

**Failed Logins by Username**

!\[Failed Logins by Username](screenshots/task2\_1\_failed\_logins\_by\_username\_result.png)

**Possible Brute Force by IP Address**

!\[Possible Brute Force by IP Address](screenshots/task2\_2\_possible\_brute\_force\_by\_ip\_result.png)

### Task 3 — Brute Force Geo-location

!\[Brute Force Geo-location Map](screenshots/task3\_1\_geolocation\_map\_result.png)

The US, China, and Brazil stand out as the top sources of failed-authentication attempts in this dataset, with the US in the highest bucket (80–100 attempts).

## Project Structure

```
splunk-ssh-siem-dashboard/
├── README.md
├── LICENSE
├── .gitignore
├── screenshots/
│   ├── 00\_data\_setup\_upload\_ssh\_logs\_new.png
│   ├── task1\_1\_total\_ssh\_events\_panel\_config.png
│   ├── task1\_1\_total\_ssh\_events\_result.png
│   ├── task1\_4\_connection\_without\_authentication\_config.png
│   ├── task1\_full\_authentication\_overview.png
│   ├── task2\_1\_failed\_logins\_by\_username\_config.png
│   ├── task2\_1\_failed\_logins\_by\_username\_result.png
│   ├── task2\_2\_possible\_brute\_force\_by\_ip\_config.png
│   ├── task2\_2\_possible\_brute\_force\_by\_ip\_result.png
│   ├── task3\_1\_geolocation\_map\_config.png
│   └── task3\_1\_geolocation\_map\_result.png
└── searches/
    ├── task1\_authentication\_overview.spl
    ├── task2\_login\_activity\_trends.spl
    └── task3\_geolocation\_visualization.spl
```

* **`screenshots/`** — Visual evidence of each dashboard panel and its search config, captured directly from Splunk.
* **`searches/`** — The raw SPL (Search Processing Language) queries used to build each panel, grouped by task, matching what's actually configured in the dashboard (not just the original task brief) so they can be copy-pasted into a new Splunk instance.

## Setup

### Data Ingestion

SSH log data (`ssh\_logs\_new.json`, sourcetype `\_json`) was uploaded via **Settings → Add Data → Upload**.

!\[Data Setup](screenshots/00\_data\_setup\_upload\_ssh\_logs\_new.png)

### Task 0 — Configure the Shared Time Range

Every panel in this dashboard reads from a single shared time input so results stay consistent across the whole page.

1. Click **Add Input** → select **Time**.
2. Click the pencil (edit) icon on the new input.
3. Set **Label** to `Time Range` and **Token** to `time\_range`.
4. Click **Add Input** again → select **Submit**.
5. For every panel added afterward, set its time source to the shared `time\_range` token.

### Task 1 — Authentication Overview Panels

Goal: give a quick summary of SSH activity.

|Panel|Visualization|Title|Result|
|-|-|-|-|
|1.1|Single Value|Total SSH Events|1,200|
|1.2|Single Value|Successful Logins|306|
|1.3|Single Value|Failed Logins|305|
|1.4|Single Value|Connection without Authentication|286|

All four panels use **Add Panel → New → Single Value**, with the shared `time\_range` time picker. Queries: [`searches/task1\_authentication\_overview.spl`](searches/task1_authentication_overview.spl).

### Task 2 — Login Activity Trends

Goal: visualize login behavior over time and detect spikes.

|Panel|Visualization|Title|Search|
|-|-|-|-|
|2.1|Bar Chart|Failed Logins by Username|[`searches/task2\_login\_activity\_trends.spl`](searches/task2_login_activity_trends.spl)|
|2.2|Statistics Table|Possible Brute Force by IP Address|same file|

Panel 2.1 surfaces which usernames are being targeted most (`root`, `backup`, and `alice` top the list). Panel 2.2 surfaces which source IPs are generating the most "Multiple Failed Authentication Attempts" events — a proxy for likely brute-force sources.

### Task 3 — Brute Force Geo-location Visualization

Goal: plot brute-force source IPs on a world map to spot geographic concentration of attacks.

|Panel|Visualization|Title|Search|
|-|-|-|-|
|3.1|Choropleth Map|Brute Force Attack with Geo-location|[`searches/task3\_geolocation\_visualization.spl`](searches/task3_geolocation_visualization.spl)|

This panel uses `iplocation` to resolve source IPs to countries, then `geom geo\_countries` to render a choropleth of attack counts by country.

## Data Sources

|Source|Sourcetype|Host|Used In|
|-|-|-|-|
|`ssh\_logs\_new.json`|`\_json`|`windows`|Task 1, Task 2|
|`ssh\_logs\_new.json`|`\_json`|`LinuxNew`|Task 3|

## Key Skills Demonstrated

* Splunk dashboard design with reusable, token-driven time controls
* SPL: `stats`, `top`, `table`, `iplocation`, `geom`
* SIEM-style triage panels: volume, success/fail ratio, unauthenticated connections
* Brute-force detection via failed-attempt aggregation by username and source IP
* Geo-enrichment and choropleth visualization of attacker origin

## License

This project is released under the [MIT License](LICENSE).

## Author

Built as a personal SOC/SIEM analyst practice project in Splunk by Kabelo Motaung.

