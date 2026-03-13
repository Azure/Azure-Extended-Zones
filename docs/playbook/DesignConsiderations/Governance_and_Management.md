# Governance and Management

This section provides guidance on **governance** and **management** considerations for workloads deployed to **Azure Extended Zones**. It covers resource organisation, policy enforcement, tagging strategies, cost management, quotas, virtual machine connectivity, image management, and update management. Where applicable, Azure Extended Zone–specific constraints and recommendations are highlighted.

## Table of Contents

- [Resource Organisation](#resource-organisation)
  - [Parent Region](#parent-region)
  - [Subscriptions](#subscriptions)
  - [Management Groups](#management-groups)
  - [Resource Groups](#resource-groups)
  - [Naming Conventions](#naming-conventions)
- [Governance](#governance)
  - [Azure Policy](#azure-policy)
  - [Azure Tags](#azure-tags)
  - [Cost Management](#cost-management)
  - [Quotas](#quotas)
- [Management](#management)
  - [Connecting to Virtual Machines](#connecting-to-virtual-machines)
    - [Azure Bastion](#azure-bastion)
  - [Azure Compute Gallery Images](#azure-compute-gallery-images)
  - [Azure Advisor](#azure-advisor)
  - [Update Management](#update-management)
    - [Automatic VM Guest Patching](#automatic-vm-guest-patching)
    - [Azure Update Manager](#azure-update-manager)

## Resource Organisation

Resource organisation within Azure necessitates a structured approach to secure, manage, and optimise cloud resources effectively. This section covers the concept of a parent region, subscriptions, management groups, resource groups, and naming conventions.

### Parent Region

As outlined in the Overview of Azure Extended Zones, the control plane for services deployed to an **Azure Extended Zone** remains situated within an Azure Region while the data plane is deployed at the Extended Zone site. Consequently, utilising services in an Azure Extended Zone necessitates the employment of certain services, resources, and features within an Azure Region, referred to as the 'parent region.'

When deploying resources within an **Azure Extended Zone**, the resource overview will indicate the Azure Extended Zone. The portal views for the resource type, such as the Virtual Network View, will display the parent region.

**Virtual Network View:**

*(screenshot)*

**Virtual Network Resource View:**

*(screenshot)*

### Subscriptions

An **Azure subscription** serves several purposes, including:

- A legal agreement
- A payment agreement
- A boundary of scale
- An administrative boundary

Every Azure resource is logically associated with one subscription. When creating a resource, you select which Azure subscription to deploy it to.

Subscriptions are not tied to a specific Azure region or Azure Extended Zone, though each Azure resource deploys to only one region or extended zone.

Access to **Azure Extended Zones** is regulated and managed through a controlled access process. It is essential to register the subscription(s) intended for Extended Zone deployments. For further information, please refer to the Onboarding section.

It is advisable to consider dedicating specific subscriptions for deployments within an **Azure Extended Zone**. This approach facilitates the monitoring of quotas within the extended zone and mitigates potential confusion with resources deployed in the parent region.

### Management Groups

If your organisation manages multiple Azure subscriptions, an efficient method is essential for overseeing access, policies, and compliance. **Management groups** offer a governance scope above subscriptions. By organising subscriptions within management groups, any governance conditions applied will cascade by inheritance to all associated subscriptions.

Management groups are not tied to any specific Azure Region and can be utilised with both Azure Extended Zone allocated subscriptions and Azure Extended Zone deployed resources.

### Resource Groups

A **resource group** is a container that facilitates the management of related resources within an Azure solution. It serves as a scope for applying Role-based Access Control (RBAC) and Azure Policy for governance purposes. While resource groups can encompass resources provisioned in any Azure Region, they are associated with a specific region for metadata storage purposes.

> **Azure Extended Zone consideration:** Creating a resource group within an Azure Extended Zone is not possible. Resource groups should be established in an Azure parent region. To avoid any confusion, it is recommended to adopt a naming convention that clearly indicates which resource groups are associated with an Azure Extended Zone.

### Naming Conventions

*(content to be added)*

## Governance

### Azure Policy

**Azure Policy** assists in enforcing organisational standards and assessing compliance at scale. Through its compliance dashboard, it provides an aggregated view for evaluating the overall state of the environment, with the capability to drill down to per-resource and per-policy details. It also aids in bringing resources into compliance through bulk remediation for existing resources and automatic remediation for new resources.

**Azure Policy** is a non-regional service and will evaluate resources deployed to an **Azure Extended Zone** for adherence to assigned policy definitions.

### Azure Tags

**Tags** are metadata elements that can be applied to Azure resources, consisting of key-value pairs that assist in identifying resources based on parameters pertinent to organisational needs.

Tags are supported within an **Azure Extended Zone** and are recommended as part of a comprehensive governance strategy for managing the Azure environment.

### Cost Management

*(content to be added)*

### Quotas

*(content to be added)*

## Management

Azure offers a comprehensive suite of tools and services designed for effective cloud resource management. The management capabilities in Azure encompass a broad range of tasks aimed at maintaining the health, performance, and security of your applications and infrastructure. These tasks include deploying and configuring resources, monitoring their performance, and ensuring compliance with organisational policies.

### Connecting to Virtual Machines

#### Azure Bastion

**Azure Bastion** is a fully managed Platform as a Service (PaaS) offering that enables secure connectivity to virtual machines using private IP addresses. It facilitates secure RDP/SSH access directly over TLS from the Azure portal, as well as through the native SSH or RDP client on your local machine. By leveraging Azure Bastion, virtual machines do not require public IP addresses, agents, or any specialised client software to connect.

> **Azure Extended Zone consideration:** To connect to a virtual machine deployed in an Azure Extended Zone, it is advisable to deploy the **Azure Bastion** service to the parent region and leverage the Azure Bastion service support for global virtual network peering. The Azure Bastion Basic, Standard, and Premium SKUs support connecting to virtual machines in peered virtual networks; however, the Developer SKU does not. For additional information, please refer to [About Azure Bastion](https://learn.microsoft.com/azure/bastion/bastion-overview) on Microsoft Learn.

### Azure Compute Gallery Images

**Azure Compute Gallery** images serve as a valuable resource for building virtual machines within **Azure Extended Zones**. However, it is important to note that this functionality is not yet accessible through the Azure Portal (UX).

Azure Compute Gallery images hosted in a primary Azure region can be replicated to Azure Extended Zones by utilising the appropriate command, as demonstrated below:

```bash
az sig image-version update --resource-group MyResourceGroup --gallery-name MyGallery --gallery-image-definition PlaceholderImage --gallery-image-version 0.0.1 --target-edge-zones australiaeast=perth=1=standardssd_lrs
```

> **Note:** `premium_lrs` is also supported as a storage type for image versions in the Perth Azure Extended Zone.

If you receive an error message about `Subscription '<SubID>' is not enrolled for Edge Zone access`, please contact MSFT Support and provide your subscription ID so access to the feature can be granted on the back end. Alternatively, it is possible to self-serve this by creating a managed identity and giving it access to an Azure compute gallery via an API operation.

### Azure Advisor

**Azure Advisor** is a non-regional service that provides recommendations applicable to resources deployed in an **Azure Extended Zone**.

### Update Management

#### Automatic VM Guest Patching

Automatic virtual machine guest patching is supported for virtual machines deployed within an **Azure Extended Zone**. This feature should be considered as part of your update management strategy, adhering to the guidelines specified in the [official documentation](https://learn.microsoft.com/azure/virtual-machines/automatic-vm-guest-patching).

#### Azure Update Manager

**Azure Update Manager** is a comprehensive service designed to assist in managing and governing updates for all your machines. It provides a unified view for monitoring Windows and Linux update compliance, allowing for real-time updates or the scheduling of updates within designated maintenance windows.

Integrating **Azure Update Manager** into your update management strategy is advisable for virtual machines deployed in **Azure Extended Zones**.

