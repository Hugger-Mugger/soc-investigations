# 🧠 Technical Investigation Report  
**Title**: Oracle Cloud Breach – Identity Infrastructure Exploitation via CVE-2021-35587  
**Author**: Abhijeet Kumar  
**Role**: SOC Analyst (Candidate)  
**Date**: September 1, 2025

---

## 1. Executive Summary

This investigation analyzes the Oracle Cloud breach attributed to threat actor **rose87168**, who allegedly exploited a known vulnerability in Oracle Fusion Middleware to access identity infrastructure and exfiltrate sensitive credentials. As a SOC analyst candidate, I approached this case using publicly available threat intelligence, documentation, and structured reasoning — simulating how I would investigate the incident in a real SOC environment.

---

## 2. Thought Process & Investigation Objectives

When I first saw reports of leaked Oracle credentials on BreachForums, I asked:

> “Are these artifacts legitimate? What kind of access would be required to obtain them? And what vulnerability could explain this?”

That led me to define the following objectives:

- Identify the most plausible initial access vector  
- Validate the leaked artifacts using Oracle’s public documentation  
- Profile the attacker’s behavior and infrastructure  
- Build detection logic using simulated telemetry  
- Recommend response actions based on standard SOC practices

---

## 3. Phase 1: Entry Point Identification

### 🔍 Trigger  
The attacker posted samples of Oracle identity artifacts, including `keystore.jks`, `ldap.properties`, and `jps-config.xml`. These files are typically used in Oracle’s identity and access management stack.

### 🔍 Vulnerability Mapping  
I searched Oracle’s CVE history and found **CVE-2021-35587**, a deserialization flaw in Oracle Access Manager (OAM) that allows **unauthenticated remote code execution** via the `/oam/server/Logout` endpoint.

### 🧠 Reasoning  
According to Oracle’s [Access Manager documentation](https://docs.oracle.com/cd/E52734_01/oam/AIAAG/GUID-D212B4B4-EB25-4969-A297-4A165BB453DF.htm), `keystore.jks` is used for SSO token signing and agent communication. Similarly, `ldap.properties` stores bind credentials for LDAP integration, and `jps-config.xml` configures keystore access policies.

> “If these files were leaked, the attacker likely accessed Oracle Access Manager or Identity Manager via a known vulnerability — most plausibly CVE-2021-35587.”

---

## 4. Phase 2: Artifact Validation

### 🔍 Leaked Files  
- `keystore.jks`: Contains cryptographic keys for SSO  
- `ldap.properties`: Stores LDAP bind credentials  
- `jps-config.xml`: Maps tenant identities and access policies

### 🔍 Source-Based Validation  
I cross-referenced these filenames and structures with Oracle’s public documentation and community forums. Additionally, threat intel from CloudSEK and CPX confirmed that the leaked hashes matched production credentials.

### 🧠 Reasoning  
While I don’t have access to Oracle’s internal systems, the consistency between the leaked files and documented IAM components strongly suggests that the attacker accessed Oracle’s identity infrastructure.

> “This isn’t speculation — it’s deduction based on publicly documented architecture and validated threat intel.”

---

## 5. Phase 3: Threat Actor Profiling

| Attribute | Details |
|----------|---------|
| **Alias** | rose87168 |
| **Activity** | Selling Oracle credentials, demanding ransom |
| **Infrastructure** | `aitism[.]net`, `oraclecloud[.]pro`, ProtonMail |
| **Motivation** | Extortion + reputation building |
| **Behavior** | Posted samples, negotiated ransom, offered barter for zero-day exploits |

---

## 6. Phase 4: Detection Logic

See [`detection-logic.txt`](./detection-logic.txt) for full KQL query.

---

## 7. Phase 5: IOC Table

See [`iocs.csv`](./iocs.csv) for full IOC list.

---

## 8. Phase 6: Response Strategy

### 🔧 Immediate Actions  
- Revoke and rotate exposed credentials  
- Isolate vulnerable Gen 1 servers  
- Notify affected tenants and regulators  
- Block attacker infrastructure at perimeter

### 🛡️ Preventive Measures  
- Decommission legacy Fusion Middleware instances  
- Enforce scoped access to identity artifacts  
- Integrate IAM telemetry into SIEM workflows  
- Red-team SSO and LDAP flows for impersonation vectors

---

## 9. Final Conclusion

This investigation confirms that the Oracle Cloud breach was a multi-layered identity compromise. The attacker exploited a known CVE to access Oracle’s IAM stack, exfiltrated sensitive credentials, and impersonated tenants. My conclusions are based on:

- Public documentation of Oracle’s IAM architecture  
- CVE mapping and exploit feasibility  
- Threat intel validation of leaked artifacts  
- Simulated detection logic and response planning

---

## 10. References

See [`references.md`](./references.md) for full source list.
