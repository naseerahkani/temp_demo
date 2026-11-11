# AAP Demo Playbooks Library

This repository contains a custom library of mock Ansible playbooks designed to demonstrate the core capabilities of **Ansible Automation Platform (AAP)**. 

These playbooks do not execute live infrastructure changes. Instead, they utilize the Ansible `debug` module to simulate common IT workflows such as onboarding, security provisioning, and infrastructure deployment. This makes them perfectly safe for demonstrating AAP features like **Projects, Job Templates, Surveys, Workflows, and Notifications** without requiring external cloud accounts or live target servers.

---

## 🚀 How to Connect this Repository to AAP

To use these playbooks in your Ansible Automation Platform environment, follow these steps to sync this repository as a **Project**:

1. **Log into AAP** and navigate to **Projects** under the *Resources* menu.
2. Click **Add** to create a new project.
3. Fill in the required details:
   * **Name**: `AAP Demo Playbooks Library` (or your preferred name)
   * **Organization**: Select your target organization (e.g., `Default`)
   * **Execution Environment**: Leave blank or select a default supported EE.
   * **Source Control Credential Type**: Select `Git`
4. **Source Control URL**: Paste the URL of this GitHub repository (e.g., `https://github.com/naseerahkani/temp_demo.git`).
5. Click **Save**. 
6. AAP will automatically trigger a **Sync** job to pull these playbooks into the platform. Once the sync status turns green, you can begin creating Job Templates for the scripts below.

---

## 📜 Playbook Directory

### 1. Create GitLab Project
* **File:** (Reference the exact filename, e.g., `create_gitlab_project.yml`)
* **Function:** Simulates the creation of a GitLab repository for a new development project and assigns it to a specific developer.
* **Purpose:** Excellent for demonstrating basic AAP Job Templates and Survey inputs for user onboarding or self-service IT.
* **Execution:** Runs locally (`hosts: localhost`).
* **Required Parameters (Extra Variables):**
  * `project_name` (String): The name of the repository to be created.
  * `developer_name` (String): The name of the user being granted ownership.

### 2. Grant Database Access
* **File:** (Reference the exact filename, e.g., `grant_db_access.yml`)
* **Function:** Simulates a database administration task that grants a specific developer access to a required database.
* **Purpose:** Designed to be chained with the "Create GitLab Project" playbook inside an **AAP Workflow Job Template**, demonstrating how variables pass downstream between different jobs.
* **Execution:** Runs locally (`hosts: localhost`).
* **Required Parameters (Extra Variables):**
  * `developer_name` (String): The name of the user receiving database access.

### 3. Provision Staging Environment
* **File:** (Reference the exact filename, e.g., `provision_staging.yml`)
* **Function:** Simulates a multi-step infrastructure deployment involving database storage sizing and AWS load balancer configuration.
* **Purpose:** Demonstrates more complex job execution. The log output references "encrypted Vault credentials," making it an ideal talking point to explain **AAP Credential Types** and external secret management (like HashiCorp Vault).
* **Execution:** Runs locally using explicit local connection plugins.
* **Required Parameters (Extra Variables):**
  * `disk_size` (Integer): The size (in GB) of the database disk.
  * `target_servers` (String/List): The target IP, hostname, or inventory group for the AWS load balancer configuration.

---

## 💡 Demo Tips

* **Surveys:** Create a Survey on the Job Templates for these playbooks to prompt the audience for the required parameters (`project_name`, `disk_size`, etc.) before the job runs. This shows the self-service capability of AAP.
* **Workflows:** Combine playbook 1 and 2 into a single Workflow Job Template to show how AAP handles orchestration across different IT domains.
* **Notifications:** Deliberately fail the Staging Environment playbook (e.g., by passing an invalid variable) to demonstrate AAP's native Slack/Teams notification integrations.
