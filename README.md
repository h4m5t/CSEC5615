# CSEC5615
**Cloud-Native Security Configuration Drift Detection and Real-Time Self-Healing System**

A Kubernetes security project that automatically detects and fixes unauthorized configuration changes using GitOps and Azure Policy.

## What This Project Does

- **Detects** when someone tries to deploy insecure containers
- **Blocks** dangerous configurations automatically  
- **Restores** secure settings within 30 seconds
- **Monitors** and alerts on security violations

## How It Works

```
Git Repo → Flux CD → Kubernetes → Azure Policy → Auto-Fix
```

1. **Git** stores secure configurations
2. **Flux** watches for changes and syncs to cluster
3. **Azure Policy** blocks bad deployments
4. **System** auto-restores if configs drift from baseline

## Directory Structure

```
CSEC5615/
├── PrivilegedAttack/
│   └── nginx-privileged-deployment.yaml    # Malicious config for testing attacks
├── clusters/
│   └── myK8SCluster/
│       ├── flux-kustomization.yaml         # Flux application sync config
│       ├── flux-system/                    # Flux CD core components
│       ├── kustomization.yaml              # Cluster-level configuration
│       └── nginx-safe.yaml                 # Secure nginx baseline definition
├── policies/
│   ├── privileged-container-policy.json    # Azure Policy blocking privileged containers
│   └── security-baseline.yaml              # Security requirements definition
├── fluxcd-gitops-report_en.sh             # Evidence collection script
└── README.md                               # This file
```

## Key Files

### Security Configurations
- **`nginx-safe.yaml`** - Defines secure container settings (non-privileged, non-root user)
- **`security-baseline.yaml`** - Security requirements and standards

### Attack Simulation
- **`nginx-privileged-deployment.yaml`** - Malicious configuration used to test drift detection

### Policy Enforcement
- **`privileged-container-policy.json`** - Azure Policy that blocks dangerous container configurations
- **`flux-kustomization.yaml`** - Configures Flux to enforce Git-based security baseline

### Automation & Evidence
- **`fluxcd-gitops-report_en.sh`** - Comprehensive report generator showing system effectiveness

## Quick Demo

```bash
# 1. Deploy secure baseline
kubectl apply -f clusters/myK8SCluster/nginx-safe.yaml

# 2. Simulate privileged container attack
kubectl apply -f PrivilegedAttack/nginx-privileged-deployment.yaml

# 3. Watch auto-healing (should restore within 30 seconds)
kubectl get deployment nginx-safe -w

# 4. Generate evidence report
./fluxcd-gitops-report_en.sh > security-evidence-report.txt
```

## Attack Scenarios Tested

- ✅ **Privileged Container Deployment** → Auto-blocked and restored
- ✅ **Root User Exploitation** → Denied by security policies  
- ✅ **Insecure Image Injection** → Replaced with secure baseline
- ✅ **Security Context Tampering** → Auto-remediated by Flux

## Evidence Collection

Run the evidence script to generate proof:
```bash
./fluxcd-gitops-report_en.sh
```

This generates comprehensive logs showing:
- Flux system health verification
- Configuration drift detection events
- Automatic remediation timeline
- Security policy compliance status

## Technologies Used

- **Kubernetes (AKS)** - Container orchestration platform
- **Flux CD** - GitOps continuous delivery operator
- **Azure Policy** - Cloud-native security policy enforcement
- **Git** - Single source of truth for configurations
