# AKS Repository - GitHub Copilot Instructions

> **Scope**: Repository-wide standards, conventions, and build commands.  
> **Module-Specific**: See `AGENTS.md` files in subdirectories.  
> **File-Specific**: See `.github/instructions/*.instructions.md` for targeted patterns.

## Repository Overview

This repository contains resources, examples, and documentation for the Azure Kubernetes Service (AKS) Engineering team:

- **Production Website**: <https://blog.aks.azure.com> (Docusaurus blog in `website/`)
- **Examples**: Real-world AKS scenarios, troubleshooting guides, and configurations
- **VHD Notes**: Node image release notes for AKS Ubuntu, Windows, Mariner, and Azure Linux
- **Community**: Open-source collaboration with AKS users and contributors

## Repository Structure

```text
AKS/
├── .github/
│   ├── copilot-instructions.md          # This file (repo-wide standards)
│   └── instructions/                    # File-specific patterns
│       └── website.blog.instructions.md # Blog post guidelines
├── website/                             # Docusaurus blog site
│   ├── AGENTS.md                       # Website module guide
│   ├── blog/                           # Blog posts
│   ├── src/                            # React components
│   └── package.json
├── examples/                            # AKS configuration examples
│   ├── fleet/                          # Azure Kubernetes Fleet Manager
│   ├── istio-based-service-mesh/       # Service mesh examples
│   ├── kube-prometheus/                # Monitoring setup
│   └── vnet/                           # Networking examples
├── vhd-notes/                          # Node image release notes
│   ├── aks-ubuntu/
│   ├── AKSMariner/
│   ├── AKSWindows/
│   └── AzureLinux/
├── README.md
└── LICENSE.MD
```

## General Standards

### Code Style

- **Markdown**: Follow [CommonMark](https://commonmark.org/) spec
- **YAML**: 2-space indentation, no tabs
- **Shell scripts**: Use shellcheck-compliant bash
- **TypeScript**: Follow [TypeScript ESLint](https://typescript-eslint.io/) recommended rules
- **React**: Functional components with hooks (no class components)

### File Naming

- **Markdown**: `kebab-case.md`
- **TypeScript/React**: `PascalCase.tsx` for components, `camelCase.ts` for utilities
- **CSS**: `kebab-case.css` or `ComponentName.module.css`
- **YAML**: `kebab-case.yaml` or `kebab-case.yml`
- **Shell scripts**: `kebab-case.sh`

### Documentation

- **README files**: Every major directory should have a README.md explaining its purpose
- **Code comments**: Explain *why*, not *what* (code should be self-documenting)
- **Examples**: Include both inline comments and accompanying README

## Build & Development

### Website (`website/`)

See `website/AGENTS.md` for detailed instructions.

**Quick commands**:

```bash
cd website
npm install       # Install dependencies
npm start         # Dev server (http://localhost:3000)
npm run build     # Production build
npm run typecheck # TypeScript validation
```

### Examples

Most examples are standalone YAML files or scripts:

```bash
# Apply example configuration
kubectl apply -f examples/fleet/kuard/deployment.yaml

# Run example script
bash examples/kernel-1095-issue/remediate.sh
```

### VHD Notes

Text files documenting node image changes. No build process required.

## Git Workflow

### Branch Strategy

- **master**: Main branch, stable code
- **Feature branches**: Short-lived, descriptive names (`add-fleet-examples`, `fix-blog-typo`)

### Commit Messages

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```text
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Formatting (no code change)
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples**:

```text
feat(website): add AKS MCP server blog post

docs(examples): add Fleet Manager multi-cluster setup

fix(website): correct broken link in webinar page
```

### Pull Requests

- **Title**: Clear, descriptive summary
- **Description**: What changed and why
- **Testing**: Describe how changes were validated
- **Screenshots**: Include for UI changes

## Azure/Kubernetes Best Practices

### YAML Manifests

```yaml
# Good: Explicit resource limits
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
  labels:
    app: example
spec:
  containers:
  - name: app
    image: myregistry.azurecr.io/app:v1.2.3
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

**Always include**:

- Resource requests and limits
- Labels for organization
- Image tags (never `:latest`)
- Security contexts where applicable

### Shell Scripts

```bash
#!/bin/bash
set -euo pipefail  # Exit on error, undefined vars, pipe failures

# Good: Check prerequisites
if ! command -v kubectl &> /dev/null; then
    echo "Error: kubectl not found"
    exit 1
fi

# Good: Use variables for reusability
CLUSTER_NAME="${CLUSTER_NAME:-my-aks-cluster}"
RESOURCE_GROUP="${RESOURCE_GROUP:-my-resource-group}"
```

### Documentation

- Include prerequisites (tools, versions, permissions)
- Provide example commands with placeholder values
- Explain expected outcomes
- Document troubleshooting steps

## Security & Privacy

### Secrets Management

- ❌ Never commit secrets, tokens, or credentials
- ✅ Use Azure Key Vault for sensitive data
- ✅ Use environment variables for configuration
- ✅ Add sensitive patterns to `.gitignore`

### Personal Information

- ❌ No personal email addresses (use generic team contacts)
- ❌ No customer-specific information
- ✅ Anonymize examples and logs

## Testing

### Website

```bash
cd website
npm run build     # Must succeed
npm run typecheck # Must pass
```

### YAML Manifests

```bash
# Validate Kubernetes YAML
kubectl apply --dry-run=client -f manifest.yaml

# Lint with kubeval (if available)
kubeval manifest.yaml
```

### Shell Scripts

```bash
# Lint with shellcheck
shellcheck script.sh

# Test execution in clean environment
docker run --rm -v $(pwd):/work -w /work ubuntu:22.04 bash script.sh
```

## Common Tasks

### Add Example Configuration

1. Create directory: `examples/my-example/`
2. Add YAML/scripts
3. Create `README.md` with:
   - Purpose
   - Prerequisites
   - Usage instructions
   - Expected output
4. Test in clean AKS cluster
5. Commit with descriptive message

### Update VHD Notes

1. Locate appropriate directory: `vhd-notes/{image-type}/`
2. Create/update file: `YYYY.MM.DD.txt` or `YYYYMMDD.VV.V.txt`
3. Document changes in chronological order
4. Include package versions, patches, feature updates

### Add Blog Post

See `.github/instructions/website.blog.instructions.md` and `website/AGENTS.md`.

## Support & Contact

- **Issues**: <https://github.com/Azure/AKS/issues>
- **Discussions**: <https://github.com/Azure/AKS/discussions>
- **Email**: brian.redmond@microsoft.com
- **Community Calls**: See <https://blog.aks.azure.com/webinars>

## Related Resources

- **AKS Documentation**: <https://learn.microsoft.com/azure/aks>
- **AKS Roadmap**: <https://github.com/orgs/Azure/projects/685>
- **AKS Releases**: <https://github.com/Azure/AKS/releases>
- **Troubleshooting**: <https://learn.microsoft.com/troubleshoot/azure/azure-kubernetes>

---

**Last Updated**: 2025-10-04  
**Maintainer**: AKS Engineering Team
