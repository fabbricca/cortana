# cortana

**cortana** is a fully declarative home Kubernetes cluster running on Talos Linux and provisioned via Terraform, with GitOps-driven application and infrastructure delivery using Flux CD.

It uses:
- **Talos Linux** as an immutable OS for Kubernetes nodes
- **Terraform** to provision infrastructure and bootstrap machine configs
- **Flux CD** to reconcile Kubernetes manifests and applications from Git

---

## 🧩 Architecture

```
├── infra/ # Terraform code for infra provisioning and Talos bootstrapping
├── cluster/ # Flux root and overlays for cluster components
├── apps/ # Flux-managed applications (Helm/Kustomize)
├── .github/workflows/ # CI/CD pipelines (Terraform, Checkov, Flux updates)
├── .sops.yaml # SOPS configuration for secret management
├── .checkov.yaml # Security policy checks for Terraform
├── LICENSE
└── README.md
```

---

## 🚀 Getting Started

### ✅ Prerequisites

- Terraform ≥ 1.11.3
- Flux CLI ≥ 2.5.1
- `talosctl` and `kubectl`
- GitHub personal access token
- Machines (bare metal or VM)

---

### 🏗️ Infrastructure Provisioning

1. **Clone the repository**
   ```
   git clone https://github.com/fabbricca/cortana.git
   cd cortana
   ```
2. **Set up Terraform**
    ```
    cd infra
    terraform init
    terraform apply
    ```
    This provisions VMs and generates Talos configs and a kubeconfig.

## ⚡ Flux Bootstrap
Install and configure Flux:
```
flux bootstrap github \
  --owner=<your-username> \
  --repository=<your-repo> \
  --branch=main \
  --path=./cluster \
  --personal
```
Flux will automatically apply everything under cluster/ and apps/.

## 🔐 Secrets Management
Secrets are encrypted using SOPS

Use:
```
sops -e secret.yaml > secret.enc.yaml
```

## 🔁 CI/CD
GitHub Actions run:

Terraform plan & apply checks

Checkov security scans

Flux image update automation

Workflows are defined in .github/workflows/.

## 🧪 Testing & Linting
Run Terraform and Checkov locally:
```
terraform fmt -recursive
checkov -d infra/
```

## 🤝 Contributing
Fork the repository

Create a feature branch

Submit a PR to main

PRs are validated by CI (Terraform, Checkov, Flux)

## 📄 License
This project is licensed under the MIT License.