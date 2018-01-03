---
title: Netwerkbeveiliging met Azure-netwerk-Watcher beveiliging groepsweergave - Azure CLI 2.0 analyseren | Microsoft Docs
description: In dit artikel wordt beschreven hoe Azure CLI 2.0 gebruiken voor het analyseren van de beveiliging van een virtuele machines met beveiliging groep weergeven.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 9ae5ec422b197b077c52dbb2e64ddab4e08e3a50
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-20"></a>De beveiliging van uw virtuele Machine met beveiliging groep weergeven met behulp van Azure CLI 2.0 analyseren

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Groep beveiligingsweergave retourneert geconfigureerd en effectieve netwerkbeveiligingsregels die worden toegepast op een virtuele machine. Deze mogelijkheid is handig om te controleren en onderzoeken van Netwerkbeveiligingsgroepen en -regels die zijn geconfigureerd op een virtuele machine om ervoor te zorgen verkeer wordt toegestaan of geweigerd correct. In dit artikel wordt beschreven hoe u voor het ophalen van de geconfigureerde en doeltreffende beveiligingsregels voor verbindingen met een virtuele machine met Azure CLI


Dit artikel wordt de volgende generatie CLI gebruikt voor de resource management-implementatiemodel, Azure CLI 2.0, die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel worden de geconfigureerde en doeltreffende beveiligingsregels voor een bepaalde virtuele machine opgehaald.

## <a name="get-a-vm"></a>Een virtuele machine ophalen

Een virtuele machine is vereist om de `vm list` cmdlet. De volgende opdracht worden de virtuele machines in een resourcegroep:

```azurecli
az vm list -resource-group resourceGroupName
```

Zodra u weet dat de virtuele machine, kunt u de `vm show` cmdlet ophalen van de resource-Id:

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Groep beveiligingsweergave ophalen

De volgende stap is het resultaat van beveiliging groep weergave ophalen.

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-the-results"></a>De resultaten weergeven

Het volgende voorbeeld wordt een kortere antwoord van de resultaten geretourneerd. De resultaten weergeven alle beveiligingsregels voor de effectieve en toegepaste op de virtuele machine onderverdeeld in groepen van **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, en **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
      "resourceGroup": "{resourceGroupName}",
      "securityRuleAssociations": {
        "defaultSecurityRules": [
          {
            "access": "Allow",
            "description": "Allow inbound traffic from all VMs in VNET",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "*",
            "direction": "Inbound",
            "etag": null,
            "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups//providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/AllowVnetInBound",
            "name": "AllowVnetInBound",
            "priority": 65000,
            "protocol": "*",
            "provisioningState": "Succeeded",
            "resourceGroup": "",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "*"
          }...
        ],
        "effectiveSecurityRules": [
          {
            "access": "Deny",
            "destinationAddressPrefix": "*",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": null,
            "expandedSourceAddressPrefix": null,
            "name": "DefaultOutboundDenyAll",
            "priority": 65500,
            "protocol": "All",
            "sourceAddressPrefix": "*",
            "sourcePortRange": "0-65535"
          },
          {
            "access": "Allow",
            "destinationAddressPrefix": "VirtualNetwork",
            "destinationPortRange": "0-65535",
            "direction": "Outbound",
            "expandedDestinationAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "expandedSourceAddressPrefix": [
              "10.1.0.0/24",
              "168.63.129.16/32"
            ],
            "name": "DefaultRule_AllowVnetOutBound",
            "priority": 65000,
            "protocol": "All",
            "sourceAddressPrefix": "VirtualNetwork",
            "sourcePortRange": "0-65535"
          },...
        ],
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "resourceGroup": "{resourceGroupName}",
          "securityRules": [
            {
              "access": "Allow",
              "description": null,
              "destinationAddressPrefix": "*",
              "destinationPortRange": "3389",
              "direction": "Inbound",
              "etag": "W/\"efb606c1-2d54-475a-ab20-da3f80393577\"",
              "id": "/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "name": "default-allow-rdp",
              "priority": 1000,
              "protocol": "TCP",
              "provisioningState": "Succeeded",
              "resourceGroup": "{resourceGroupName}",
              "sourceAddressPrefix": "*",
              "sourcePortRange": "*"
            }
          ]
        },
        "subnetAssociation": null
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Ga naar [controle Netwerkbeveiligingsgroep groepen (NSG) met de netwerk-Watcher](network-watcher-nsg-auditing-powershell.md) voor informatie over het automatiseren van validatie van Netwerkbeveiligingsgroepen.

Meer informatie over de beveiligingsregels voor verbindingen die worden toegepast op uw netwerkbronnen in via [beveiligingsoverzicht groep weergeven](network-watcher-security-group-view-overview.md)
