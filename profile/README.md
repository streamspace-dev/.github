# StreamSpace

<div align="center">

![StreamSpace Logo](https://via.placeholder.com/800x200/1a1a2e/00d4ff?text=StreamSpace)

### Stream Any App to Your Browser

**Open-source platform for delivering containerized applications through your web browser**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Kubernetes](https://img.shields.io/badge/kubernetes-1.19+-blue.svg)](https://kubernetes.io/)
[![Go Version](https://img.shields.io/badge/go-1.21+-00ADD8.svg)](https://golang.org/)
[![React](https://img.shields.io/badge/react-18+-61DAFB.svg)](https://reactjs.org/)

[Website](https://streamspace.dev) • [Documentation](https://github.com/streamspace-dev/streamspace/tree/main/docs) • [Getting Started](https://github.com/streamspace-dev/streamspace#quick-start) • [Contributing](https://github.com/streamspace-dev/streamspace/blob/main/CONTRIBUTING.md)

</div>

---

## 🚀 What is StreamSpace?

StreamSpace is a **Kubernetes-native platform** that streams GUI applications directly to web browsers using VNC technology. No client software required—just open your browser and access any containerized application.

Perfect for:
- 🖥️ **Remote Desktops** - Full Linux desktop environments in the browser
- 🛠️ **Development Environments** - VS Code, IDEs, and dev tools accessible anywhere
- 🌐 **Web Browsers** - Firefox, Chrome in isolated containers
- 📊 **Enterprise Applications** - CAD, data analysis, legacy apps
- 🎓 **Education & Training** - Consistent environments for students
- 🧪 **Testing & QA** - Isolated browser testing environments

## ✨ Key Features

### Core Platform
- **Browser-Based Access** - No VPN, no RDP clients, just a web browser
- **Multi-User Support** - Isolated sessions with resource quotas
- **Auto-Hibernation** - Scale to zero when idle, save resources
- **Persistent Storage** - NFS-backed home directories
- **200+ Templates** - Pre-built applications ready to deploy

### Enterprise Ready
- **Multiple Auth Methods** - Local, SAML 2.0 (Okta, Azure AD), OIDC, MFA
- **Compliance Built-In** - SOC2, HIPAA, GDPR frameworks
- **Audit Logging** - Complete activity tracking
- **Plugin System** - Extensible webhooks and integrations
- **High Availability** - Kubernetes-native with automatic failover

### Developer Friendly
- **REST API** - Full programmatic control
- **Custom Resources** - Kubernetes CRDs for sessions and templates
- **Webhook Support** - Integrate with Slack, Teams, Discord, PagerDuty
- **Multi-Platform** - Kubernetes (production), Docker/VM/Cloud (coming soon)

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Web Browser                          │
│              (VNC Viewer via WebSocket)                 │
└────────────────────┬────────────────────────────────────┘
                     │ HTTPS/WSS
                     │
┌────────────────────▼────────────────────────────────────┐
│                Control Plane (API)                       │
│  ┌──────────┐  ┌───────────┐  ┌──────────────┐        │
│  │   API    │  │ VNC Proxy │  │ Agent Hub    │        │
│  │ Handlers │  │  (WebRTC) │  │ (WebSocket)  │        │
│  └──────────┘  └───────────┘  └──────────────┘        │
└────────────────────┬────────────────────────────────────┘
                     │ WebSocket Commands
                     │
      ┌──────────────┴──────────────┬──────────────┐
      │                             │              │
┌─────▼─────┐              ┌────────▼──────┐      │
│    K8s    │              │    Docker     │      │
│   Agent   │              │    Agent      │   (Future)
│           │              │   (v2.1+)     │
└─────┬─────┘              └───────────────┘
      │
┌─────▼──────────────────────────────────────────┐
│           Kubernetes Cluster                    │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐     │
│  │ Session  │  │ Session  │  │ Session  │     │
│  │   Pod    │  │   Pod    │  │   Pod    │     │
│  └──────────┘  └──────────┘  └──────────┘     │
└─────────────────────────────────────────────────┘
```

## 📦 Repositories

### Core Platform
- **[streamspace](https://github.com/streamspace-dev/streamspace)** - Main platform (API, UI, Controllers, Agents)
- **[streamspace-plugins](https://github.com/streamspace-dev/streamspace-plugins)** - Official and community plugins
- **[streamspace-templates](https://github.com/streamspace-dev/streamspace-templates)** - 200+ application templates

### Enterprise (Private)
- **[streamspace-saas](https://github.com/streamspace-dev/streamspace-saas)** - Managed hosting infrastructure

## 🎯 Current Status

### v2.0-beta (November 2025)

**Development Status**: 100% Complete - Integration Testing Phase

#### What's Working
- ✅ Multi-platform Control Plane + Agent architecture
- ✅ End-to-end VNC proxy through Control Plane
- ✅ Kubernetes Agent with full session lifecycle
- ✅ VNC tunneling and real-time streaming
- ✅ Agent management UI with monitoring
- ✅ WebSocket command channel
- ✅ 70%+ test coverage on all v2.0 code
- ✅ Comprehensive documentation (6,700+ lines)

#### In Progress
- 🔄 Integration testing with live K8s clusters
- 🔄 Performance benchmarking
- 🔄 Multi-agent deployment testing

#### Coming in v2.1+
- 📋 Docker Agent (local development)
- 📋 VM platforms (Hyper-V, vCenter)
- 📋 Cloud platforms (AWS, Azure, GCP)

## 🚀 Quick Start

```bash
# Prerequisites: Kubernetes 1.19+, Helm 3.0+, PostgreSQL, NFS storage

# Clone repository
git clone https://github.com/streamspace-dev/streamspace.git
cd streamspace

# Deploy via Helm
helm install streamspace ./chart -n streamspace --create-namespace

# Create a session
kubectl apply -f - <<EOF
apiVersion: stream.space/v1alpha1
kind: Session
metadata:
  name: my-firefox
  namespace: streamspace
spec:
  user: myuser
  template: firefox
  state: running
  resources:
    memory: 2Gi
    cpu: 1000m
EOF

# Access at https://<your-domain>/sessions/my-firefox
```

[Full Installation Guide →](https://github.com/streamspace-dev/streamspace/blob/main/docs/V2_DEPLOYMENT_GUIDE.md)

## 🛠️ Technology Stack

**Backend**
- Go 1.21+ (API backend, controllers, agents)
- PostgreSQL (87 tables)
- Kubebuilder 3.x (Kubernetes controller framework)

**Frontend**
- React 18+ with TypeScript
- Material-UI (MUI)
- noVNC (VNC client)

**Infrastructure**
- Kubernetes 1.19+ (production)
- Docker (local development)
- NFS (persistent storage)
- Traefik/Nginx (ingress)

**Streaming**
- VNC over WebSocket
- TigerVNC + noVNC stack (v2.0+)
- WebRTC (planned for v3.0)

## 🤝 Contributing

We welcome contributions! StreamSpace uses a multi-agent development model:

- **Architect** - Research, planning, architecture decisions
- **Builder** - Feature implementation, bug fixes
- **Validator** - Testing, QA, coverage expansion
- **Scribe** - Documentation, guides, examples

See our [Contributing Guide](https://github.com/streamspace-dev/streamspace/blob/main/CONTRIBUTING.md) to get started.

## 📖 Documentation

- **[Architecture Overview](https://github.com/streamspace-dev/streamspace/blob/main/docs/V2_ARCHITECTURE.md)** - System design and data flow
- **[Deployment Guide](https://github.com/streamspace-dev/streamspace/blob/main/docs/V2_DEPLOYMENT_GUIDE.md)** - Installation and configuration
- **[Agent Guide](https://github.com/streamspace-dev/streamspace/blob/main/docs/V2_AGENT_GUIDE.md)** - Deploying and managing agents
- **[Migration Guide](https://github.com/streamspace-dev/streamspace/blob/main/docs/V2_MIGRATION_GUIDE.md)** - Upgrading from v1.x
- **[API Reference](https://github.com/streamspace-dev/streamspace/blob/main/docs/API.md)** - REST API documentation
- **[Plugin Development](https://github.com/streamspace-dev/streamspace/blob/main/docs/PLUGIN_API.md)** - Building plugins

## 📊 Project Stats

- **87** Database Tables
- **70+** REST API Handlers
- **50+** React Components
- **200+** Application Templates
- **28** Plugin Framework Hooks
- **500+** Test Cases
- **70%+** Test Coverage
- **66,000+** Lines of Go Code
- **6,700+** Lines of Documentation

## 🌟 Use Cases

### Remote Work
Enable secure remote access to corporate applications without VPN complexity.

### Education
Provide students with consistent development environments accessible from any device.

### Testing & QA
Run automated browser tests in isolated containers with full GUI access.

### Legacy Applications
Containerize and stream legacy Windows/Linux applications through modern browsers.

### Development Environments
Give developers pre-configured environments with all tools installed, accessible anywhere.

## 📜 License

StreamSpace is open source software licensed under the [MIT License](https://github.com/streamspace-dev/streamspace/blob/main/LICENSE).

## 🔗 Links

- **Website**: [streamspace.dev](https://streamspace.dev)
- **Documentation**: [GitHub Docs](https://github.com/streamspace-dev/streamspace/tree/main/docs)
- **Community**: [GitHub Discussions](https://github.com/streamspace-dev/streamspace/discussions)
- **Issues**: [GitHub Issues](https://github.com/streamspace-dev/streamspace/issues)
- **Security**: [Security Policy](https://github.com/streamspace-dev/streamspace/blob/main/SECURITY.md)

## 💬 Support

- 📖 Check our [Documentation](https://github.com/streamspace-dev/streamspace/tree/main/docs)
- 💡 Ask in [GitHub Discussions](https://github.com/streamspace-dev/streamspace/discussions)
- 🐛 Report bugs via [GitHub Issues](https://github.com/streamspace-dev/streamspace/issues)
- 📧 Contact: [conduct@streamspace.dev](mailto:conduct@streamspace.dev)

---

<div align="center">

**Built with ❤️ by the StreamSpace Community**

⭐ Star us on GitHub if you find StreamSpace useful!

</div>
