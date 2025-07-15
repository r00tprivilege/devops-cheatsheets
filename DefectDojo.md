```markdown
# 🧩 DefectDojo Essentials for DevSecOps Engineers

> A comprehensive guide to using DefectDojo for vulnerability management, including automation and integration with core DevSecOps tools.

---

## 🛠️ What is DefectDojo?

**DefectDojo** is an open-source application vulnerability management platform designed for DevSecOps workflows. It centralizes findings from static (SAST), dynamic (DAST), dependency (SCA), and runtime scans, helping you:

- Track vulnerabilities over time
- Normalize and deduplicate findings
- Trigger remediation workflows
- Generate metrics and reports

---

## 🚀 Installation (Quick Docker Setup)

```bash
git clone https://github.com/DefectDojo/django-DefectDojo
cd django-DefectDojo
docker compose up
```

Access the UI at: `http://localhost:8000`  
Default credentials: `admin / admin123`

---

## 🧪 Core Concepts

| Concept     | Description                                  |
|-------------|----------------------------------------------|
| **Product** | Your project/app; root context for findings  |
| **Engagement** | A time-boxed scan or assessment           |
| **Test**    | Actual execution of a scan tool              |
| **Finding** | Vulnerability or issue reported              |

---

## 🔐 Authentication

- Generate API token via UI:  
  `Profile → API Key`

- Save your token:
  ```bash
  export DD_API_TOKEN=<your_token>
  export DD_API_URL=http://localhost:8000
  ```

---

## 🧾 Creating a Product & Engagement

```bash
curl -X POST "$DD_API_URL/api/v2/products/" \
  -H "Authorization: Token $DD_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "SecureApp", "description": "DevSecOps stack"}'
```

```bash
curl -X POST "$DD_API_URL/api/v2/engagements/" \
  -H "Authorization: Token $DD_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name": "Weekly Scan", "product": 1, "status": "In Progress"}'
```
---

# 🧩 DevSecOps Vulnerability Aggregation with DefectDojo

Centralizing results from tools like **Semgrep**, **Trivy**, **SonarQube**, **Checkmarx**, and more into DefectDojo empowers unified tracking, deduplication, and visualization. Below is the essential guidance to implement and scale this pipeline.

---

## 🔗 Supported Tools and Integration Summary

| Tool           | Format     | Method                         | Notes |
|----------------|------------|----------------------------------|-------|
| Dependency-Check | JSON      | Native import or API             | `dependency-check-report.json` |
| SonarQube        | JSON      | Plugin or API                    | Use [Sonar plugin](https://github.com/dependency-check/dependency-check-sonar-plugin) |
| Checkmarx        | XML/CSV   | Plugin or native import          | Optionally normalize categories |
| Semgrep          | JSON      | Native import                    | `semgrep.json` |
| Bandit           | JSON      | Native import                    | `bandit.json` |
| OWASP ZAP        | XML       | Native import                    | `report.xml` |
| Nikto            | XML       | Native import                    | `nikto.xml` |
| Nuclei           | JSON      | Native import                    | `nuclei.json` |
| Trivy            | JSON      | Native import                    | `result.json` |
| Renovate         | JSON      | Generic import (custom parser)   | Convert advisory format if needed |
| Terrascan        | JSON      | Native import                    | `terrascan.json` |
| KICS             | JSON      | Native import                    | `kics.json` |
| Gitleaks         | JSON      | Native import                    | `gitleaks.json` |
| TruffleHog       | JSON      | Native import                    | `trufflehog.json` |
| Tekton           | Custom    | API upload via CI/CD script      | Aggregate logs or scan results |

📎 Sample scan files: [DefectDojo’s GitHub](https://github.com/DefectDojo/sample-scan-files)

---

## 🔧 CI/CD Integration Snippet (Python + DefectDojo API)

```python
import requests

headers = {
    "Authorization": "Token YOUR_API_TOKEN"
}

files = {
    "file": open("semgrep.json", "rb")
}

data = {
    "scan_type": "Semgrep Scan",
    "minimum_severity": "Low",
    "active": "true",
    "verified": "true",
    "scan_date": "2025-07-15",
    "product_name": "MyApp",
    "engagement_name": "CI/CD Pipeline",
    "close_old_findings": "false"
}

response = requests.post("https://defectdojo.example.com/api/v2/import-scan/", headers=headers, files=files, data=data)
print(response.json())
```

📌 Repeat this workflow per tool; automation through Jenkins, GitHub Actions, or Tekton recommended.

---

## 📊 Visualization in DefectDojo

### 🧱 Report Builder

- Drag-and-drop widgets (Findings, Vulnerable Endpoints, SLA summaries)
- Export as **PDF** or **HTML**
- Custom branding and metrics

📖 Guide: [Report Builder](https://docs.defectdojo.com/en/share_your_findings/pro_reports/using_the_report_builder/)

---

### 📈 Dashboard Capabilities

- Severity breakdown across tools
- Vulnerability timelines and trends
- Top assets and endpoints under risk
- Remediation SLA visualization

---

### 🔍 Advanced Reporting & External Dashboards

- Use DefectDojo API to extract metrics
- Integrate with **Grafana**, **Power BI**, or custom reporting
- Example metrics:
  - Vulnerability burn-down over time
  - Findings per scanner tool
  - Time-to-remediate comparisons

---

## 🧠 Recommendations & Best Practices

- 🏷️ Normalize tags across tools: `source=semgrep`, `type=SAST`
- 📦 Use **engagements** like `Daily CI/CD Scan`, `Post-Release Audit`
- 🧹 Enable deduplication to cut down false positives
- 📊 Monitor **IRS (Inherited Risk Score)**, Vulnerability Density
- 🧩 Implement feedback loop into ticketing (e.g., Jira/GitHub Issues)

---

