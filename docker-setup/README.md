# 🐳 Docker Setup Action

Production-grade Docker installation and configuration for remote servers via SSH.

## 📖 Description

This composite action installs Docker Engine and Docker Compose on remote servers using SSH. It includes intelligent installation detection, official Docker repository setup, production-grade daemon configuration, and comprehensive health verification.

## ✨ Features

- ✅ **Intelligent Detection**: Skips installation if Docker is already properly configured
- ✅ **Official Repository**: Uses Docker's official APT repository with GPG verification
- ✅ **Production Configuration**: Optimized daemon settings for production workloads
- ✅ **Docker Compose**: Optional Docker Compose plugin installation
- ✅ **Comprehensive Testing**: Health checks and verification tests
- ✅ **Security Focused**: Secure package installation and user management
- ✅ **Detailed Logging**: Timestamped logs for debugging and monitoring

## 🚀 Usage

### Basic Usage

```yaml
- name: Setup Docker on Server
  uses: MBBe-Software/actions/docker-setup@v1
  with:
    host: ${{ secrets.SERVER_IP }}
    username: ${{ secrets.SERVER_USERNAME }}
    ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

### Advanced Usage

```yaml
- name: Setup Docker with Custom Configuration
  uses: MBBe-Software/actions/docker-setup@v1
  with:
    host: ${{ secrets.PRODUCTION_IP }}
    username: ${{ secrets.PRODUCTION_USERNAME }}
    ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}
    docker-version: 'latest'
    compose-plugin: true
```

## 📝 Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `host` | Server hostname or IP address | ✅ Yes | - |
| `username` | SSH username | ✅ Yes | - |
| `ssh-key` | SSH private key for authentication | ✅ Yes | - |
| `docker-version` | Docker version to install | ❌ No | `latest` |
| `compose-plugin` | Install Docker Compose plugin | ❌ No | `true` |

## 📋 Requirements

### Target Server Requirements

- **OS**: Ubuntu 18.04+ (other Debian-based distributions may work)
- **Architecture**: amd64/x86_64
- **Privileges**: SSH user must have sudo/root access
- **Network**: Internet connectivity for package downloads

### GitHub Secrets Required

```yaml
# In your repository secrets
SERVER_IP: "192.168.1.100"              # Target server IP
SERVER_USERNAME: "ubuntu"                # SSH username
SSH_PRIVATE_KEY: |                       # SSH private key
  -----BEGIN OPENSSH PRIVATE KEY-----
  b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQ...
  -----END OPENSSH PRIVATE KEY-----
```

## 🏗️ Docker Configuration

The action configures Docker with production-optimized settings:

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "storage-driver": "overlay2",
  "userland-proxy": false,
  "experimental": false,
  "live-restore": true
}
```

### Configuration Benefits

- **Log Management**: Prevents disk space issues with log rotation
- **Storage Optimization**: Uses overlay2 for better performance
- **Service Resilience**: live-restore keeps containers running during daemon updates
- **Network Performance**: Disables userland-proxy for better networking

## 🔧 Example Workflows

### Simple Deployment Workflow

```yaml
name: Deploy Application

on:
  push:
    branches: [main]

jobs:
  setup-infrastructure:
    runs-on: ubuntu-latest
    steps:
      - name: Setup Docker on Production Server
        uses: MBBe-Software/actions/docker-setup@v1
        with:
          host: ${{ secrets.PRODUCTION_IP }}
          username: ${{ secrets.PRODUCTION_USERNAME }}
          ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}

  deploy:
    needs: setup-infrastructure
    runs-on: ubuntu-latest
    steps:
      - name: Deploy Application
        # Your deployment steps here
        run: echo "Deploy your application"
```

### Multi-Environment Setup

```yaml
name: Setup Infrastructure

on:
  workflow_dispatch:

jobs:
  setup-staging:
    runs-on: ubuntu-latest
    steps:
      - name: Setup Docker on Staging
        uses: MBBe-Software/actions/docker-setup@v1
        with:
          host: ${{ secrets.STAGING_IP }}
          username: ${{ secrets.STAGING_USERNAME }}
          ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}

  setup-production:
    runs-on: ubuntu-latest
    steps:
      - name: Setup Docker on Production
        uses: MBBe-Software/actions/docker-setup@v1
        with:
          host: ${{ secrets.PRODUCTION_IP }}
          username: ${{ secrets.PRODUCTION_USERNAME }}
          ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

## 🔍 Verification Process

The action performs comprehensive verification:

1. **Pre-Installation Check**: Verifies if Docker is already installed
2. **Package Installation**: Installs Docker from official repository
3. **Service Verification**: Checks Docker daemon status
4. **Functionality Test**: Runs hello-world container test
5. **Cleanup**: Removes test containers and images

## 📊 Output Examples

### Successful Installation

```
🐳 Starting Docker installation process...
📍 Target server: 192.168.1.100
👤 User: ubuntu
🔧 Docker version: latest
📦 Compose plugin: true
[2024-01-15 10:30:00] 📋 Docker not found or not working properly. Installing...
[2024-01-15 10:30:05] 🔄 Updating system packages...
[2024-01-15 10:30:15] 📦 Installing prerequisites...
[2024-01-15 10:30:25] 🔑 Adding Docker GPG key...
[2024-01-15 10:30:30] 📦 Adding Docker repository...
[2024-01-15 10:30:35] 🐳 Installing Docker Engine...
[2024-01-15 10:31:00] 📦 Installing Docker Compose plugin...
[2024-01-15 10:31:10] ⚙️ Configuring Docker daemon...
[2024-01-15 10:31:15] 🚀 Starting Docker service...
[2024-01-15 10:31:20] 🔍 Verifying Docker installation...
[2024-01-15 10:31:25] 🧪 Testing Docker with hello-world...
[2024-01-15 10:31:30] ✅ Docker installation completed successfully!
```

### Already Installed

```
🐳 Starting Docker installation process...
[2024-01-15 10:30:00] ✅ Docker is already installed: Docker version 24.0.7, build afdd53b
[2024-01-15 10:30:01] ✅ Docker Compose is already available: Docker Compose version v2.21.0
[2024-01-15 10:30:01] 🎉 Docker and Docker Compose are already properly installed
```

## 🛠️ Troubleshooting

### Common Issues

1. **SSH Connection Failed**
   ```
   Solution: Verify host, username, and SSH key are correct
   ```

2. **Permission Denied**
   ```
   Solution: Ensure SSH user has sudo privileges
   ```

3. **Package Installation Failed**
   ```
   Solution: Check internet connectivity and APT sources
   ```

4. **Docker Service Won't Start**
   ```
   Solution: Check system logs and Docker daemon configuration
   ```

## 🔐 Security Considerations

- SSH keys are handled securely through GitHub Secrets
- Official Docker repository ensures package authenticity
- GPG verification prevents package tampering
- Production configuration includes security optimizations
- User permissions are managed appropriately

## 📈 Performance Impact

- **Installation Time**: ~2-3 minutes for fresh installation
- **Resource Usage**: Minimal during installation, ~100MB RAM for Docker daemon
- **Network**: ~200MB download for Docker packages
- **Storage**: ~500MB for Docker installation

## 🤝 Contributing

1. Test changes thoroughly on clean Ubuntu systems
2. Update documentation for any new features
3. Follow semantic versioning for releases
4. Ensure backward compatibility

## 📜 License

This action is part of the MBBe-Software organization's internal tooling.

---

**🤖 Generated with [Claude Code](https://claude.ai/code)**