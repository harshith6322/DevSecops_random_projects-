Excellent, Harshith 👏 — your Markdown is 99% perfect!
The only small issues are:

* Code block fences (` ``` ` and ` `) were mismatched.
* Some extra quote marks and spacing needed cleanup.
* The nested code block (for the init script) inside a markdown code block needed correct escaping.

Here’s the **fixed and fully valid Markdown version** ✅ — you can paste this directly into your GitHub `README.md`:



# 🚀 Jenkins Dynamic EC2 Agent Setup (Amazon Linux)

This guide explains how to configure **Jenkins** to automatically launch and terminate **Amazon EC2 instances** as build agents using the **Jenkins EC2 Plugin**.


## 🧩 Prerequisites

Before you begin, make sure you have:

- ✅ An AWS account with EC2 access.
- ✅ Jenkins server installed (controller node).
- ✅ An IAM user with EC2 permissions.
- ✅ AWS key pair (.pem) file downloaded.
- ✅ Security group allowing SSH (port 22) and Jenkins (port 8080).



## ⚙️ Step 1: Install Jenkins EC2 Plugin

1. Go to **Manage Jenkins → Plugins → Available Plugins**  
2. Search for **Amazon EC2** and install it.  
3. Restart Jenkins after installation.



## ☁️ Step 2: Configure AWS Credentials in Jenkins

1. Navigate to **Manage Jenkins → Credentials → System → Global credentials (unrestricted)**  
2. Click **Add Credentials → Kind: AWS Credentials**  
3. Enter your **Access Key ID** and **Secret Access Key**  
4. Save the credentials.

📸 *Screenshot:*  
<img width="1692" height="368" alt="Screenshot 2025-10-07 215232" src="https://github.com/user-attachments/assets/15f95cf8-bcab-43b7-9222-81b9b5da1281" />


## 🏗️ Step 3: Create a New EC2 Cloud in Jenkins

1. Go to **Manage Jenkins → Clouds → Add a new cloud → Amazon EC2**  
2. Enter:
   - **Name:** `Dynamic-ec2`
   - **Use EC2 Instance Metadata for Credentials:** (Optional)
   - **Region:** Choose your region (e.g., `us-east-1`)

📸 *Screenshot:*  
<img width="1143" height="609" alt="Screenshot 2025-10-07 215150" src="https://github.com/user-attachments/assets/41753ae1-1f3b-46ef-b353-4984ff7a06eb" />



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

<img width="1328" height="720" alt="Screenshot 2025-10-07 224202" src="https://github.com/user-attachments/assets/f631b898-6af6-4dbc-bdc3-876e0784829e" />



## 🔑 Step 5: SSH & Host Key Settings

In the **Advanced** section:

- **SSH Username:** `ec2-user`  
- **Host Key Verification Strategy:** `check-new-soft` (recommended for Amazon Linux)  
- **Launch Timeout:** `300` seconds  

📸 *Screenshot:*  
![Host Key Verification](images/step5-ssh-verification.png)



## 🪄 Step 6: Init Script (Optional)

If your AMI does **not include Java**, you can add this **init script**:


#!/bin/bash
set -euo pipefail
set -x

sudo yum update -y
sudo yum install -y java-21-amazon-corretto
java -version




## 🧰 Step 7: Verify Instance Creation

Once saved:

1. Trigger a build that uses the `dynamic-ec2` label.
2. Jenkins will automatically:

   * Launch an EC2 instance.
   * Connect via SSH.
   * Run the init script.
   * Register it as an agent.
3. After the build finishes, Jenkins terminates the EC2 instance.

📸 *Screenshot:* <img width="1087" height="500" alt="Screenshot 2025-10-07 231522" src="https://github.com/user-attachments/assets/12794050-cd4f-45e7-a2c1-a2b23ff872f5" /> 
---

## 🧾 Logs and Troubleshooting

* Check **Manage Jenkins → Nodes → EC2 (Dynamic)** for connection logs.
* Common issues:

  * ❌ Wrong SSH user → use `ec2-user`
  * ❌ Missing Java → install in AMI or via init script
  * ❌ “Cannot check key” → change verification to `check-new-soft`
  * ❌ Init script fails → increase launch timeout to `300s`



---

## ✅ Final Result

Once everything is set up, Jenkins will dynamically scale your agents:

* Spin up EC2 instances **on demand**.
* Run builds automatically.
* **Terminate** idle agents after completion.

📸 *Screenshot:*
<img width="626" height="770" alt="Screenshot 2025-10-07 231645" src="https://github.com/user-attachments/assets/b6d4fe0a-8419-491a-b4bf-581e1d7dc4e3" />



---

## 📘 References

* [Jenkins EC2 Plugin Documentation](https://plugins.jenkins.io/ec2/)
* [AWS EC2 User Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)



### 👨‍💻 Author

**Harshith Reddy**
DevOps Engineer
GitHub: [@harshith6322](https://github.com/harshith6322)



---
