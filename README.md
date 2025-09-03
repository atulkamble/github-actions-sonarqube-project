Here’s a **complete GitHub Actions + SonarQube project setup** 🚀.
This will let you run SonarQube locally with Docker Compose, analyze your code during CI/CD, and upload the results to SonarQube.

---

# 🔹 **SonarQube with GitHub Actions Project**

## 📂 Project Structure

```
sonarqube-github-actions/
├── .github/
│   └── workflows/
│       └── ci.yml
├── src/
│   └── app.py
├── tests/
│   └── test_app.py
├── sonar-project.properties
├── docker-compose.yml
├── requirements.txt
└── README.md
```

---

## ⚙️ **1. Docker Compose – SonarQube & PostgreSQL**

```yaml
version: "3.8"

services:
  postgres:
    image: postgres:15
    container_name: pg-sonar
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonarpass
      POSTGRES_DB: sonarqube
    volumes:
      - pg_data:/var/lib/postgresql/data
    restart: unless-stopped

  sonarqube:
    image: sonarqube:community
    container_name: sonarqube
    depends_on:
      - postgres
    ports:
      - "9000:9000"
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://postgres:5432/sonarqube
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonarpass
    restart: unless-stopped

volumes:
  pg_data:
```

Start SonarQube:

```bash
docker-compose up -d
```

Open: [http://localhost:9000](http://localhost:9000)
(Default login: `admin` / `admin`)

---

## 📄 **2. Sonar Project Config**

`sonar-project.properties`

```properties
sonar.projectKey=github-actions-sonar
sonar.projectName=GitHub Actions Sonar Project
sonar.projectVersion=1.0
sonar.sources=./src
sonar.tests=./tests
sonar.python.coverage.reportPaths=coverage.xml
sonar.sourceEncoding=UTF-8
```

---

## 🐍 **3. Example App**

`src/app.py`

```python
def add(x, y):
    return x + y

if __name__ == "__main__":
    print(add(2, 3))
```

`tests/test_app.py`

```python
from src.app import add

def test_add():
    assert add(2, 3) == 5
```

`requirements.txt`

```
pytest
pytest-cov
```

---

## 🤖 **4. GitHub Actions Workflow**

`.github/workflows/ci.yml`

```yaml
name: CI with SonarQube

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Run Tests with Coverage
        run: |
          pytest --cov=src --cov-report=xml

      - name: SonarQube Scan
        uses: sonarsource/sonarqube-scan-action@v3
        with:
          projectBaseDir: .
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: http://<your-sonarqube-server>:9000
```

---

## 🔑 **5. Setup Secrets in GitHub**

1. Go to **GitHub repo → Settings → Secrets and variables → Actions**.
2. Add:

   * `SONAR_TOKEN` → Generate from **SonarQube → My Account → Security → Tokens**.

---

## ✅ **6. Run the Workflow**

* Push code → GitHub Actions runs tests → Uploads coverage & code analysis → View results in **SonarQube UI**.

---

📌 You now have:

* Local **SonarQube + PostgreSQL** with Docker Compose
* GitHub Actions pipeline with **pytest + coverage + SonarQube scan**
* Secure integration with **GitHub Secrets**

---

👉 Do you want me to also create a **ready-to-use GitHub repo template** (with README badges, diagrams, and sample issues) so you can push directly?
