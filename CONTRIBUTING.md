# Contributing to s3-azure-gateway-proxy-openshift

Thank you for your interest in contributing! This document provides guidelines and instructions for contributing to this project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Ways to Contribute](#ways-to-contribute)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Making Changes](#making-changes)
- [Pull Request Process](#pull-request-process)
- [Style Guidelines](#style-guidelines)
- [Community](#community)

## Code of Conduct

This project adheres to the [Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code. Please report unacceptable behavior to the project maintainers.

## Ways to Contribute

There are many ways to contribute to this project:

### 🐛 Report Bugs

Found a bug? Please [open an issue](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/issues/new?template=bug_report.md) with:

- A clear, descriptive title
- Steps to reproduce the issue
- Expected vs. actual behavior
- Your environment:
  - OpenShift/Kubernetes version
  - Helm version (if applicable)
  - Operating system
  - Any relevant configuration
- Screenshots, logs, or stack traces if applicable

### 💡 Suggest Features or Improvements

Have an idea for improvement? Please [open a feature request](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/issues/new?template=feature_request.md) with:

- A clear description of the use case
- Proposed solution or implementation approach
- Any alternatives you've considered
- Expected benefits and potential impact

### 📝 Improve Documentation

- Fix typos or unclear explanations
- Add examples or tutorials
- Improve API/configuration documentation
- Translate documentation to other languages
- Add troubleshooting guides

### 💻 Submit Code Changes

- Fix bugs or resolve issues
- Implement new features
- Improve deployment efficiency
- Add or improve tests
- Optimize performance
- Refactor code for clarity

## Getting Started

### First-Time Contributors

New to the project? Look for issues labeled:
- `good first issue` - Simple issues perfect for beginners
- `help wanted` - Issues where we need community help
- `documentation` - Documentation improvements

### Before You Start

1. **Check for existing work**: Search issues and PRs to avoid duplicate effort
2. **Open an issue**: For significant changes, open an issue first to discuss your approach
3. **Wait for feedback**: Allow maintainers to provide guidance before starting major work
4. **Fork the repository**: Create your own copy to work on

## Development Setup

### Prerequisites

- **Git**: For version control
- **Docker/Podman**: For building and testing images
- **OpenShift/Kubernetes**: For testing deployments (v1.20+)
- **Helm**: For testing Helm charts (v3.5+)
- **kubectl/oc**: Command-line tools for cluster access
- **Text editor**: Your preferred editor (VS Code, vim, etc.)

### Setting Up Your Environment

1. **Fork the repository**
   
   Click the "Fork" button on GitHub to create your own copy.

2. **Clone your fork**
   ```bash
   git clone https://github.com/YOUR_USERNAME/s3-azure-gateway-proxy-openshift.git
   cd s3-azure-gateway-proxy-openshift
   ```

3. **Add upstream remote**
   ```bash
   git remote add upstream https://github.com/flexifyio/s3-azure-gateway-proxy-openshift.git
   ```

4. **Create development branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

5. **Verify your setup**
   ```bash
   # Check repository structure
   ls -la
   
   # Validate Helm chart syntax
   helm lint flchart/
   
   # Validate Kubernetes manifests
   kubeval k8s/*.yaml
   ```

## Making Changes

### Branch Naming

Use descriptive branch names with the following format:

```
<type>/<description>

Types: feature, fix, docs, refactor, test, chore

Examples:
- feature/add-pvc-backup
- fix/health-check-script
- docs/azure-setup-guide
- refactor/helm-templates
```

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Types**:
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation changes
- `refactor`: Code refactoring without feature or bug changes
- `test`: Adding or updating tests
- `chore`: Dependency updates, maintenance tasks

**Examples**:
```
feat(helm): add persistence backup configuration

fix(k8s): correct service port binding for Azure connectivity

docs(readme): add Azure credential setup instructions

refactor(templates): simplify deployment template structure
```

### Code Style

#### YAML Files

- **Indentation**: Use 2 spaces (not tabs)
- **Comments**: Add comments for complex configurations
- **Examples**: Include annotated examples in ConfigMaps and values.yaml

```yaml
# Good: Clear, well-indented
apiVersion: v1
kind: ConfigMap
metadata:
  name: example
data:
  key: value
```

#### Shell Scripts

- **Indentation**: Use 2 or 4 spaces consistently
- **Shebang**: Start with `#!/bin/bash`
- **Error handling**: Always check command results
- **Comments**: Document non-obvious logic

```bash
#!/bin/bash

set -e  # Exit on error

# Check prerequisites
if ! command -v kubectl &> /dev/null; then
  echo "Error: kubectl not found"
  exit 1
fi
```

### Testing Your Changes

#### Helm Chart Testing

```bash
# Lint the chart
helm lint flchart/

# Dry-run to see generated manifests
helm install flexify ./flchart --dry-run --debug
```

#### Kubernetes Manifests Testing

```bash
# Validate syntax
kubeval k8s/*.yaml

# Apply to test cluster
kubectl apply -f k8s/ --dry-run=client -o yaml
```

#### Integration Testing

```bash
# Deploy to test cluster
helm install test-release ./flchart -n test-ns --create-namespace

# Verify deployment
kubectl rollout status deployment/flexify-gateway -n test-ns
```

## Pull Request Process

### Before Creating a PR

1. **Update from upstream**:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Test your changes** thoroughly (see Testing section above)

3. **Update documentation** if needed (README, comments, etc.)

4. **Sign off on your changes**: Ensure you're providing contributions you own the rights to

### Creating a Pull Request

1. **Push your branch**:
   ```bash
   git push origin feature/your-feature-name
   ```

2. **Create PR on GitHub** with:
   - Clear title describing the change
   - Reference to related issues (#123)
   - Description of what changed and why
   - Screenshots or examples for visual changes
   - Checklist of testing performed

3. **PR Title Convention**:
   ```
   feat: Add configurable replica count to Helm chart
   fix: Correct Azure endpoint URL validation
   docs: Update installation instructions for Azure setup
   ```

### PR Checklist

- [ ] I have tested these changes locally
- [ ] I have updated documentation (README, comments, etc.)
- [ ] My changes follow the code style guidelines
- [ ] I have added comments for complex logic
- [ ] I have verified that this doesn't break existing functionality
- [ ] I have linked related issues
- [ ] My commit messages follow Conventional Commits format

### Review Process

1. **Automated checks** will run (linting, validation)
2. **Maintainers will review** your changes
3. **Feedback will be provided** on any issues or improvements
4. **You may need to make changes** based on review feedback
5. **Approval and merge** once all checks pass

### After Merge

Once merged, your changes will be:
- Included in the next release
- Recognized in release notes
- Available in the main branch immediately

## Style Guidelines

### Documentation

- Use clear, concise language
- Include examples where helpful
- Keep consistent formatting
- Link to related documentation
- Use markdown formatting correctly

### Comments and Documentation Strings

- Explain **why**, not just **what**
- Keep comments up-to-date with code
- Remove obsolete comments
- Use meaningful variable names

Example:
```bash
# Bad comment
x=5  # set x to 5

# Good comment
REPLICA_COUNT=5  # Default replicas for load distribution
```

### Configuration Files

- Add comments for non-obvious settings
- Group related configurations
- Provide examples in comments
- Document environment variables

Example:
```yaml
# Azure storage configuration
azure:
  # Storage account name
  account: "my-account"
  # Authentication key or connection string
  key: "key-value"
```

## Reporting Vulnerabilities

**Do not report security vulnerabilities through public issues.** See [SECURITY.md](SECURITY.md) for responsible disclosure process.

## Community

- **Questions?** Use [GitHub Discussions](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/discussions)
- **Chat**: Check for community Slack channel
- **Updates**: Watch releases and announcements
- **Contributing**: This guide is a living document—suggest improvements!

## License

By contributing, you agree that your contributions will be licensed under the same Apache License 2.0 that covers the project.

---

Thank you for contributing to make this project better! 🎉