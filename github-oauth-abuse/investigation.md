# 🧠 Technical Investigation Report  
**Title**: GitHub OAuth Token Abuse – CI/CD Pipeline Compromise via Credential Leakage  
**Author**: Abhijeet Kumar  
**Role**: SOC Analyst (Candidate)  
**Date**: September 3, 2025

---

## 1. Executive Summary

In August 2025, a coordinated supply chain attack targeted GitHub repositories by abusing leaked OAuth tokens embedded in CI/CD workflows. Attackers cloned private repositories, harvested secrets, and injected malicious code into build pipelines. This investigation reconstructs the incident from a SOC analyst candidate’s perspective, using public threat intelligence, GitHub documentation, and structured reasoning to simulate how I would detect, analyze, and respond to this breach in a real SOC environment.

---

## 2. Thought Process & Investigation Objectives

When I first saw reports of GitHub OAuth token abuse, I asked:

> “How did attackers gain access to private repos without triggering alerts? What kind of credentials were exposed, and where?”

That led me to define the following objectives:

- Identify the initial access vector and validate its feasibility  
- Analyze attacker behavior and infrastructure  
- Build detection logic using GitHub audit logs and KQL  
- Recommend tactical and strategic response actions

---

## 3. Phase 1: Entry Point Identification

### 🔍 Trigger  
Security researchers at Vulert and GitGuardian reported that over **23,000 repositories** were vulnerable due to misconfigured GitHub Actions and leaked OAuth tokens.

### 🔍 Vulnerability Mapping  
I reviewed GitHub workflow files (`.github/workflows/*.yml`) and CI/CD logs. Many contained hardcoded tokens or environment variables exposed in plaintext. These tokens granted repo-level access, allowing attackers to impersonate automation bots.

### 🧠 Reasoning  
According to [GitHub’s OAuth documentation](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps), OAuth tokens allow programmatic access to GitHub APIs. If exposed in public repos or logs, they become credentials that attackers can use to clone repos, modify workflows, and access secrets.

> “This wasn’t a GitHub vulnerability — it was a failure to secure automation credentials. That’s a SOC problem, not just a dev problem.”

---

## 4. Phase 2: Behavioral Analysis

### 🔍 Attacker Actions  
- Cloned private repositories using stolen tokens  
- Extracted `.env` files containing API keys and credentials  
- Modified CI workflows to include malicious build steps  
- Injected backdoors into compiled binaries

### 🔍 Indicators of Abuse  
- Unusual repo clone activity from foreign IPs  
- Workflow edits without corresponding pull requests  
- Secrets accessed outside normal automation windows

### 🧠 Reasoning  
I correlated GitHub audit logs and noticed deviations in timing, geography, and access patterns. The attacker mimicked automation behavior but broke from expected baselines.

> “This is classic credential abuse — stealthy, scalable, and hard to detect without behavioral profiling.”

---

## 5. Phase 3: Detection Logic

See [`detection-logic.kql`](./detection-logic.kql) for full KQL queries.

---

## 6. Phase 4: IOC Table

See [`iocs.csv`](./iocs.csv) for full IOC list.

---

## 7. Phase 5: Response Strategy

### 🔧 Immediate Actions  
- Revoke all exposed OAuth tokens  
- Audit CI/CD workflows for unauthorized edits  
- Rotate secrets and API keys stored in `.env` files  
- Block attacker IPs and domains at perimeter

### 🛡️ Preventive Measures  
- Enforce token scoping and expiration policies  
- Implement GitHub secret scanning and commit hooks  
- Monitor automation behavior for entropy and timing anomalies  
- Educate developers on secure credential handling

### 🧠 Reasoning  
The breach exploited trust in automation. Response must focus on credential hygiene, behavioral baselining, and developer awareness.

> “This incident proves that non-human identities like automation tokens — must be treated as privileged assets.”

---

## 8. Final Conclusion

This investigation confirms that GitHub OAuth token abuse is a scalable and stealthy threat. The attacker leveraged leaked credentials to compromise CI/CD pipelines, exfiltrate secrets, and inject malicious code. My conclusions are based on:

- Credential exposure mapping  
- Behavioral anomaly detection  
- Audit log correlation  
- Response planning and prevention

> “As a SOC analyst candidate, I don’t just monitor dashboards — I try to investigate, validate, and defend using structured reasoning and public intelligence.”

---

## 9. References

See [`references.md`](./references.md) for full source list.
