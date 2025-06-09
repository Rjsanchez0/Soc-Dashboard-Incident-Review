# 🛡️ Remediation Strategy – SOC Incident Findings

This document outlines the containment, investigation, and hardening actions following the alert triage and incident review conducted on the SOC dashboard.

---

## 🔒 Containment Steps

1. **Escalate to Tier 2/3 or Incident Response Team**
   - Immediately notify the senior SOC team or dedicated Incident Response (IR) team to take over high-risk findings involving disabled account use, privilege escalation, and unauthorized root access attempts.
   - Ensure the escalation includes all supporting evidence, logs, and timestamps.

2. **Isolate Affected Systems**
   - Temporarily disconnect `PKI`, `WS001`, and any host accessed by `sql-svc1` or Administrator during the window of suspicious activity.
   - Place these systems in a quarantined VLAN for offline forensic analysis.

3. **Reset Credentials**
   - Force credential resets for all accounts involved:
     - `Administrator`
     - `sql-svc1` (service account showing anomalous behavior)
     - Any user accounts added to the Administrators group
   - Invalidate all sessions and tokens associated with those accounts.

4. **Disable Suspect Accounts**
   - Temporarily disable `sql-svc1` if it is not business-critical until further analysis validates its use.
   - Investigate and disable any accounts using unknown or non-standard SIDs added to privileged groups.

---

## 🧪 Forensic Review

1. **Analyze Logs from SIEM**
   - Investigate source IPs for failed and successful logons: e.g., `192.168.28.132`, `.200`, or any unusual internal or external addresses.
   - Cross-reference timestamps across systems to detect lateral movement or coordinated access attempts.

2. **Review Event IDs**
   - Audit logon Event IDs (e.g., 4624, 4625) and group membership changes (e.g., 4732).
   - Investigate any signs of privilege escalation, token impersonation, or unusual user context switching.

3. **Validate SID Membership Changes**
   - Resolve unidentified SIDs added to the Administrators group.
   - Verify if this was an internal tool misconfiguration or a sign of attacker privilege escalation.

4. **Correlate Across Assets**
   - Check if the same accounts or IPs appeared in multiple dashboards or systems.
   - Use timeline analysis to build a full picture of the attacker’s path (initial access → privilege escalation → lateral movement).

---

## 🔐 Hardening Measures

1. **Restrict Interactive Logons for Service Accounts**
   - Modify local GPO or AD policies to prevent accounts like `sql-svc1` from initiating RDP sessions.
   - Ensure service accounts are flagged as `LOGON_TYPE = 4` (batch) or `5` (service), not interactive.

2. **Disable Root Login Over SSH**
   - Enforce this in `/etc/ssh/sshd_config`:
     ```bash
     PermitRootLogin no
     ```
   - Restart the SSH daemon (`systemctl restart sshd`) after updating.

3. **Apply Network-Based Controls**
   - Restrict SSH/RDP access by IP whitelist (jump servers, admin subnets only).
   - Add alerts for SSH access attempts outside of approved IP ranges.

4. **Enforce MFA (Multi-Factor Authentication)**
   - Implement MFA for:
     - All administrative logins
     - VPN access
     - Critical cloud dashboards or control planes

5. **Implement Least Privilege Review**
   - Audit group membership (e.g., Domain Admins, Local Admins)
   - Remove excessive privileges
   - Apply Just-in-Time (JIT) access where possible

6. **Enable Alerting for High-Value Actions**
   - Alert when:
     - A disabled account attempts to authenticate
     - A service account is used for interactive login
     - A new user is added to privileged groups
     - Admin logins are detected from non-PAW machines

7. **Logging & Monitoring Enhancements**
   - Ensure endpoint logs are forward to SIEM in near real-time
   - Validate parsing and enrichment for key log fields (e.g., username, hostname, IP)

---

## ✅ Summary

This remediation plan aims to contain potential threats, investigate the root cause, and harden the environment to prevent recurrence. The combination of containment, forensic validation, and policy enforcement reflects SOC best practices and supports blue team maturity.

