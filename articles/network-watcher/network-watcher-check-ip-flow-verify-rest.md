---
title: "Controleer of verkeer verifiëren met Azure-netwerk-Watcher IP-flow - REST | Microsoft Docs"
description: Dit artikel wordt beschreven hoe u kunt controleren als verkeer naar of van een virtuele machine wordt toegestaan of geweigerd
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 3307a79f-03be-46a0-aaaf-b2079cb5f3b2
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 3ccef9ef521b86ffc1eb6047174f4f9e5d9e4296
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Controleer of het verkeer wordt toegestaan of geweigerd met IP-stroom controleren of een onderdeel van Azure-netwerk-Watcher

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST-API](network-watcher-check-ip-flow-verify-rest.md)


IP-stroom controleren is een functie van netwerk-Watcher die u controleren kunt of er verkeer is toegestaan naar of van een virtuele machine. De validatie kan worden uitgevoerd voor binnenkomend of uitgaand verkeer. Dit scenario is nuttig voor het ophalen van de huidige status of een virtuele machine contact met een externe bron of de back-end opnemen kunt. IP-stroom controleren om te controleren of als uw regels Netwerkbeveiligingsgroep (NSG) juist zijn geconfigureerd en problemen oplossen stromen die worden geblokkeerd door het NSG-regels kunnen worden gebruikt. Een andere reden voor het gebruik van IP-stroom controleren om ervoor te zorgen verkeer dat u blokkeren wilt is goed door het NSG worden geblokkeerd.

## <a name="before-you-begin"></a>Voordat u begint

ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

## <a name="scenario"></a>Scenario

Dit scenario maakt gebruik van IP-stroom controleren om te controleren of als een virtuele machine naar een andere computer via poort 443 communiceren kan. Als het verkeer wordt geweigerd, wordt de beveiligingsregel die dat verkeer wordt geweigerd. Voor meer informatie over IP-stroom controleren, gaat u naar [IP-stroom overzicht controleren](network-watcher-ip-flow-verify-overview.md)

In dit scenario u:

* Een virtuele machine ophalen
* Aanroep van IP-stroom controleren
* Resultaten controleren

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

```PowerShell
armclient login
```

## <a name="retrieve-a-virtual-machine"></a>Een virtuele machine ophalen

Voer het volgende script om te retourneren van een virtuele machine. De volgende code moet waarden voor de variabelen:

* **subscriptionId** -de abonnements-Id te gebruiken.
* **resourceGroupName** -de naam van een resourcegroep die virtuele machines bevatten.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"

armclient get https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Compute/virtualMachines?api-version=2015-05-01-preview
```

De informatie die nodig is de id onder het type `Microsoft.Compute/virtualMachines`. De resultaten moeten zijn vergelijkbaar met het volgende codevoorbeeld:

```json
...,
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft
.Network/networkInterfaces/contosovm842"
            }
          ]
        },
        "provisioningState": "Succeeded"
      },
      "resources": [
        {
          "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Com
pute/virtualMachines/ContosoVM/extensions/CustomScriptExtension"
        }
      ],
      "type": "Microsoft.Compute/virtualMachines",
      "location": "westcentralus",
      "id": "/subscriptions/{00000000-0000-0000-0000-000000000000}/resourceGroups/ContosoExampleRG/providers/Microsoft.Compute
/virtualMachines/ContosoVM",
      "name": "ContosoVM"
    }
  ]
}
```

## <a name="call-ip-flow-verify"></a>Aanroep van IP-stroom controleren

Het volgende voorbeeld wordt een verzoek om te controleren of het verkeer voor een opgegeven virtuele machine. Het antwoord retourneert als het verkeer wordt toegestaan of als het verkeer wordt geweigerd. Als verkeer wordt geweigerd dat ook wordt het verkeer wordt geblokkeerd door welke regel.

> [!NOTE]
> IP-stroom controleren vereist dat de VM-resource wordt toegewezen.

Het script is vereist voor de resource-Id van een virtuele machine en een netwerkinterfacekaart op de virtuele machine. Deze waarden worden geleverd door de voorgaande uitvoer.

> [!Important]
> De naam van de resourcegroep in de aanvraag-URI is voor alle netwerk-Watcher REST-aanroepen die het bevat de netwerk-Watcher-instantie, niet de bronnen die u wilt de diagnostische acties uitvoeren op.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>"
$networkWatcherName = "<network watcher name>"
$vmName = "<vm name>"
$vmNICName = "<vm NIC name>"
$direction = "<direction of traffic>" # Examples are: Inbound or Outbound
$localIP = "<source IP>"
$localPort = "<source Port>"
$remoteIP = "<destination IP>"
$remotePort = "<destination Port>" # Examples are: 80, or 80-120
$protocol = "<UDP, TCP or *>"
$targetUri = "<uri of target resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.compute/virtualMachine/${vmName}
$targetNic = "<uri of target nic resource>" # Example: /subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkInterfaces/${vmNICName}

$requestBody = @"
{
    'targetResourceId':  '$targetUri',
    'direction':  '$direction',
    'protocol':  '$protocol',
    'localPort':  '$localPort',
    'remotePort':  '$remotePort',
    'localIPAddress':  '$localIP',
    'remoteIPAddress':  '$remoteIP',
    'targetNICResourceId':  '$targetNic'
}
"@
        
armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/ipFlowVerify?api-version=2016-12-01" $requestBody -verbose
```

## <a name="understanding-the-results"></a>Inzicht in de resultaten

Het antwoord dat u terughalen vertelt u of het verkeer wordt toegestaan of geweigerd. Het antwoord ziet eruit als een van de volgende voorbeelden:

**Toegestaan**

```json
{
  "access": "Allow",
  "ruleName": "defaultSecurityRules/AllowInternetOutBound"
}
```

**Geweigerd**

```json
{
  "access": "Deny",
  "ruleName": "defaultSecurityRules/DefaultInboundDenyAll"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie als verkeer wordt geblokkeerd en mag geen [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor meer informatie over Netwerkbeveiligingsgroepen.












