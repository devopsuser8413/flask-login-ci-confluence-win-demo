# 🚀 Software Installation and Jenkins CI/CD Setup Guide

A complete step-by-step guide to set up Java, Python, Jenkins, and a Jenkins CI/CD pipeline that automates testing and publishes reports to Confluence.

---

## 🧩 1. Software Installation

### ☕ 1.1 Install Java

1. **Download Java JDK** (Oracle or OpenJDK):  
   🔗 [https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/)
2. **Install Java** following on-screen instructions.
3. **Verify installation:**
   ```bash
   java -version
Set JAVA_HOME in environment variables.

🐍 1.2 Install Python
Download Python 3.x:
🔗 https://www.python.org/downloads/

During installation, select ✅ “Add Python to PATH”.

Verify installation:

bash
Copy code
python --version
🧰 1.3 Install Jenkins
Download Jenkins LTS:
🔗 https://www.jenkins.io/download/

Run Jenkins as a Windows service or via command line.

Access Jenkins UI at:
👉 http://localhost:8080

Unlock Jenkins using:

makefile
Copy code
C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword
⚙️ 2. Jenkins UI and Plugin Setup
Login to Jenkins UI.

Navigate to Manage Jenkins → Manage Plugins.

Install the following plugins:

🔹 GitHub Plugin

🔹 Email Extension Plugin

🔹 Pipeline Plugin

🔹 Python Plugin

🔹 Confluence Publisher Plugin

Restart Jenkins after installation.

🔑 3. Jenkins Credentials Setup
Go to Manage Jenkins → Credentials → Global credentials.

Add the following credentials:

🧭 GitHub Credentials
Field	Value
ID	github-credentials
Username	xxxxxxxxx1234@gmail.com
Password / Token	xxxxxxxxxxxxxxxxxxxxxxxxxxx

✉️ SMTP (Email) Credentials
Credential ID	Description	Secret
smtp-host	SMTP server host	smtp.gmail.com
smtp-user	SMTP sender email	xxxxxxxxxx@gmail.com
smtp-pass	SMTP password or app password	xxxxxxxxxxxx

📘 Confluence Credentials
Credential ID	Description	Secret
confluence-user	Confluence username	xxxxxxxxxx@gmail.com
confluence-token	API token	xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
confluence-base	Base URL	https://xxxxxxxxxxxxxxx.atlassian.net/wiki

🔐 4. Generate API Tokens
🧭 4.1 GitHub Personal Access Token
Login to GitHub.

Navigate to Settings → Developer Settings → Personal Access Tokens → Tokens (classic).

Click Generate new token, set:

Expiration date

Permissions: repo, workflow

Copy and store the token securely.

📘 4.2 Confluence API Token
Login to Atlassian account:
🔗 https://id.atlassian.com/manage/api-tokens

Click “Create API Token”.

Label it (e.g., Jenkins-Confluence).

Copy and store the token securely.

🧾 5. Jenkinsfile Script Creation
In your GitHub repository, create a file named Jenkinsfile.

Define the following pipeline stages:

🧩 Checkout code from GitHub

🐍 Install Python dependencies

⚙️ Create virtual environment

🧪 Run tests (e.g., Pytest/Selenium)

📄 Generate HTML report

✉️ Send report via email

📘 Publish report to Confluence

Example:

groovy
Copy code
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/your-repo.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'python -m venv .venv'
                sh '.venv/bin/pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh '.venv/bin/pytest --html=report.html'
            }
        }
        stage('Send Email Report') {
            steps {
                sh '.venv/bin/python send_report_email.py'
            }
        }
        stage('Publish to Confluence') {
            steps {
                sh '.venv/bin/python publish_to_confluence.py'
            }
        }
    }
}
📁 6. Pipeline Dependency Files
Ensure these files are added in your repository root:

Copy code
requirements.txt
check_api_token.py
publish_to_confluence.py
send_report_email.py
🔄 7. CI/CD Pipeline Workflow
Developer pushes code to GitHub.

Jenkins automatically triggers the build.

The pipeline executes:

✅ Checkout code

✅ Install dependencies

✅ Run test suite

✅ Generate HTML test report

✅ Send email notification

✅ Publish results to Confluence

Jenkins marks the build as:

🟢 SUCCESS — All tests passed

🔴 FAILURE — Tests failed or publish error

