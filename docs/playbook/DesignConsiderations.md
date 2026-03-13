# Design Considerations

## Resource organisation

Resource organization within Azure necessitates a structured approach to secure, manage, and optimize cloud resources effectively. This section covers the concept of a parent region, subscriptions, management groups, and resource groups.

### Parent Region

As outlined in the Overview of Azure Extended Zones section, the control plane for services deployed to an Azure Extended Zone remains situated within an Azure Region while the data plane is deployed at the Extended Zone site. Consequently, utilizing services in an Azure Extended Zone necessitates the employment of certain services, resources, and features within an Azure Region, referred to as the 'parent region.'

When deploying resources within an Azure Extended Zone, the resource overview will indicate the Azure Extended Zone. The portal views for the resource type, such as the Virtual Network View, will display the parent region.

**Virtual Network View:**

*(screenshot)*

**Virtual Network Resource View:**

*(screenshot)*

### Subscriptions

An Azure subscription serves several purposes, including:

- A legal agreement
- A payment agreement
- A boundary of scale
- An administrative boundary

Every Azure resource is logically associated with one subscription. When creating a resource, you select which Azure subscription to deploy it to.

Subscriptions are not tied to a specific Azure region or Azure Extended Zone, though each Azure resource deploys to only one region or extended zone.

Access to Azure Extended Zones is regulated and managed through a controlled access process. It is essential to register the subscription(s) intended for Extended Zone deployments. For further information, please refer to the Onboarding section.

It is advisable to consider dedicating specific subscriptions for deployments within an Azure Extended Zone. This approach facilitates the monitoring of quotas within the extended zone and mitigates potential confusion with resources deployed in the parent region.

### Management Groups

If your organization manages multiple Azure subscriptions, an efficient method is essential for overseeing access, policies, and compliance. Management groups offer a governance scope above subscriptions. By organizing subscriptions within management groups, any governance conditions applied will cascade by inheritance to all associated subscriptions.

Management groups are not tied to any specific Azure Region and can be utilized with both Azure Extended Zone allocated subscriptions and Azure Extended Zone deployed resources.

### Resource Groups

A resource group is a container that facilitates the management of related resources within an Azure solution. It serves as a scope for applying Role-based Access Control (RBAC) and Azure Policy for governance purposes. While resource groups can encompass resources provisioned in any Azure Region, they are associated with a specific region for metadata storage purposes.

It is important to note that creating a resource group within an Azure Extended Zone is not possible. Therefore, resource groups should be established in an Azure parent region. To avoid any confusion, it is recommended to adopt a naming convention that clearly indicates which resource groups are associated with an Azure Extended Zone.

---

## Management

Azure offers a comprehensive suite of tools and services designed for effective cloud resource management. The management capabilities in Azure encompass a broad range of tasks aimed at maintaining the health, performance, and security of your applications and infrastructure. These tasks include deploying and configuring resources, monitoring their performance, and ensuring compliance with organizational policies.

Key areas of Azure management include:

- **Connecting to Virtual Machines:** Establishing secure and efficient connections to your virtual machines for remote management and troubleshooting.
- **Well-Architected Recommendations:** Following best practices to design and operate reliable, secure, efficient, and cost-effective systems in the cloud.
- **Governance:** Implementing policies and controls to ensure resources are used appropriately and in compliance with organizational standards.
- **Monitoring:** Continuously tracking the performance, health, and availability of your resources to detect and respond to issues promptly.
- **Update Management:** Keeping your systems up-to-date with the latest patches and updates to maintain security and performance.

By leveraging these management capabilities, you can ensure that your Azure environment is optimized, secure, and aligned with your business objectives.

### Connecting to Virtual Machines

#### Azure Bastion

Azure Bastion is a fully managed Platform as a Service (PaaS) offering that enables secure connectivity to virtual machines using private IP addresses. It facilitates secure RDP/SSH access directly over TLS from the Azure portal, as well as through the native SSH or RDP client on your local machine. By leveraging Azure Bastion, virtual machines do not require public IP addresses, agents, or any specialized client software to connect.

To connect to a virtual machine deployed in an Azure Extended Zone, it is advisable to deploy the Azure Bastion service to the parent region and leverage the Azure Bastion service support for global virtual network peering. It is important to note that the Azure Bastion Basic, Standard, and Premium SKUs support connecting to virtual machines in peered virtual networks; however, the Developer SKU does not. For additional information, please refer to "About Azure Bastion" on Microsoft Learn.

### Well-Architected Recommendations

#### Azure Advisor

Azure Advisor is a non-regional service that provides recommendations applicable to resources deployed in an Azure Extended Zone.

#### Azure Compute Gallery Images

Azure Compute Gallery images serve as a valuable resource for building virtual machines within Azure Extended Zones. However, it is important to note that this functionality is not yet accessible through the Azure Portal (UX).

Azure Compute Gallery images hosted in a primary Azure region can be replicated to Azure Extended Zones by utilizing the appropriate command, as demonstrated below:

```bash
az sig image-version update --resource-group MyResourceGroup --gallery-name MyGallery --gallery-image-definition PlaceholderImage --gallery-image-version 0.0.1 --target-edge-zones australiaeast=perth=1=standardssd_lrs
```

> **Note:** `premium_lrs` is also supported as a storage type for image versions in the Perth Azure Extended Zone.

If you receive an error message about `Subscription '<SubID>' is not enrolled for Edge Zone access`, please contact MSFT Support and provide your subscription ID so access to the feature can be granted on the back end. Alternatively, it is possible to self-serve this by creating a managed identity and giving it access to an Azure compute gallery via an API operation.

## Governance

### Azure Policy

Azure Policy assists in enforcing organizational standards and assessing compliance at scale. Through its compliance dashboard, it provides an aggregated view for evaluating the overall state of the environment, with the capability to drill down to per-resource and per-policy details. It also aids in bringing resources into compliance through bulk remediation for existing resources and automatic remediation for new resources.

Azure Policy is a non-regional service and will evaluate resources deployed to an Azure Extended Zone for adherence to assigned policy definitions.

### Azure Tags

Tags are metadata elements that can be applied to Azure resources, consisting of key-value pairs that assist in identifying resources based on parameters pertinent to organizational needs.

Tags are supported within an Azure Extended Zone and are recommended as part of a comprehensive governance strategy for managing the Azure environment.

---

## Observability

### Alerts, Actions and Notifications

#### Action Groups

Action groups cannot be created in an Azure Extended Zone. It is recommended to create a global action group for monitoring resources within such zones. Typically, action groups are global services, with regional variations only available in select Azure regions.

Global action groups can process client requests from any region. If a specific region's service is unavailable, the requests are automatically routed to and processed by services in other regions, ensuring uninterrupted service. This global approach provides a robust disaster recovery solution. Regional requests, on the other hand, rely on availability zone redundancy to comply with privacy requirements and offer a similar level of disaster recovery.

#### Service Health

Azure Service Health comprises three integral services:

- **Azure Status** informs users of service outages in Azure on the Azure Status page. This page provides a global view of the health of all Azure services across all regions.
- **Service Health** offers a personalized view of the health of the Azure services and regions in use. This is the optimal source for service-impacting communications about outages, planned maintenance activities, and other health advisories.
- **Resource Health** provides information about the health of individual cloud resources, such as specific virtual machine instances.

Service Health is supported for resources deployed to an Azure Extended Zone.

### Logs

It is advisable to create Log Analytics workspaces in a parent region of your choice, as they cannot be established within an Azure Extended Zone. Be aware that additional network costs may be incurred, which can vary based on the region from which the telemetry originates and its destination. For further details, please refer to the Azure Bandwidth pricing documentation.

### Metrics

Azure Monitor Metrics is a sophisticated feature within Azure Monitor that aggregates numerical data from monitored resources into a comprehensive time-series database. These metrics, collected at regular intervals, provide critical insights into various aspects of system performance at specific points in time.

Azure Metrics is compatible with resources deployed within an Azure Extended Zone, ensuring precise and reliable monitoring capabilities.

### Insights

Insights can be utilized with resources deployed within an Azure Extended Zone.

### Workbooks

Workbooks can be utilized with resources deployed within an Azure Extended Zone.

---

## Application Performance Monitoring

### Application Insights

Application Insights can be utilized with resources deployed in an Azure Extended Zone. For additional information on Workspace-based Application Insights resources, please refer to the Logs section of this document.

### Network Watcher

An Azure Network Watcher resource is automatically created when you create or update a virtual network in Azure. This resource provides a comprehensive suite of tools for monitoring and diagnosing network conditions within, to, and from Azure. These tools include:

- **Monitoring:** Network Watcher offers tools to monitor resources and visualize the network topology, aiding in understanding the network configuration and relationships between resources.
- **Diagnostics:** The resource includes various diagnostic tools to troubleshoot and diagnose network issues, such as IP flow verification, NSG diagnostics, next hop analysis, effective security rules, connection troubleshooting, packet capture, and VPN troubleshooting.
- **Traffic Analysis:** Network Watcher allows for logging and visualizing network traffic using flow logs and traffic analytics, facilitating the understanding of network performance and the identification of anomalies.
- **Connection Monitoring:** It provides end-to-end connection monitoring for both Azure and hybrid endpoints, offering insights into network performance between various endpoints in the network infrastructure.

Network Watcher is automatically created to provide immediate access to its monitoring and diagnostic capabilities for your virtual network, eliminating the need for additional configuration. When a virtual network is created or updated in an Azure Extended Zone, the Network Watcher resource is instantiated in the parent region, which hosts the metadata of the virtual network resource group.

### Connection Monitor

Connection Monitor provides unified, end-to-end connection monitoring in Network Watcher, supporting both hybrid and Azure cloud deployments. Network Watcher provides tools to monitor, diagnose, and view connectivity-related metrics for your Azure deployments.

It is not possible to create a connection monitor resource in an Azure Extended Zone; however, it is possible to select a parent region for the connection monitor.

For information regarding configuring a log analytics workspace for connection monitoring, please refer to the Logs section of this document.

### Network Security Group Flow Logs

When creating a flow log for a network security group deployed in an Azure Extended Zone, the region will be locked to the parent region — the region hosting the metadata of the virtual network resource group. Consequently, the storage account used by the network security group flow logs must also reside in the parent region.

Key considerations for this configuration include:

- **Performance and Latency:** It is recommended to use a storage account in the same region as your network security group and associated virtual network resources to minimise latency and maximise performance.
- **Data Transfer Costs:** Storing flow logs in a different region may incur additional data transfer costs.
- **Compliance and Data Residency:** Ensure that your configuration adheres to any data residency requirements your organisation may have.

### Virtual Network Flow Logs

When creating a virtual network flow log for a virtual network deployed in an Azure Extended Zone, the region will be locked to the parent region — the region hosting the metadata of the virtual network resource group. Consequently, the storage account used by the virtual network flow logs must also reside in the parent region.

Considerations for this configuration include:

- **Performance and Latency:** It is recommended to use a storage account in the same region as your virtual network and its connected resources to minimise latency and maximise performance.
- **Data Transfer Costs:** Storing flow logs in a different region may incur additional data transfer costs.
- **Compliance and Data Residency:** Ensure that your configuration complies with any data residency requirements your organisation might have.

Please refer to the Logs section of this document for detailed information on configuring a log analytics workspace for traffic analysis.

### Virtual Machine Boot Diagnostics

The configuration of Azure boot diagnostics is supported for virtual machines deployed in an Azure Extended Zone as part of the process for creating a new virtual machine. Boot diagnostics is a crucial debugging feature for Azure virtual machines that facilitates the diagnosis of VM boot failures, allowing users to monitor the state of their VM during the boot process by collecting serial log information and screenshots.

It should be noted that the portal experience for creating a virtual machine will automatically create a storage account in the same region as the resource group's region, which is considered the parent region for a new Azure Extended Zone virtual machine. Users cannot opt to use a custom storage account instead of the pre-provisioned Microsoft-managed storage account that is created when default settings are used during virtual machine creation. Managed boot diagnostics are exclusively supported on Azure Extended Zones.

---

## Quotas

---

## Update Management

### Automatic VM Guest Patching

Automatic virtual machine guest patching is supported for virtual machines deployed within an Azure Extended Zone. This feature should be considered as part of your update management strategy, adhering to the guidelines specified in the official documentation.

### Azure Update Manager

Azure Update Manager is a comprehensive service designed to assist in managing and governing updates for all your machines. It provides a unified view for monitoring Windows and Linux update compliance, allowing for real-time updates or the scheduling of updates within designated maintenance windows.

Integrating Azure Update Manager into your update management strategy is advisable for virtual machines deployed in Azure Extended Zones.


## Networking and Connectivity

### Network Topology

Azure Extended Zones can be deployed in the following scenarios:

- Azure Extended Zone - Standalone
- Azure Extended Zone - Extension

#### Azure Extended Zone - Standalone

When considering the use of an Azure Extended Zone in a standalone scenario, there are two networking topologies to consider:

- Traditional network
- Azure Virtual WAN

##### Traditional Network

Traditional Azure network topology configurations can be effectively deployed within an Azure Extended Zone. This includes configurations such as a single virtual network (vNet) or a Hub and Spoke network topology.

*(diagram)*

##### Azure Virtual WAN

*(content to be added)*

#### Azure Extended Zone - Extension

When considering the implementation of an Azure Extended Zone in a regional extension scenario, there are two primary networking topologies to evaluate:

- Traditional network
- Azure Virtual WAN

##### Traditional Network

Traditional Azure network topology configurations can be effectively deployed within an Azure Extended Zone and extended back to the parent region's network topology. Connectivity between the Extended Zone Hub vNet and the Parent Region Hub vNet will be established via a vNet Peering connection.

*(diagram)*

##### Virtual WAN (Microsoft Managed) - Hybrid

Traditional Azure network topology configurations can be effectively deployed within an Azure Extended Zone and extended back to a Parent Region Azure Virtual WAN. This connection would be established via a vNet peering connection.

*(diagram)*

---

### Connectivity Options

Hybrid network connectivity to resources within an Azure Extended Zone can be established through the following methods:

- ExpressRoute
- Site-to-site VPN

#### ExpressRoute

ExpressRoute is the preferred solution to extend an on-premises network into the Azure Extended Zone.

The ExpressRoute circuits can be established through the following partners:

- Equinix
- Megaport
- NextDC

Or by using ExpressRoute Direct.

The ExpressRoute circuit are available in the following SKUs:

- **ExpressRoute Local** — currently not supported for Azure Extended Zones.
- **ExpressRoute Standard** — provides connectivity to resources within the geopolitical boundary (e.g. Oceania includes Australia East, Australia Southeast, New Zealand North).
- **ExpressRoute Premium** — provides global connectivity over the Microsoft core network, allowing you to link a vNet in one geopolitical region with an ExpressRoute circuit in another region.

##### Availability

ExpressRoute is a highly reliable service, supported by a 99.95% uptime SLA. Many of Microsoft's largest customers rely on ExpressRoute to connect their on-premises networks to Microsoft resources.

Microsoft incorporates high availability into each layer of the ExpressRoute connection, providing two separate links for each circuit, each terminating in different physical hardware inside the PoP. If the primary link becomes unavailable, the secondary link can continue to be used. Additionally, ExpressRoute PoPs are designed with a high degree of redundancy and resiliency.

High availability is a shared responsibility. Clients need to utilise both physical links and regularly test their configuration and failover processes. Deployment of your ExpressRoute configuration should adhere to Microsoft's high availability guidance, ensuring that your side of the connection is free from single points of failure. Customer premises equipment (CPE) must be evaluated to meet high availability requirements, and application workloads must be capable of handling retries properly, as short, intermittent outages of the ExpressRoute connection are normal and expected within the SLA.

*(diagram)*

##### Disaster Recovery

Instances of degradation or outages at ExpressRoute peering locations or across an entire regional service can occur, often due to natural calamities. Hence, it is crucial to develop a disaster recovery plan to ensure business continuity and support mission-critical applications.

Although there will be two PoPs in Perth, there is only a single ExpressRoute peering location. To minimise the impact of a peering location failure, the following mitigation options are recommended:

- Deploy a second ExpressRoute circuit connecting to another PoP in the parent region (e.g. Sydney). This method is fully detailed in the ExpressRoute disaster recovery guidance. While this approach may result in increased latency due to cross-region traffic, this trade-off is often considered acceptable during disaster scenarios.
- Use a site-to-site VPN as a fallback.

Each mitigation option introduces additional complexity and expense, making it essential to carefully assess the necessity of such measures before implementation.

*(diagram)*

#### Site-to-Site VPN

ISV VPN solutions can be used to establish a site-to-site VPN connection to the Azure Extended Zone to support hybrid connectivity to an on-premises network.

---

### Outbound Internet Access

To provide outbound internet access, you should implement one of the following solutions:

- Network Virtual Appliance
- Azure Load Balancer (SNAT)
- Instance Level Public IP
- Azure Firewall *(Roadmap)*
- NAT Gateway *(Roadmap)*

#### Network Virtual Appliance

Network Virtual Appliances (NVA) from ISV vendors (e.g. F5 Networks, Palo Alto, Cisco) can be utilised in the Azure Extended Zone to:

- Inspect egress traffic from virtual machines to the internet and prevent data exfiltration.
- Inspect ingress traffic from the internet to virtual machines and prevent attacks.
- Filter traffic between virtual machines in Azure to prevent lateral movement of compromised systems.
- Filter traffic between on-premises systems and Azure virtual machines if they are considered to belong to different security levels — for example, if Azure hosts the DMZ and on-premises hosts the internal applications.

#### Azure Load Balancer

Azure Standard Load Balancer (public) can be utilised to provide outbound internet access via source network address translation (SNAT) for backend instances. This configuration uses SNAT to translate a virtual machine's private IP address into the load balancer's public IP address, thereby preventing external sources from directly accessing the backend instances.

*(diagram)*

#### Instance Level Public IP

Assigning a public IP address to a virtual machine instance enables the instance to have direct access to the internet.

*(diagram)*

#### Azure Firewall

*(content to be added)*

#### NAT Gateway

*(content to be added)*

---

### Inbound Internet Access

Azure Extended Zones will offer several solutions to ensure secure access to resources from the internet:

- Azure Load Balancer
- Network Virtual Appliance
- Application Gateway *(Roadmap)*

#### Azure Load Balancer

When utilising Azure Load Balancers within an Extended Zone, the following constraints must be considered:

- Only the Azure Standard Load Balancer SKU is supported. Gateway and Basic Load Balancer SKUs are not supported.
- Only the Regional tier is supported.

*(diagram)*

#### Network Virtual Appliance

Network Virtual Appliances (NVA) from ISV vendors (e.g. F5 Networks, Palo Alto, Cisco) can be utilised in the Azure Extended Zone to:

- Inspect egress traffic from virtual machines to the internet and prevent data exfiltration.
- Inspect ingress traffic from the internet to virtual machines and prevent attacks.
- Filter traffic between virtual machines in Azure to prevent lateral movement of compromised systems.
- Filter traffic between on-premises systems and Azure virtual machines if they are considered to belong to different security levels — for example, if Azure hosts the DMZ and on-premises hosts the internal applications.

#### Application Gateway

*(content to be added)*

---

### Name Resolution

#### Hybrid DNS Resolution

Hybrid DNS resolution — which allows Azure resources to resolve on-premises domains and enables on-premises DNS to resolve Azure private DNS zones — can be implemented through the following solutions:

- Customer managed DNS server
- Azure Private DNS Resolver

##### Customer Managed DNS Server

By deploying a customer managed DNS server into the Azure Extended Zone you will be able to:

- Resolve on-premises computer and service names from VMs or role instances in Azure.
- Resolve Azure hostnames from on-premises computers.

##### Azure Private DNS Resolver

*(content to be added)*

---

## Business Continuity, Disaster Recovery and Migration

Business Continuity and Disaster Recovery (BCDR) are essential components of a comprehensive cloud strategy, ensuring that applications and data remain accessible and recoverable during disruptions. This section addresses the availability and recovery strategies specific to Azure Extended Zones.

### Availability

#### Compute

##### Availability Sets

*(content to be added)*

##### Azure Virtual Machine Scale Sets

Virtual Machine Scale Sets allow for the creation and management of a group of load-balanced virtual machines. The number of VM instances can automatically scale in accordance with demand or a predefined schedule. Key benefits of scale sets include:

- Streamlined creation and management of multiple VMs.
- Enhanced high availability and application resiliency by distributing VMs across availability zones or fault domains.
- Automatic scaling of applications in response to fluctuating resource demand.
- Capability to operate at large scale.

#### Network

##### Azure Load Balancer

Azure Load Balancer can significantly enhance application resilience by efficiently distributing incoming network traffic across multiple instances of an application. Key benefits include:

- **Fault Tolerance:** By distributing traffic across multiple instances, Azure Load Balancer ensures that if one instance fails, traffic is automatically redirected to healthy instances, minimising downtime.
- **Scalability:** Supports the scaling of applications by adding more instances as required to handle increased traffic, ensuring the application remains responsive under load.
- **Health Probes:** Azure Load Balancer uses health probes to monitor the status of application instances. If an instance is found to be unhealthy, the load balancer will stop sending traffic to it until it recovers.
- **Low Latency and High Throughput:** Provides low latency and high throughput, which is crucial for maintaining performance and reliability in high-traffic scenarios.

##### Azure Application Gateway

*(content to be added)*

#### Storage

##### Locally Redundant Storage

Locally Redundant Storage (LRS) is supported by Azure Extended Zones, offering data replication three times within the Azure Extended Zone. While this does not safeguard against Azure Extended Zone failures, it provides protection against hardware issues within the zone.

In addition, Azure Storage offers several features to enhance data resiliency:

- **Soft Delete:** Helps protect your data from accidental or malicious deletion. When enabled, deleted blobs or containers are retained for a specified period, allowing you to restore them if necessary.
- **Versioning:** Blob versioning automatically maintains previous versions of an object. Each time a blob is modified, a new version is created, allowing you to restore previous versions if necessary.

---

### Recovery

#### Data Protection

Azure Backup is an essential service for safeguarding resources within Azure, including virtual machines, Azure Files, Azure Disks, and Azure Blobs. Azure Extended Zones support the use of Azure Backup to protect assets within the Extended Zone.

It is important to note that Recovery Services Vaults can only be created in an Azure Region and not within Azure Extended Zones. To ensure robust data protection, it is advisable to replicate backup data to another Azure region. For Azure Extended Zones, a parent region must act as the control plane, providing oversight and management capabilities for these zones.

The following Azure Backup scenarios are currently supported:

- Azure Extended Zone > Parent region

The following Azure Backup scenarios are not currently supported:

- On-premises > Azure Extended Zone

*(diagram)*

#### Disaster Recovery

Azure Site Recovery (ASR) is a Microsoft Azure service designed to ensure business continuity by maintaining the operation of applications and workloads during outages. ASR accomplishes this by replicating workloads running on physical and virtual machines from a primary site to a secondary location. In the event of an outage at the primary site, the service enables failover to the secondary location, ensuring continuous access to applications. Once the primary site is restored, you can fail back to it seamlessly.

The following ASR scenarios are currently supported:

- Azure Extended Zone > Parent region

The following ASR scenarios are not currently supported:

- Azure Extended Zone > Non-parent region
- Azure Extended Zone > Azure Extended Zone
- On-premises > Azure Extended Zone
- On-premises > Azure Extended Zone – Storage Account (for Cache) > Region

### Migration

If you are considering migrating workloads from on-premises to an Azure Extended Zone, consult with your Microsoft account team for tailored recommendations and guidance.

---

## Security, Compliance and Data Residency

Azure provides a comprehensive framework to ensure the security, compliance, and proper data residency of your cloud resources. This framework is designed to protect your data, adhere to regulatory requirements, and ensure that data is stored and processed in appropriate locations.

### Microsoft Cloud Security Benchmark

The Microsoft Cloud Security Benchmark (MCSB) provides prescriptive best practices and recommendations that enhance the security of workloads, data, and services on Azure and across your multi-cloud environment. This benchmark focuses on cloud-centric control areas with input from various Microsoft and industry security guidelines, including:

- **Cloud Adoption Framework:** Offers guidance on strategy, roles and responsibilities, Azure Top 10 Security Best Practices, and reference implementation.
- **Azure Well-Architected Framework:** Provides insights on securing your workloads on Azure.
- **Chief Information Security Officer (CISO) Workshop:** Delivers program guidance and strategies to accelerate security modernisation using Zero Trust principles.
- **Industry and cloud service provider security best practice standards and frameworks:** Including the AWS Well-Architected Framework, Center for Internet Security (CIS) Controls, NIST, and PCI-DSS.

It is recommended to consult MCSB and the service baselines for any service deployed to an Azure Extended Zone to plan for the security configuration of each service.

### Cloud Security Posture Management

Microsoft Defender for Cloud is a comprehensive cloud-native application protection platform (CNAPP) designed to safeguard cloud-based applications from a wide range of cyber threats and vulnerabilities. Defender for Cloud integrates the following capabilities:

- **Development Security Operations (DevSecOps):** Unifies security management at the code level across multicloud and multiple-pipeline environments.
- **Cloud Security Posture Management (CSPM):** Surfaces actionable insights to prevent breaches and strengthen security posture.
- **Cloud Workload Protection Platform (CWPP):** Provides specific protections for servers, containers, storage, databases, and other workloads.

It is advisable to utilise Microsoft Defender for Cloud to manage and enhance the security posture of the Azure Extended Zone.

### Security Information and Event Management

Microsoft Sentinel is a scalable, cloud-native Security Information and Event Management (SIEM) system that offers a comprehensive solution for both SIEM and Security Orchestration, Automation, and Response (SOAR). This service provides robust cyber threat detection, investigation, response, and proactive threat hunting across your entire enterprise infrastructure.

Microsoft Sentinel seamlessly integrates with proven Azure services such as Log Analytics and Logic Apps, enhancing investigation and detection capabilities with advanced artificial intelligence. For effective SIEM and SOAR within the Azure Extended Zone, Microsoft Sentinel is highly recommended. Detailed information on deploying the Log Analytics workspace for Microsoft Sentinel can be found in the Logs section of this document.

### Azure Security Services

#### Network Security Groups

An Azure Network Security Group (NSG) filters network traffic between Azure resources within an Azure virtual network. Each NSG comprises security rules that either allow or deny inbound and outbound network traffic based on specified source and destination, port, and protocol parameters.

It is noteworthy that when a virtual machine is created within an Azure Extended Zone and the option to create an NSG is selected, the NSG will automatically be established in the resource group's region corresponding to the Azure Extended Zone VM — i.e. the parent region.

#### Azure Private Link

Azure Private Link enables secure and private access to Azure PaaS services — such as Azure Storage and SQL Database — as well as customer-owned and partner services hosted on Azure, via a private endpoint within your virtual network. This ensures that traffic between your virtual network and the service traverses the Microsoft backbone network, eliminating the need for exposure to the public internet.

#### DDoS Protection

Azure DDoS Protection, combined with application design best practices, provides enhanced DDoS mitigation features to defend against DDoS attacks. It is automatically tuned to help protect your specific Azure resources in a virtual network, and is simple to enable on any new or existing virtual network with no application or resource changes required.

Azure DDoS Protection protects at layer 3 and layer 4 network layers. For web application protection at layer 7, you need to add protection at the application layer using a WAF offering.

It is necessary to create the DDoS Protection plan in the parent region rather than the Azure Extended Zone. Customers and partners should utilise the DDoS Protection plan in the parent region to safeguard their resources deployed in Azure Extended Zones.

For protection against L7 application layer attacks, deploy Azure Web Application Firewall (WAF) with either Azure Front Door Premium or Application Gateway WAF v2 SKU. A multi-layered security approach, incorporating network, application, and data protection, should always be implemented.

#### Azure Firewall

Azure Firewall is a cloud-native and intelligent network firewall security service designed to offer superior threat protection for cloud workloads operating within Azure. It is a fully stateful firewall service with built-in high availability and unlimited cloud scalability, providing comprehensive traffic inspection for both east-west and north-south traffic flows.

---

### Data Residency

You might choose to utilise an Azure Extended Zone to meet data residency requirements for your workloads within the Microsoft cloud.

It is important to note that in certain limited scenarios, data may be stored outside of your selected geography. For further details, please refer to the Data residency in Azure documentation.

Additionally, if you employ a broad range of Azure services, multiple regions may be necessary as not all services are available in all regions or within the Azure Extended Zone. Please consult the Service availability and timelines section for more information. If your required services are not available in the parent region(s) or Azure Extended Zone, you should evaluate other regions that offer an optimal balance between data residency requirements, resource costs, and latency.

### Extended Security Updates

Extended Security Updates (ESUs) will be provided at no additional cost for customers utilising Azure services. This includes workloads operating on Azure Virtual Machines, Azure Dedicated Host, Azure VMware Solutions, Nutanix Cloud Clusters on Azure, and Azure Stack Hub/Edge/HCI. In the Azure Extended Zone, eligible virtual machines configured to receive updates will automatically benefit from ESUs, ensuring continuous compliance and security updates without incurring extra charges.

For more information refer to the FAQ in Microsoft Docs.

### Compliance and Regulatory Standards

Azure Extended Zones meet the following compliance standards:

- ISO 27001
- SOC 2 Type II
- PCI DSS
