# Deployment Scenarios

Azure Extended Zones are available for deployment in the following scenarios:

- **Standalone**
- **Region Extension**

---

## Scenario 1: Azure Extended Zone – Standalone

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

## Scenario 2: Azure Extended Zone – Region Extension

Azure customers with existing landing zones might consider extending their presence to include **Azure Extended Zones** (e.g., **Perth Extended Zone**).

Access to workloads within the Extended Zone can be facilitated through:

### Private Connectivity
- **ExpressRoute**
- **Site-to-Site VPN**¹

### Public Connectivity
- **External Load Balancer**
- **Virtual machine with a Public IP**

### Microsoft Backbone Connectivity
- **VNet Peering** or similar peering connections to an existing landing zone in a **parent Azure region** (e.g., Australia East), allowing traffic to traverse the **Microsoft network backbone**.

---

¹ *Note: Azure VPN is a roadmap item for Azure Extended Zones. Site-to-Site VPN connectivity would currently need to be implemented using a hird-party solution*
