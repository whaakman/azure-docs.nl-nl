---
title: Implementeren van een IPv6 dual stack-toepassing in Azure-netwerk - Resource Manager-sjabloon (preview)
titlesuffix: Azure Virtual Network
description: In dit artikel toont hoe een toepassing IPv6 dual-stack in Azure-netwerk met behulp van Azure Resource Manager-VM-sjablonen implementeren.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: ae90bc4a12763803f38224d917c4644a68ae7d6b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130930"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure---template-preview"></a>Implementeren van een IPv6 dual stack-toepassing in Azure - sjabloon (Preview)

Dit artikel bevat een lijst met taken voor IPv6-configuratie met het gedeelte van de Azure Resource Manager-VM-sjabloon die van toepassing op. De sjabloon die worden beschreven in dit artikel gebruiken voor het implementeren van een dual-stack (IPv4 + IPv6)-toepassing in Azure met een dual-stack virtueel netwerk met IPv4 en IPv6-subnetten, een load balancer met dual (IPv4 + IPv6) front-configuraties, VM's met NIC's waarvoor een dubbele IP-adres configuratie, netwerkbeveiligingsgroep en openbare IP-adressen. 

## <a name="required-configurations"></a>Vereiste configuraties

Zoeken naar de sjabloonsecties in de sjabloon om te zien waar ze moeten plaatsvinden.

### <a name="ipv6-addressspace-for-the-virtual-network"></a>De addressSpace IPv6 voor het virtuele netwerk

De sjabloonsectie om toe te voegen:

```JSON
        "addressSpace": {
          "addressPrefixes": [
            "[variables('vnetv4AddressRange')]",
            "[variables('vnetv6AddressRange')]"    
```

### <a name="ipv6-subnet-within-the-ipv6-virtual-network-addressspace"></a>IPv6-subnet binnen de addressSpace van het IPv6-netwerk

De sjabloonsectie om toe te voegen:
```JSON
          {
            "name": "V6Subnet",
            "properties": {
              "addressPrefix": "[variables('subnetv6AddressRange')]"
            }

```

### <a name="ipv6-configuration-for-the-nic"></a>IPv6-configuratie voor de NIC

De sjabloonsectie om toe te voegen:
```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

### <a name="ipv6-network-security-group-nsg-rules"></a>IPv6-netwerk (NSG) regels voor netwerkbeveiligingsgroepen

```JSON
          {
            "name": "default-allow-rdp",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "33819-33829",
              "destinationPortRange": "5000-6000",
              "sourceAddressPrefix": "ace:cab:deca:deed::/64",
              "destinationAddressPrefix": "cab:ace:deca:deed::/64",
              "access": "Allow",
              "priority": 1003,
              "direction": "Inbound"
            }
```

## <a name="conditional-configuration"></a>Voorwaardelijke configuratie

Als u een virtueel netwerkapparaat, kunt u de IPv6-routes toevoegen in de routetabel. Anders wordt is deze configuratie optioneel.

```JSON
    {
      "type": "Microsoft.Network/routeTables",
      "name": "v6route",
      "apiVersion": "[variables('ApiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "routes": [
          {
            "name": "v6route",
            "properties": {
              "addressPrefix": "ace:cab:deca:deed::/64",
              "nextHopType": "VirtualAppliance",
              "nextHopIpAddress": "deca:cab:ace:f00d::1"
            }
```

## <a name="optional-configuration"></a>Optionele configuratie

### <a name="ipv6-internet-access-for-the-virtual-network"></a>IPv6-Internet toegang voor het virtuele netwerk

```JSON
{
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-public-ip-addresses"></a>Openbare IPv6-IP-adressen

```JSON
    {
      "apiVersion": "[variables('ApiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "lbpublicip-v6",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "publicIPAddressVersion": "IPv6"
      }
```

### <a name="ipv6-front-end-for-load-balancer"></a>IPv6-Front-end voor Load Balancer

```JSON
          {
            "name": "LBFE-v6",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses','lbpublicip-v6')]"
              }
```

### <a name="ipv6-back-end-address-pool-for-load-balancer"></a>IPv6-Back-end-adresgroep voor de Load Balancer

```JSON
              "backendAddressPool": {
                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', 'loadBalancer'), '/backendAddressPools/LBBAP-v6')]"
              },
              "protocol": "Tcp",
              "frontendPort": 8080,
              "backendPort": 8080
            },
            "name": "lbrule-v6"
```

### <a name="ipv6-load-balancer-rules-to-associate-incoming-and-outgoing-ports"></a>IPv6-load balancer-regels om te koppelen van binnenkomende en uitgaande poorten

```JSON
          {
            "name": "ipconfig-v6",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
          "privateIPAddressVersion":"IPv6",
              "subnet": {
                "id": "[variables('v6-subnet-id')]"
              },
              "loadBalancerBackendAddressPools": [
                {
                  "id": "[concat(resourceId('Microsoft.Network/loadBalancers','loadBalancer'),'/backendAddressPools/LBBAP-v6')]"
                }
```

## <a name="sample-vm-template-json"></a>Voorbeeld van een VM-sjabloon JSON
Klik op [hier](https://azure.microsoft.com/resources/templates/ipv6-in-vnet/) een IPv6 dual stack-toepassing in Azure-netwerk met behulp van Azure Resource Manager-sjabloon te implementeren.

## <a name="next-steps"></a>Volgende stappen

U vindt meer informatie over de prijzen voor [openbare IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses/), [netwerkbandbreedte](https://azure.microsoft.com/pricing/details/bandwidth/), of [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/).
