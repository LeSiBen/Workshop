---
title: "Install Docker Desktop"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>2.2 </b>"
---

# Install Docker Desktop

## Tổng quan

Docker Desktop là essential tool để build và test container images locally trước khi deploy lên AWS Lambda. Section này sẽ hướng dẫn chi tiết cách install và configure Docker Desktop trên các operating systems khác nhau.

### ⏱️ **Thời gian ước tính**: 15-20 phút
### 💾 **Disk space required**: 4-6 GB

## Bước 1: System Requirements Check

### **Windows Requirements**
- **OS**: Windows 10 64-bit Pro, Enterprise, hoặc Education (Build 19041+)
- **Memory**: 4 GB RAM minimum (8 GB khuyến nghị)
- **Virtualization**: Hyper-V và Containers Windows features enabled
- **WSL 2**: Windows Subsystem for Linux version 2

### **macOS Requirements**
- **OS**: macOS 10.15 hoặc newer
- **Memory**: 4 GB RAM minimum
- **Hardware**: 2010 hoặc newer Mac với Intel processor, hoặc Apple Silicon

### **Linux Requirements**
- **OS**: Ubuntu 18.04+, CentOS 7+, Debian 9+, Fedora 30+
- **Memory**: 4 GB RAM minimum
- **Architecture**: x86_64/amd64

## Bước 2: Download Docker Desktop

### **Bước 2.1: Windows Installation**

#### **Download Docker Desktop**
1. Vào [Docker Desktop for Windows](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe)
2. Hoặc vào [docker.com](https://www.docker.com/products/docker-desktop/) và click **"Download for Windows"**
3. File size: ~500MB, download time depends on connection

![Docker Download Windows](/images/prerequisites/docker-download-windows.png)

#### **Pre-installation Setup**
1. **Enable WSL 2**:
   - Open PowerShell as Administrator
   - Run: `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
   - Run: `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
   - Restart computer

2. **Install WSL 2 Kernel Update**:
   - Download: [WSL2 Linux kernel update package](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
   - Run installer
   - Set WSL 2 as default: `wsl --set-default-version 2`

#### **Install Docker Desktop**
1. **Run Installer**:
   - Double-click `Docker Desktop Installer.exe`
   - Check **"Use WSL 2 instead of Hyper-V"** (khuyến nghị)
   - Check **"Add shortcut to desktop"**
   - Click **"Ok"**

![Docker Installation Windows](/images/prerequisites/docker-install-windows.png)

2. **Installation Process**:
   - Wait for installation (5-10 phút)
   - Click **"Close and restart"** when prompted
   - Computer sẽ restart

3. **First Launch**:
   - Docker Desktop sẽ auto-start sau restart
   - Accept license agreement
   - Skip tutorial (optional)
   - Wait for Docker engine to start

### **Bước 2.2: macOS Installation**

#### **Download Docker Desktop**
1. **For Intel Macs**:
   - Download: [Docker Desktop for Mac with Intel chip](https://desktop.docker.com/mac/main/amd64/Docker.dmg)

2. **For Apple Silicon Macs**:
   - Download: [Docker Desktop for Mac with Apple chip](https://desktop.docker.com/mac/main/arm64/Docker.dmg)

![Docker Download macOS](/images/prerequisites/docker-download-macos.png)

#### **Install Docker Desktop**
1. **Mount DMG**:
   - Double-click downloaded `.dmg` file
   - Drag Docker icon to Applications folder

![Docker Install macOS](/images/prerequisites/docker-install-macos.png)

2. **Launch Docker**:
   - Open Applications folder
   - Double-click Docker
   - Click **"Open"** nếu security warning appears
   - Enter admin password when prompted

3. **Initial Setup**:
   - Accept license agreement
   - Provide admin credentials for privileged access
   - Wait for Docker engine to start

### **Bước 2.3: Linux Installation**

#### **Ubuntu/Debian Installation**
```bash
# Update package index
sudo apt-get update

# Install prerequisites
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Add Docker's official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Set up repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Add user to docker group
sudo usermod -aG docker $USER

# Logout and login again
```

#### **CentOS/RHEL Installation**
```bash
# Install yum-utils
sudo yum install -y yum-utils

# Add Docker repository
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# Install Docker Engine
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Start Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group
sudo usermod -aG docker $USER

# Logout and login again
```

## Bước 3: Configure Docker Desktop

### **Bước 3.1: Basic Configuration**

#### **Windows/macOS Settings**
1. **Open Docker Desktop Settings**:
   - Click Docker icon trong system tray/menu bar
   - Click **"Settings"** hoặc **"Preferences"**

![Docker Settings](/images/prerequisites/docker-settings.png)

2. **General Settings**:
   - ✅ **Start Docker Desktop when you log in**
   - ✅ **Send usage statistics** (optional)
   - ✅ **Use Docker Compose V2**

3. **Resources Settings**:
   - **Memory**: Allocate 4-6 GB (default usually OK)
   - **CPUs**: Use 2-4 CPUs (based on your system)
   - **Disk image size**: 60 GB (default)
   - **Swap**: 1 GB

![Docker Resources](/images/prerequisites/docker-resources.png)

4. **Docker Engine Settings**:
   - Keep default JSON configuration
   - Add logging configuration nếu needed:
   ```json
   {
     "builder": {
       "gc": {
         "defaultKeepStorage": "20GB",
         "enabled": true
       }
     },
     "experimental": false,
     "features": {
       "buildkit": true
     },
     "log-driver": "json-file",
     "log-opts": {
       "max-size": "10m",
       "max-file": "3"
     }
   }
   ```

### **Bước 3.2: Advanced Configuration**

#### **Enable Kubernetes (Optional)**
1. Go to **Settings** → **Kubernetes**
2. Check **"Enable Kubernetes"**
3. Click **"Apply & Restart"**
4. Wait for Kubernetes to start (có thể mất 5-10 phút)

**Note**: Kubernetes không required cho workshop này, nhưng useful cho future container orchestration.

#### **Configure File Sharing (Windows)**
1. Go to **Settings** → **Resources** → **File Sharing**
2. Ensure C: drive được shared
3. Add additional drives nếu needed
4. Click **"Apply & Restart"**

## Bước 4: Verify Docker Installation

### **Bước 4.1: Basic Verification**

#### **Check Docker Version**
```bash
# Check Docker version
docker --version

# Expected output:
# Docker version 20.10.x, build xxxxx

# Check Docker info
docker info

# Should show Docker engine running
```

#### **Test Docker Functionality**
```bash
# Run hello-world container
docker run hello-world

# Expected output:
# Hello from Docker!
# This message shows that your installation appears to be working correctly.
```

![Docker Hello World](/images/prerequisites/docker-hello-world.png)

### **Bước 4.2: Advanced Verification**

#### **Test Container Building**
```bash
# Create test Dockerfile
cat > Dockerfile.test << EOF
FROM alpine:latest
RUN echo "Docker build test successful"
CMD ["echo", "Container running successfully"]
EOF

# Build test image
docker build -f Dockerfile.test -t docker-test .

# Run test container
docker run --rm docker-test

# Clean up
rm Dockerfile.test
docker rmi docker-test
```

#### **Test Multi-stage Build**
```bash
# Create multi-stage Dockerfile
cat > Dockerfile.multistage << EOF
FROM alpine:latest as builder
RUN echo "Building in stage 1"

FROM alpine:latest
COPY --from=builder /etc/alpine-release /tmp/
CMD ["cat", "/tmp/alpine-release"]
EOF

# Build multi-stage image
docker build -f Dockerfile.multistage -t multistage-test .

# Run container
docker run --rm multistage-test

# Clean up
rm Dockerfile.multistage
docker rmi multistage-test
```

### **Bước 4.3: Performance Test**

#### **Test Build Performance**
```bash
# Time a simple build
time docker build -t perf-test - << EOF
FROM alpine:latest
RUN apk add --no-cache curl
CMD ["curl", "--version"]
EOF

# Should complete in < 30 seconds
docker run --rm perf-test

# Clean up
docker rmi perf-test
```

## Troubleshooting Common Issues

### **Issue 1: Docker Desktop Won't Start**

#### **Windows Symptoms**
- "Docker Desktop starting..." forever
- "WSL 2 installation is incomplete"

#### **Solutions**
1. **Restart Docker Service**:
   ```powershell
   # In PowerShell as Administrator
   Restart-Service -Name "com.docker.service"
   ```

2. **Reset Docker Desktop**:
   - Right-click Docker icon → **"Troubleshoot"**
   - Click **"Reset to factory defaults"**
   - Restart computer

3. **Check WSL 2**:
   ```powershell
   # Check WSL version
   wsl --list --verbose
   
   # Should show version 2
   # If not, update:
   wsl --set-version <distro-name> 2
   ```

### **Issue 2: Permission Denied (Linux)**

#### **Symptoms**
- "permission denied while trying to connect to Docker daemon"

#### **Solutions**
1. **Add User to Docker Group**:
   ```bash
   sudo usermod -aG docker $USER
   
   # Logout and login again
   # Or run:
   newgrp docker
   ```

2. **Start Docker Service**:
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

### **Issue 3: Slow Performance**

#### **Symptoms**
- Docker builds very slow
- Container startup takes long time

#### **Solutions**
1. **Increase Resources**:
   - Allocate more RAM (6-8 GB)
   - Allocate more CPUs (4+)

2. **Enable BuildKit**:
   ```bash
   # Set environment variable
   export DOCKER_BUILDKIT=1
   
   # Or add to Docker Engine config:
   # "features": { "buildkit": true }
   ```

3. **Clean Up Docker**:
   ```bash
   # Remove unused containers, networks, images
   docker system prune -a
   
   # Remove build cache
   docker builder prune
   ```

### **Issue 4: Network Issues**

#### **Symptoms**
- Cannot pull images
- DNS resolution fails in containers

#### **Solutions**
1. **Check DNS Settings**:
   - Docker Desktop → Settings → Docker Engine
   - Add DNS configuration:
   ```json
   {
     "dns": ["8.8.8.8", "8.8.4.4"]
   }
   ```

2. **Corporate Firewall**:
   - Configure proxy settings in Docker Desktop
   - Add corporate certificates nếu needed

## Docker Best Practices for Workshop

### **Image Management**
```bash
# List all images
docker images

# Remove unused images
docker image prune

# Remove specific image
docker rmi <image-name>

# Tag images properly
docker tag <source> <target>
```

### **Container Management**
```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Stop container
docker stop <container-id>

# Remove container
docker rm <container-id>

# Remove all stopped containers
docker container prune
```

### **Build Optimization**
```bash
# Use .dockerignore file
cat > .dockerignore << EOF
.git
.gitignore
README.md
Dockerfile*
.dockerignore
node_modules
npm-debug.log
EOF

# Use build cache effectively
docker build --cache-from <previous-image> -t <new-image> .

# Multi-platform builds (if needed)
docker buildx build --platform linux/amd64,linux/arm64 -t <image> .
```

## Next Steps

### **Verification Checklist**

Trước khi tiếp tục, ensure:
- [ ] Docker Desktop installed và running
- [ ] `docker --version` shows version info
- [ ] `docker run hello-world` works successfully
- [ ] Can build simple Dockerfile
- [ ] Multi-stage builds work
- [ ] No permission errors
- [ ] Performance acceptable (builds < 1 minute)

### **Ready for Next Step?**

Nếu tất cả checks pass, bạn ready cho [Install Terraform](../2.3-install-terraform/)!

**What's Next**: Terraform installation để manage AWS infrastructure as code.

---

**🎯 Success Criteria**: Docker Desktop running smoothly, can build và run containers locally. Ready for container development!