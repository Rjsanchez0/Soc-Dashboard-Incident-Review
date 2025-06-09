# SOC Dashboard Incident Review & Remediation Plan

This project simulates real-world SOC alert triaging, where I took on the role of a newly hired Tier 1 SOC Analyst. The goal was to review activity from a security dashboard, analyze suspicious behavior, determine escalation paths, and propose remediation steps.

---

## 🎯 Scenario Overview

> **Role:** Tier 1 SOC Analyst at Eagle Corp  
> **Environment:** Cloud-only infrastructure, hardened endpoints, restricted admin access via PAW  
> **Goal:** Identify anomalies and escalate valid threats using visualizations from the SOC dashboard

You have just completed onboarding at Eagle Corp, and your task is to start monitoring alerts within the company’s internal SOC dashboard (Kibana). The senior analyst provided the following background:

- The organization migrated to the cloud; legacy DMZ servers are decommissioned.
- Only four IT Operations admins have privileged access. They are known to use the default `Administrator` account frequently.
- Admin tasks must be conducted from Privileged Access Workstations (PAWs).
- Service accounts follow naming conventions like `*-svc` and are non-interactive.
- Root SSH login is disabled, and sudo escalation is enforced for Linux servers.


You reviewed the following SOC dashboard visualizations:
1. Failed logon attempts (all users, admin users, disabled users)
2. RDP logons by service accounts
3. Admin logons not from PAWs
4. SSH root login attempts
5. Local group membership changes

---

📁 Full analysis: [`incident-summary/SOC_Alerts_Incident_Analysis_Report.md`](incident-summary/SOC_Alerts_Incident_Analysis_Report.md)  
📁 Remediation plan: [`remediation-plan/Remediation_Strategy.md`](remediation-plan/Remediation_Strategy.md)

---
