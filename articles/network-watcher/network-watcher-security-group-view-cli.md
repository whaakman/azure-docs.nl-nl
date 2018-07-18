---
title: Analyseren van netwerkbeveiliging met Azure Network Watcher weergave van de beveiligingsgroep - Azure CLI 2.0 | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Azure CLI 2.0 gebruiken voor het analyseren van een beveiliging voor virtuele machines met de weergave van de beveiligingsgroep.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: a986ff4f-7e0c-4994-95e1-4ac824986500
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 82cd0d97a64819ae8528850ba9a44800bf960afc
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090552"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-20"></a>Analyseren van de beveiliging van uw virtuele machines met de weergave van de beveiligingsgroep met behulp van Azure CLI 2.0

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [Azure-CLI](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Weergave van de beveiligingsgroep retourneert geconfigureerde en toegepaste netwerkbeveiligingsgroepsregels beveiligingsregels die worden toegepast op een virtuele machine. Deze mogelijkheid is handig om te controleren en onderzoeken van Netwerkbeveiligingsgroepen en regels die zijn geconfigureerd op een virtuele machine om te controleren of verkeer wordt toegestaan of geweigerd correct. In dit artikel hebben we laten zien hoe u aan een virtuele machine met behulp van Azure CLI de geconfigureerde en effectieve beveiligingsregels ophalen


In dit artikel wordt onze CLI van de volgende generatie voor het implementatiemodel resource management, Azure CLI 2.0 die beschikbaar is voor Windows, Mac en Linux.

Als u wilt de stappen in dit artikel uitvoert, moet u [installeren van de Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher.

## <a name="scenario"></a>Scenario

Het scenario in dit artikel besproken wordt de geconfigureerde en effectieve beveiligingsregels voor een bepaalde virtuele machine opgehaald.

## <a name="get-a-vm"></a>Een VM ophalen

Een virtuele machine is vereist om uit te voeren de `vm list` cmdlet. De volgende opdracht worden de virtuele machines in een resourcegroep:

```azurecli
az vm list -resource-group resourceGroupName
```

Zodra u weet de virtuele machine dat, kunt u de `vm show` cmdlet om op te halen van de resource-Id:

```azurecli
az vm show -resource-group resourceGroupName -name virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Weergave van de beveiligingsgroep ophalen

De volgende stap is om op te halen van het resultaat van beveiliging groep weergeven.

```azurecli
az network watcher show-security-group-view --resource-group resourceGroupName --vm vmName
```

## <a name="viewing-the-results"></a>De resultaten weer te geven

Het volgende voorbeeld wordt een afgekorte respons van de geretourneerde resultaten. De resultaten staat dat alle de effectief en toegepaste beveiligingsregels op de virtuele machine onderverdeeld in groepen van **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, en  **EffectiveSecurityRules**.

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

Ga naar [controle Netwerkbeveiligingsgroep groepen (NSG) met Network Watcher](network-watcher-nsg-auditing-powershell.md) voor informatie over het automatiseren van validatie van Netwerkbeveiligingsgroepen.

Meer informatie over de beveiligingsregels die worden toegepast op uw netwerkbronnen, recentst [overzicht van Netwerkbeveiligingsgroepen weergeven](network-watcher-security-group-view-overview.md)
