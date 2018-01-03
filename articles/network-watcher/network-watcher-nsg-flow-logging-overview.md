---
title: Inleiding tot registratie van de stroom voor Netwerkbeveiligingsgroepen met Azure-netwerk-Watcher | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u NSG stroom Logboeken gebruiken een functie van Azure-netwerk-Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 4eaffba08ccf601e440709d804891668340a376d
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Inleiding tot registratie van de stroom voor Netwerkbeveiligingsgroepen

Netwerkbeveiligingsgroep stroom logboeken zijn een functie van netwerk-Watcher waarmee u informatie bekijken over inkomende en uitgaande IP-verkeer via een Netwerkbeveiligingsgroep. Deze stroom logboeken zijn geschreven in json-indeling en binnenkomende en uitgaande stromen weergeven op basis van een per regel, de NIC die de stroom van toepassing, 5-tuple informatie over de stroom (bron/het doel-IP, bron/het doel-poort, het Protocol) en als het verkeer is toegestaan of geweigerd.

![Overzicht van de stroom-Logboeken][1]

Stroom registreert Netwerkbeveiligingsgroepen doel, zijn ze niet dezelfde als de andere logboeken worden weergegeven. Stroom logboeken worden alleen binnen een opslagaccount en het pad van de logboekregistratie opgeslagen, zoals wordt weergegeven in het volgende voorbeeld:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Het bewaarbeleid dezelfde Denk op andere logboeken van toepassing op de logboeken van de stroom. Logboeken hebben een bewaarbeleid die kan worden ingesteld van 1 dag en 365 dagen. Als geen bewaarbeleid is ingesteld, worden de logboeken voor altijd bewaard.

## <a name="log-file"></a>Logboekbestand

Stroom logboeken hebben meerdere eigenschappen. De volgende lijst bevat een overzicht van de eigenschappen die worden geretourneerd binnen het NSG stroom logboek:

* **tijd** - tijd wanneer de gebeurtenis is vastgelegd
* **systeem-id** -Netwerkbeveiligingsgroep resource-id.
* **categorie** -de categorie van de gebeurtenis is dit altijd NetworkSecurityGroupFlowEvent zijn
* **ResourceID** -de resource-Id van de NSG
* **operationName** -altijd NetworkSecurityGroupFlowEvents
* **eigenschappen** -een verzameling eigenschappen van de stroom
    * **Versie** -versienummer van het schema van de event Log stromen
    * **stromen** -een verzameling van stromen. Deze eigenschap heeft meerdere vermeldingen voor verschillende regels
        * **regel** -regel voor de stromen worden weergegeven
            * **stromen** -een verzameling van stromen
                * **Mac** -het MAC-adres van de NIC voor de virtuele machine waar de stroom is verzameld
                * **flowTuples** -een tekenreeks waarin meerdere eigenschappen voor de stroom-tuple in CSV-indeling
                    * **Tijdstempel** -deze waarde is de tijdstempel van wanneer de stroom is opgetreden in de indeling van de UNIX-EPOCHE
                    * **Bron-IP** -de bron-IP
                    * **Bestemming IP** -de doel-IP
                    * **Bronpoort** -de bronpoort
                    * **Doelpoort** -poort van de bestemming
                    * **Protocol** -het protocol van de stroom. Geldige waarden zijn **T** voor TCP- en **U** voor UDP
                    * **Verkeer van de stroom** -de richting van het netwerkverkeer. Geldige waarden zijn **ik** voor binnenkomend en **O** voor uitgaand.
                    * **Verkeer** - of verkeer is toegestaan of geweigerd. Geldige waarden zijn **A** voor toegestaan en **D** voor geweigerd.


Hier volgt een voorbeeld van een stroom voor logboekbestanden. Zoals u er zijn meerdere records die de lijst met eigenschappen die worden beschreven in de vorige sectie volgen ziet. 

> [!NOTE]
> Waarden in de eigenschap flowTuples zijn een door komma's gescheiden lijst.
 
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

Informatie over het inschakelen van stroom Logboeken in via [logboekregistratie inschakelen stromen](network-watcher-nsg-flow-logging-portal.md).

Meer informatie over NSG logboekregistratie in via [Meld analytics voor netwerkbeveiligingsgroepen (nsg's)](../virtual-network/virtual-network-nsg-manage-log.md).

Ontdek als verkeer wordt toegestaan of geweigerd op een virtuele machine in via [controleren verkeer met IP-stroom controleren](network-watcher-check-ip-flow-verify-portal.md)

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png

