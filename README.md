# sigma-detection-rules

> **MITRE ATT&CK-mapped Sigma detection rules with Splunk, Sentinel & Elastic query translations**

---

## Why Detection-as-Code?

Storing detection logic in version-controlled files — rather than locked inside a SIEM GUI — gives security teams:

- **Consistency** — rules are tested, reviewed, and deployed the same way across every environment  
- **Portability** — a single Sigma rule translates to Splunk SPL, Microsoft Sentinel KQL, and Elastic EQL/KQL with no rewrites  
- **Auditability** — every rule change is tracked in git with author, date, and reasoning  
- **Collaboration** — detection engineers can submit PRs, run CI checks, and peer-review logic before it hits production  
- **Speed** — new threat intelligence can be operationalized in minutes, not weeks waiting on SIEM GUI access

---

## Repository Structure

```
sigma-detection-rules/
├── rules/                        # Sigma rules (.yml), organized by MITRE ATT&CK tactic
│   ├── credential-access/
│   ├── initial-access/
│   ├── lateral-movement/
│   ├── exfiltration/
│   ├── persistence/
│   ├── defense-evasion/
│   └── impact/
├── queries/
│   ├── splunk/                   # Splunk SPL (.spl)
│   ├── sentinel/                 # Microsoft Sentinel KQL (.kql)
│   └── elastic/                  # Elastic/EQL queries (.txt)
└── docs/                         # Supporting documentation
```

---

## Detection Rules — Full Index

| # | Rule Name | MITRE Technique | Tactic | Level |
|---|-----------|----------------|--------|-------|
| 1 | Brute Force Authentication | [T1110](https://attack.mitre.org/techniques/T1110/) | Credential Access | Medium |
| 2 | Password Spray | [T1110.003](https://attack.mitre.org/techniques/T1110/003/) | Credential Access | High |
| 3 | Impossible Travel / Anomalous Sign-In | [T1078](https://attack.mitre.org/techniques/T1078/) | Credential Access | High |
| 4 | MFA Fatigue / Push Bombing | [T1621](https://attack.mitre.org/techniques/T1621/) | Credential Access | High |
| 5 | Phishing Inbound with Malicious Attachment | [T1566.001](https://attack.mitre.org/techniques/T1566/001/) | Initial Access | High |
| 6 | Suspicious External Login to Valid Account | [T1078](https://attack.mitre.org/techniques/T1078/) | Initial Access | Medium |
| 7 | Lateral Movement via RDP | [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | Lateral Movement | Medium |
| 8 | Lateral Movement via SMB / Admin Shares | [T1021.002](https://attack.mitre.org/techniques/T1021/002/) | Lateral Movement | Medium |
| 9 | Pass-the-Hash | [T1550.002](https://attack.mitre.org/techniques/T1550/002/) | Lateral Movement | High |
| 10 | DNS Tunneling / Exfiltration over DNS | [T1071.004](https://attack.mitre.org/techniques/T1071/004/) | Exfiltration | Medium |
| 11 | Large Outbound Data Transfer | [T1048](https://attack.mitre.org/techniques/T1048/) | Exfiltration | High |
| 12 | New Account Creation for Persistence | [T1136.001](https://attack.mitre.org/techniques/T1136/001/) | Persistence | Medium |
| 13 | Scheduled Task Created for Persistence | [T1053.005](https://attack.mitre.org/techniques/T1053/005/) | Persistence | High |
| 14 | Disabling Security Tools / Windows Defender | [T1562.001](https://attack.mitre.org/techniques/T1562/001/) | Defense Evasion | High |
| 15 | Ransomware Mass Encryption + Shadow Copy Deletion | [T1486](https://attack.mitre.org/techniques/T1486/) + [T1490](https://attack.mitre.org/techniques/T1490/) | Impact | Critical |

---

## How to Use the Queries

### Microsoft Sentinel (KQL)
1. Open **Microsoft Sentinel** → **Logs**
2. Copy the contents of the relevant `.kql` file from `queries/sentinel/`
3. Paste into the query editor and run — or save as a **Scheduled Alert Rule**
4. Set the threshold and time window per the comments in each file

### Splunk
1. Open **Splunk** → **Search & Reporting**
2. Copy the contents of the relevant `.spl` file from `queries/splunk/`
3. Paste into the search bar and run — or save as a **Scheduled Alert**
4. Adjust `index=` values to match your environment's index names

### Elastic / Elastic SIEM
1. Open **Kibana** → **Security** → **Rules** → **Create New Rule**
2. For threshold-based rules, select **Threshold** rule type and use the filter expression in the `.txt` file
3. For match rules, select **Custom Query** and paste the query expression
4. Configure field groupings and thresholds per the inline comments in each file

---

## Sigma Rules

Each `.yml` rule follows the [Sigma specification](https://github.com/SigmaHQ/sigma) and can be converted to any SIEM format using [sigma-cli](https://github.com/SigmaHQ/sigma-cli):

```bash
# Install sigma-cli
pip install sigma-cli

# Convert a rule to Splunk SPL
sigma convert -t splunk rules/credential-access/password_spray.yml

# Convert a rule to Microsoft Sentinel KQL
sigma convert -t microsoft365defender rules/lateral-movement/pass_the_hash.yml
```

---

## Author

**Oluwaseyi Michael Falode**  
Detection Engineer | Cloud & Cybersecurity  

- Email: [seyi.falode@yahoo.com](mailto:seyi.falode@yahoo.com)  
- LinkedIn: [linkedin.com/in/oluwaseyi-falode](https://linkedin.com/in/oluwaseyi-falode)  
- GitHub: [github.com/seyifalode-cmd](https://github.com/seyifalode-cmd)

---

*Detection engineering is an ongoing discipline. Rules in this repo are marked `experimental` and should be tuned to your environment's baseline before deploying to production.*
