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
ms.openlocfilehash: d9d87e0e6427c0a0d4b16947fd0427e1c79d8f0c
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56341042"
---
# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Inleiding tot stroomlogboeken voor netwerkbeveiligingsgroepen

Stroomlogboeken van Network security group (NSG) zijn een functie van Network Watcher waarmee u informatie wilt weergeven over inkomende en uitgaande IP-verkeer via een NSG. Logboeken van de stroom worden geschreven in JSON-indeling en weergeven van uitgaande en inkomende stromen op basis van per regel, de netwerkinterface (NIC) de stroom is van toepassing op, 5-tuple-informatie over de stroom (bron-/ doel-IP, bron-/ doel-poort en protocol), als het verkeer is toegestane of geweigerde, en in versie 2, informatie over de doorvoer (Bytes en pakketten).


![stroom-Logboeken, overzicht](./media/network-watcher-nsg-flow-logging-overview/figure1.png)

Stroomlogboeken voor nsg's doel, zijn ze niet hetzelfde als de andere logboeken weergegeven. Logboeken van de stroom alleen binnen een storage-account worden opgeslagen en volg het pad naar logboek wordt weergegeven in het volgende voorbeeld:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```
U kunt stroomlogboeken analyseren en inzicht verkrijgen in uw netwerkverkeer met [traffic analytics](traffic-analytics.md).

De dezelfde bewaarbeleid zichtbaar voor andere logboeken van toepassing op Logboeken van de stroom. U kunt beleid voor het bewaren van logboek ingesteld van 1 dag op 2147483647 dagen. Als geen bewaarbeleid is ingesteld, worden de logboeken voor altijd bewaard.

> [!NOTE] 
> Met de functie voor het beleid van retentie met NSG Flow logboekregistratie kan resulteren in een groot aantal opslagbewerkingen en de bijbehorende kosten. Als u de functie voor retentie beleid vereist, wordt u aangeraden dat u deze waarde ingesteld op 0.


## <a name="log-file"></a>Logboekbestand

Logboeken van de stroom zijn onder andere de volgende eigenschappen:

* **tijd** - tijd waarop de gebeurtenis is vastgelegd
* **systeem-id** -Netwerkbeveiligingsgroep resource-id.
* **categorie** -de categorie van de gebeurtenis. De categorie is altijd **NetworkSecurityGroupFlowEvent**
* **ResourceID** -de resource-Id van de NSG
* **operationName** - Always NetworkSecurityGroupFlowEvents
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
                    * **Verkeer besluit** - of verkeer is toegestaan of geweigerd. Geldige waarden zijn **A** voor toegestaan en **D** voor geweigerd.
                    * **Stroom State - versie 2 alleen** -de status van de stroom worden vastgelegd. Mogelijke statussen zijn **B**: Begin, wanneer een stroom wordt gemaakt. Er worden geen statistische gegevens geleverd. **C**: Continu, voor een actieve stroom. Statistische gegevens worden geleverd met intervallen van 5 minuten. **E**: Eind, wanneer een stroom is beëindigd. Er worden statistische gegevens geleverd.
                    * **Pakketten - bron naar bestemming - versie 2 alleen** het totale aantal TCP of UDP-pakketten worden verzonden vanuit de bron naar bestemming sinds de laatste update.
                    * **Bytes verzonden - bron naar doel - versie 2 alleen** het totale aantal TCP of UDP-pakketten bytes verzonden van bron naar bestemming sinds de laatste update. Pakketbytes omvatten de pakket-header en -nettolading.
                    * **Pakketten - doel naar bron - versie 2 alleen** het totale aantal TCP of UDP-pakketten van doel naar bron verzonden sinds de laatste update.
                    * **Bytes verzonden - doel naar bron - versie 2 alleen** het totale aantal TCP en UDP-pakket verzonden bytes van doel naar bron sinds de laatste update. Pakketbytes omvatten een pakket-header en -nettolading.

## <a name="nsg-flow-logs-version-2"></a>NSG-stroomlogboeken versie 2

Versie 2 van de logboeken introduceert stroom staat. U kunt configureren welke versie van de logboeken van de stroom die u ontvangt. Zie voor informatie over het inschakelen van Logboeken van de stroom, [inschakelen van NSG-stroomlogboeken](network-watcher-nsg-flow-logging-portal.md).

Status Flow *B* wordt geregistreerd wanneer een stroom wordt gestart. Status Flow *C* en de status van de stroom *E* Staten die de voortzetting van een stroom en de stroom wordt beëindigd, respectievelijk markeren. Beide *C* en *E* Staten bevatten informatie over de bandbreedte van verkeer.

Voor Vervolg *C* en end *E* stroom staat, aantal bytes en pakket cumulatieve aantal vanaf het moment van de vorige stroom tuple record zijn. Verwijst naar het vorige voorbeeld-gesprek, is het totale aantal pakketten die zijn overgebracht 1021 + 52 + 8005 + 47 = 9125. Het totale aantal bytes dat wordt overgebracht is 588096 + 29952 + 4610880 + 27072 = 5256000.

**Voorbeeld**: Stroom tuples van een TCP-conversatie tussen 185.170.185.105:35370 en 10.2.0.4:23:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

Voor Vervolg *C* en end *E* stroom staat, aantal bytes en pakket cumulatieve aantal vanaf het moment van de vorige stroom tuple record zijn. Verwijst naar het vorige voorbeeld-gesprek, is het totale aantal pakketten die zijn overgebracht 1021 + 52 + 8005 + 47 = 9125. Het totale aantal bytes dat wordt overgebracht is 588096 + 29952 + 4610880 + 27072 = 5256000.

De tekst die volgt is een voorbeeld van een stroomlogboek. Zoals u ziet, zijn er meerdere records die de lijst met eigenschappen die worden beschreven in de voorgaande sectie volgen.

## <a name="nsg-flow-logging-considerations"></a>Overwegingen voor logboekregistratie van NSG-stroom

**NSG Flow logboekregistratie inschakelen op alle nsg's die zijn gekoppeld aan een resource**: Stroom logboekregistratie in Azure is geconfigureerd op de NSG-resource. Een stroom alleen worden gekoppeld aan een NSG-regel. In scenario's waarbij meerdere nsg's worden gebruikt, raden wij of NSG-stroomlogboeken is ingeschakeld op alle nsg's toegepast van de resource-subnet of netwerkinterface om ervoor te zorgen dat al het verkeer wordt vastgelegd. Zie [hoe verkeer wordt geëvalueerd](../virtual-network/security-overview.md#how-traffic-is-evaluated) voor meer informatie over Netwerkbeveiligingsgroepen. 

**Logboekregistratie kosten Flow**: NSG-stroomlogboeken wordt in rekening gebracht op het volume van de logboeken die wordt geproduceerd. Intensief verkeersvolume kan resulteren in grote stroom logboekvolume en de bijbehorende kosten. Prijzen voor log Stroomlogboeken omvat niet de onderliggende kosten van opslag. Met de functie voor het beleid van retentie met NSG Flow logboekregistratie kan resulteren in een groot aantal opslagbewerkingen en de bijbehorende kosten. Als u de functie voor retentie beleid vereist, wordt u aangeraden dat u deze waarde ingesteld op 0. Zie [prijzen voor Network Watcher](https://azure.microsoft.com/en-us/pricing/details/network-watcher/) en [prijzen voor Azure Storage](https://azure.microsoft.com/en-us/pricing/details/storage/) voor meer informatie.

## <a name="sample-log-records"></a>Voorbeeld van records in logboek registreren

De tekst die volgt is een voorbeeld van een stroomlogboek. Zoals u ziet, zijn er meerdere records die de lijst met eigenschappen die worden beschreven in de voorgaande sectie volgen.


> [!NOTE]
> De waarden in de **flowTuples* eigenschap zijn een door komma's gescheiden lijst.
 
### <a name="version-1-nsg-flow-log-format-sample"></a>Voorbeeld van versie 1 NSG stroom log-indeling
```json
{
    "records": [
        {
            "time": "2017-02-16T22:00:32.8950000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A",
                                    "1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A",
                                    "1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A",
                                    "1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2017-02-16T22:01:32.8960000Z",
            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 1,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "UserRule_default-allow-rdp",
                        "flows": [
                            {
                                "mac": "000D3AF8801A",
                                "flowTuples": [
                                    "1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A",
                                    "1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A",
                                    "1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
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
### <a name="version-2-nsg-flow-log-format-sample"></a>Versie 2 NSG stroom log indeling voorbeeld
```json
 {
    "records": [
        {
            "time": "2018-11-13T12:00:35.3899262Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110402,94.102.49.190,10.5.16.4,28746,443,U,I,D,B,,,,",
                                    "1542110424,176.119.4.10,10.5.16.4,56509,59336,T,I,D,B,,,,",
                                    "1542110432,167.99.86.8,10.5.16.4,48495,8088,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    },
                    {
                        "rule": "DefaultRule_AllowInternetOutBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110377,10.5.16.4,13.67.143.118,59831,443,T,O,A,B,,,,",
                                    "1542110379,10.5.16.4,13.67.143.117,59932,443,T,O,A,E,1,66,1,66",
                                    "1542110379,10.5.16.4,13.67.143.115,44931,443,T,O,A,C,30,16978,24,14008",
                                    "1542110406,10.5.16.4,40.71.12.225,59929,443,T,O,A,E,15,8489,12,7054"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        {
            "time": "2018-11-13T12:01:35.3918317Z",
            "systemId": "a0fca5ce-022c-47b1-9735-89943b42f2fa",
            "category": "NetworkSecurityGroupFlowEvent",
            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
            "operationName": "NetworkSecurityGroupFlowEvents",
            "properties": {
                "Version": 2,
                "flows": [
                    {
                        "rule": "DefaultRule_DenyAllInBound",
                        "flows": [
                            {
                                "mac": "000D3AF87856",
                                "flowTuples": [
                                    "1542110437,125.64.94.197,10.5.16.4,59752,18264,T,I,D,B,,,,",
                                    "1542110475,80.211.72.221,10.5.16.4,37433,8088,T,I,D,B,,,,",
                                    "1542110487,46.101.199.124,10.5.16.4,60577,8088,T,I,D,B,,,,",
                                    "1542110490,176.119.4.30,10.5.16.4,57067,52801,T,I,D,B,,,,"
                                ]
                            }
                        ]
                    }
                ]
            }
        },
        ...
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor informatie over het inschakelen van Logboeken van de stroom, [inschakelen van NSG-stroomlogboeken](network-watcher-nsg-flow-logging-portal.md).
- Zie voor meer informatie over het lezen van de logboeken van de stroom, [lezen NSG-stroomlogboeken](network-watcher-read-nsg-flow-logs.md).
- Zie voor meer informatie over NSG-Logboeken, [Log analytics voor netwerkbeveiligingsgroepen (nsg's)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- Om te bepalen of verkeer is toegestaan of vanaf of naar een virtuele machine verboden, Zie [een probleem VM-netwerk-verkeersfilter vaststellen](diagnose-vm-network-traffic-filtering-problem.md)
