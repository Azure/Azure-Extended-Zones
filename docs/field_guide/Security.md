# Security, Compliance and Data Residency

This section provides guidance on **security**, **compliance**, and **data residency** considerations for workloads deployed to **Azure Extended Zones**. It covers security benchmarks, posture management, threat detection, identity and access controls, network security services, encryption and key management, as well as compliance standards and data residency requirements. Where applicable, Azure Extended Zone–specific constraints and recommendations are highlighted.

## Table of Contents

- [Security](#security)
  - [Microsoft Cloud Security Benchmark](#microsoft-cloud-security-benchmark)
  - [Cloud Security Posture Management](#cloud-security-posture-management)
  - [Security Information and Event Management](#security-information-and-event-management)
  - [Identity and Access Management](#identity-and-access-management)
  - [Secrets and Key Management](#secrets-and-key-management)
  - [Network Security](#network-security)
    - [Network Security Groups](#network-security-groups)
    - [Azure Private Link](#azure-private-link)
    - [DDoS Protection](#ddos-protection)
    - [Azure Firewall](#azure-firewall)
  - [Encryption](#encryption)
- [Compliance and Data Residency](#compliance-and-data-residency)
  - [Data Residency](#data-residency)
  - [Compliance and Regulatory Standards](#compliance-and-regulatory-standards)
  - [Extended Security Updates](#extended-security-updates)

## Security

Azure provides a comprehensive framework to ensure the security, compliance, and proper data residency of your cloud resources. This framework is designed to protect your data, adhere to regulatory requirements, and ensure that data is stored and processed in appropriate locations.

### Microsoft Cloud Security Benchmark

The **Microsoft Cloud Security Benchmark (MCSB)** provides prescriptive best practices and recommendations that enhance the security of workloads, data, and services on Azure and across your multi-cloud environment. This benchmark focuses on cloud-centric control areas with input from various Microsoft and industry security guidelines, including:

- **Cloud Adoption Framework:** Offers guidance on strategy, roles and responsibilities, Azure Top 10 Security Best Practices, and reference implementation.
- **Azure Well-Architected Framework:** Provides insights on securing your workloads on Azure.
- **Chief Information Security Officer (CISO) Workshop:** Delivers program guidance and strategies to accelerate security modernisation using Zero Trust principles.
- **Industry and cloud service provider security best practice standards and frameworks:** Including the AWS Well-Architected Framework, Center for Internet Security (CIS) Controls, NIST, and PCI-DSS.

Consult MCSB and service baselines for any service deployed to an **Azure Extended Zone** to plan the security configuration of each service.

### Cloud Security Posture Management

**Microsoft Defender for Cloud** is a comprehensive cloud-native application protection platform (CNAPP) designed to safeguard cloud-based applications from a wide range of cyber threats and vulnerabilities. Defender for Cloud integrates the following capabilities:

- **Development Security Operations (DevSecOps):** Unifies security management at the code level across multicloud and multiple-pipeline environments.
- **Cloud Security Posture Management (CSPM):** Surfaces actionable insights to prevent breaches and strengthen security posture.
- **Cloud Workload Protection Platform (CWPP):** Provides specific protections for servers, containers, storage, databases, and other workloads.

Use **Microsoft Defender for Cloud** to manage and improve the security posture of the **Azure Extended Zone**.

### Security Information and Event Management

**Microsoft Sentinel** is a scalable, cloud-native Security Information and Event Management (SIEM) system that offers a comprehensive solution for both SIEM and Security Orchestration, Automation, and Response (SOAR). This service provides robust cyber threat detection, investigation, response, and proactive threat hunting across your entire enterprise infrastructure.

**Microsoft Sentinel** seamlessly integrates with proven Azure services such as **Log Analytics** and **Logic Apps**, enhancing investigation and detection capabilities with advanced artificial intelligence. For effective SIEM and SOAR within the **Azure Extended Zone**, **Microsoft Sentinel** is highly recommended. Detailed information on deploying the Log Analytics workspace for Microsoft Sentinel can be found in the [Observability and Monitoring](Observability_and_Monitoring.md) section.

### Identity and Access Management

Use Microsoft Entra ID, Azure RBAC, and Privileged Identity Management (PIM) to enforce least-privilege access for Azure Extended Zone workloads.

Recommended practices:

- Use role-based access assignments instead of broad owner-level permissions.
- Separate platform, security, and operations responsibilities with dedicated roles.
- Enable conditional access and multifactor authentication for privileged access paths.
- Review privileged role assignments regularly and remove stale access.


### Secrets and Key Management

Use **Azure Key Vault** in the parent region to store and manage secrets, keys, and certificates used by workloads in Azure Extended Zones.

Recommended practices:

- Keep application secrets out of code and configuration files.
- Use managed identities for secret retrieval where possible.
- Rotate secrets and certificates on a defined schedule.
- Enable logging and alerting for secret and key access operations.

### Network Security

The following Azure network security services can be used to protect workloads deployed to **Azure Extended Zones**. Where the service behavior differs in an Extended Zone context, specific considerations are noted.

#### Network Security Groups

Network Security Groups (NSG) will automatically be established in the resource group's region corresponding to the Azure Extended Zone VM — i.e. the parent region.

#### Azure Private Link

**Azure Private Link** enables secure and private access to Azure PaaS services — such as **Azure Storage** and **SQL Database** — as well as customer-owned and partner services hosted on Azure, via a private endpoint within your virtual network. This ensures that traffic between your virtual network and the service traverses the Microsoft backbone network, eliminating the need for exposure to the public internet.

#### DDoS Protection

The DDoS Protection plan must be created in the parent region rather than the Azure Extended Zone. Use the parent region DDoS Protection plan to safeguard resources deployed in Azure Extended Zones.

For protection against L7 application layer attacks, deploy **Azure Web Application Firewall (WAF)** with either **Azure Front Door Premium** or **Application Gateway WAF v2** SKU. A multi-layered security approach, incorporating network, application, and data protection, should always be implemented.

#### Azure Firewall

**Azure Firewall** is in preview for Azure Extended Zones.

### Encryption

Encryption for Azure Extended Zone workloads should align with standard Azure encryption controls for data at rest and data in transit.

Recommended practices:

- Use platform-managed encryption by default for supported services.
- Use customer-managed keys where regulatory or internal policy requires key ownership.
- Enforce TLS for application and management traffic.
- Validate encryption configuration through policy and posture monitoring controls.

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


