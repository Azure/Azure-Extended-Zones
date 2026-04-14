# Availability, Business Continuity, Disaster Recovery and Migration

Ensuring workloads remain resilient and recoverable is critical when deploying to an Azure Extended Zone. This section provides guidance on designing for **high availability** across compute, network, and storage services, **data protection** using Azure Backup, **disaster recovery** using Azure Site Recovery, and **migration** pathways for moving workloads into an Azure Extended Zone. It outlines the supported scenarios, current limitations, and key considerations to help organizations build robust continuity and recovery strategies.

## Table of Contents

- [Availability](#availability)
  - [Compute](#compute)
    - [Availability Sets](#availability-sets)
    - [Azure Virtual Machine Scale Sets](#azure-virtual-machine-scale-sets)
  - [Storage](#storage)
    - [Locally Redundant Storage](#locally-redundant-storage)
  - [Networking](#networking)
- [Business Continuity and Disaster Recovery](#business-continuity-and-disaster-recovery)
  - [Data Protection](#data-protection)
  - [Disaster Recovery](#disaster-recovery)
- [Migration](#migration)

---

## Availability

### Compute

#### Availability Sets

Availability sets will not be supported in Azure Extended Zones. For compute resilience within an Extended Zone, Virtual Machine Scale Sets should be utilized.

#### Azure Virtual Machine Scale Sets

Virtual Machine Scale Sets allow for the creation and management of a group of load-balanced virtual machines. The number of VM instances can automatically scale in accordance with demand or a predefined schedule. Key benefits of scale sets include:

- Streamlined creation and management of multiple VMs.
- Enhanced high availability and application resiliency by distributing VMs across availability zones or fault domains.
- Automatic scaling of applications in response to fluctuating resource demand.
- Capability to operate at large scale.

### Storage

#### Locally Redundant Storage

Locally Redundant Storage (LRS) is supported by Azure Extended Zones, offering data replication three times within the Azure Extended Zone. While this does not safeguard against Azure Extended Zone failures, it provides protection against hardware issues within the zone.

In addition, Azure Storage offers several features to enhance data resiliency:

- **Soft Delete:** Helps protect your data from accidental or malicious deletion. When enabled, deleted blobs or containers are retained for a specified period, allowing you to restore them if necessary.
- **Versioning:** Blob versioning automatically maintains previous versions of an object. Each time a blob is modified, a new version is created, allowing you to restore previous versions if necessary.

### Networking

For network resilience guidance — including ExpressRoute high availability, redundancy considerations, and disaster recovery options such as deploying a secondary circuit to the parent region — refer to the [Networking and Connectivity](./Networking_and_Connectivity.md#expressroute) section.

---

## Business Continuity and Disaster Recovery

### Data Protection

Azure Extended Zones support the use of Azure Backup to protect assets within the Extended Zone.

Recovery Services Vaults can only be created in an Azure Region, not in an Azure Extended Zone. To improve resiliency, replicate backup data to another Azure region. For Azure Extended Zones, the parent region provides the control plane and management capabilities.

The following Azure Backup scenarios are currently supported:

- Azure Extended Zone > Parent region

The following Azure Backup scenarios are not currently supported:

- On-premises > Azure Extended Zone

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

