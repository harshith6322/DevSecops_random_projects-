Perfect 🔥 Harshith! You want to create a **Markdown (MD) project documentation** showing how to set up **dynamic EC2 agents in Jenkins** — step-by-step, including screenshots (like the one you uploaded).

Here’s a **ready-to-use `README.md`** template for your project — clean, professional, and GitHub-ready 👇

---

````markdown
# 🚀 Jenkins Dynamic EC2 Agent Setup (Amazon Linux)

This guide explains how to configure **Jenkins** to automatically launch and terminate **Amazon EC2 instances** as build agents using the **Jenkins EC2 Plugin**.

---

## 🧩 Prerequisites

Before you begin, make sure you have:

- ✅ An AWS account with EC2 access.
- ✅ Jenkins server installed (controller node).
- ✅ An IAM user with EC2 permissions.
- ✅ AWS key pair (.pem) file downloaded.
- ✅ Security group allowing SSH (port 22) and Jenkins (port 8080).

---

## ⚙️ Step 1: Install Jenkins EC2 Plugin

1. Go to **Manage Jenkins → Plugins → Available Plugins**  
2. Search for **Amazon EC2** and install it.  
3. Restart Jenkins after installation.

📸 *Screenshot:*  
![Install EC2 Plugin](images/step1-ec2-plugin-install.png)

---

## ☁️ Step 2: Configure AWS Credentials in Jenkins

1. Navigate to **Manage Jenkins → Credentials → System → Global credentials (unrestricted)**  
2. Click **Add Credentials → Kind: AWS Credentials**  
3. Enter your **Access Key ID** and **Secret Access Key**  
4. Save the credentials.

📸 *Screenshot:*  
![AWS Credentials Configuration](images/step2-aws-credentials.png)

---

## 🏗️ Step 3: Create a New EC2 Cloud in Jenkins

1. Go to **Manage Jenkins → Clouds → Add a new cloud → Amazon EC2**  
2. Enter:
   - **Name:** `Dynamic-ec2`
   - **Use EC2 Instance Metadata for Credentials:** (Optional)
   - **Region:** Choose your region (e.g., `us-east-1`)

📸 *Screenshot:*  
![EC2 Cloud Configuration](images/step3-ec2-cloud-config.png)

---

## 💡 Step 4: Add a New AMI Template

Click **Add** under the AMI section and fill out the details:

| Field | Description | Example |
|--------|--------------|----------|
| **Description** | Label for your EC2 agent | Java-based builds |
| **AMI ID** | Your custom AMI ID | `ami-xxxxxxxx` |
| **Instance Type** | EC2 size | `t3.micro` |
| **Remote FS root** | Jenkins workspace directory | `/home/ec2-user` |
| **Remote user** | SSH username | `ec2-user` |
| **Labels** | Agent labels | `java`, `dynamic-ec2` |
| **Init Script** | Optional setup commands | `yum update -y` |

📸 *Screenshot:*  
![AMI Template Configuration](images/step4-ami-template.png)

---

## 🔑 Step 5: SSH & Host Key Settings

In the **Advanced** section:

- **SSH Username:** `ec2-user`  
- **Host Key Verification Strategy:** `check-new-soft` (recommended for Amazon Linux)  
- **Launch Timeout:** `300` seconds  

📸 *Screenshot:*  
![Host Key Verification](images/step5-ssh-verification.png)

---

## 🪄 Step 6: Init Script (Optional)

If your AMI does **not include Java**, you can add this **init script**:

```bash
#!/bin/bash
set -euo pipefail
set -x

sudo yum update -y
sudo yum install -y java-21-amazon-corretto
java -version
````

📸 *Screenshot:*
![Init Script Configuration](images/step6-init-script.png)

---

## 🧰 Step 7: Verify Instance Creation

Once saved:

1. Trigger a build that uses the `dynamic-ec2` label.
2. Jenkins will automatically:

   * Launch an EC2 instance.
   * Connect via SSH.
   * Run the init script.
   * Register it as an agent.
3. After the build finishes, Jenkins terminates the EC2 instance.

📸 *Screenshot:*
![Instance Launch in Jenkins](images/step7-instance-launch.png)

---

## 🧾 Logs and Troubleshooting

* Check **Manage Jenkins → Nodes → EC2 (Dynamic)** for connection logs.
* Common issues:

  * ❌ Wrong SSH user → use `ec2-user`
  * ❌ Missing Java → install in AMI or via init script
  * ❌ “Cannot check key” → change verification to `check-new-soft`
  * ❌ Init script fails → increase launch timeout to `300s`

📸 *Screenshot:*
![Debug Logs](images/step8-debug-logs.png)

---

## ✅ Final Result

Once everything is set up, Jenkins will dynamically scale your agents:

* Spin up EC2 instances **on demand**.
* Run builds automatically.
* **Terminate** idle agents after completion.

📸 *Screenshot:*
![Final Jenkins Dashboard](images/step9-final-dashboard.png)

---

## 📘 References

* [Jenkins EC2 Plugin Documentation](https://plugins.jenkins.io/ec2/)
* [AWS EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)

---

### 👨‍💻 Author

**Harshith Reddy**
 DevOps Engineer
GitHub: [@harshith6322](https://github.com/harshith6322)

```

---


```
