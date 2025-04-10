# 🚀 Apache Web Server Setup using Docker on Windows CMD

This guide walks you through the **step-by-step process** for setting up an **Apache web server** using **Docker on Windows CMD**.

---

## 🔹 Step 1: Install Docker

1. Download and install **Docker Desktop** from the [official Docker site](https://www.docker.com/get-started).  
2. Open **Command Prompt (CMD)** and verify the installation:
   ```bash
   docker --version
   ```
3. Start Docker Desktop and ensure it's running.

---

## 🔹 Step 2: Pull the Apache (httpd) Image

Download the Apache web server Docker image:
```bash
docker pull httpd
```

---

## 🔹 Step 3: Run the Apache Web Server

Start the Apache server and make sure it auto-restarts after reboots:
```bash
docker run -d -p 80:80 --restart always --name my-apache httpd
```

**Options Explained:**
- `-d`: Run container in detached mode (in the background)
- `-p 80:80`: Map container port 80 to host port 80
- `--restart always`: Restart container automatically after reboots
- `--name my-apache`: Name the container `my-apache`

---

## 🔹 Step 4: Verify Apache is Running

Check the running containers:
```bash
docker ps -a
```

If `my-apache` appears in the list, the server is active ✅

---

## 🔹 Step 5: View the Web Server

### Option 1: Open in Browser
Visit:
```
http://localhost
```
You should see the default Apache welcome page.

### Option 2: Use Curl in CMD
```bash
curl http://localhost
```
This will display the web page content in the terminal.

---

## 🔹 Step 6: Start or Stop the Server

To start the Apache server if it's stopped:
```bash
docker start my-apache
```

To stop the server:
```bash
docker stop my-apache
```

---

## 🔹 Step 7: Remove the Container

If you no longer need the Apache container and want to remove it:
```bash
docker rm my-apache
```

---

> Simple, portable Apache web server setup with Docker on Windows.
