# Azure Extended Zones

This repository is the community and collaboration space for **Azure Extended Zones**.

## What you'll find here
This repository focuses on:
- [Azure Extended Zones Field Guide](./docs/field_guide/README.md)
- Community Q&A and troubleshooting patterns
- A place to propose improvements to repo docs, templates, and collaboration workflows

## Documentation
- Product documentation (Learn): https://learn.microsoft.com/en-us/azure/extended-zones/
- Start here in this repo: [docs/index.md](docs/index.md)

## Authoritative documentation
See the Microsoft Learn landing page:
https://learn.microsoft.com/en-us/azure/extended-zones/

# What are Azure Extended Zones?

Azure Extended Zones are small-footprint extensions of Azure placed in metros, industry centers, or a specific jurisdiction to serve low latency and data residency workloads. Azure Extended Zones supports virtual machines (VMs), containers, storage, and a selected set of Azure services and can run latency-sensitive and throughput-intensive applications close to end users and within approved data residency boundaries.
 
Azure Extended Zones are part of the Microsoft global network that provides secure, reliable, high-bandwidth connectivity between applications that run on an Azure Extended Zone close to the user. Extended Zones address low latency and data residency by bringing all the goodness of the Azure ecosystem (access, user experience, automation, security, etc.) closer to the customer or their jurisdiction. Azure customers can provision and manage their Azure Extended Zones resources, services, and workloads through the Azure portal and other essential Azure tools.

## Key scenarios

The key scenarios that Azure Extended Zones enable are: 

- **Latency**: users want to run their resources, for example, media editing software, remotely with low latency.

- **Data residency**: users want their applications data to stay within a specific geography and might essentially want to host locally for various privacy, regulatory, and compliance reasons.

The following diagram shows some of the industries and use cases where Azure Extended Zones can provide benefits.

![Azure Extended Zone industries](/media/azure-extended-zones-industries.png)

## Availability and access

See [Request access to Azure Extended Zones](https://learn.microsoft.com/en-us/azure/extended-zones/request-access?tabs=powershell) to learn how to request access to Extended Zones. A comprehensive list of Extended Zones will be listed in the process.

## Service offerings for Azure Extended Zones

Azure Extended Zones enable some key Azure services for customers to deploy. The control plane for these services remains in the region and the data plane is deployed at the Extended Zone site, resulting in a smaller Azure footprint.

The following diagram shows how Azure services are deployed at the Azure Extended Zones location.

![Azure Extended Zone services](/media/azure-extended-zones-services.png)


The following table lists key services that are available in Azure Extended Zones:

| Service category | Available Azure services and features | How to Guides |
| ------------------ | ------------------- | ------------------- |
| **Compute** | - [Virtual machines](https://learn.microsoft.com/en-us/azure/virtual-machines/overview) (general purpose: A, B, D, E, and F series and GPU NVadsA10 v5 series**) <br> - [Virtual Machine Scale Sets](https://learn.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview) <br> - [Azure Kubernetes Service](https://learn.microsoft.com/en-us/azure/aks/extended-zones?tabs=azure-resource-manager#what-is-aks-for-extended-zones)* <br> - [Azure Virtual Desktop](https://learn.microsoft.com/en-au/azure/virtual-desktop/azure-extended-zones)* <br>   | - [Deploy a virtual machine in an Extended Zone using the Azure portal](https://learn.microsoft.com/en-us/azure/extended-zones/deploy-aks-cluster)<br> - [Deploy an Azure Kubernetes Service (AKS) cluster in an Azure extended zone](https://learn.microsoft.com/en-us/azure/extended-zones/deploy-aks-cluster) <br> - [Azure Virtual Desktop on Azure Extended Zones](https://learn.microsoft.com/en-au/azure/virtual-desktop/azure-extended-zones) <br>|
| **Networking** | - [DDoS](https://learn.microsoft.com/en-us/azure/ddos-protection/ddos-protection-overview) (Standard protection) <br> - [ExpressRoute](https://learn.microsoft.com/en-us/azure/expressroute/expressroute-introduction) <br> - [Private Link](https://learn.microsoft.com/en-us/azure/private-link/private-link-overview) <br> - [Standard Load Balancer](https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-overview) <br> - [Standard public IP](https://learn.microsoft.com/en-us/azure/virtual-network/ip-services/public-ip-addresses) <br> - [Virtual Network](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview) <br> - [Virtual Network Peering](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview) <br> - [Azure Firewall (API version)](https://learn.microsoft.com/en-us/azure/firewall/overview) | - [Deploy Azure Firewall in Azure Extended Zones](https://learn.microsoft.com/en-us/azure/extended-zones/deploy-azure-firewall)
| **Storage** | - [Managed disks](https://learn.microsoft.com/en-us/azure/virtual-machines/managed-disks-overview) <br> &nbsp;&nbsp;- Premium SSD <br> &nbsp;&nbsp;- Standard SSD <br> - [Storage Account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview) <br> &nbsp;&nbsp;- [Premium Page Blobs](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-pageblob-overview) <br> &nbsp;&nbsp;- [Premium Block Blobs]() <br> &nbsp;&nbsp;- [Premium Files](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-introduction) <br> &nbsp;&nbsp;- [Data Lake Storage Gen2 Hierarchical Namespace](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction) <br> &nbsp;&nbsp;- [Data Lake Storage Gen2 Flat Namespace](https://learn.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-namespace)  | - [Create a storage account in an Azure extended zone](https://learn.microsoft.com/en-us/azure/extended-zones/create-storage-account)|
| **BCDR** | [Azure Site Recovery](https://learn.microsoft.com/en-us/azure/site-recovery/site-recovery-overview)* (Extended Zone to parent region) <br> [Azure Backup](https://learn.microsoft.com/en-us/azure/backup/backup-overview) |
| **Arc-enabled PaaS** | - [ContainerApps](https://learn.microsoft.com/en-us/azure/container-apps/azure-arc-overview)* <br> - [ManagedSQL](https://learn.microsoft.com/en-us/azure/azure-arc/data/managed-instance-overview)* | - [Deploy Arc-enabled workloads in an Extended Zone: ContainerApp](https://learn.microsoft.com/en-us/azure/extended-zones/arc-enabled-workloads-container-apps) <br> - [Deploy Arc-enabled workloads in an Extended Zone: Managed SQL Instance](https://learn.microsoft.com/en-us/azure/extended-zones/arc-enabled-workloads-managed-sql)
| **Other** | - [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/overview) (with encryption resources in parent region, targeting the Extended Zone) <br> - [Azure Policy](https://learn.microsoft.com/en-us/azure/governance/policy/overview) <br> - [Reserved Instances](https://learn.microsoft.com/en-us/azure/cost-management-billing/reservations/save-compute-costs-reservations) (through recommendations flow) <br> - [Savings Plans](https://learn.microsoft.com/en-us/azure/cost-management-billing/savings-plan/savings-plan-overview) | - [Encrypt disks with customer-managed keys in an Azure extended zone](https://learn.microsoft.com/en-us/azure/extended-zones/key-vault-encrypt-azure-extended-zone-disk) <br> - [Create a custom Azure policy in an Azure extended zone](https://learn.microsoft.com/en-us/azure/extended-zones/create-azure-policy) <br> - [Purchase reservations or savings plans in the Azure portal](https://learn.microsoft.com/en-us/azure/extended-zones/purchase-reservations-savings-plans)

\* While these services are GA in Azure Regions, they are currently in Preview in Azure Extended Zones.  
** [Learn more about Virtual Machine family series here](/azure/virtual-machines/sizes/overview?tabs=breakdownseries%2Cgeneralsizelist%2Ccomputesizelist%2Cmemorysizelist%2Cstoragesizelist%2Cgpusizelist%2Cfpgasizelist%2Chpcsizelist). You can obtain a detailed VM list in the Azure Extended Zones environment. 

## Supported Independent Software Vendors (ISVs)

The following table lists the key Independent Software Vendors services that are supported in Azure Extended Zones:

| Service Provider | Supported services and features |
| ------------------ | ------------------- |
| **Aviatrix** | Cloud Native Security Fabric (CNSF) |
| **Check Point** | Firewall |
| **Fortinet** | Firewall |
| **HPE Aruba Networking** | [EdgeConnect SD-WAN](https://arubanetworking.hpe.com/techdocs/sdwan-PDFs/deployments/dg_ECV-Azure_latest.pdf) |

## Code of conduct
This project follows the [Microsoft Open Source Code of Conduct](CODE_OF_CONDUCT.md).

## How this repo works & Getting support
- Use **Discussions** for questions and design conversations.
- Use **Issues** for actionable work items (bugs, docs gaps, requests).
- Submit changes via **Pull Requests**.
- Support and escalation: see [SUPPORT.md](SUPPORT.md)

## Contributing
Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting issues or pull requests.

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit [Contributor License Agreements](https://cla.opensource.microsoft.com).

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft
trademarks or logos is subject to and must follow
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
