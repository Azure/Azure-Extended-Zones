# Azure Extended Zones Overview

[Azure Extended Zones](https://learn.microsoft.com/en-us/azure/extended-zones/overview) are **small-footprint extensions of an Azure region**, strategically located in **metropolitan areas, industry hubs, or specific jurisdictions**.

Extended Zones support **virtual machines, containers, storage, and a selection of Azure services**, enabling the execution of **latency-sensitive and throughput-intensive applications** closer to end users while adhering to approved **data residency requirements**.

![Azure Extended Zones Overview](./media/AzureExtendedZone01.png)

Azure Extended Zones are integrated into the **Microsoft global network**, providing **secure, reliable, high-bandwidth connectivity** between applications running in an Extended Zone and their users.

Azure customers can provision and manage Azure Extended Zone resources, services, and workloads through the **Azure portal and other core Azure management tools**.

The **control plane** for services running within an Extended Zone remains in the **parent Azure region**, while the **data plane** is deployed at the **Extended Zone site**, resulting in a **smaller Azure footprint located closer to users and workloads**.

---

## Key Scenarios for Azure Extended Zones

Azure Extended Zones are designed to address two primary scenarios:

### Latency
Users may need to operate resources such as **media editing software, real-time analytics platforms, or interactive applications** remotely with **minimal latency**. Deploying workloads closer to end users reduces network round-trip time and improves performance.

### Data Residency
Some organisations require application data to remain within a **specific geographic location** due to **privacy, regulatory, or compliance requirements**. Azure Extended Zones enable workloads and data to be hosted locally while still leveraging Azure services.

---

## Deployment Considerations for Azure Extended Zones

When considering the deployment of workloads to an **Azure Extended Zone**, the following questions should be evaluated.

### 1. What are your timelines?

Services within Azure Extended Zones are released **in phases**.

If your project has a **strict delivery timeline**, ensure it aligns with the **service availability schedule** and includes contingency time for potential delays.

For more information, refer to **Service availability and timelines**.


### 2. What services will you use?

Azure Extended Zones support:

- Virtual Machines  
- Containers  
- Storage  
- A **select range of Azure services**

You should confirm whether your solution requires **services that may not yet be available** within the Extended Zone.

If your architecture relies on **third-party solutions from the Azure Marketplace**, those services may not be available until the **independent software vendor (ISV)** has validated them for deployment within the Extended Zone.

Refer to **Service availability and timelines** for further details.

### 3. How will your use of services grow?

Planning for **future capacity requirements** is important.

To support capacity planning for Azure Extended Zones, Microsoft monitors **anticipated growth in service consumption**. Organisations should discuss their **expected usage patterns and future growth projections** with Microsoft or their primary partner.


### 4. What are your high availability and disaster recovery requirements?

Azure Extended Zones **do not currently support Availability Zones**.

However, additional Azure services can be used to meet **high availability and disaster recovery (HA/DR)** requirements, such as:

- Replication to a **parent Azure region**
- Backup and recovery services
- Cross-region failover architectures

Designing appropriate **HA/DR strategies** is critical when deploying workloads in an Extended Zone.

---
# Deployment Scenarios

Azure Extended Zones are available for deployment in the following scenarios:

- **Standalone**
- **Region Extension**


## Scenario 1: Standalone

Organizations can choose to deploy workloads within the Azure Extended Zone (e.g., **Perth Extended Zone**) without the necessity of connecting to a parent region's landing zone (e.g., **Australia East**).

Access to workloads within the Extended Zone can be facilitated through:

### Private Connectivity
- **ExpressRoute**
- **Site-to-Site VPN**  
  - *Note:* Azure VPN is a roadmap item for Azure Extended Zones. Site-to-Site VPN connectivity would currently need to be implemented using a **third-party solution**.

### Public Connectivity
- **External Load Balancer**
- **Virtual machine with a Public IP**

![Standalone Deployment](./media/Deployment-Standalone.png)

## Scenario 2: Region Extension

Azure customers with existing landing zones might consider extending their presence to include **Azure Extended Zones** (e.g., **Perth Extended Zone**).

Access to workloads within the Extended Zone can be facilitated through:

### Private Connectivity
- **ExpressRoute**
- **Site-to-Site VPN**¹

### Public Connectivity
- **External Load Balancer**
- **Virtual machine with a Public IP**

### Microsoft Backbone Connectivity
- **VNet Peering** to an existing landing zone in a region (e.g., Australia East), allowing traffic to traverse the **Microsoft network backbone**.

¹ *Note: Azure VPN is a roadmap item for Azure Extended Zones. Site-to-Site VPN connectivity would currently need to be implemented using a hird-party solution*

---

# Service Availability and Timelines

Azure Extended Zones enable the deployment of key Azure services closer to users and workloads. The **control plane** for these services operates in the **primary Azure region**, while the **data plane** is deployed at the **Extended Zone site**, resulting in a streamlined Azure footprint.

The following diagram illustrates the deployment model of Azure services within an **Azure Extended Zone**.

![Azure Extended Zone Service Offering](/media/azure-extended-zones-services.png)

Review the
[Azure Extended Zone Services](https://learn.microsoft.com/en-us/azure/extended-zones/overview#service-offerings-for-azure-extended-zones) documentation for a list of services currently available.

The below table provides a list of planned services:


| Service Category | Services and Features |
|---|---|
| **Compute** | 
| **Networking** |
| **Storage** |
| **Business Continuity and Disaster Recovery** | 

---

# Planning for Service Availability

If you are planning to utilize an **Azure Extended Zone**, it is recommended to evaluate the **services and SKUs** required for your solution to confirm their availability. Contact your **Microsoft account team** for guidance on:

- Expected **service timelines**
- **Scope of availability**
- Potential **alternative solutions**

If required services are not yet available in the Extended Zone, consider the following options.

## 1. Delay Production Deployment

Wait until the required services become available in the **Extended Zone** before deploying your production workloads.

For new workloads, consider:

- Deploying **non-production environments** in the **parent Azure region**
- Migrating **production services** to the **Azure Extended Zone** once services become available

## 2. Use Alternative Services or SKUs

Deploy workloads using **available services or SKUs** in the Extended Zone with a plan to **migrate to the preferred service or SKU** once it becomes available.