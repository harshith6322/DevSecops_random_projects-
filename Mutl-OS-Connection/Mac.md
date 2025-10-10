Excellent ✅ — here’s the **complete Markdown (`.md`) note** for creating a **macOS server (Mac instance) on AWS EC2 using a key pair**.
This will help you set up, launch, and connect to a **macOS EC2 instance** using **SSH** or **Apple Remote Desktop (ARD)**.

---

# 🍎 How to Create a macOS Server in AWS EC2 using a Key Pair

## 📘 Overview

AWS offers **macOS instances** that run on **dedicated Apple hardware (Mac mini)**.
These instances are ideal for building, testing, and signing Apple applications (iOS, macOS, watchOS, etc.).
You connect to the instance using a **key pair (SSH)** or **Apple Remote Desktop (RDP/ARD)**.

---

## 🧩 Prerequisites

* AWS account with EC2 access
* You are in an **AWS Region that supports macOS** (e.g., `us-east-1`, `us-west-2`)
* **SSH client** (built into macOS/Linux) or **Apple Remote Desktop app**
* Basic AWS Console knowledge

> ⚠️ **Note:** macOS instances run on **dedicated hosts** and are **not Free Tier eligible**.
> You’ll be billed for at least **24 hours** even if you stop the instance earlier.

---

## ⚙️ Step-by-Step Setup

### **1. Log in to AWS Console**

* Go to 👉 [https://console.aws.amazon.com](https://console.aws.amazon.com)
* Search for **EC2** and open the **EC2 Dashboard**
* Click **Instances → Launch Instances**

---

### **2. Configure Basic Information**

* **Name:** `my-macos-server`
* **AMI (Amazon Machine Image):**
  In the list, choose:

  * `macOS Monterey`
  * `macOS Ventura`
  * `macOS Sonoma`

  Example:
  `macOS Ventura 13 (ARM64)`

---

### **3. Choose Instance Type**

* Choose a **mac instance type**, e.g.:

  * `mac1.metal` (Intel-based)
  * `mac2.metal` (Apple M1/M2 chip)

> **Note:** mac2.metal = newer & faster, but slightly higher cost.

---

### **4. Create / Select a Key Pair**

#### 🔹 If you don’t have one:

1. Click **Create new key pair**
2. **Name:** `my-keypair`
3. **Key pair type:** `RSA`
4. **Private key format:** `.pem`
5. Click **Create key pair**
6. Download and save the `.pem` file safely.

#### 🔹 If you already have one:

* Select the existing key pair from the dropdown.

---

### **5. Configure Network Settings**

* **VPC:** Default
* **Subnet:** Choose any availability zone
* **Auto-assign Public IP:** ✅ Enable
* **Security Group:**

  * Create a new one or use an existing one
  * Add inbound rules:

    ```
    Type: SSH
    Protocol: TCP
    Port: 22
    Source: My IP
    ```

    *(Optional for ARD access)*

    ```
    Type: Custom TCP
    Port Range: 5900
    Source: My IP
    ```

---

### **6. Configure Storage**

* Default EBS storage: 60 GB (can increase if needed)

---

### **7. Launch the Instance**

* Review all configurations
* Click **Launch Instance**
* Wait until **Instance State = Running**

---

## 🧭 Step 8: Connect to macOS Instance (SSH)

Once running, you can connect via **SSH** using your key pair.

### **1. Get Public IP**

* Go to **EC2 → Instances**
* Copy your instance’s **Public IPv4 address**

### **2. Set PEM Permissions**

```bash
chmod 400 my-keypair.pem
```

### **3. Connect using SSH**

```bash
ssh -i my-keypair.pem ec2-user@<public-ip>
```

> Replace `<public-ip>` with your instance’s public IPv4.

**Example:**

```bash
ssh -i my-keypair.pem ec2-user@18.235.91.52
```

You’ll now be logged into the macOS terminal.

---

## 🖥️ Step 9: Connect via Apple Remote Desktop (Optional GUI Access)

### **1. Enable Remote Management on macOS Instance**

Run the following inside SSH:

```bash
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -activate -configure -access -on -users ec2-user -privs -all -restart -agent
```

### **2. Open Apple Remote Desktop App (on your Mac)**

* Click ➕ → **Add by IP Address**
* Enter your instance’s **Public IP**
* Username: `ec2-user`
* Password: *(You’ll need to set one using the next command)*

Set a password for `ec2-user`:

```bash
sudo passwd ec2-user
```

Then, connect via Apple Remote Desktop using:

* IP: your instance’s public IP
* Username: `ec2-user`
* Password: the one you set above

---

## 🧹 Optional: Update macOS Packages

```bash
softwareupdate -l
softwareupdate -i -a
```

---

## 🧠 Common Troubleshooting

| Issue                  | Cause                   | Solution                                  |
| ---------------------- | ----------------------- | ----------------------------------------- |
| SSH timeout            | Security group not open | Allow port 22 from your IP                |
| Permission denied      | Wrong key or username   | Ensure correct `.pem` and user `ec2-user` |
| Cannot connect via ARD | Port 5900 closed        | Add inbound rule for port 5900            |
| Slow connection        | Region or instance type | Try another region or M2 instance         |

---

## ✅ Summary

| Step | Action                                         |
| ---- | ---------------------------------------------- |
| 1    | Choose macOS AMI (Ventura / Monterey / Sonoma) |
| 2    | Select instance type (`mac2.metal`)            |
| 3    | Create or select key pair                      |
| 4    | Enable SSH (and optionally 5900 for ARD)       |
| 5    | Launch instance                                |
| 6    | Connect using SSH                              |
| 7    | (Optional) Enable GUI access via ARD           |

---

## 📎 Notes

* Default username for macOS EC2: **`ec2-user`**
* macOS instances are **not Free Tier eligible**
* Minimum billing duration: **24 hours per instance**
* macOS instances require **Dedicated Hosts**, so AWS automatically allocates one.
* Stop the instance when not in use to avoid extra charges.

---

