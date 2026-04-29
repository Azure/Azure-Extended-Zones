# Observability and Monitoring

Effective observability is essential for maintaining the health, performance, and security of workloads deployed in an Azure Extended Zone. This section provides guidance on configuring **alerts and notifications**, **log collection**, **metrics**, **insights**, and **workbooks** for Azure Monitor, as well as **application performance monitoring** tools including Application Insights, Network Watcher, flow logs, and boot diagnostics. It highlights key constraints — such as the requirement to create Log Analytics workspaces and certain resources in the parent region — and the cost implications of cross-region telemetry.

## Table of Contents

- [Observability](#observability)
  - [Alerts, Actions and Notifications](#alerts-actions-and-notifications)
  - [Service Health](#service-health)
  - [Logs](#logs)
  - [Metrics](#metrics)
  - [Insights](#insights)
  - [Workbooks](#workbooks)
- [Application Performance Monitoring](#application-performance-monitoring)
  - [Application Insights](#application-insights)
  - [Network Watcher](#network-watcher)
  - [Connection Monitor](#connection-monitor)
  - [Network Security Group Flow Logs](#network-security-group-flow-logs)
  - [Virtual Network Flow Logs](#virtual-network-flow-logs)
  - [Virtual Machine Boot Diagnostics](#virtual-machine-boot-diagnostics)

---

## Observability

### Alerts, Actions and Notifications

#### Action Groups

Action groups cannot be created in an Azure Extended Zone. Create a global action group to monitor resources in Extended Zones. Action groups are typically global services, with regional variants available only in select Azure regions.

Global action groups can process client requests from any region. If a specific region's service is unavailable, the requests are automatically routed to and processed by services in other regions, ensuring uninterrupted service. This global approach provides a robust disaster recovery solution. Regional requests, on the other hand, rely on availability zone redundancy to comply with privacy requirements and offer a similar level of disaster recovery.

### Service Health

Azure Service Health comprises three integral services:

- **Azure Status** informs users of service outages in Azure on the Azure Status page. This page provides a global view of the health of all Azure services across all regions.
- **Service Health** offers a personalized view of the health of the Azure services and regions in use. This is the optimal source for service-impacting communications about outages, planned maintenance activities, and other health advisories.
- **Resource Health** provides information about the health of individual cloud resources, such as specific virtual machine instances.

Service Health is supported for resources deployed to an Azure Extended Zone.

### Logs

Create Log Analytics workspaces in a parent region because they cannot be created within an Azure Extended Zone. Cross-region telemetry may incur additional network costs based on source and destination regions. For details, see Azure Bandwidth pricing documentation.

### Metrics

Azure Monitor Metrics is a sophisticated feature within Azure Monitor that aggregates numerical data from monitored resources into a comprehensive time-series database. These metrics, collected at regular intervals, provide critical insights into various aspects of system performance at specific points in time.

Azure Metrics is compatible with resources deployed within an Azure Extended Zone, ensuring precise and reliable monitoring capabilities.

### Insights

Insights can be used with resources deployed within an Azure Extended Zone.

### Workbooks

Workbooks can be used with resources deployed within an Azure Extended Zone.

---

## Application Performance Monitoring

### Application Insights

Application Insights can be used with resources deployed in an Azure Extended Zone. For additional information on workspace-based Application Insights resources, see the [Logs](#logs) section of this document.

### Network Watcher

Network Watcher is automatically created to provide immediate access to its monitoring and diagnostic capabilities for your virtual network, eliminating the need for additional configuration. When a virtual network is created or updated in an Azure Extended Zone, the Network Watcher resource is instantiated in the parent region, which hosts the metadata of the virtual network resource group.

### Connection Monitor

It is not possible to create a connection monitor resource in an Azure Extended Zone; however, it is possible to select a parent region for the connection monitor.

For information regarding configuring a log analytics workspace for connection monitoring, please refer to the [Logs](#logs) section of this document.

### Network Security Group Flow Logs

When creating a flow log for a network security group deployed in an Azure Extended Zone, the region will be locked to the parent region — the region hosting the metadata of the virtual network resource group. Consequently, the storage account used by the network security group flow logs must also reside in the parent region.

Key considerations for this configuration include:

- **Performance and Latency:** Use a storage account in the same region as your network security group and associated virtual network resources to minimize latency and maximize performance.
- **Data Transfer Costs:** Storing flow logs in a different region may incur additional data transfer costs.
- **Compliance and Data Residency:** Ensure that your configuration adheres to any data residency requirements your organization may have.

### Virtual Network Flow Logs

When creating a virtual network flow log for a virtual network deployed in an Azure Extended Zone, the region will be locked to the parent region — the region hosting the metadata of the virtual network resource group. Consequently, the storage account used by the virtual network flow logs must also reside in the parent region.

Considerations for this configuration include:

- **Performance and Latency:** Use a storage account in the same region as your virtual network and connected resources to minimize latency and maximize performance.
- **Data Transfer Costs:** Storing flow logs in a different region may incur additional data transfer costs.
- **Compliance and Data Residency:** Ensure that your configuration complies with any data residency requirements your organization might have.

Please refer to the [Logs](#logs) section of this document for detailed information on configuring a Log Analytics workspace for traffic analysis.

### Virtual Machine Boot Diagnostics

The configuration of Azure boot diagnostics is supported for virtual machines deployed in an Azure Extended Zone as part of the process for creating a new virtual machine. 

When creating a virtual machine in the portal, Azure automatically creates a storage account in the resource group's region (the parent region for a new Azure Extended Zone virtual machine). Users cannot choose a custom storage account when default settings are used during virtual machine creation. Managed boot diagnostics are exclusively supported on Azure Extended Zones.

