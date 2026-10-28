# Windows Failed Logon Investigation — Event ID 4625

## 1. Overview

This investigation documents a controlled Windows failed-logon test in my Home SOC Lab using Wazuh SIEM.

The objective was to verify that Windows Security events were collected by Wazuh and to practice investigating authentication-related events from a SOC analyst's perspective.

## 2. Lab Environment

| Component | Details |
|---|---|
| Host operating system | Windows 11 |
| SIEM platform | Wazuh 4.14.2 |
| Wazuh deployment | Docker Compose |
| Wazuh server | Ubuntu Desktop VM |
| Endpoint | Windows 11 host |
| Log source | Windows Security Event Log |
| Investigation tool | Wazuh Dashboard — Threat Hunting |
| Event ID | 4625 |

## 3. Objective

- Verify Windows endpoint connectivity with Wazuh.
- Confirm collection of Windows Security events.
- Generate a controlled failed-logon event.
- Locate and analyze the event in Wazuh.
- Understand relevant event fields and detection-rule details.

## 4. Investigation Procedure

### Step 1: Verify Endpoint Connectivity

Checked the Wazuh Dashboard to confirm that the Windows endpoint was active and communicating with the Wazuh server.

### Step 2: Verify Windows Auditing

Checked the Windows logon auditing policy. Both successful and failed logon auditing were enabled.

### Step 3: Generate a Controlled Test Event

Performed a controlled failed-login test on the Windows endpoint for lab validation.

### Step 4: Confirm the Event in Windows

Opened Windows Event Viewer and verified that the Security log contained Event ID `4625`.

### Step 5: Find the Event in Wazuh

Opened Wazuh Dashboard → Threat Hunting and searched for Event ID `4625` to locate the corresponding Windows Security event.

### Step 6: Analyze the Alert

Reviewed the event fields, Wazuh rule description, severity value, logon type, and status codes.

## 5. Event Details

The following values were observed in the investigated event.

| Field | Observed value |
|---|---|
| Windows Event ID | `4625` |
| Severity value | `AUDIT_FAILURE` |
| Logon type | `2` |
| Status | `0xC000006D` |
| Substatus | `0xC0000380` |
| Wazuh rule description | `Logon Failure - Unknown user or bad password` |
| Wazuh rule groups | `windows`, `windows_security`, `authentication_failed` |

### Field Interpretation

- **Event ID 4625:** Indicates that an account failed to log on.
- **AUDIT_FAILURE:** Indicates a failed-audit event.
- **Logon type 2:** Represents an interactive logon, such as a local sign-in.
- **Status `0xC000006D`:** A general logon failure status.
- **Substatus `0xC0000380`:** Requires additional verification against the full Windows event details before attributing a specific cause.

The Wazuh rule description is a useful starting point for investigation, but it should not replace analysis of the complete event.

## 6. Detection and MITRE ATT&CK Context

The alert included a MITRE ATT&CK mapping to `T1531` — Account Access Removal.

This mapping alone does not establish that account access was removed or that the event represents that technique. The observed event was a failed logon, so the mapping should be validated against the rule logic and event context before being used in a final incident classification.

## 7. SOC Investigation Assessment

**Result: Windows event collection verified for the tested event.**

The investigation confirmed that:

1. The Windows endpoint was active in Wazuh.
2. Windows recorded the controlled failed-logon event.
3. Event ID 4625 was available in Wazuh Threat Hunting.
4. The event fields and associated Wazuh detection could be reviewed.

Because the event was generated intentionally during testing, it was consistent with the controlled lab activity.

A single failed logon is not sufficient evidence of malicious activity. In a real investigation, an analyst should review the account, source information, surrounding logon events, frequency of failures, and any related alerts before deciding whether escalation is required.

## 8. Key Learnings

- Validated Windows Security event collection in Wazuh.
- Practiced searching for Windows Event IDs.
- Learned the meaning of failed-logon Event ID 4625.
- Reviewed logon type, status, and substatus fields.
- Practiced alert triage and contextual analysis.
- Learned to validate detection-rule mappings rather than treating them as proof of an attack.

## 9. Conclusion

This exercise demonstrated the basic workflow for investigating Windows authentication failures using Wazuh SIEM. It extended my Home SOC Lab beyond Linux authentication monitoring and provided practical experience with Windows Security logs, alert analysis, and evidence-based SOC investigation.

## 10. Scope and Disclaimer

This was an educational test performed on an authorized lab endpoint. The findings describe the observed test event and do not establish that a real-world compromise occurred.

