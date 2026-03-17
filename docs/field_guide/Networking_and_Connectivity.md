# Networking and Connectivity

Networking is a foundational component of any Azure Extended Zone deployment. This section provides guidance on designing and implementing network architectures that support workloads within an Azure Extended Zone, covering **network topology** options for both standalone and region extension scenarios, **hybrid connectivity** to on-premises environments using ExpressRoute and VPN, **inbound and outbound internet access** patterns, and **name resolution** strategies. The goal is to help organisations make informed decisions that balance performance, security, and resilience when extending their network into an Azure Extended Zone.

## Table of Contents

- [Network Topology](#network-topology)
  - [Standalone](#azure-extended-zone---standalone)
  - [Extension](#azure-extended-zone---extension)
- [Connectivity Options](#connectivity-options)
  - [ExpressRoute](#expressroute)
    - [Availability](#availability)
    - [Disaster Recovery](#disaster-recovery)
  - [Site-to-Site VPN](#site-to-site-vpn)
- [Outbound Internet Access](#outbound-internet-access)
  - [Network Virtual Appliance](#network-virtual-appliance)
  - [Azure Load Balancer](#azure-load-balancer)
  - [Instance Level Public IP](#instance-level-public-ip)
  - [Azure Firewall](#azure-firewall)
  - [NAT Gateway](#nat-gateway)
- [Inbound Internet Access](#inbound-internet-access)
  - [Azure Load Balancer](#azure-load-balancer-1)
  - [Network Virtual Appliance](#network-virtual-appliance-1)
  - [Application Gateway](#application-gateway)
- [Name Resolution](#name-resolution)
  - [Hybrid DNS Resolution](#hybrid-dns-resolution)

---

## Network Topology

Azure Extended Zones can be deployed in the following scenarios:

- Azure Extended Zone - Standalone
- Azure Extended Zone - Extension

### Azure Extended Zone - Standalone

When considering the use of an Azure Extended Zone in a standalone scenario, there are two networking topologies to consider:

- Traditional network
- Azure Virtual WAN

#### Traditional Network

Traditional Azure network topology configurations can be effectively deployed within an Azure Extended Zone. This includes configurations such as a single virtual network (vNet) or a Hub and Spoke network topology.

![Traditional Network Design](./media/Networking-Traditional.png)

#### Azure Virtual WAN

> [!NOTE]
> The deployment of Azure Virtual WAN Hub within Azure Extended Zone is currently not supported.

### Azure Extended Zone - Extension

When considering the implementation of an Azure Extended Zone in a regional extension scenario, there are two primary networking topologies to evaluate:

- Traditional network
- Azure Virtual WAN

#### Traditional Network

Traditional Azure network topology configurations can be effectively deployed within an Azure Extended Zone and extended back to the parent region's network topology. Connectivity between the Extended Zone Hub vNet and the Parent Region Hub vNet will be established via a vNet Peering connection.

![Hub and Spoke](./media/Networking-Hub-and-Spoke.png)

#### Virtual WAN (Microsoft Managed) - Hybrid

Traditional Azure network topology configurations can be effectively deployed within an Azure Extended Zone and extended back to a Parent Region Azure Virtual WAN. This connection would be established via a vNet peering connection.

> [!NOTE]
> The deployment of Azure Virtual WAN Hub within Azure Extended Zone is currently not supported.

![vWAN](./media/Networking-vWAN.png)

---

## Connectivity Options

Hybrid network connectivity to resources within an Azure Extended Zone can be established through the following methods:

- ExpressRoute
- Site-to-site VPN

### ExpressRoute

ExpressRoute is the preferred solution to extend an on-premises network into the Azure Extended Zone.

> [!NOTE]
> Perth Extended Zone <br>
> While there are two Microsoft Points of Presence (PoP) in Western Australia, there is only a single ExpressRoute peering location that is located at Next DC P1.
>
> ![Western Australia - Express Route](./media/Networking-ER-Perth.png)

The ExpressRoute circuits can be established through the following partners:

- Equinix
- Megaport
- NextDC

Or by using ExpressRoute Direct.

The ExpressRoute circuit are available in the following SKUs:

- **ExpressRoute Local** — currently not supported for Azure Extended Zones.
- **ExpressRoute Standard** — provides connectivity to resources within the geopolitical boundary (e.g. Oceania includes Australia East, Australia Southeast, New Zealand North).
- **ExpressRoute Premium** — provides global connectivity over the Microsoft core network, allowing you to link a vNet in one geopolitical region with an ExpressRoute circuit in another region.

#### Availability

ExpressRoute is a highly reliable service, supported by a 99.95% uptime SLA. Many of Microsoft's largest customers rely on ExpressRoute to connect their on-premises networks to Microsoft resources.

Microsoft incorporates high availability into each layer of the ExpressRoute connection, providing two separate links for each circuit, each terminating in different physical hardware inside the PoP. If the primary link becomes unavailable, the secondary link can continue to be used. Additionally, ExpressRoute PoPs are designed with a high degree of redundancy and resiliency.

High availability is a shared responsibility. Clients need to utilise both physical links and regularly test their configuration and failover processes. Deployment of your ExpressRoute configuration should adhere to Microsoft's high availability guidance, ensuring that your side of the connection is free from single points of failure. Customer premises equipment (CPE) must be evaluated to meet high availability requirements, and application workloads must be capable of handling retries properly, as short, intermittent outages of the ExpressRoute connection are normal and expected within the SLA.

*(diagram)*

#### Disaster Recovery

Instances of degradation or outages at ExpressRoute peering locations or across an entire regional service can occur, often due to natural calamities. Hence, it is crucial to develop a disaster recovery plan to ensure business continuity and support mission-critical applications.

Although there will be two PoPs in Perth, there is only a single ExpressRoute peering location. To minimise the impact of a peering location failure, the following mitigation options are recommended:

- Deploy a second ExpressRoute circuit connecting to another PoP in the parent region (e.g. Sydney). This method is fully detailed in the ExpressRoute disaster recovery guidance. While this approach may result in increased latency due to cross-region traffic, this trade-off is often considered acceptable during disaster scenarios.
- Use a site-to-site VPN as a fallback.

Each mitigation option introduces additional complexity and expense, making it essential to carefully assess the necessity of such measures before implementation.

*(diagram)*

### Site-to-Site VPN

ISV VPN solutions can be used to establish a site-to-site VPN connection to the Azure Extended Zone to support hybrid connectivity to an on-premises network.

---

## Outbound Internet Access

To provide outbound internet access, you should implement one of the following solutions:

- Network Virtual Appliance
- Azure Load Balancer (SNAT)
- Instance Level Public IP
- Azure Firewall *(Roadmap)*
- NAT Gateway *(Roadmap)*

### Network Virtual Appliance

Network Virtual Appliances (NVA) from ISV vendors (e.g. F5 Networks, Palo Alto, Cisco) can be utilised in the Azure Extended Zone to:

- Inspect egress traffic from virtual machines to the internet and prevent data exfiltration.
- Inspect ingress traffic from the internet to virtual machines and prevent attacks.
- Filter traffic between virtual machines in Azure to prevent lateral movement of compromised systems.
- Filter traffic between on-premises systems and Azure virtual machines if they are considered to belong to different security levels — for example, if Azure hosts the DMZ and on-premises hosts the internal applications.

### Azure Load Balancer

Azure Standard Load Balancer (public) can be utilised to provide outbound internet access via source network address translation (SNAT) for backend instances. This configuration uses SNAT to translate a virtual machine's private IP address into the load balancer's public IP address, thereby preventing external sources from directly accessing the backend instances.

*(diagram)*

### Instance Level Public IP

Assigning a public IP address to a virtual machine instance enables the instance to have direct access to the internet.

*(diagram)*

### Azure Firewall

*(content to be added)*

### NAT Gateway

*(content to be added)*

---

## Inbound Internet Access

Azure Extended Zones will offer several solutions to ensure secure access to resources from the internet:

- Azure Load Balancer
- Network Virtual Appliance
- Application Gateway *(Roadmap)*

### Azure Load Balancer

When utilising Azure Load Balancers within an Extended Zone, the following constraints must be considered:

- Only the Azure Standard Load Balancer SKU is supported. Gateway and Basic Load Balancer SKUs are not supported.
- Only the Regional tier is supported.

*(diagram)*

### Network Virtual Appliance

Network Virtual Appliances (NVA) from ISV vendors (e.g. F5 Networks, Palo Alto, Cisco) can be utilised in the Azure Extended Zone to:

- Inspect egress traffic from virtual machines to the internet and prevent data exfiltration.
- Inspect ingress traffic from the internet to virtual machines and prevent attacks.
- Filter traffic between virtual machines in Azure to prevent lateral movement of compromised systems.
- Filter traffic between on-premises systems and Azure virtual machines if they are considered to belong to different security levels — for example, if Azure hosts the DMZ and on-premises hosts the internal applications.

### Application Gateway

*(content to be added)*

---

## Name Resolution

### Hybrid DNS Resolution

Hybrid DNS resolution — which allows Azure resources to resolve on-premises domains and enables on-premises DNS to resolve Azure private DNS zones — can be implemented through the following solutions:

- Customer managed DNS server
- Azure Private DNS Resolver

#### Customer Managed DNS Server

By deploying a customer managed DNS server into the Azure Extended Zone you will be able to:

- Resolve on-premises computer and service names from VMs or role instances in Azure.
- Resolve Azure hostnames from on-premises computers.

#### Azure Private DNS Resolver

*(content to be added)*
