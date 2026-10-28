# Linux Authentication Failure Investigation — Wazuh SIEM

## 1. Overview

This report documents Linux authentication monitoring and failed-login investigation in my Home SOC Lab using Wazuh SIEM.

The objective was to monitor authentication activity on a Kali Linux endpoint, generate controlled authentication events, and investigate the resulting alerts through the Wazuh Dashboard.

## 2. Lab Environment

| Component | Details |
|---|---|
| Host operating system | Windows 11 |
| Virtualization | VMware |
| SIEM platform | Wazuh 4.14.2 |
| Deployment | Docker Compose |
| Wazuh server | Ubuntu Desktop VM |
| Monitored endpoint | Kali Linux VM |
| Log sources | Linux authentication logs, PAM events |
| Investigation interface | Wazuh Dashboard |

## 3. Objectives

- Monitor Linux authentication activity using Wazuh.
- Generate controlled successful and failed authentication events.
- Identify relevant authentication alerts.
- Analyze Wazuh rule IDs, descriptions, and severity levels.
- Understand how authentication detections may relate to MITRE ATT&CK.
- Practice a basic SOC alert-investigation workflow.

## 4. Investigation Procedure

### Step 1: Verify Endpoint Connectivity

Checked the Wazuh Dashboard to confirm that the Kali Linux endpoint was connected to the Wazuh manager.

### Step 2: Generate Controlled Authentication Events

Performed authentication tests on the Kali Linux lab endpoint using the test account `soc_test_user`. An intentionally incorrect password was entered to generate a failed-authentication event.

### Step 3: Search for Authentication Alerts

Opened the Wazuh Dashboard and examined authentication-related events from the monitored endpoint.

### Step 4: Filter Relevant Events

Used Wazuh rule IDs and event details to narrow the results to relevant authentication detections.

### Step 5: Analyze the Alerts

Reviewed the alert descriptions, rule IDs, severity levels, and available event details to understand why Wazuh generated each alert.

### Step 6: Document the Findings

Compared the observed alerts with the controlled test activity and recorded the investigation results.

## 5. Detection Results

The following Wazuh detections were observed during the lab tests.

| Rule ID | Detection description | Level |
|---|---|---|
| `5503` | PAM: User login failed | `5` |
| `5557` | unix_chkpwd: Password check failed | `5` |
| `5301` | User missed the password to change UID | `5` |

These are the rule IDs and descriptions recorded during the lab investigation. The exact meaning of each alert should be confirmed using the complete event details and the applicable Wazuh rule definition.

## 6. Detection Analysis

### Failed Authentication

A failed authentication event indicates that an authentication attempt did not succeed. This can happen for legitimate reasons, including an incorrectly entered password.

During this lab exercise, a failed login was generated intentionally to validate the monitoring workflow.

### PAM Authentication Events

Linux systems can use Pluggable Authentication Modules (PAM) to manage authentication. Wazuh can analyze relevant authentication logs and generate alerts when configured rules match the recorded events.

### Rule IDs and Severity

Wazuh rule IDs identify the detection logic associated with an alert. The alert level indicates its configured severity, but it does not independently determine whether the activity is malicious.

The surrounding events and test context must be considered before making a SOC assessment.

## 7. MITRE ATT&CK Context

**Reference:** `T1110.001` — Password Guessing, where applicable.

Password guessing involves attempting passwords to gain access to an account. A failed-login event alone does not prove password guessing occurred. The classification should be supported by the observed behavior, such as repeated attempts or other relevant evidence.

## 8. SOC Investigation Assessment

**Result: Linux authentication monitoring and alert investigation demonstrated.**

The exercise provided practical experience with:

1. Monitoring a Linux endpoint through Wazuh.
2. Generating controlled authentication events.
3. Searching and filtering authentication alerts.
4. Reviewing Wazuh rule IDs and severity levels.
5. Comparing alerts with the known test activity.
6. Recording findings without assuming that every alert represents an attack.

The observed failed-login activity was consistent with the controlled test. Additional evidence would be required to classify similar activity as malicious in a real environment.

## 9. Key Learnings

- Linux authentication monitoring with Wazuh.
- Basic understanding of PAM authentication events.
- Searching and filtering SIEM alerts.
- Interpreting Wazuh rule IDs and severity levels.
- Understanding the context and limitations of MITRE ATT&CK mappings.
- Practicing basic SOC alert triage and investigation documentation.

## 10. Conclusion

This exercise demonstrated how Wazuh can be used to monitor Linux authentication activity and investigate failed-login alerts. It helped develop practical skills in endpoint monitoring, event analysis, and evidence-based SOC investigation.

## 11. Scope and Disclaimer

This investigation was performed in an authorized virtual lab using controlled authentication tests. The documented detections are evidence of the lab's monitoring workflow, not proof of a real-world compromise.

