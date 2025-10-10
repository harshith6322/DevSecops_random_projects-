
---

# 🐧 How to Create a Linux Server (EC2 Instance) in AWS using a Key Pair

## 📘 Overview

Amazon EC2 (Elastic Compute Cloud) allows you to launch and manage virtual servers in the cloud.
You can use a **key pair** to securely connect (via SSH) to your Linux instance.

---

## 🧩 Prerequisites

* An **AWS Account**
* Basic understanding of AWS Console
* Installed **SSH client** (on macOS/Linux) or **PuTTY** (on Windows)

---

## ⚙️ Step-by-Step Setup

### **1. Login to AWS Management Console**

* Go to [https://console.aws.amazon.com](https://console.aws.amazon.com)
* Search for **EC2** in the top search bar.
* Click **EC2** → **Instances → Launch Instance**

---

### **2. Configure Basic Details**

* **Name:** `my-linux-server`
* **AMI (Amazon Machine Image):**
  Select **Amazon Linux 2023 (Free Tier Eligible)**
  Example: `Amazon Linux 2023 AMI`

---

### **3. Choose Instance Type**

* For free tier: `t2.micro` (1 vCPU, 1 GB RAM)

---

### **4. Create / Select Key Pair**

Key pairs are used for SSH authentication.

#### 🔹 If you don’t have one:

1. Click **Create new key pair**
2. **Name:** `my-keypair`
3. **Key pair type:** `RSA`
4. **Private key format:**

   * `.pem` (for Linux/macOS)
   * `.ppk` (for Windows PuTTY)
5. Click **Create key pair**
6. The key file (`my-keypair.pem`) will automatically download.

> ⚠️ **Keep this file safe!** You cannot download it again.

#### 🔹 If you already have one:

* Select the existing key pair from the dropdown.

---

### **5. Configure Network Settings**

* **VPC:** default
* **Subnet:** choose any availability zone (e.g., `us-east-1a`)
* **Auto-assign Public IP:** Enable
* **Firewall (Security Group):**

  * Create a new one or select existing
  * Add rule:

    ```
    Type: SSH
    Protocol: TCP
    Port Range: 22
    Source: My IP
    ```

---

### **6. Configure Storage**

* Default: **8 GB gp3 (EBS)**
  You can increase if needed.

---

### **7. Launch the Instance**

* Review configuration
* Click **Launch Instance**
* Wait until **Instance State = Running**

---

## 🧠 Verify the Instance

* In the EC2 Dashboard → Instances
* You’ll see:

  * Instance ID
  * Public IPv4 address
  * Instance state: `running`

---

## 🔑 Connect to the Instance (SSH)

### **1. Set Permissions (Linux/macOS)**

```bash
chmod 400 my-keypair.pem
```

### **2. Connect via SSH**

```bash
ssh -i my-keypair.pem ec2-user@<public-ip>
```

> Replace `<public-ip>` with your instance’s public IPv4 address.

### Example:

```bash
ssh -i my-keypair.pem ec2-user@54.210.33.18
```

---

## 🧭 Common SSH Issues

| Issue                         | Cause                  | Solution                                    |
| ----------------------------- | ---------------------- | ------------------------------------------- |
| Permission denied (publickey) | Wrong key file or user | Use correct `.pem` file and `ec2-user`      |
| Connection timed out          | Security group issue   | Ensure port `22` is open                    |
| Host not found                | Wrong IP               | Use the **Public IPv4 DNS/IP** from console |

---

## 🧹 Optional: Update Server Packages

Once connected:

```bash
sudo dnf update -y
```

---

## 🧰 Optional: Install Common Tools

```bash
sudo dnf install git -y
sudo dnf install nginx -y
```

To start NGINX:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## ✅ Summary

| Step | Action                             |
| ---- | ---------------------------------- |
| 1    | Choose Amazon Linux AMI            |
| 2    | Select instance type (t2.micro)    |
| 3    | Create/Select key pair             |
| 4    | Configure network & security group |
| 5    | Launch instance                    |
| 6    | Connect using SSH                  |
| 7    | Manage and update server           |

---
## 📸 Screenshot 
<img width="629" height="796" alt="Screenshot 2025-10-10 151641" src="https://github.com/user-attachments/assets/3e00cf7a-174a-4fba-af10-ba166f9d4f37" />


---

## 📎 Notes

* Default username for **Amazon Linux** → `ec2-user`
* Default username for **Ubuntu** → `ubuntu`
* Key pair file must have **400 permissions** on Linux/macOS.
* Always restrict SSH access to your IP for security.

---
