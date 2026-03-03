# s3-azure-gateway-proxy-openshift

> **Production-ready OpenShift deployment for Flexify.IO S3-Azure Gateway Proxy**

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Helm Chart](https://img.shields.io/badge/helm-available-green.svg)](flchart/)
[![OpenShift Compatible](https://img.shields.io/badge/OpenShift-compatible-red.svg)]()

An open-source deployment automation toolkit developed by Amadeus IT Group S.A for running the **Flexify.IO S3-Azure Gateway Proxy** on OpenShift and Kubernetes. Flexify.IO transparently translates S3 API calls to Azure Blob Storage API, enabling seamless integration between S3-based applications and Azure Blob Storage infrastructure.

## Table of Contents

- [About](#about)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)
- [Support](#support)

## About

This repository provides production-ready deployment artifacts for the Flexify.IO S3-Azure Gateway Proxy on OpenShift platforms. It includes:

- **Kubernetes manifests** for direct deployment on any Kubernetes cluster
- **Helm charts** for streamlined installation and configuration management
- **Example configurations** for Azure Blob Storage integration
- **Health check scripts** for monitoring proxy availability

### Key Features

- **S3 API Compatibility**: Full S3 protocol support with transparent translation to Azure Blob Storage
- **OpenShift Ready**: Optimized for OpenShift deployments with proper security contexts
- **Helm Charts**: Simple, repeatable deployments using Helm
- **Stateful Configuration**: Persistent volume support for configuration and certificates
- **Health Monitoring**: Built-in health checks and monitoring integration
- **Multi-Cloud Ready**: Base templates adaptable for other cloud providers (AWS, GCP, etc.)

### Use Cases

- Migrate existing S3-based applications to Azure Blob Storage without code changes
- Bridge S3 and Azure ecosystems in hybrid cloud deployments
- Transparently route S3 traffic through gateway policies
- Enable S3 API compatibility for legacy applications on Azure

## Getting Started

### Prerequisites

- **Flexify.IO License**: Can be ordered at https://flexify.io/?tab=enterprise#get-flexify
- **OpenShift/Kubernetes**: Version 1.20+ (or OpenShift 4.6+)
- **kubectl or oc**: Command-line tools for cluster access
- **Helm**: Version 3.4+ (if using Helm deployment)
- **Azure Blob Storage Account**: Active Azure storage account with access credentials

### Installation

#### Option 1: Using Helm (Recommended)

1. **Clone the repository**
   ```bash
   git clone https://github.com/flexifyio/s3-azure-gateway-proxy-openshift.git
   cd s3-azure-gateway-proxy-openshift
   ```

2. **Create namespace** (optional)
   ```bash
   oc new-project flexify-gateway
   # or for standard Kubernetes
   kubectl create namespace flexify-gateway
   ```

3. **Prepare configuration** (see [Configuration](#configuration) section)
   ```bash
   # Edit values.yaml with your Azure credentials and proxy settings
   vi flchart/values.yaml
   ```

4. **Install Helm chart**
   ```bash
   helm install flexify-gateway ./flchart \
     --namespace flexify-gateway \
     -f flchart/values.yaml
   ```

5. **Verify deployment**
   ```bash
   oc rollout status deployment/flexify-gateway -n flexify-gateway
   oc logs -n flexify-gateway deployment/flexify-gateway
   ```

#### Option 2: Using Kubernetes Manifests

1. **Clone the repository**
   ```bash
   git clone https://github.com/flexifyio/s3-azure-gateway-proxy-openshift.git
   cd s3-azure-gateway-proxy-openshift
   ```

2. **Create namespace**
   ```bash
   oc new-project flexify-gateway
   ```

3. **Prepare manifests** (edit k8s/*.yaml with your configuration)
   ```bash
   vi k8s/app-properties-cm.yaml
   vi k8s/keys-secret.yaml
   ```

4. **Apply manifests**
   ```bash
   oc apply -f k8s/ -n flexify-gateway
   ```

5. **Verify deployment**
   ```bash
   oc rollout status deployment/flexify-gateway -n flexify-gateway
   ```

## Usage

### Basic S3 Operations

Once deployed, interact with the gateway using standard S3 tools:

```bash
# Configure S3 client
export AWS_ACCESS_KEY_ID=<your-key>
export AWS_SECRET_ACCESS_KEY=<your-secret>
export AWS_ENDPOINT_URL=<configured_endpoint>

# List containers (maps to Azure containers)
aws s3 ls

# Upload file
aws s3 cp myfile.txt s3://my-container/myfile.txt

# Download file
aws s3 cp s3://my-container/myfile.txt .
```

### Monitoring and Health Checks

The deployment includes built-in health checks:

```bash
# Check pod health
oc get pods -n flexify-gateway

# View proxy logs
oc logs -n flexify-gateway deployment/flexify-gateway -f

# Execute health check script
oc exec -it deployment/flexify-gateway -n flexify-gateway -- /scripts/health_check.sh
```

### Updating Configuration

To update proxy configuration without redeployment:

```bash
# Edit ConfigMap
oc edit cm flexify-config -n flexify-gateway

# Pod will automatically reload configuration
oc rollout restart deployment/flexify-gateway -n flexify-gateway
```

## Troubleshooting

### Pod Fails to Start

**Symptoms**: Pod in CrashLoopBackOff or Pending state

**Solutions**:
1. Check pod logs: `oc logs <pod-name> -n flexify-gateway`
2. Verify image is available: `oc describe pod <pod-name>`
3. Check resource availability: `oc describe node <node-name>`

### Cannot Connect to S3 Endpoint

**Symptoms**: Connection timeout or refused errors

**Solutions**:
1. Verify Service is running: `oc get svc -n flexify-gateway`
2. Check network policies: `oc get networkpolicies -n flexify-gateway`
3. Test connectivity from pod: `oc exec <pod> -- curl localhost:8080`

### Azure Credentials Error

**Symptoms**: 401/403 authentication errors

**Solutions**:
1. Verify secret is mounted: `oc describe pod <pod-name>`
2. Check credential format in secret
3. Verify Azure account has sufficient permissions

### Configuration Not Applied

**Symptoms**: Changes to ConfigMap not reflected in proxy

**Solutions**:
1. Restart deployment: `oc rollout restart deployment/flexify-gateway`
2. Verify ConfigMap was updated: `oc get cm flexify-config -o yaml`
3. Check if pod has proper mount permissions

## Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details on:

- How to report bugs
- How to suggest features
- Development and testing guidelines
- Pull request process

## Security

Please report security vulnerabilities responsibly. See [SECURITY.md](SECURITY.md) for details on:

- Vulnerability reporting process
- Security best practices
- Supported versions

## License

This project is licensed under the **Apache License 2.0** - see the [LICENSE](LICENSE) file for full details.

## Support

- **Bug Reports & Features**: [Open an issue](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/issues)
- **Security Issues**: See [SECURITY.md](SECURITY.md)
- **Documentation**: [Full Documentation](docs/) (coming soon)
- **Community**: [GitHub Discussions](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/discussions)
- **Flexify Support**: [Flexify Support](https://flexify.io/#contacts)

---


