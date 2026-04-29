# Security, Compliance and Data Residency

This section provides guidance on **security**, **compliance**, and **data residency** considerations for workloads deployed to **Azure Extended Zones**. It covers secrets and key management, network security services, compliance standards, data residency requirements, and extended security updates. Where applicable, Azure Extended Zone–specific constraints and recommendations are highlighted.

## Table of Contents

- [Security](#security)
  - [Secrets and Key Management](#secrets-and-key-management)
  - [Network Security](#network-security)
    - [Network Security Groups](#network-security-groups)
    - [DDoS Protection](#ddos-protection)
    - [Azure Firewall](#azure-firewall)
- [Compliance and Data Residency](#compliance-and-data-residency)
  - [Data Residency](#data-residency)
  - [Compliance and Regulatory Standards](#compliance-and-regulatory-standards)
  - [Extended Security Updates](#extended-security-updates)

## Security

Azure provides a comprehensive framework to ensure the security, compliance, and proper data residency of your cloud resources. This framework is designed to protect your data, adhere to regulatory requirements, and ensure that data is stored and processed in appropriate locations.

### Secrets and Key Management

The **Azure Key Vault** service is supported in the Extended Zone to store and manage secrets, keys, and certificates used by workloads.


### Network Security

The following Azure network security services can be used to protect workloads deployed to **Azure Extended Zones**. Where the service behavior differs in an Extended Zone context, specific considerations are noted.

#### Network Security Groups

Network Security Groups (NSG) can be utilized to protect workloads in the Extended Zone, however, will need to be created in the parent region.


#### DDoS Protection

The DDoS Protection plan must be created in the parent region rather than the Azure Extended Zone. Use the parent region DDoS Protection plan to safeguard resources deployed in Azure Extended Zones.


#### Azure Firewall

[**Azure Firewall**](./preview_services/AzureFirewall.md) is in preview for Azure Extended Zones.

## Compliance and Data Residency

### Data Residency

Use an **Azure Extended Zone** when workloads require data residency within a specific geography in the Microsoft cloud.

### Compliance and Regulatory Standards

**Azure Extended Zones** meet the following compliance standards:

- **ISO 27001**
- **SOC 2 Type II**
- **PCI DSS**

### Extended Security Updates

**Extended Security Updates (ESUs)** will be provided at no additional cost for customers using Azure services. This includes workloads operating on **Azure Virtual Machines**, **Azure Dedicated Host**, **Azure VMware Solutions**, **Nutanix Cloud Clusters on Azure**, and **Azure Stack Hub/Edge/HCI**. In the **Azure Extended Zone**, eligible virtual machines configured to receive updates will automatically benefit from ESUs, ensuring continuous compliance and security updates without incurring extra charges.

For more information refer to the [Extended Security Updates FAQ](https://learn.microsoft.com/lifecycle/faq/extended-security-updates) in Microsoft Docs.


