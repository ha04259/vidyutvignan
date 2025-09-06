
> Cloud resources can be provisioned using three main approaches:  
> 1. **Cloud GUI** – through web-based dashboards  
> 2. **API/CLI** – using programmatic or command-line tools  
> 3. **IaC** – by defining infrastructure using code (e.g., Terraform, CloudFormation)
---

### What is Infrastructure as Code (IaC)?
Infrastructure as Code (IaC) is the practice of managing and provisioning computing infrastructure through machine-readable configuration files, rather than manual processes. It enables automation, consistency, and scalability in cloud and on-prem environments.

---
### Categories of IaC Tools

1. **Ad Hoc Scripts** – Custom shell or Python scripts used for quick, one-off automation  
2. **Configuration Management Tools** – Tools like Ansible, Chef, or Puppet that manage system state and enforce desired configurations  
3. **Server Templating Tools** – Tools such as Packer that create reusable machine images  
4. **Orchestration Tools** – Platforms like Kubernetes or ArgoCD that coordinate deployment and scaling of services  
5. **Provisioning Tools** – Tools like Terraform or CloudFormation that define and deploy infrastructure resources declaratively
---

### IaC Provisioning Tools Landscape

**Cloud-Specific Tools:**
1. **AWS CloudFormation** – for provisioning AWS resources  
2. **Azure Resource Manager (ARM)** – for managing Azure infrastructure  
3. **Google Cloud Deployment Manager** – for deploying resources on GCP  

**Cloud-Agnostic Tools:**
- **Terraform** – supports multiple cloud providers with a declarative syntax  
- **Pulumi** – enables infrastructure provisioning using general-purpose programming languages

---


Part - 2 Terraform Overview + Setup
---

### What is Terraform?

Terraform is an open-source tool for **building**, **changing**, and **versioning** infrastructure safely and efficiently. It brings **application software best practices**—such as modularity, reusability, and version control—to infrastructure management.

Terraform is **cloud-agnostic**, meaning it's compatible with many cloud providers and services, including AWS, Azure, GCP, and more. It uses **declarative configuration files** to define infrastructure, enabling automation, consistency, and scalability across environments.

---
### Common Patterns in Infrastructure Automation

**1. Terraform (Provisioning) vs Configuration Management (Ansible)**  
- **Terraform**: Defines and provisions infrastructure resources (e.g., VMs, networks, storage)  
- **Ansible**: Manages the configuration and state of existing systems (e.g., installing packages, updating files)

**2. Terraform (Provisioning) vs Server Templating (Packer)**  
- **Terraform**: Creates infrastructure dynamically at runtime  
- **Packer**: Builds reusable machine images ahead of time (e.g., AMIs, Docker images)

**3. Terraform (Provisioning) vs Orchestration (Kubernetes)**  
- **Terraform**: Provisions the underlying infrastructure (e.g., clusters, load balancers)  
- **Kubernetes**: Orchestrates containerized applications on top of provisioned infrastructure

---
