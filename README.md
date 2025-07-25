# 🚀 Install and Run n8n using Docker on WSL (Ubuntu)

**n8n** (short for *"nodemation"*) is a powerful open-source workflow automation tool that lets you connect different apps and services using a visual interface. It enables you to automate repetitive tasks, build custom integrations, and manage complex data flows — all without writing much code.  

This guide walks you through installing and running [n8n](https://n8n.io/) using Docker on a WSL2 Ubuntu environment.

---

## 📦 Prerequisites

Before start the installation process, make sure you meet the following prerequisites:

- A Windows 10 operating system or higher with WSL support.
- WSL enabled with Ubuntu (e.g., Ubuntu 22.04)
- Docker engine in WSL

---

## 🐳 Step-by-Step Docker Installation and Setup

### 🔹 Step 1: Install Docker on Ubuntu (WSL)

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
<br>

### 🔹 Step 2: Verify Docker installation

Close the open terminal on Ubuntu.
Restart WSL via the Windows command line (Powershell).
```bash
wsl --shutdown
```

Access Ubuntu again. Check if Docker was installed correctly on the Ubuntu terminal:
```bash
docker --version
```
You should see something like ```Docker version 24.0.2, build cb74dfc```.

<br>

### 🔹 Step 3: Start up Docker

#### 🅰️ Option 1: Start/Stop Docker Manually (No systemd required)
To start the Docker service run the following on the Ubuntu terminal:


```bash
sudo service docker start
```
⚠️ Warning: This command has to be run everytime you start the WSL instance.

To stop the Docker service run the following on the Ubuntu terminal:
```bash
sudo service docker stop
```
<hr style="border:2px solid gray; margin: 1em 0;">

#### 🅱️ Option 2 (✅Recommended): Enable Docker to Start Automatically on WSL Startup (with systemd)

```bash
sudo systemctl start docker
sudo systemctl enable docker
```
ℹ️ This will start the Docker daemon now and ensure it starts automatically every time your WSL instance boots (only if systemd is enabled).

To check whether systemd is enabled in your WSL instance run the following command: 
```bash
ps -p 1 -o comm=
```
You should see an output ```systemd``` if enabled

❗ Note: On WSL 2, systemctl will not work unless you have explicitly enabled systemd support via .wslconfig or tools like genie, wsl-conf, or systemd-launcher. By default, most WSL distributions do not start with systemd.

---


## <img src="n8n-logo.png" alt="n8n logo" width="32" style="vertical-align: bottom;"/> Step-by-Step n8n Installation via Docker

### 🔹 Step 1: Pull the n8n Docker Image and create a directory

Pull the image:
```bash
docker pull n8nio/n8n
```

Create a directory to store n8n's data (configs, workflows, DB):
```bash
mkdir -p ~/.n8n
```
<br>

### 🔹 Step 2: Start the n8n container

#### 🅰️ Option 1: Run Temporary Container
This method will start the container and automatically delete it when you close the terminal.


```bash
docker run -it --rm -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
```
🔗 The n8n UI will be available at: http://localhost:5678

💡 Once you close the terminal, the container is stopped and deleted.

<hr style="border:2px solid gray; margin: 1em 0;">

#### 🅱️ Option 2: Run Persistent Container
This method allows you to retain the container and reuse it easily.

#### 💬 Interactive Mode
```bash
docker run -it --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
```
💡 Once you close the terminal, the container is stopped but not deleted.
<br>

#### 🟢 Detached Mode (✅Recommended)
```bash
docker run -d --name n8n -p 5678:5678 -v ~/.n8n:/home/node/.n8n n8nio/n8n
```

▶️ Start the container anytime:
```bash
docker start n8n
```

🛑 Stop the container to save RAM and CPU:
```bash
docker stop n8n
```

❌ If you wish to remove the container entirely, run the following:
```bash
docker rm -f n8n
```

🔗 Access the n8n UI at: http://localhost:5678

---

<br>

## ⚙️ Understanding Docker Run Modes

📊 Why Detached Mode is Better for Most Use Cases
| ✅ Advantage             | 💡 Why It Matters for n8n                                       |
| ----------------------- | --------------------------------------------------------------- |
| 1️⃣ Runs in background  | n8n is a background service — keep your terminal free           |
| 2️⃣ Survives SSH/logout | Container continues running even if terminal/SSH session closes |
| 3️⃣ Clean management    | Easily start/stop with `docker start/stop n8n`                  |
| 4️⃣ Better stability    | No accidental shutdowns from terminal closure                   |

<br>

🧪 Use Interactive Mode only in these edge cases:

- Debugging startup errors
- Testing configuration changes
- Developing custom n8n Docker images




