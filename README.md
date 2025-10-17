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
   ```
4. **Set `JAVA_HOME`** in environment variables.

##### Option A — Using GUI

1. Press **Windows + R**, type `sysdm.cpl`, and press **Enter**.  
2. Go to **Advanced** → click **Environment Variables...**  
3. Under **System variables**, click **New**:  
   - **Variable name:** `JAVA_HOME`  
   - **Variable value:** `C:\Program Files\Java\jdk-17`  
4. Click **OK**, then edit the **Path** variable → click **New**, and add:  
%JAVA_HOME%\bin
5. Click **OK** on all dialogs to save and apply changes.

##### Option B — Using PowerShell (Admin)

Run the following commands in an **Administrator PowerShell** window:
```powershell
setx JAVA_HOME "C:\Program Files\Java\jdk-17"
setx PATH "%PATH%;%JAVA_HOME%\bin"

✅ Verify

Run the following commands to confirm setup:

   ```bash
   echo %JAVA_HOME%
   java -version
   ```
---

### 1.2 Install Python

#### 1.2.1 Download Python
Download **Python 3.x** from the official website:  
👉 [https://www.python.org/downloads/](https://www.python.org/downloads/)

During installation, make sure to **select** the option **“Add Python to PATH”**.

---

#### 1.2.2 Option A — Add Python to PATH (If not selected during installation)

1. Press **Windows + R**, type `sysdm.cpl`, and press **Enter**.  
2. Go to **Advanced → Environment Variables...**  
3. Under **System variables**, select **Path → Edit**.  
4. Click **New** and add the following Python installation paths (example below):

C:\Users<YourUsername>\AppData\Local\Programs\Python\Python311
C:\Users<YourUsername>\AppData\Local\Programs\Python\Python311\Scripts\

5. Click **OK** on all dialogs to save changes.

---

#### 1.2.3 Option B — Using PowerShell (Admin)

Run the following command in **PowerShell (Administrator)**:

```powershell
setx PATH "%PATH%;C:\Users\<YourUsername>\AppData\Local\Programs\Python\Python311;C:\Users\<YourUsername>\AppData\Local\Programs\Python\Python311\Scripts" /M

✅ Verify Installation

Run the following command to confirm setup:

python --version

---

### 1.3 Install Jenkins

#### 1.3.1 Download Jenkins LTS
Download the latest **Jenkins LTS** installer for Windows from the official website:  
🔗 [https://www.jenkins.io/download/](https://www.jenkins.io/download/)

---

### 1.3 Install Jenkins

#### 1.3.1 Download Jenkins LTS
Download the latest **Jenkins LTS** installer for Windows from the official website:  
🔗 [https://www.jenkins.io/download/](https://www.jenkins.io/download/)

---

#### 1.3.2 Install Jenkins
You can install Jenkins in one of two ways:

- **Option A — As a Windows Service:**  
  Run the installer and choose the option to **install Jenkins as a service**.

- **Option B — Via Command Line:**  
  Run Jenkins manually using the `.war` file:  
  ```bash
  java -jar jenkins.war

Once installed, Jenkins will typically start automatically.

1.3.3 Access Jenkins UI

After installation, open your browser and go to:
👉 http://localhost:8080

1.3.4 Unlock Jenkins

To unlock Jenkins for the first time, retrieve the admin password from:

C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword


Copy the password and paste it into the Jenkins setup wizard.

1.3.5 🧰 Add Jenkins to System PATH (Windows)

If Jenkins is not recognized in the command prompt, you can manually add it to your PATH.

1. Press Windows + R, type sysdm.cpl, and press Enter.

2. Go to Advanced → Environment Variables...

3. Under System variables, select Path → Edit.

4. Click New and add the Jenkins installation path (for example):

C:\Program Files\Jenkins

5. Click OK to save and close all dialogs.

Option B — PowerShell (Admin)

Run the following command to add Jenkins to the PATH:

setx PATH "%PATH%;C:\Program Files\Jenkins" /M

✅ Verify Jenkins PATH Setup

To verify Jenkins is accessible from the command line, open Command Prompt or PowerShell and run:

jenkins --version
---

## ⚙️ 2. Jenkins UI and Plugin Setup

1. Login to **Jenkins UI**.
2. Navigate to **Manage Jenkins → Manage Plugins**.
3. **Install the following plugins:**
   - 🔹 GitHub Plugin  
   - 🔹 Email Extension Plugin  
   - 🔹 Pipeline Plugin  
   - 🔹 Python Plugin  
   - 🔹 Confluence Publisher Plugin
4. Restart Jenkins after installation.

---

## 🔑 3. Jenkins Credentials Setup

1. Go to **Manage Jenkins → Credentials → Global credentials**.
2. Add the following credentials:

### 🧭 GitHub Credentials
| Field | Value |
|-------|--------|
| **ID** | `github-credentials-demo` |
| **Username** | `xxxxxxxxx1234@gmail.com` |
| **Password / Token** | `xxxxxxxxxxxxxxxxxxxxxxxxxxx` |

---

### ✉️ SMTP (Email) Credentials
| Credential ID | Description | Secret |
|----------------|--------------|---------|
| `smtp-host-demo` | SMTP server host | `smtp.gmail.com` |
| `smtp-user-demo` | SMTP sender email | `xxxxxxxxxx@gmail.com` |
| `smtp-pass-demo` | SMTP password or app password | `xxxxxxxxxxxx` |

---

### 📘 Confluence Credentials
| Credential ID | Description | Secret |
|----------------|-------------|---------|
| `confluence-user-demo` | Confluence username | `xxxxxxxxxx@gmail.com` |
| `confluence-token-demo` | API token | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| `confluence-base-demo` | Base URL | `https://xxxxxxxxxxxxxxx.atlassian.net/wiki` |

---

## 🔐 4. Generate API Tokens

### 🧭 4.1 GitHub Personal Access Token

1. Login to **GitHub**.  
2. Navigate to **Settings → Developer Settings → Personal Access Tokens → Tokens (classic)**.  
3. Click **Generate new token**, set:
   - Expiration date  
   - Permissions: `repo`, `workflow`  
4. Copy and store the token securely.

---

### 📘 4.2 Confluence API Token

1. Login to Atlassian account:  
   🔗 [https://id.atlassian.com/manage/api-tokens](https://id.atlassian.com/manage/api-tokens)
2. Click **“Create API Token”**.  
3. Label it (e.g., `Jenkins-Confluence`).  
4. Copy and store the token securely.

---

## 🧾 5. Jenkinsfile Script Creation

1. In your GitHub repository, create a file named **`Jenkinsfile`**.
2. Define the following **pipeline stages**:
   - 🧩 Checkout code from GitHub  
   - 🐍 Install Python dependencies  
   - ⚙️ Create virtual environment  
   - 🧪 Run tests (e.g., Pytest/Selenium)  
   - 📄 Generate HTML report  
   - ✉️ Send report via email  
   - 📘 Publish report to Confluence  

Example:
```groovy
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
```

---

## 📁 6. Pipeline Dependency Files

Ensure these files are added in your **repository root**:

```
requirements.txt
check_api_token.py
publish_to_confluence.py
send_report_email.py
```

---

## 🔄 7. CI/CD Pipeline Workflow

1. Developer **pushes code** to GitHub.  
2. Jenkins **automatically triggers** the build.  
3. The pipeline executes:
   - ✅ Checkout code  
   - ✅ Install dependencies  
   - ✅ Run test suite  
   - ✅ Generate HTML test report  
   - ✅ Send email notification  
   - ✅ Publish results to Confluence  
4. Jenkins marks the build as:
   - 🟢 **SUCCESS** — All tests passed  
   - 🔴 **FAILURE** — Tests failed or publish error  

---

## 🏁 End of Guide

> 💡 **Tip:** You can extend this setup to include static code analysis, Docker builds, and deployment steps for full CI/CD automation.

---

📧 **Maintainer:** [Your Name]  
💼 **Team / Department:** [Your Department Name]  
🏢 **Organization:** [Your Company Name]
