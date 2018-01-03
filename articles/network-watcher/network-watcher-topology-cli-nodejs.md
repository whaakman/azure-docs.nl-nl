---
title: Azure-netwerk-Watcher-topologie - Azure CLI 1.0 weergeven | Microsoft Docs
description: In dit artikel wordt beschreven hoe u met Azure CLI 1.0 query uitvoeren op uw netwerktopologie.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 5cd279d7-3ab0-4813-aaa4-6a648bf74e7b
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: bf12e1bde56c06e496d29ad27ba3da65cd94629e
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="view-network-watcher-topology-with-azure-cli-10"></a>Netwerk-Watcher-topologie met Azure CLI 1.0 weergeven

> [!div class="op_single_selector"]
> - [PowerShell](network-watcher-topology-powershell.md)
> - [CLI 1.0](network-watcher-topology-cli-nodejs.md)
> - [CLI 2.0](network-watcher-topology-cli.md)
> - [REST API](network-watcher-topology-rest.md)

De functie van de topologie van netwerk-Watcher biedt een visuele representatie van de netwerkbronnen in een abonnement. In de portal voor is deze visualisatie aan u gepresenteerd automatisch. De gegevens achter de Topologieweergave in de portal kan worden opgehaald via PowerShell.
Hierdoor is de topologische informatie flexibeler omdat de gegevens kunnen worden gebruikt door andere hulpprogramma's voor het bouwen van de visualisatie.

Dit artikel wordt platformoverschrijdende Azure CLI 1.0 gebruikt die beschikbaar is voor Windows, Mac en Linux. 

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

In dit scenario gebruikt u de `network watcher topology` cmdlet voor het ophalen van de topologische informatie. Er is ook een artikel over het [ophalen netwerktopologie met REST-API](network-watcher-topology-rest.md).

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

## <a name="scenario"></a>Scenario

Het scenario beschreven in dit artikel wordt het antwoord van de topologie voor een opgegeven resourcegroep opgehaald.

## <a name="retrieve-topology"></a>Topologie ophalen

De `network watcher topology` cmdlet haalt de topologie voor een bepaalde groep. Voeg het argument '--json ' de oput weergeven in json-indeling

```azurecli
azure network watcher topology -g resourceGroupName -n networkWatcherName -r topologyResourceGroupName --json
```

## <a name="results"></a>Resultaten

De resultaten hebben een eigenschap name 'Resources', waarin de json-antwoordtekst voor de `network watcher topology` cmdlet.  Het antwoord bevat de resources in de Netwerkbeveiligingsgroep en de bijbehorende koppelingen (dat wil zeggen, bevat, gekoppelde).

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "createdDateTime": "2017-02-17T22:20:59.461Z",
  "lastModified": "2016-12-19T22:23:02.546Z",
  "resources": [
    {
      "name": "testrg-vnet",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet",
      "location": "westcentralus",
      "associations": [
        {
          "name": "default",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
          "associationType": "Contains"
        }
      ]
    },
    {
      "name": "default",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testrg-vnet/subnets/default",
      "location": "westcentralus",
      "associations": []
    },
    {
      "name": "testclient",
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Compute/virtualMachines/testclient",
      "location": "westcentralus",
      "associations": [
        {
          "name": "testNic",
          "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/Microsoft.Network/networkInterfaces/testNic",
          "associationType": "Contains"
        }
      ]
    },
    ...    
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de beveiligingsregels voor verbindingen die worden toegepast op uw netwerkbronnen in via [beveiligingsoverzicht groep weergeven](network-watcher-security-group-view-overview.md)
