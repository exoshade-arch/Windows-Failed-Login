# Windows-Failed-Login
Lab project for monitoring failed login attempts on Windows Home using Splunk Universal Forwarder. Logs from PowerShell-generated failed login attempts are collected in a failed_logins.log file and visualized in a Timechart panel on Splunk Enterprise.

# Windows Failed Login Monitoring Lab

This project demonstrates how to monitor and visualize failed login attempts on a Windows Home machine using Splunk Enterprise and Splunk Universal Forwarder. PowerShell-generated failed login events are collected into failed_logins.log and displayed in a Timechart panel on Splunk Enterprise for analysis.

---

## Topology

Splunk Enterprise (Indexer): 192.168.50.20

Windows Home (Forwarder): 192.168.50.30

---

## Steps to reproduce

Create log file on Windows Home using PowerShell:

1..10 | ForEach-Object { Add-Content "C:\temp\failed_logins.log" "Failed login for user vboxuser from 192.168.50.$_" }

---

## Configure inputs.conf to monitor the log:

[monitor://C:\temp\failed_logins.log]
disabled = 0
index = wineventlog
sourcetype = wineventlog

---

## Add forward-server to point to Splunk Enterprise via CMD:

"C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" add forward-server 192.168.50.30:9997 -auth admin:YourPassword


Restart the forwarder.

---

## Dashboard

Name: Windows Failed Logins

Panel: Timechart

index=wineventlog sourcetype=wineventlog
| timechart span=1m count


Time picker: All time

---

## Notes

C:\temp folder must exist for the log file.

All testing done in isolated VM environment.

Screenshots attached for visual reference of dashboard.
