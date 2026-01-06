# Infrastructure Deployment and Management

This repository contains scripts and Makefile commands for deploying and managing infrastructure on Azure Kubernetes Service (AKS) with MongoDB and Typesense databases.

## 📋 Prerequisites

### Required Tools

Before running any commands, ensure you have the following installed:

1. **Python 3.8+** (for DNS updates)
   ```bash
   # Check if installed
   python3 --version
   
   # Ubuntu/Debian installation
   sudo apt update && sudo apt install python3 python3-pip
   
   # macOS installation
   brew install python3
   ```

2. **Azure CLI** (for Azure operations)
   ```bash
   # Check if installed
   az --version

   # Installation
   # macOS
   brew install azure-cli

   # Linux
   curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

   # Windows (PowerShell)
   Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi
   ```

3. **kubectl** (for Kubernetes operations)
   ```bash
   # Check if installed
   kubectl version --client


   # Installation
   # macOS
   brew install kubectl

   # Linux
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

   # Azure CLI can also install kubectl
   az aks install-cli
   ```

4. **Terraform** (Infrastructure as Code)
   ```bash
   # Check if installed
   terraform --version


   # Installation

   # macOS
   brew install terraform

   # Linux
   sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
   wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee/etc/apt/sources.list.d/hashicorp.list
   sudo apt update && sudo apt install terraform
   ```

5. **GNU Make** (for running workflows)
   ```bash
   # Check if installed
   make --version


   # Installation

   # Ubuntu/Debian
   sudo apt install make

   # macOS (installed with Xcode command line tools)
   xcode-select --install
   ```

🔐 Authentication Setup
6. **Azure Login** (Required)
   ```bash# Login to Azure
   az login

   # List subscriptions
   az account list --output table

   # Set the correct subscription
   az account set --subscription "Your-Subscription-Name"

   # Verify login
   az account show
   ```
   **Kubernetes Context**

   AKS credentials are automatically configured by the scripts:
   ```bash
   az aks get-credentials
   ```

   ✅ No manual Kubernetes configuration is required.


🚀 ## Quick Start
7.   **First-Time Setup**
   ```bash
   # Clone the repository
   git clone https://github.com/valoriz/ambient.git
   cd .ambient/devops/scripts/internal/Iac/internal-projects/environment/common


   ## Login to Azure
   az login
   # Verify all required tools
   make quick-ref
   ```


 ===============================
 ## MAKEFILE WORKFLOWS
 ===============================

  This repository is operated entirely via Makefile commands.
  All infrastructure, DNS, database backups/restores, and Kubernetes
  deployments must be executed using the commands below.

 ===============================
 ## INFRASTRUCTURE
 ===============================

 make apply-infra        # Create infrastructure
 make plan-infra         # Show planned infrastructure changes
 make destroy-infra      # Destroy infrastructure (UNSAFE – no backups)

 ===============================
 ## DNS MANAGEMENT
 ===============================

 make dns-update         # Create / update DNS records
 make dns-delete         # Delete DNS records

  ===============================
  ## MONGODB OPERATIONS
  ===============================

 make mongo-snapshot           # Create MongoDB snapshot (pre-destroy)
 make mongo-restore-disk-only  # Restore MongoDB disk
 make mongo-attach             # Attach MongoDB disk to pod
 make mongo-restore            # Restore MongoDB (disk + attach)

 ===============================
 ## TYPESENSE OPERATIONS
 ===============================

 make typesense-snapshot       # Create Typesense snapshot
 make typesense-restore        # Restore Typesense (disk + PV/PVC + attach)

 ===============================
 ## KUBERNETES
 ===============================

 make k8s-deploy          # Deploy Kubernetes resources

 ===============================
 ## FULL WORKFLOWS
 ===============================

 make apply               # Full deployment workflow:
                           # 1. Infrastructure creation
                           # 2. DNS update
                           # 3. MongoDB disk restore
                           # 4. MongoDB disk attach
                           # 5. Typesense restore
                           # 6. Kubernetes deployment

 make destroy             # Safe destroy workflow:
                           # 1. MongoDB snapshot
                           # 2. Typesense snapshot
                           # 3. DNS deletion
                           # 4. Infrastructure destroy 

  ===============================
 ## HELPERS
  ===============================

 make workflow-deploy     # Print deployment workflow steps
 make workflow-migrate    # Print migration workflow steps
 make quick-ref           # Print all available Makefile commands
