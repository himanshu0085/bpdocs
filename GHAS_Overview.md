# 🚀 GitHub Advanced Security (GHAS) Overview Document
**Owner:** Himanshu Parashar  
**Mentors:** Deepak Gupta / Deepak Chauhan  
**Date:** 18 Nov 2025  

---

# 1. What is GHAS?

GitHub Advanced Security (GHAS) is a suite of built‑in security tools designed to help developers identify, prevent, and remediate vulnerabilities directly inside GitHub. It brings automated code scanning, secret detection, and dependency security into the developer workflow.

---

# 2. Why GHAS?

GHAS improves security by:

- Detecting vulnerabilities early (shift‑left security)
- Reducing reliance on external security tools
- Providing actionable insights directly inside GitHub
- Automatically scanning code, secrets, and dependencies
- Helping maintain compliance and secure SDLC practices

---

# 3. Features of GHAS

### ✔ Code Scanning (CodeQL)
- Performs static analysis to detect vulnerabilities  
- Uses semantic queries to find security flaws  
- Works via CLI, GitHub Actions, or API  

### ✔ Secret Scanning
- Detects hardcoded credentials like API keys, tokens, private keys  
- Alerts developers in real time  
- Supports custom secret patterns  

### ✔ Dependency Management (Dependabot)
- Identifies vulnerable dependencies  
- Automates PRs with updated versions  
- Uses GitHub's advisory vulnerability database  

---

# 4. Components of GHAS

| Component | Description | Key Capabilities |
|----------|-------------|------------------|
| Code Scanning (CodeQL) | Static code analysis engine | Detects vulnerabilities, supports multiple languages |
| Secret Scanning | Detects exposed secrets | Alerts developers, supports custom patterns |
| Dependabot | Dependency vulnerability management | Auto‑fixes vulnerabilities via PRs |

---

# 5. Conclusion

GHAS provides a powerful, integrated set of security features within GitHub. With CodeQL scanning, Secret Scanning, and Dependabot, teams can proactively detect weaknesses, secure codebases, and improve overall security posture—without needing external tools.

---

# 6. References
- https://docs.github.com/en/code-security  
- https://codeql.github.com/docs/  
- https://docs.github.com/en/rest/security-advisories  
- https://docs.github.com/en/actions/security-guides  
