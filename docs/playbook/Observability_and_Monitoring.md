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

Action groups cannot be created in an Azure Extended Zone. It is recommended to create a global action group for monitoring resources within such zones. Typically, action groups are global services, with regional variations only available in select Azure regions.

Global action groups can process client requests from any region. If a specific region's service is unavailable, the requests are automatically routed to and processed by services in other regions, ensuring uninterrupted service. This global approach provides a robust disaster recovery solution. Regional requests, on the other hand, rely on availability zone redundancy to comply with privacy requirements and offer a similar level of disaster recovery.

### Service Health

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

Application Insights can be utilized with resources deployed in an Azure Extended Zone. For additional information on Workspace-based Application Insights resources, please refer to the [Logs](#logs) section of this document.

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

For information regarding configuring a log analytics workspace for connection monitoring, please refer to the [Logs](#logs) section of this document.

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

Please refer to the [Logs](#logs) section of this document for detailed information on configuring a log analytics workspace for traffic analysis.

### Virtual Machine Boot Diagnostics

The configuration of Azure boot diagnostics is supported for virtual machines deployed in an Azure Extended Zone as part of the process for creating a new virtual machine. Boot diagnostics is a crucial debugging feature for Azure virtual machines that facilitates the diagnosis of VM boot failures, allowing users to monitor the state of their VM during the boot process by collecting serial log information and screenshots.

It should be noted that the portal experience for creating a virtual machine will automatically create a storage account in the same region as the resource group's region, which is considered the parent region for a new Azure Extended Zone virtual machine. Users cannot opt to use a custom storage account instead of the pre-provisioned Microsoft-managed storage account that is created when default settings are used during virtual machine creation. Managed boot diagnostics are exclusively supported on Azure Extended Zones.

---
