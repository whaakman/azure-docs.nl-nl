---
title: Inleiding tot stroomlogboeken voor netwerkbeveiliging groepen met Azure Network Watcher | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u de functie NSG stroom logboeken van Azure Network Watcher.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: ae4edb82fa5e192a30d297dae82199bb7efca0c2
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344968"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Inleiding tot stroomlogboeken voor netwerkbeveiligingsgroepen

Stroomlogboeken van Network security group (NSG) zijn een functie van Network Watcher waarmee u informatie wilt weergeven over inkomende en uitgaande IP-verkeer via een NSG. Logboeken van de stroom worden geschreven in json-indeling en weergeven van binnenkomende en uitgaande stromen op basis van een per regel, de netwerkinterface (NIC) de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP, bron-/ doel-poort en protocol), en als het verkeer is toegestaan of geweigerd.

![stroom-Logboeken, overzicht](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Stroomlogboeken voor nsg's doel, zijn ze niet hetzelfde als de andere logboeken weergegeven. Logboeken van de stroom alleen binnen een storage-account worden opgeslagen en volg het pad naar logboek wordt weergegeven in het volgende voorbeeld:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

De dezelfde bewaarbeleid zichtbaar voor andere logboeken van toepassing op Logboeken van de stroom. U kunt beleid voor het bewaren van logboek ingesteld van 1 dag op 2147483647 dagen. Als geen bewaarbeleid is ingesteld, worden de logboeken voor altijd bewaard.

## <a name="log-file"></a>Logboekbestand

Logboeken van de stroom zijn onder andere de volgende eigenschappen:

* **tijd** - tijd waarop de gebeurtenis is vastgelegd
* **systeem-id** -Netwerkbeveiligingsgroep resource-id.
* **categorie** -de categorie van de gebeurtenis. De categorie is altijd **NetworkSecurityGroupFlowEvent**
* **ResourceID** -de resource-Id van de NSG
* **operationName** -altijd NetworkSecurityGroupFlowEvents
* **Eigenschappen van** -een verzameling eigenschappen van de stroom
    * **Versie** -versienummer van het schema van de event Log Flow
    * **stromen** -een verzameling van stromen. Deze eigenschap heeft meerdere vermeldingen voor verschillende regels
        * **regel** -regel voor de stromen worden weergegeven
            * **stromen** -een verzameling van stromen
                * **Mac** -de MAC-adres van de NIC voor de virtuele machine waarop de stroom is verzameld
                * **flowTuples** -een tekenreeks met meerdere eigenschappen voor de stroom tuple in indeling met door komma's gescheiden
                    * **Tijdstempel** -deze waarde is de tijdstempel van wanneer de stroom is opgetreden in de indeling van de UNIX-EPOCHE
                    * **Bron-IP** -het bron-IP
                    * **Bestemming IP** -de doel-IP
                    * **Bronpoort** -de bronpoort
                    * **Doelpoort** -de doel-poort
                    * **Protocol** -het protocol van de stroom. Geldige waarden zijn **T** voor TCP- en **U** voor UDP
                    * **Verkeersstroom** -de richting van het netwerkverkeer. Geldige waarden zijn **ik** voor binnenkomend en **O** voor uitgaande.
                    * **Verkeer** - of verkeer is toegestaan of geweigerd. Geldige waarden zijn **A** voor toegestaan en **D** voor geweigerd.

De tekst die volgt is een voorbeeld van een stroomlogboek. Zoals u ziet, zijn er meerdere records die de lijst met eigenschappen die worden beschreven in de voorgaande sectie volgen.

> [!NOTE]
> De waarden in de **flowTuples* eigenschap zijn een door komma's gescheiden lijst.
 
```json
{
    "records":
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over het inschakelen van Logboeken van de stroom, [inschakelen van NSG-stroomlogboeken](network-watcher-nsg-flow-logging-portal.md).
- Zie voor meer informatie over NSG-Logboeken, [Log analytics voor netwerkbeveiligingsgroepen (nsg's)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Om te bepalen of verkeer is toegestaan of vanaf of naar een virtuele machine verboden, Zie [een probleem VM-netwerk-verkeersfilter vaststellen](diagnose-vm-network-traffic-filtering-problem.md)
