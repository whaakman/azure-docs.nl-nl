---
title: Analyseren van netwerkbeveiliging met Azure Network Watcher weergave van de beveiligingsgroep - REST-API | Microsoft Docs
description: In dit artikel wordt beschreven hoe u PowerShell gebruikt voor het analyseren van een beveiliging voor virtuele machines met de weergave van de beveiligingsgroep.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: a2f418fe-f5d2-43ed-8dc3-df0ed2a4d4ac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 77519c57c9a603ae0eff46d248ff15432d0404fa
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485387"
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-rest-api"></a>Analyseren van de beveiliging van uw virtuele machines met de weergave van de beveiligingsgroep met behulp van REST-API

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [Azure-CLI](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Weergave van de beveiligingsgroep retourneert geconfigureerde en toegepaste netwerkbeveiligingsgroepsregels beveiligingsregels die worden toegepast op een virtuele machine. Deze mogelijkheid is handig om te controleren en onderzoeken van Netwerkbeveiligingsgroepen en regels die zijn geconfigureerd op een virtuele machine om te controleren of verkeer wordt toegestaan of geweigerd correct. In dit artikel hebben we laten zien hoe u om op te halen van de kracht en toegepaste beveiligingsregels aan een virtuele machine met behulp van REST-API

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario, moet u de netwerk-Watcher Rest-API voor de weergave van de beveiligingsgroep voor een virtuele machine aanroepen. ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

In dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een Network Watcher](network-watcher-create.md) te maken van een Network Watcher. Het scenario ook van uitgegaan dat er een resourcegroep met een geldige virtuele machine bestaat om te worden gebruikt.

## <a name="scenario"></a>Scenario

Het scenario in dit artikel besproken haalt de effectief en toegepaste beveiligingsregels voor een bepaalde virtuele machine.

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

```powershell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Een virtuele machine ophalen

Voer het volgende script om te retourneren van een virtuele machineThe moet variabelen de volgende code:

- **subscriptionId** -de abonnements-id kan ook worden opgehaald met de **Get-AzureRMSubscription** cmdlet.
- **resourceGroupName** -de naam van een resourcegroep met virtuele machines.

```powershell
$subscriptionId = '<subscription id>'
$resourceGroupName = '<resource group name>'

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

De informatie die nodig is, is de **id** onder het type `Microsoft.Compute/virtualMachines` antwoord, zoals te zien is in het volgende voorbeeld:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft
.Network/networkInterfaces/{nicName}"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Com
pute/virtualMachines/{vmName}/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute
/virtualMachines/{vmName}",
      "name": "{vmName}"
    }
  ]
}
```

## <a name="get-security-group-view-for-virtual-machine"></a>Weergave van de beveiligingsgroep voor de virtuele machine ophalen

Het volgende voorbeeld vraagt de weergave van de beveiligingsgroep van een bepaalde virtuele machine. De resultaten van dit voorbeeld kunnen worden gebruikt om te vergelijken met de regels en beveiliging die zijn gedefinieerd door de oorspronkelijke configuratie drift gezocht.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.compute/virtualMachine/$vmName

$requestBody = @"
{
    'targetResourceId': '${targetUri}'

}
"@
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/securityGroupView?api-version=2016-12-01" $requestBody -verbose
```

## <a name="view-the-response"></a>Bekijk het antwoord

Het volgende voorbeeld is het antwoord geretourneerd door de voorgaande opdracht. De resultaten staat dat alle de effectief en toegepaste beveiligingsregels op de virtuele machine onderverdeeld in groepen van **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, en  **EffectiveSecurityRules**.

```json

{
  "networkInterfaces": [
    {
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkInterfaces/{nicName}",
          "securityRules": [
            {
              "name": "default-allow-rdp",
              "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/securityRules/default-allow-rdp",
              "etag": "W/\"d4c411d4-0d62-49dc-8092-3d4b57825740\"",
              "properties": {
                "provisioningState": "Succeeded",
                "protocol": "TCP",
                "sourcePortRange": "*",
                "destinationPortRange": "3389",
                "sourceAddressPrefix": "*",
                "destinationAddressPrefix": "*",
                "access": "Allow",
                "priority": 1000,
                "direction": "Inbound"
              }
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "name": "AllowVnetInBound",
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}/defaultSecurityRules/",
            "properties": {
              "provisioningState": "Succeeded",
              "description": "Allow inbound traffic from all VMs in VNET",
              "protocol": "*",
              "sourcePortRange": "*",
              "destinationPortRange": "*",
              "sourceAddressPrefix": "VirtualNetwork",
              "destinationAddressPrefix": "VirtualNetwork",
              "access": "Allow",
              "priority": 65000,
              "direction": "Inbound"
            }
          },
          ...
        ],
        "effectiveSecurityRules": [
          {
            "name": "DefaultOutboundDenyAll",
            "protocol": "All",
            "sourcePortRange": "0-65535",
            "destinationPortRange": "0-65535",
            "sourceAddressPrefix": "*",
            "destinationAddressPrefix": "*",
            "access": "Deny",
            "priority": 65500,
            "direction": "Outbound"
          },
          ...
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Ga naar [controle Netwerkbeveiligingsgroep groepen (NSG) met Network Watcher](network-watcher-security-group-view-powershell.md) voor informatie over het automatiseren van validatie van Netwerkbeveiligingsgroepen.


