# 🚀 DevOps Flask CI/CD Project

This project demonstrates a **complete DevOps pipeline** using **Flask**, **Docker**, **GitHub Actions (CI/CD)**, and **VMware-hosted deployment (Kali Linux)**.  
It serves as a hands-on example of how to build, test, containerize, and deploy a Python web application automatically.

---

## ⚙️ Technologies Used

- **Flask** → Web framework  
- **Docker** → Containerization  
- **GitHub Actions** → CI/CD automation  
- **Kali Linux VM (VMware)** → Deployment server  
- **Self-hosted GitHub Runner** → CI/CD execution on local VM
- **Bridged Network** → VM accessible on local network  

---

## 🧱 CI/CD Overview

### ✅ Continuous Integration (CI)
The `ci.yml` workflow is triggered on each push or pull request to:
1. Install dependencies  
2. Run unit tests  
3. Build the Docker image  
4. Push the image to **GitHub Container Registry (GHCR)** or **Docker Hub**

### 🚀 Continuous Deployment (CD)
The `cd.yml` workflow automatically:
1. Waits for CI to complete successfully
2. Pulls the latest Docker image from Docker Hub
3. Stops and removes any existing container  
4. Deploys new container on the self-hosted runner
5. Runs health checks to verify deployment

 
## 🐍 Run Locally

### 1. Clone the repo
```bash
git clone https://github.com/yourusername/devops-project1.git
cd devops-project1
```
## 2. Create a virtual environment
It's best practice to use a virtual environment to manage project dependencies.
```
python3 -m venv venv
source venv/bin/activate   # For macOS/Linux
# OR
venv\Scripts\activate      # For Windows
```
## 3. Install Dependencies
```
pip install -r requirements.txt
```
## 4. Run the Flask Application
```
python app.py
```
Now open your browser and navigate to:
👉 http://127.0.0.1:5001

## 🐳 Run with Docker
Containerizing the application provides a consistent environment across development, testing, and production.
## 1. Build the Docker Image
```
docker build -t devops-app .
```
## 2. Run the Container
```
docker run -d -p 5001:5001 --name devops-container devops-app
```
## 3. Verify It’s Running
```
docker ps
```
## 4. Test the Endpoint
```
curl http://localhost:5001
```
## 🤖 Self-Hosted Runner Setup

This project uses a **self-hosted GitHub Actions runner** on the Kali VM.

### Setup Steps:
```bash
# 1. Create runner directory
mkdir actions-runner && cd actions-runner

# 2. Download runner
curl -o actions-runner-linux-x64-2.329.0.tar.gz -L \
  https://github.com/actions/runner/releases/download/v2.329.0/actions-runner-linux-x64-2.329.0.tar.gz
tar xzf ./actions-runner-linux-x64-2.329.0.tar.gz

# 3. Configure runner
./config.sh --url https://github.com/FatiXa00/devops-project1 --token YOUR_TOKEN

# 4. Install as service
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status
```

### Allow Docker without sudo:
```bash
sudo usermod -aG docker $USER
newgrp docker
```
## 🌐 Access from Host (VM Network Setup)
- Depending on your VMware network configuration, you can access the Flask application from your host machine.
- 
- - If your VM is on NAT mode:
-   The container listens on 0.0.0.0:5001.
-   Access inside the VM at http://127.0.0.1:5001.
-   Note: Direct access from the host to the VM's application port in NAT mode might require port forwarding configured in VMware.
- 
- - If your VM is on Bridged mode:

+ The VM is configured in **Bridged mode** and has a local network IP.

Get the VM IP:
```
ip a
```
Access from your host (Mac/Windows):
```
curl http://<vm-ip>:5001
```
Example:
```
curl http://192.168.1.113:5001
```
## ⚙️ Useful Docker Commands

- List running containers	          => docker ps
- Stop a container	                => docker stop <container_id>
- Remove a container	              => docker rm <container_id>
- List images	                      => docker images
- Remove an image	                  => docker rmi <image_id>
- Access container shell	          => docker exec -it <container_id> bash

## 🔐 GitHub Secrets (for CD)
To enable the Continuous Deployment workflow, you need to configure the following secrets in your GitHub repository:
1. Navigate to your repository on GitHub.
2. Go to Settings → Secrets and variables → Actions.
3. Add these secrets:

- SSH_HOST	                      => IP address of your Kali VM
- SSH_USER	                      => SSH username on your Kali VM (e.g., kali)
- SSH_KEY	                        => Private SSH key (content of your devops_cd_key file)
- DOCKER_USERNAME	                => Your Docker Hub username
- DOCKER_PASSWORD	                => Your Docker Hub Access Token (not your password)

## ✨ Expected Output
When the container runs correctly, you should see logs similar to this:
```
C#
* Running on http://0.0.0.0:5001
* Press CTRL+C to quit
```
You can then visit:
🌍 http://<your-vm-ip>:5001
📸 Example Output
```
Hello, DevOps World!
```
## 🐛 Troubleshooting

**Issue: `externally-managed-environment` error**
```bash
pip3 install --break-system-packages -r requirements.txt
```

**Check container logs:**
```bash
docker logs devops-app
```

**Verify runner status:**
```bash
cd ~/actions-runner
sudo ./svc.sh status
```
## 🧾 License
This project is open-source under the MIT License. See the LICENSE file for details.
## 💡 Author
Fati
DevOps & Software Engineer
💻 Passionate about automation, CI/CD pipelines, and scalable architecture.
 
✅ Automate → Build → Test → Deploy — The DevOps Way!
