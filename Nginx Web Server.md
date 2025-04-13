
# 🚀 Deploy Open-Source NGINX Web Server on AWS EC2

A step-by-step guide to deploying an open-source NGINX server on an AWS EC2 instance.

---

## 🖥️ Step 1: Launch an EC2 Instance

1. Go to **AWS Console** > **EC2** > *Launch Instance*.
2. Name your instance (e.g., `nginx-server`).
3. **Choose AMI**: Select **Ubuntu Server 22.04 LTS** *(or Amazon Linux)*.
4. **Instance Type**: Choose `t2.micro` *(Free Tier eligible)*.
5. **Key Pair**: Select or create one to connect via SSH.
6. **Network Settings**:
   - Allow **SSH (port 22)**
   - Allow **HTTP (port 80)**
   - *(Optional)* Allow **HTTPS (port 443)**
7. Leave other settings as default and click **Launch Instance**.

---

## 🔐 Step 2: Connect to the EC2 Instance

1. After launch, go to **EC2 > Instances** and copy the **Public IPv4 address**.
2. In your terminal, run:

```bash
ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>
```

Replace with your actual `.pem` file path and public IP.

---

## 🧰 Step 3: Install NGINX (Open Source)

### On Ubuntu:
```bash
sudo apt update
sudo apt install nginx -y
```

### On Amazon Linux:
```bash
sudo yum update -y
sudo amazon-linux-extras install nginx1 -y
sudo yum install nginx -y
```

---

## ▶️ Step 4: Start and Enable NGINX

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 🌐 Step 5: Verify the Web Server

Open your browser and visit:  
```
http://<your-ec2-public-ip>
```

You should see the **NGINX default welcome page**.

---

## 📂 Step 6 (Optional): Host Your Website

1. Upload your HTML files to:

```bash
/var/www/html/
```

2. Replace the existing `index.html` with your custom file.

---
