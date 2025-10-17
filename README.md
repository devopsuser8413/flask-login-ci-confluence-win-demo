# Software Installation and Jenkins CI/CD Setup Guide

## 1. Software Installation
### 1.1 Install Java

1.1.1 Download Java JDK from the official Oracle website or use OpenJDK.
     https://www.oracle.com/java/technologies/downloads/

1.1.2 Install Java following the on-screen instructions.

1.1.3 Verify installation using:
      java -version

1.1.4 Set JAVA_HOME in environment variables.

### 1.2 Install Python

1.2.1 Download Python 3.x from https://www.python.org/downloads/

1.2.2 During installation, select “Add Python to PATH”.

1.2.3 Verify installation using:
      python --version

### 1.3 Install Jenkins

1.3.1 Download Jenkins LTS from https://www.jenkins.io/download/

1.3.2 Run Jenkins as a Windows service or via command line.

1.3.3 Access Jenkins UI at http://localhost:8080

1.3.4 Unlock Jenkins using the initial admin password.
 C:\ProgramData\Jenkins\.jenkins\secrets\initialAdminPassword

## 2. Jenkins UI and Plugin Setup

1. Login to Jenkins UI.

2. Navigate to Manage Jenkins → Manage Plugins.

3. Install the following plugins:
   - GitHub Plugin
   - Email Extension Plugin
   - Pipeline Plugin
   - Python Plugin
   - Confluence Publisher Plugin

4. Restart Jenkins after installation.

## 3. Jenkins Credentials Setup

1. Go to Jenkins → Manage Jenkins → Credentials → Global credentials.

2. Add the following credentials:
   - GitHub credentials (username & personal access token)
	[github-credentials]
    ID:	     : github-credentials
    username : xxxxxxxxx1234@gmail.com
    password : xxxxxxxxxxxxxxxxxxxxxxxxxxx

   - SMTP credentials (username & password)
    [smtp-host]
    secret   : smtp.gmail.com
    ID	: smtp-host

    [smtp-user]
    secret	: xxxxxxxxxx@gmail.com
    ID	: smtp-user

    [smtp-pass]
    secret	: xxxxxxxxxxxxxxxx
    ID	: smtp-pass

   - Confluence credentials (username & API token)
    [confluence-user]
    secret	: xxxxxxxxxxx@gmail.com
    ID	: confluence-user

    [confluence-token]
    secret	: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ID	: confluence-token

    [confluence-base]
    secret	: https://xxxxxxxxxxxxxxx.atlassian.net/wiki
    ID	: confluence-base

## 4. Generate API Tokens

### 4.1 Generate GitHub Personal Access Token

4.1.1 Login to your GitHub account.

4.1.2 Go to Settings → Developer settings → Personal access tokens → Tokens (classic).

4.1.3 Click “Generate new token” → Set expiration and permissions (repo, workflow).

4.1.4 Copy and save the token securely.

### 4.2 Generate Confluence API Token

4.2.1 Login to Atlassian account at https://id.atlassian.com/manage/api-tokens.

4.2.2 Click “Create API Token”.

4.2.3 Give a label (e.g., Jenkins-Confluence).

4.2.4 Copy and save the token securely.

## 5. Jenkinsfile Script Creation

1. In your GitHub repository, create a file named Jenkinsfile.

2. Define the following stages in the Jenkins pipeline:
   - Checkout code from GitHub
   - Install Python dependencies
   - Create Python virtual environment
   - Run Python tests
   - Generate HTML report
   - Send report via email
   - Publish report to Confluence

## 6. CI/CD Pipeline Workflow

Add the following dependency files for the Jenkinsfile pipeline script in the github repository root path.
- requirements.txt
- check_api_token.py
- publish_to_confluence.py
- send_report_email.py


## 7. CI/CD Pipeline Workflow

1. Developer pushes code changes to GitHub repository.

2. Jenkins automatically triggers the pipeline build.

3. The pipeline performs the following steps:
   - Checks out code from GitHub.
   - Installs Python and dependencies.
   - Creates a virtual environment.
   - Executes Selenium/Pytest test cases.
   - Generates an HTML test report.
   - Sends the report to team via email.
   - Publishes the report to a Confluence page.
   
4. Jenkins marks the build as SUCCESS or FAILURE.

