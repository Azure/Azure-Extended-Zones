# Azure Firewall in Azure Edge Zones (Perth)

## Audience

This document is intended for Perth Azure Edge Zone customers who want to deploy Azure Firewall in an Azure Edge Zone (Extended Zone) using the Azure portal or ARM templates. It provides full end-to-end setup instructions including ARM snippets, plus a validate deployment section.

---

## 1. What is an Azure Edge Zone (Extended Zone)?

Azure Edge Zones (Extended Zones) extend Azure infrastructure closer to customers to support low-latency and data-residency scenarios. Edge Zones are parented to an Azure region; the Azure control plane (portal/ARM/APIs) remains in the parent region.

In ARM templates:
- `location` = parent region
- `extendedLocation` = Edge Zone name

---

## 2. Azure Firewall in Extended Zones

Azure Firewall is supported in Edge Zones and behaves the same as Azure Firewall in public Azure regions.

**Same as public Azure:**
- SKUs (Standard/Premium)
- Firewall Policy and rule collections
- Autoscaling and availability
- Portal/ARM/CLI/REST APIs

**Edge Zone specific:**
- Firewall and its Public IP are created with `extendedLocation`
- `AzureFirewallSubnet` is service-managed

---

## 3. Architecture Overview

A typical deployment includes:
- VNet (Edge Zone)
- Workload subnets
- Azure Firewall
- Standard Public IP
- Route table forcing traffic via firewall
- Optional Firewall Policy

> **Important:** Customers do NOT create `AzureFirewallSubnet` manually; it is created by the Azure Firewall service.

---

## 4. Deployment Steps

### Step 1: Create a Virtual Network in the Edge Zone
- Set `location` (parent region) and `extendedLocation` (Edge Zone).
- Create workload subnets only.

### Step 2: Create a Standard Public IP in the Edge Zone
- SKU: Standard; Allocation: Static.
- Use the same `location` and `extendedLocation` as the firewall.

### Step 3: Create Azure Firewall
- Select SKU: Standard or Premium.
- (Recommended) Attach Azure Firewall Policy.
- Associate the Standard Public IP created in Step 2.

### Step 4: Configure Routing
- Create a route table.
- Add default route `0.0.0.0/0` → VirtualAppliance → firewall private IP.
- Associate the route table to workload subnets.

### Step 5: Configure Firewall Rules
- Use Firewall Policy (recommended) or classic rules.
- Network rules (L4), Application rules (L7), NAT rules (as needed).

---

## 5. ARM Template Snippets (Key Resources)

Use the same pattern for all Edge Zone resources:

```
location = <parent-region>
extendedLocation = { type: EdgeZone, name: <edge-zone-name> }
```

### 5.1 Virtual Network (Edge Zone)

```json
{
  "type": "Microsoft.Network/virtualNetworks",
  "apiVersion": "2024-05-01",
  "name": "[parameters('vnetName')]",
  "location": "[parameters('location')]",
  "extendedLocation": {
    "type": "EdgeZone",
    "name": "[parameters('edgeZoneName')]"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [ "[parameters('vnetAddressPrefix')]" ]
    },
    "subnets": [
      {
        "name": "[parameters('workloadSubnetName')]",
        "properties": {
          "addressPrefix": "[parameters('workloadSubnetPrefix')]"
        }
      }
    ]
  }
}
```

### 5.2 Standard Public IP (Edge Zone)

```json
{
  "type": "Microsoft.Network/publicIPAddresses",
  "apiVersion": "2024-05-01",
  "name": "[parameters('publicIpName')]",
  "location": "[parameters('location')]",
  "extendedLocation": {
    "type": "EdgeZone",
    "name": "[parameters('edgeZoneName')]"
  },
  "sku": {
    "name": "Standard"
  },
  "properties": {
    "publicIPAllocationMethod": "Static"
  }
}
```

### 5.3 Azure Firewall (Edge Zone)

```json
{
  "type": "Microsoft.Network/azureFirewalls",
  "apiVersion": "2024-05-01",
  "name": "[parameters('firewallName')]",
  "location": "[parameters('location')]",
  "extendedLocation": {
    "type": "EdgeZone",
    "name": "[parameters('edgeZoneName')]"
  },
  "properties": {
    "sku": {
      "name": "AZFW_VNet",
      "tier": "[parameters('firewallSkuTier')]"
    },
    "firewallPolicy": {
      "id": "[resourceId('Microsoft.Network/firewallPolicies', parameters('firewallPolicyName'))]"
    },
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "publicIPAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIpName'))]"
          },
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('vnetName'), 'AzureFirewallSubnet')]"
          }
        }
      }
    ]
  }
}
```

### 5.4 Route Table (Default Route to Firewall)

```json
{
  "type": "Microsoft.Network/routeTables",
  "apiVersion": "2024-05-01",
  "name": "[parameters('routeTableName')]",
  "location": "[parameters('location')]",
  "extendedLocation": {
    "type": "EdgeZone",
    "name": "[parameters('edgeZoneName')]"
  },
  "properties": {
    "routes": [
      {
        "name": "default-to-firewall",
        "properties": {
          "addressPrefix": "0.0.0.0/0",
          "nextHopType": "VirtualAppliance",
          "nextHopIpAddress": "[parameters('firewallPrivateIp')]"
        }
      }
    ]
  }
}
```

---

## 6. Validate Deployment

- Verify resource placement: Firewall, Public IP, and VNet show the intended Edge Zone (Perth).
- Verify `AzureFirewallSubnet` exists after deployment (service-managed).
- Verify routing: route table associated to workload subnets; `0.0.0.0/0` routes to firewall private IP.
- Verify rules/policy attached and expected allow/deny behavior.
- Validate traffic: test from workload VM; confirm allowed traffic works and denied traffic fails; check rule hits/logs if enabled.

---

## 7. Common Mistakes

- Public IP created only in parent region (missing `extendedLocation`).
- Manually creating `AzureFirewallSubnet`.
- Missing route table association to workload subnets.
- Using Basic Public IP SKU instead of Standard.

---

## 8. Deployment Checklist

**Before:**
- Edge Zone enabled subscription
- Parent region selected
- Edge Zone name identified

**Create:**
- VNet with `extendedLocation`
- Workload subnets
- Standard Public IP with `extendedLocation`
- Azure Firewall
- Optional Firewall Policy

**After:**
- Route table default route to firewall private IP
- Associate to workload subnets
- Configure rules
- Optional diagnostics

---

## 9. Summary

Azure Firewall in Azure Edge Zones (Perth) provides the same enterprise-grade security and operational experience as public Azure. Edge Zone placement is controlled using `extendedLocation`, while Azure manages firewall infrastructure, scaling, and availability.
