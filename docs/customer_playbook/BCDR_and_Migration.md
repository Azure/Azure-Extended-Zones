# Business Continuity, Disaster Recovery and Migration

Ensuring workloads remain resilient and recoverable is critical when deploying to an Azure Extended Zone. This section provides guidance on designing for **high availability** across compute, network, and storage services, **data protection** using Azure Backup, **disaster recovery** using Azure Site Recovery, and **migration** pathways for moving workloads into an Azure Extended Zone. It outlines the supported scenarios, current limitations, and key considerations to help organisations build robust continuity and recovery strategies.

## Table of Contents

- [Availability](#availability)
  - [Compute](#compute)
    - [Availability Sets](#availability-sets)
    - [Azure Virtual Machine Scale Sets](#azure-virtual-machine-scale-sets)
  - [Network](#network)
    - [Azure Load Balancer](#azure-load-balancer)
    - [Azure Application Gateway](#azure-application-gateway)
  - [Storage](#storage)
    - [Locally Redundant Storage](#locally-redundant-storage)
- [Recovery](#recovery)
  - [Data Protection](#data-protection)
  - [Disaster Recovery](#disaster-recovery)
- [Migration](#migration)

---

## Availability

### Compute

#### Availability Sets

*(content to be added)*

#### Azure Virtual Machine Scale Sets

Virtual Machine Scale Sets allow for the creation and management of a group of load-balanced virtual machines. The number of VM instances can automatically scale in accordance with demand or a predefined schedule. Key benefits of scale sets include:

- Streamlined creation and management of multiple VMs.
- Enhanced high availability and application resiliency by distributing VMs across availability zones or fault domains.
- Automatic scaling of applications in response to fluctuating resource demand.
- Capability to operate at large scale.

### Network

#### Azure Load Balancer

Azure Load Balancer can significantly enhance application resilience by efficiently distributing incoming network traffic across multiple instances of an application. Key benefits include:

- **Fault Tolerance:** By distributing traffic across multiple instances, Azure Load Balancer ensures that if one instance fails, traffic is automatically redirected to healthy instances, minimising downtime.
- **Scalability:** Supports the scaling of applications by adding more instances as required to handle increased traffic, ensuring the application remains responsive under load.
- **Health Probes:** Azure Load Balancer uses health probes to monitor the status of application instances. If an instance is found to be unhealthy, the load balancer will stop sending traffic to it until it recovers.
- **Low Latency and High Throughput:** Provides low latency and high throughput, which is crucial for maintaining performance and reliability in high-traffic scenarios.

#### Azure Application Gateway

*(content to be added)*

### Storage

#### Locally Redundant Storage

Locally Redundant Storage (LRS) is supported by Azure Extended Zones, offering data replication three times within the Azure Extended Zone. While this does not safeguard against Azure Extended Zone failures, it provides protection against hardware issues within the zone.

In addition, Azure Storage offers several features to enhance data resiliency:

- **Soft Delete:** Helps protect your data from accidental or malicious deletion. When enabled, deleted blobs or containers are retained for a specified period, allowing you to restore them if necessary.
- **Versioning:** Blob versioning automatically maintains previous versions of an object. Each time a blob is modified, a new version is created, allowing you to restore previous versions if necessary.

---

## Recovery

### Data Protection

Azure Backup is an essential service for safeguarding resources within Azure, including virtual machines, Azure Files, Azure Disks, and Azure Blobs. Azure Extended Zones support the use of Azure Backup to protect assets within the Extended Zone.

It is important to note that Recovery Services Vaults can only be created in an Azure Region and not within Azure Extended Zones. To ensure robust data protection, it is advisable to replicate backup data to another Azure region. For Azure Extended Zones, a parent region must act as the control plane, providing oversight and management capabilities for these zones.

The following Azure Backup scenarios are currently supported:

- Azure Extended Zone > Parent region

The following Azure Backup scenarios are not currently supported:

- On-premises > Azure Extended Zone

*(diagram)*

### Disaster Recovery

Azure Site Recovery (ASR) is a Microsoft Azure service designed to ensure business continuity by maintaining the operation of applications and workloads during outages. ASR accomplishes this by replicating workloads running on physical and virtual machines from a primary site to a secondary location. In the event of an outage at the primary site, the service enables failover to the secondary location, ensuring continuous access to applications. Once the primary site is restored, you can fail back to it seamlessly.

The following ASR scenarios are currently supported:

- Azure Extended Zone > Parent region

The following ASR scenarios are not currently supported:

- Azure Extended Zone > Non-parent region
- Azure Extended Zone > Azure Extended Zone
- On-premises > Azure Extended Zone
- On-premises > Azure Extended Zone – Storage Account (for Cache) > Region

---

## Migration

If you are considering migrating workloads from on-premises to an Azure Extended Zone, consult with your Microsoft account team for tailored recommendations and guidance.

