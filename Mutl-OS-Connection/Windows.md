
---

# 🪟 How to Create a Windows Server in AWS EC2 using a Key Pair

## 📘 Overview

Amazon EC2 (Elastic Compute Cloud) allows you to launch Windows servers in the cloud.
You connect to it securely using **RDP (Remote Desktop Protocol)** and a **key pair** to decrypt the Administrator password.

---

## 🧩 Prerequisites

* AWS account
* Basic AWS Console knowledge
* RDP client (built-in on Windows)

---

## ⚙️ Step-by-Step Setup

### **1. Log in to AWS Console**

* Go to 👉 [https://console.aws.amazon.com](https://console.aws.amazon.com)
* In the search bar, type **EC2** and open the **EC2 Dashboard**
* Click **Instances → Launch Instances**

---

### **2. Configure Basic Information**

* **Name:** `my-windows-server`
* **AMI (Amazon Machine Image):**

  * Choose **Microsoft Windows Server 2022 Base (Free Tier Eligible)**
    Example: `Microsoft Windows Server 2022 Base`
* Click **Select**

---

### **3. Choose Instance Type**

* Choose: `t2.micro` (Free Tier Eligible)

---

### **4. Create / Select Key Pair**

#### 🔹 If you don’t have a key pair:

1. Click **Create new key pair**
2. **Name:** `my-keypair`
3. **Key pair type:** `RSA`
4. **Private key format:** `.pem`
5. Click **Create key pair**
6. Save the downloaded file `my-keypair.pem` safely.

> ⚠️ You’ll need this `.pem` file later to decrypt your Windows Administrator password.

#### 🔹 If you already have one:

* Select an existing key pair.

---

### **5. Configure Network Settings**

* **VPC:** default
* **Subnet:** choose any availability zone
* **Auto-assign Public IP:** Enable
* **Security Group:**
  Create a new one or select existing and allow:

  ```
  Type: RDP
  Protocol: TCP
  Port Range: 3389
  Source: My IP
  ```

---

### **6. Configure Storage**

* Default: 30 GB gp3 (recommended for Windows)

---

### **7. Launch the Instance**

* Review configuration
* Click **Launch Instance**
* Wait until the instance status = **Running**

---

## 🧭 Step 8: Retrieve the Windows Administrator Password

Once your instance is running:

1. Select your instance in **EC2 → Instances**

2. Click **Connect**

3. Choose the **RDP Client** tab

4. Click **Get Password**

   It will ask for your **key pair file**.

5. Upload your `.pem` key file (e.g., `my-keypair.pem`)

6. Click **Decrypt Password**

You’ll see:

* **Username:** Administrator
* **Password:** (decrypted string)

> Copy this password — you’ll need it to log in via RDP.

---

## 🖥️ Step 9: Connect Using Remote Desktop (RDP)

### **Option 1: Using AWS Console**

* In the **Connect** section, click **Download Remote Desktop File**
* Open the downloaded `.rdp` file
* When prompted:

  * **Username:** `Administrator`
  * **Password:** paste the one you decrypted earlier

### **Option 2: Using Windows RDP App**

1. Press `Windows + R`, type:

   ```
   mstsc
   ```

   → Press Enter
2. In **Computer**, paste your instance’s **Public IPv4 address**
3. Click **Connect**
4. Enter:

   * Username: `Administrator`
   * Password: (decrypted one)
5. Click **OK**

You are now connected to your **Windows Server EC2 instance** 🎉

---

## 🧹 Optional: Configure Your Server

Once inside the server:

* Update Windows:

  * Open **Windows Update** → Install updates
* Rename computer if needed
* Configure IIS (Web Server):

  ```powershell
  Install-WindowsFeature -name Web-Server -IncludeManagementTools
  ```

---

## 🧠 Troubleshooting

| Issue                  | Cause                  | Solution                                       |
| ---------------------- | ---------------------- | ---------------------------------------------- |
| Cannot connect via RDP | Port 3389 not open     | Check Security Group inbound rule              |
| Wrong password         | Wrong PEM file used    | Use the correct `.pem` key to decrypt password |
| Connection timeout     | Public IP not assigned | Enable Auto-assign Public IP when launching    |

---

## ✅ Summary

| Step | Action                                   |
| ---- | ---------------------------------------- |
| 1    | Choose Windows Server AMI                |
| 2    | Select instance type (t2.micro)          |
| 3    | Create or select key pair                |
| 4    | Enable RDP (port 3389)                   |
| 5    | Launch instance                          |
| 6    | Decrypt Administrator password using PEM |
| 7    | Connect via Remote Desktop               |

---

## 📎 Notes

* Default username: **Administrator**
* Key pair (.pem) is used only once to decrypt the password.
* Keep your `.pem` file and password secure.
* Use **Elastic IP** for a permanent public IP if you stop/start the instance often.

---

Would you like me to make a **combined version (Linux + Windows EC2 setup)** in one Markdown file with a side-by-side comparison table for your DevOps notes repo? It’s very handy for interview prep and practical revision.
