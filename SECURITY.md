# Security Policy

## Supported Versions

We provide security updates for the following versions:

| Version | Status | Security Updates | End of Life |
|---------|--------|------------------|-------------|
| 1.x.x   | Active | ✅ Yes | TBD |
| 0.1.x   | Alpha  | ⚠️ Limited | Pre-release only |

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability, please report it responsibly and do not disclose it publicly until we have had time to address it.

### How to Report

**⚠️ IMPORTANT: Please do NOT report security vulnerabilities through public GitHub issues.**

Instead, please use one of the following methods:

#### 1. GitHub Security Advisories (Preferred)

1. Go to the [Security tab](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/security)
2. Click "Report a vulnerability"
3. Fill out the private security advisory form with details
4. Submit for review

This creates a private advisory that only you and the maintainers can see until it's published.

#### 2. Email

Send vulnerability reports to the maintainers with the subject line **"Security Vulnerability Report"**:
- Contact: security@flexify.io

### What to Include

Please provide as much of the following information as possible to help us understand and address the issue quickly:

- **Vulnerability Type**: (e.g., authentication bypass, configuration exposure, container escape)
- **Component**: Which part is affected (e.g., Helm chart templates, Kubernetes manifests, health check script)
- **Severity**: Your assessment of impact (Critical, High, Medium, Low)
- **Affected Files/Paths**: Specific files or configuration areas
- **Steps to Reproduce**: Clear, step-by-step instructions to trigger the issue
- **Proof of Concept**: Code, configuration, or demonstration (if applicable)
- **Impact Assessment**: 
  - What can an attacker do?
  - What data could be exposed or compromised?
  - How many deployments could be affected?
- **Suggested Fix**: Any remediation approaches you've considered

### Example Report

```
Vulnerability Type: Unencrypted Credential Storage
Component: k8s/keys-secret.yaml ConfigMap
Severity: High
Affected Files: k8s/app-properties-cm.yaml

Description:
The current deployment stores Azure credentials in a plain ConfigMap instead of a Secret.
This allows any user with pod access to read credentials.

Steps to Reproduce:
1. Deploy using provided manifests
2. Run: kubectl get cm flexify-config -o yaml
3. Credentials are visible in plain text

Suggested Fix:
Use Kubernetes Secrets for sensitive data and reference them via environment variables.
```

## Safe Harbor

We will not take legal action against researchers or individuals who:

1. Make a good faith effort to follow this policy
2. Avoid accessing, modifying, or deleting user data
3. Do not interrupt or degrade service availability
4. Keep information confidential until we disclose it publicly
5. Report findings promptly

We consider security research conducted under these terms to be:
- **Authorized** under applicable anti-hacking and computer fraud laws
- **Exempt** from restrictions in our Terms of Service
- **Appreciated** and will be acknowledged publicly (if desired)

## Security Practices for Contributors

### When Contributing

1. **Never commit secrets**:
   - No API keys, tokens, or credentials in code
   - Use environment variables or Secrets for sensitive data
   - Use `.gitignore` to prevent accidental commits

2. **Review dependencies**:
   - Check for known vulnerabilities before adding dependencies
   - Use tools like `npm audit`, `pip audit`, or similar
   - Keep dependencies up-to-date

3. **Follow secure coding practices**:
   - Input validation and sanitization
   - Proper error handling without exposing internals
   - Avoid hardcoded configurations
   - Use principle of least privilege

4. **Documentation**:
   - Document security-relevant design decisions
   - Include security considerations in comments
   - Update security documentation with changes

### Kubernetes Security Best Practices

This project follows Kubernetes security best practices:

- **Non-root containers**: Containers run as non-root users
- **Read-only filesystems**: Containers use read-only root when possible
- **Resource limits**: CPU and memory limits defined
- **Network policies**: Restrict pod-to-pod communication
- **RBAC**: Use role-based access control for permissions
- **Secrets management**: Use Kubernetes Secrets for credentials
- **Image scanning**: Container images scanned for vulnerabilities

## User Security Guidance

### For Users Deploying This

1. **Keep everything updated**:
   - Use the latest stable version
   - Regularly update Kubernetes/OpenShift
   - Update container images regularly

2. **Secure your credentials**:
   - Never store credentials in version control
   - Use Kubernetes Secrets properly
   - Rotate credentials regularly
   - Restrict Secret access with RBAC

3. **Network security**:
   - Use network policies to restrict traffic
   - Implement ingress authentication
   - Use TLS for all external connections
   - Consider service mesh security

4. **Monitoring and auditing**:
   - Monitor proxy logs for suspicious activity
   - Enable Kubernetes audit logging
   - Set up alerts for error conditions
   - Regularly review access logs

5. **Deployment hardening**:
   - Run in dedicated namespaces
   - Use resource quotas
   - Implement network policies
   - Configure RBAC properly
   - Use PodSecurityPolicies (or Pod Security Standards)

## Security Updates and Announcements

Security patches will be announced via:

1. **GitHub Security Advisories**: Published in the [Security tab](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/security/advisories)
2. **Release Notes**: Available on [Releases page](https://github.com/flexifyio/s3-azure-gateway-proxy-openshift/releases)
3. **GitHub Notifications**: Subscribe to repository updates
4. **Direct Email**: Contact maintainers for critical vulnerabilities

## Acknowledgments

We deeply appreciate security researchers and community members who responsibly report vulnerabilities:

| Researcher | Date | Vulnerability | Resolution |
|-----------|------|-----------------|-----------|
| To be updated | | | |

## Additional Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Kubernetes Security Best Practices](https://kubernetes.io/docs/concepts/security/)
- [Container Security Guide](https://kubernetes.io/docs/concepts/security/pod-security-standards/)
- [Azure Security Best Practices](https://docs.microsoft.com/en-us/azure/security/)

---

Thank you for helping keep this project secure! 🔒
