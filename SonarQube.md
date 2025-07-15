```markdown
# 📊 SonarQube Essentials for DevSecOps Engineers

> Core CLI/API commands and configurations to streamline code quality checks, vulnerability analysis, and CI/CD integration.

---

## 🚀 Setup & Authentication

- **Generate Authentication Token (via UI)**
  - Go to: `My Account > Security`
  - Create a token for CI usage

- **Use token for CLI or API**
  ```bash
  export SONAR_TOKEN=<your-token>
  ```

- **Server health check**
  ```bash
  curl -u $SONAR_TOKEN: https://sonarqube.example.com/api/system/health
  ```

---

## 🔍 Project Scanning with SonarScanner

- **Run analysis for a generic project**
  ```bash
  sonar-scanner \
    -Dsonar.projectKey=secure-app \
    -Dsonar.sources=. \
    -Dsonar.host.url=https://sonarqube.example.com \
    -Dsonar.login=$SONAR_TOKEN
  ```

- **Run with language-specific options (e.g., Java)**
  ```bash
  sonar-scanner \
    -Dsonar.java.binaries=target/classes \
    -Dsonar.projectKey=secure-java \
    -Dsonar.sources=src/main/java \
    -Dsonar.host.url=https://sonarqube.example.com \
    -Dsonar.login=$SONAR_TOKEN
  ```

---

## 🔄 CI/CD Integration Example (GitHub Actions)

```yaml
jobs:
  sonar-analysis:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Run SonarQube scan
        run: |
          sonar-scanner \
            -Dsonar.projectKey=secure-app \
            -Dsonar.sources=. \
            -Dsonar.host.url=https://sonarqube.example.com \
            -Dsonar.login=${{ secrets.SONAR_TOKEN }}
```

---

## 📊 Reporting & Querying API

- **Check quality gate status**
  ```bash
  curl -u $SONAR_TOKEN: \
    https://sonarqube.example.com/api/qualitygates/project_status?projectKey=secure-app
  ```

- **Get latest scan results**
  ```bash
  curl -u $SONAR_TOKEN: \
    https://sonarqube.example.com/api/issues/search?projectKeys=secure-app
  ```

- **List vulnerabilities or security hotspots**
  ```bash
  curl -u $SONAR_TOKEN: \
    "https://sonarqube.example.com/api/issues/search?componentKeys=secure-app&types=VULNERABILITY"
  ```

---

## 🛡️ DevSecOps Best Practices

- Enforce pull request gating with quality thresholds
- Tag SonarQube reports to build IDs for audit trails
- Treat "Security Hotspots" as red flags during code review
- Integrate SonarLint into local IDEs for early detection
