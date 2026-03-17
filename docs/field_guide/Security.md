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

It is recommended to consult MCSB and the service baselines for any service deployed to an **Azure Extended Zone** to plan for the security configuration of each service.

### Cloud Security Posture Management

**Microsoft Defender for Cloud** is a comprehensive cloud-native application protection platform (CNAPP) designed to safeguard cloud-based applications from a wide range of cyber threats and vulnerabilities. Defender for Cloud integrates the following capabilities:

- **Development Security Operations (DevSecOps):** Unifies security management at the code level across multicloud and multiple-pipeline environments.
- **Cloud Security Posture Management (CSPM):** Surfaces actionable insights to prevent breaches and strengthen security posture.
- **Cloud Workload Protection Platform (CWPP):** Provides specific protections for servers, containers, storage, databases, and other workloads.

It is advisable to utilise **Microsoft Defender for Cloud** to manage and enhance the security posture of the **Azure Extended Zone**.

### Security Information and Event Management

**Microsoft Sentinel** is a scalable, cloud-native Security Information and Event Management (SIEM) system that offers a comprehensive solution for both SIEM and Security Orchestration, Automation, and Response (SOAR). This service provides robust cyber threat detection, investigation, response, and proactive threat hunting across your entire enterprise infrastructure.

**Microsoft Sentinel** seamlessly integrates with proven Azure services such as **Log Analytics** and **Logic Apps**, enhancing investigation and detection capabilities with advanced artificial intelligence. For effective SIEM and SOAR within the **Azure Extended Zone**, **Microsoft Sentinel** is highly recommended. Detailed information on deploying the Log Analytics workspace for Microsoft Sentinel can be found in the [Observability and Monitoring](Observability_and_Monitoring.md) section.

### Identity and Access Management

*(content to be added)*

### Secrets and Key Management

*(content to be added)*

### Network Security

The following Azure network security services can be used to protect workloads deployed to **Azure Extended Zones**. Where the service behaviour differs in an Extended Zone context, specific considerations are noted.

#### Network Security Groups

An **Azure Network Security Group (NSG)** filters network traffic between Azure resources within an Azure virtual network. Each NSG comprises security rules that either allow or deny inbound and outbound network traffic based on specified source and destination, port, and protocol parameters.

> **Azure Extended Zone consideration:** When a virtual machine is created within an Azure Extended Zone and the option to create an NSG is selected, the NSG will automatically be established in the resource group's region corresponding to the Azure Extended Zone VM — i.e. the parent region.

#### Azure Private Link

**Azure Private Link** enables secure and private access to Azure PaaS services — such as **Azure Storage** and **SQL Database** — as well as customer-owned and partner services hosted on Azure, via a private endpoint within your virtual network. This ensures that traffic between your virtual network and the service traverses the Microsoft backbone network, eliminating the need for exposure to the public internet.

#### DDoS Protection

**Azure DDoS Protection**, combined with application design best practices, provides enhanced DDoS mitigation features to defend against DDoS attacks. It is automatically tuned to help protect your specific Azure resources in a virtual network, and is simple to enable on any new or existing virtual network with no application or resource changes required.

**Azure DDoS Protection** protects at layer 3 and layer 4 network layers. For web application protection at layer 7, you need to add protection at the application layer using a WAF offering.

> **Azure Extended Zone consideration:** The DDoS Protection plan must be created in the parent region rather than the Azure Extended Zone. Customers and partners should utilise the DDoS Protection plan in the parent region to safeguard their resources deployed in Azure Extended Zones.

For protection against L7 application layer attacks, deploy **Azure Web Application Firewall (WAF)** with either **Azure Front Door Premium** or **Application Gateway WAF v2** SKU. A multi-layered security approach, incorporating network, application, and data protection, should always be implemented.

#### Azure Firewall

**Azure Firewall** is a cloud-native and intelligent network firewall security service designed to offer superior threat protection for cloud workloads operating within Azure. It is a fully stateful firewall service with built-in high availability and unlimited cloud scalability, providing comprehensive traffic inspection for both east-west and north-south traffic flows.

### Encryption

*(content to be added)*

## Compliance and Data Residency

### Data Residency

You might choose to utilise an **Azure Extended Zone** to meet data residency requirements for your workloads within the Microsoft cloud.

It is important to note that in certain limited scenarios, data may be stored outside of your selected geography. For further details, please refer to the [Data residency in Azure](https://learn.microsoft.com/azure/reliability/availability-zones-overview) documentation.

Additionally, if you employ a broad range of Azure services, multiple regions may be necessary as not all services are available in all regions or within the Azure Extended Zone. Please consult the Service availability and timelines section for more information. If your required services are not available in the parent region(s) or Azure Extended Zone, you should evaluate other regions that offer an optimal balance between data residency requirements, resource costs, and latency.

### Compliance and Regulatory Standards

**Azure Extended Zones** meet the following compliance standards:

- **ISO 27001**
- **SOC 2 Type II**
- **PCI DSS**

### Extended Security Updates

**Extended Security Updates (ESUs)** will be provided at no additional cost for customers utilising Azure services. This includes workloads operating on **Azure Virtual Machines**, **Azure Dedicated Host**, **Azure VMware Solutions**, **Nutanix Cloud Clusters on Azure**, and **Azure Stack Hub/Edge/HCI**. In the **Azure Extended Zone**, eligible virtual machines configured to receive updates will automatically benefit from ESUs, ensuring continuous compliance and security updates without incurring extra charges.

For more information refer to the [Extended Security Updates FAQ](https://learn.microsoft.com/lifecycle/faq/extended-security-updates) in Microsoft Docs.


