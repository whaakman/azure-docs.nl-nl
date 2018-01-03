---
title: Weergeven van de Azure-netwerk-Watcher-topologie - REST-API | Microsoft Docs
description: In dit artikel wordt beschreven hoe REST-API gebruiken om op te vragen uw netwerktopologie.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: de9af643-aea1-4c4c-89c5-21f1bf334c06
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: fefeae4e816994d3ee69d6ac1c1cbe6cf8bbd06e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="view-network-watcher-topology-with-rest-api"></a>Netwerk-Watcher-topologie met REST-API weergeven

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

De functie van de topologie van netwerk-Watcher biedt een visuele representatie van de netwerkbronnen in een abonnement. In de portal voor is deze visualisatie aan u gepresenteerd automatisch. De gegevens achter de Topologieweergave in de portal kan worden opgehaald via PowerShell.
Hierdoor is de topologische informatie flexibeler omdat de gegevens kunnen worden gebruikt door andere hulpprogramma's voor het bouwen van de visualisatie.

De koppeling is gemodelleerd onder twee-relaties.

- **Containment** -voorbeeld: VNet bevat een Subnet bevat een NIC
- **Gekoppeld** -voorbeeld: NIC is gekoppeld aan een virtuele machine

De volgende lijst bevat eigenschappen die worden geretourneerd bij het opvragen van de topologie REST-API.

* **naam** -de naam van de resource
* **id** -de uri van de resource.
* **locatie** -de locatie waar de resource is opgeslagen.
* **koppelingen** -een lijst met koppelingen naar het object waarnaar wordt verwezen.
    * **naam** -de naam van de bron waarnaar wordt verwezen.
    * **resourceId** -resourceId van de is de uri van de bron waarnaar wordt verwezen in de koppeling.
    * **associationType** -deze waarde verwijst naar de relatie tussen het onderliggende object en het bovenliggende item. Geldige waarden zijn **bevat** of **gekoppelde**.

## <a name="before-you-begin"></a>Voordat u begint

In dit scenario moet u de topologische informatie ophalen. ARMclient wordt gebruikt voor het aanroepen van de REST-API met behulp van PowerShell. ARMClient is gevonden op chocolatey op [ARMClient op Chocolatey](https://chocolatey.org/packages/ARMClient)

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel wordt het antwoord van de topologie voor een opgegeven resourcegroep opgehaald.

## <a name="log-in-with-armclient"></a>Meld u aan met ARMClient

Aanmelden bij armclient met uw Azure-referenties.

```PowerShell
armclient login
```

## <a name="retrieve-topology"></a>Topologie ophalen

Het volgende voorbeeld vraagt de topologie van de REST-API.  In het voorbeeld met parameters om toe te staan voor flexibiliteit bij het maken van een voorbeeld.  Vervang alle waarden met \< \> omsluiten.

```powershell
$subscriptionId = "<subscription id>"
$resourceGroupName = "<resource group name>" # Resource group name to run topology on
$NWresourceGroupName = "<resource group name>" # Network Watcher resource group name
$networkWatcherName = "<network watcher name>"
$requestBody = @"
{
    'targetResourceGroupName': '${resourceGroupName}'
}
"@

armclient POST "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${NWresourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/topology?api-version=2016-07-01" $requestBody
```

Het volgende antwoord is een voorbeeld van een kortere reactie die wordt geretourneerd wanneer de topologie voor een resourcegroup ophalen:

```json
{
  "id": "ecd6c860-9cf5-411a-bdba-512f8df7799f",
  "createdDateTime": "2017-01-18T04:13:07.1974591Z",
  "lastModified": "2017-01-17T22:11:52.3527348Z",
  "resources": [
    {
      "name": "virtualNetwork1",
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/{virtualNetworkName}",
      "location": "westcentralus",
      "associations": [
        {
          "name": "{subnetName}",
          "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/virtualNetworks/(virtualNetworkName)/subnets
/{subnetName}",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "webtestnsg-wjplxls65qcto",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}
s65qcto",
      "associationType": "Associated"
    },
    ...
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw NSG stroom logboeken met Power BI in via [visualiseren NSG loopt logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

