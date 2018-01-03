---
title: Netwerkbeveiliging met Azure-netwerk-Watcher beveiliging groepsweergave - Azure CLI 1.0 analyseren | Microsoft Docs
description: In dit artikel wordt beschreven hoe Azure CLI 1.0 gebruiken voor het analyseren van de beveiliging van een virtuele machines met beveiliging groep weergeven.
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
ms.openlocfilehash: f27157129bea4e47a2e0e6cc1169b9e4887bdd78
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="analyze-your-virtual-machine-security-with-security-group-view-using-azure-cli-10"></a>De beveiliging van uw virtuele Machine met beveiliging groep weergeven met behulp van Azure CLI 1.0 analyseren

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-security-group-view-powershell.md)
> - [CLI 1.0](network-watcher-security-group-view-cli-nodejs.md)
> - [CLI 2.0](network-watcher-security-group-view-cli.md)
> - [REST API](network-watcher-security-group-view-rest.md)

Groep beveiligingsweergave retourneert geconfigureerd en effectieve netwerkbeveiligingsregels die worden toegepast op een virtuele machine. Deze mogelijkheid is handig om te controleren en onderzoeken van Netwerkbeveiligingsgroepen en -regels die zijn geconfigureerd op een virtuele machine om ervoor te zorgen verkeer wordt toegestaan of geweigerd correct. In dit artikel wordt beschreven hoe u voor het ophalen van de geconfigureerde en doeltreffende beveiligingsregels voor verbindingen met een virtuele machine met Azure CLI

Dit artikel wordt platformoverschrijdende Azure CLI 1.0 gebruikt die beschikbaar is voor Windows, Mac en Linux. Netwerk-Watcher gebruikt momenteel de Azure CLI 1.0 voor CLI-ondersteuning.

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel worden de geconfigureerde en doeltreffende beveiligingsregels voor een bepaalde virtuele machine opgehaald.

## <a name="get-a-vm"></a>Een virtuele machine ophalen

Een virtuele machine is vereist om de `vm list` cmdlet. De volgende opdracht worden de virtuele machinese in een resourcegroep:

```azurecli
azure vm list -g resourceGroupName
```

Zodra u weet dat de virtuele machine, kunt u de `vm show` cmdlet ophalen van de resource-Id:

```azurecli
azure vm show -g resourceGroupName -n virtualMachineName
```

## <a name="retrieve-security-group-view"></a>Groep beveiligingsweergave ophalen

De volgende stap is het resultaat van beveiliging groep weergave ophalen. Toevoegen van de '--json ' vlag indeling van de resultaten in json.

```azurecli
azure network watcher security-group-view -g resourceGroupName -n networkWatcherName -t targetResourceId --json
```

## <a name="viewing-the-results"></a>De resultaten weergeven

Het volgende voorbeeld wordt een kortere antwoord van de resultaten geretourneerd. De resultaten weergeven alle beveiligingsregels voor de effectieve en toegepaste op de virtuele machine onderverdeeld in groepen van **NetworkInterfaceSecurityRules**, **DefaultSecurityRules**, en **EffectiveSecurityRules**.

```json
{
  "networkInterfaces": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testnic",
      "securityRuleAssociations": {
        "networkInterfaceAssociation": {
          "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testvm",
          "securityRules": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/test-nsg/securityRules/default-allow-rdp",
              "protocol": "TCP",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "*",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 1000,
              "direction": "Inbound",
              "provisioningState": "Succeeded",
              "name": "default-allow-rdp",
              "etag": "W/\"00000000-0000-0000-0000-000000000000\""
            }
          ]
        },
        "defaultSecurityRules": [
          {
            "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/networkSecurityGroups//defaultSecurityRules/",
            "description": "Allow inbound traffic from all VMs in VNET",
            "protocol": "*",
            "sourcePortRange": "*",
            "destinationPortRange": "*",
            "sourceAddressPrefix": "VirtualNetwork",
            "destinationAddressPrefix": "VirtualNetwork",
            "access": "Allow",
            "priority": 65000,
            "direction": "Inbound",
            "provisioningState": "Succeeded",
            "name": "AllowVnetInBound"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Ga naar [controle Netwerkbeveiligingsgroep groepen (NSG) met de netwerk-Watcher](network-watcher-nsg-auditing-powershell.md) voor informatie over het automatiseren van validatie van Netwerkbeveiligingsgroepen.

Meer informatie over de beveiligingsregels voor verbindingen die worden toegepast op uw netwerkbronnen in via [beveiligingsoverzicht groep weergeven](network-watcher-security-group-view-overview.md)
