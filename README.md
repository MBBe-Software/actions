# 🚀 MBBe-Software Actions

Centralized repository for reusable GitHub Actions used across MBBe-Software organization projects.

## 📦 Available Actions

### 🐳 [docker-setup](./docker-setup)
Install and configure Docker on remote servers via SSH with production-grade configuration.

```yaml
- name: Setup Docker on Server
  uses: MBBe-Software/actions/docker-setup@v1
  with:
    host: ${{ secrets.SERVER_IP }}
    username: ${{ secrets.SERVER_USERNAME }}
    ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}
    docker-version: 'latest'  # optional
    compose-plugin: true      # optional
```

**Features:**
- ✅ Intelligent Docker installation detection
- ✅ Official Docker repository with GPG verification
- ✅ Production-grade daemon configuration
- ✅ Docker Compose plugin installation
- ✅ Comprehensive error handling and logging
- ✅ Health verification and testing

## 🏗️ Repository Structure

```
actions/
├── docker-setup/           # Docker installation action
│   ├── action.yml          # Action definition
│   └── README.md           # Action documentation
├── future-action/          # Future actions will go here
│   ├── action.yml
│   └── README.md
└── README.md              # This file
```

## 🎯 Usage Philosophy

Each action in this repository follows these principles:

1. **🔒 Security First**: Secure secret handling and validation
2. **🚀 Production Ready**: Tested in real production environments
3. **📝 Well Documented**: Clear usage examples and parameters
4. **🔄 Reusable**: Works across different projects and environments
5. **🛡️ Error Handling**: Comprehensive error checking and reporting
6. **📊 Logging**: Detailed execution logs for debugging

## 🤝 Contributing

When adding new actions:

1. Create a new directory for each action
2. Include `action.yml` with proper metadata
3. Add comprehensive `README.md` with usage examples
4. Test thoroughly before tagging versions
5. Follow semantic versioning for releases

## 🏷️ Versioning

- Use semantic versioning: `v1.0.0`, `v1.1.0`, etc.
- Tag major versions: `v1`, `v2` for easier consumption
- Document breaking changes in releases

---

**🤖 Generated with [Claude Code](https://claude.ai/code)**