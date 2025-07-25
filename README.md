# 🚀 Install and Run n8n using Docker on WSL (Ubuntu)

This guide walks you through installing and running [n8n](https://n8n.io/) using Docker on a WSL2 Ubuntu environment.

---

## 📦 Prerequisites

Before start the installation process, make sure you meet the following prerequisites:

- A Windows 10 operating system or higher with WSL support.
- Docker engine in WSL
- WSL enabled with Ubuntu (e.g., Ubuntu 20.04 or 22.04)

---

## 🐳 Step-by-Step Docker Installation and Setup for n8n on Ubuntu (WSL)

## 🔹 Step 1: Install Docker on Ubuntu (WSL)

If you haven’t installed Docker on your WSL Ubuntu environment, follow these steps:

#### 1️⃣ Update your package index:
Open your Ubuntu terminal and run the following:
```bash
sudo apt update
```

#### 2️⃣ Install required dependencies:
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

#### 3️⃣ Add Docker’s official GPG key:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

#### 4️⃣ Set up the Docker stable repository:
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### 5️⃣ Update and install Docker Engine:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
```
#### 6️⃣ Add you user to Docker Group:
Add your user to the Docker group to avoid the need to use sudo on every Docker command:
```bash
sudo usermod -aG docker $USER
```

## 🔹 Step 2: Verify Docker installation

Close the open terminal on Ubuntu.
Restart WSL via the Windows command line (Powershell).
```bash
wsl --shutdown
```

Access Ubuntu again. Check if Docker was installed correctly on the Ubuntu terminal:
```bash
docker --version
```
You should see something like Docker version 24.0.2, build cb74dfc.

## 🔹 Step 3: Start up Docker

To start the Docker service run the following on the Ubuntu terminal:
```bash
sudo service docker start
```

To stop the Docker service run the following on the Ubuntu terminal:
```bash
sudo service docker stop
```

Starts Docker service and enables it to start automatically on WSL Startup (recommended):
```bash
sudo systemctl start docker
sudo systemctl enable docker
```






