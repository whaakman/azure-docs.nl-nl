---
title: Azure-netwerk stroom logboek wijzigingen | Microsoft Docs
description: Meer informatie over Azure network security group stroom log wijzigingen.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: b3a5ca929c83f70c31d667c2d9c811623691cff8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180769"
---
# <a name="network-security-group-flow-logs--version-2--upcoming-changes"></a>Network stroomlogboeken security group - versie 2 – aanstaande wijzigingen

De indeling van network security groep stroom Logboeken wordt gewijzigd, vanaf op 15 September 2018. Toegevoegde velden bieden u informatie over de stroom staats- en incrementele tellingen van bytes en pakketten in beide richtingen worden overgebracht. Toepassingen die parseren van Logboeken van de stroom worden beïnvloed door deze wijziging en moeten worden dienovereenkomstig bijgewerkt. In dit artikel bevat een overzicht van de details van de wijziging.

## <a name="what-is-changing"></a>Wat is gewijzigd?

De versie van de stroomlogboeken van netwerk wordt gewijzigd van 'Versie': '-versie 1: 2, met aanvullende velden die zijn toegevoegd aan de *flowTuples* eigenschap in de logboeken. Meer informatie over de indeling vindt u in [hoe een stroom is gemodelleerd in versie 2](#how-is-a-flow-modeled-in-version-2).

### <a name="version-1-flow-tuple-format"></a>Versie 1 flow tuple indeling

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"
```

### <a name="version-2-flow-tuple-format"></a>Versie 2 flow tuple indeling

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186"
```

## <a name="when-will-this-change-take-place"></a>Wanneer wordt deze wijziging plaatsvinden?

Deze wijziging gaat in vanaf effect **15 September 2018** in de regio West-Centraal VS. Implementatie van de wijziging in de openbare cloud-regio's wordt voltooid door November 31 mei 2018, na welke alleen versie 2-logboeken beschikbaar zal zijn.

## <a name="am-i-affected-by-the-change"></a>Ben ik beïnvloed door de wijziging?

U mag worden beïnvloed door deze wijziging als u bouwen of een toepassing die network security group stroom logboekgegevens verwerkt.

### <a name="if-i-use-traffic-analytics"></a>Als ik gebruikmaak van Traffic Analytics?

Nee. Traffic Analytics wordt niet beïnvloed tijdens de overgang van versie 1 naar stroomlogboeken van versie 2. Verbeteringen die binnenkort wordt profiteren van de extra sessie en bandbreedte informatie in stroomlogboeken van versie 2.

### <a name="if-im-using-third-party-tooling"></a>Als ik een derde partij tooling?

De wijziging in de indeling van Logboeken is vooraf meegedeeld met alle [Network Watcher partners](https://azure.microsoft.com/services/network-watcher). Contact opnemen met de leverancier voor meer informatie.

### <a name="if-i-have-built-a-custom-application-or-integration"></a>Als ik een aangepaste toepassing of integratie heb gebouwd?

**Ja**, moet u uw toepassing proces Flow NSG-Logboeken in de nieuwe indeling wijzigen.

## <a name="what-is-the-new-flow-logging-format"></a>Wat is de indeling voor logboekregistratie van nieuwe flow?

Stroom logboeken versie 2 bevat flow tuples in de volgende indeling:

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186".
```

De volgende tabel bevat de eigenschapnamen en beschrijvingen voor de network security group versie 2 flow tuple. Compleet voorbeeldrecords kunnen worden gevonden [versie 2 voorbeeldgebeurtenis](#version2sampleevent).

| Naam van eigenschap                        | Waarde           | Beschrijving                                                                                                                                                 |
| ------------------------------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tijdstempel                           | 1493763938      | Tijdstempel overeenkomt met de stroom-vermelding. UNIX-EPOCHE-indeling wordt gebruikt.                                                                                       |
| IP-adres van bron                    | 185.170.185.105 |                                                                                                                                                             |
| IP-adres van doel               | 10.2.0.4        |                                                                                                                                                             |
| Bronpoort                          | 35370           |                                                                                                                                                             |
| Doelpoort                     | 23              |                                                                                                                                                             |
| Verkeer protocol                     | T               | **T**: TCP en **U**: UDP.                                                                                                                                  |
| Stroom de richting van verkeer               | I               | **Ik**: binnenkomend verkeer en **O**: uitgaand verkeer.                                                                                                         |
| Verkeer besluit                     | A               | **Een**: Flow is toegestaan en **D**: Flow is geweigerd.                                                                                                         |
| Status van de stroom                           | C               | **B**: Begin, als u een stroom maakt. Statistieken zijn niet opgegeven. **C**: voor een continue stroom wordt voortgezet. Statistieken worden geleverd met 5 minuten durende intervallen. **E**: einde, als een stroom wordt beëindigd. Statistieken worden geleverd.                                                                                                                                                        |
| Pakketten - bron naar doel      | 1               | Het totale aantal TCP en UDP-pakketten die worden verzonden vanuit de bron naar bestemming sinds de laatste update.                                                                  |
| Bytes verzonden - bron naar doel   | 103             | Het totale aantal TCP en UDP-pakketten bytes verzonden van bron naar bestemming sinds de laatste update. Pakket bytes zijn de header van het pakket en -nettolading.         |
| Pakketten die zijn verzonden - doel naar bron | 1               | Het totale aantal TCP en UDP-pakketten van doel naar bron verzonden sinds de laatste update.                                                                  |
| Bytes verzonden - doel naar bron   | 186             | Het totale aantal TCP en UDP-pakketten bytes verzonden van doel naar bron sinds de laatste update. Pakket bytes zijn pakketheader en -nettolading.             |

## <a name="how-is-a-flow-modeled-in-version-2"></a>Hoe wordt een stroom gemodelleerd in versie 2?

Versie 2 van de logboeken introduceert stroom staat. Status Flow *B* wordt geregistreerd wanneer een stroom wordt gestart. Status Flow *C* en de status van de stroom *E* Staten die de voortzetting van een stroom en de stroom wordt beëindigd, respectievelijk markeren. Beide *C* en *E* Staten bevatten informatie over de bandbreedte van verkeer.

**Voorbeeld**: tuples van een TCP-conversatie tussen 185.170.185.105:35370 en 10.2.0.4:23 Flow:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

### <a name="how-does-the-format-differ-from-version-1"></a>Hoe verschilt de indeling van versie 1?

In versie 1, een stroom tuple ["1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"] gemaakt wanneer er een stroom is tot stand gebracht of poging tot het tot stand worden gebracht (aanvraag weigeren).

### <a name="how-are-bytes-and-packets-accounted-for"></a>Hoe worden bytes en pakketten verwerkt?

Voor Vervolg *C* en end *E* stroom staat, aantal bytes en pakket cumulatieve aantal vanaf het moment van de vorige stroom tuple record zijn. Verwijst naar het vorige voorbeeld-gesprek, is het totale aantal pakketten die zijn overgebracht 1021 + 52 + 8005 + 47 = 9125. Het totale aantal bytes dat wordt overgebracht is 588096 + 29952 + 4610880 + 27072 = 5256000.

### <a name="if-my-application-doesnt-understand-the-traffic-bandwidth-fields-how-does-version-2-affect-the-application"></a>Als mijn toepassing niet bekend met de velden van de bandbreedte netwerkverkeer bent, hoe versie 2 beïnvloedt de toepassing?

Toepassingen die gebruikmaken van versie 1-netwerkbeveiliging groep stroom logboekregistratie doorgaans tellen het aantal unieke stromen. Als er geen wijziging bij het parseren van wordt aangebracht in het account voor de status van de stroom, ziet u mogelijk een onjuiste toename van het aantal stromen die zijn gerapporteerd. Stromen van unieke tellingen kan worden uitgevoerd door wordt genegeerd stroom tuples in *C* en *E* flow staten.

## <a name="can-i-control-the-version-format-i-receive"></a>Kan ik beheren aan de versie-indeling dat ik ontvang?

Nee. Inschakelen en uitschakelen van Logboeken van de stroom heeft geen invloed op de indeling van de uitvoer van de logboeken. De wijziging van versie 1 van versie 2 Logboeken wordt uitgevoerd op basis van de regio van de regio. Bij een regio een van versie 1 naar versie 2 upgrade is, ziet u NSG-stroomlogboeken in beide indelingen. Nadat de implementatie is voltooid, wordt alleen versie 2 logboeken beschikbaar zijn.

## <a name="while-the-change-occurs-can-i-see-both-formats-for-the-same-network-security-group"></a>Terwijl de wijziging wordt uitgevoerd, kan ik beide indelingen voor de dezelfde netwerkbeveiligingsgroep zien?

Ja. Binnen een regio, wordt de overgang plaatsvinden op basis van per-mac-adres. Omdat een netwerkbeveiligingsgroep kan worden toegepast op veel computers, ziet u de logboeken in beide indelingen geschreven naar opslag. Logboeken is versie 1 of 2.

## <a name="will-i-see-duplicate-data"></a>Zie ik dubbele gegevens?

Er is niet duplicatie van flow logboekregistratiegegevens over indelingen. Een stroom wordt geregistreerd in versie 1 of 2 zijn versie-indeling, terwijl de wijziging wordt uitgevoerd.

## <a name="how-can-i-test-the-new-format-ahead-of-time"></a>Hoe kan ik de nieuwe indeling tevoren testen?

Ja. U kunt [downloaden](https://aka.ms/nsgflowlogsv2blobsample) versie 2 flow logboekbestand moet worden gebruikt voor het testen van uw oplossing.

## <a name="are-there-changes-to-configuration-and-management-of-network-security-group-flow-logging"></a>Zijn er wijzigingen in de configuratie en beheer van stroomlogboeken van netwerkbeveiligingsgroep netwerk?

Nee. Ondersteuning voor Azure Storage-accounts voor abonnementen die dezelfde Azure Active Directory-tenant delen is [die zijn uitgebracht](https://azure.microsoft.com/blog/new-azure-network-watcher-integrations-and-network-security-group-flow-logging-updates/) eerder dit jaar. Deze wijziging kunt u Verminder het aantal opslagaccounts die nodig zijn bij het beheren van stroomlogboeken van netwerk.

## <a name="questions-or-feedback"></a>Vragen of feedback?

We horen graag over uw ervaring met stroomlogboeken van netwerk- en Network Watcher. U kunt online of via e-mail op feedback of suggesties bieden AzureNetworkWatcher@microsoft.com.

## <a name="version-2-sample"></a>Voorbeeld van versie 2

```json
{

"records": [

{

"time": "2018-08-03T17:00:54.5657764Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315592,10.1.1.6,204.79.197.200,62375,80,T,O,A,B,,,,",

"1533315595,10.1.1.6,204.79.197.200,62373,80,T,O,A,E,30,1784,92,123631",

"1533315597,10.1.1.6,204.79.197.200,62376,80,T,O,A,B,,,,",

"1533315601,10.1.1.6,204.79.197.200,62374,80,T,O,A,E,13,866,87,123079",

"1533315603,10.1.1.6,204.79.197.200,62377,80,T,O,A,B,,,,",

"1533315604,10.1.1.6,204.79.197.200,62375,80,T,O,A,E,33,1946,90,123247",

"1533315608,10.1.1.6,204.79.197.200,62378,80,T,O,A,B,,,,",

"1533315610,10.1.1.6,204.79.197.200,62376,80,T,O,A,E,20,1244,92,123355",

"1533315613,10.1.1.6,204.79.197.200,62379,80,T,O,A,B,,,,",

"1533315616,10.1.1.6,204.79.197.200,62377,80,T,O,A,E,24,1460,92,123355",

"1533315619,10.1.1.6,204.79.197.200,62380,80,T,O,A,B,,,,",

"1533315622,10.1.1.6,204.79.197.200,62378,80,T,O,A,E,23,1406,93,123409",

"1533315624,10.1.1.6,204.79.197.200,62381,80,T,O,A,B,,,,",

"1533315628,10.1.1.6,204.79.197.200,62379,80,T,O,A,E,16,1028,88,123133",

"1533315630,10.1.1.6,204.79.197.200,62382,80,T,O,A,B,,,,",

"1533315631,10.1.1.6,204.79.197.200,62380,80,T,O,A,E,13,866,87,123079",

"1533315635,10.1.1.6,204.79.197.200,62384,80,T,O,A,B,,,,",

"1533315637,10.1.1.6,204.79.197.200,62381,80,T,O,A,E,15,974,86,123025",

"1533315640,10.1.1.6,204.79.197.200,62385,80,T,O,A,B,,,,",

"1533315643,10.1.1.6,204.79.197.200,62382,80,T,O,A,E,16,1028,88,123139",

"1533315646,10.1.1.6,204.79.197.200,62386,80,T,O,A,B,,,,",

"1533315649,10.1.1.6,204.79.197.200,62384,80,T,O,A,E,21,1298,92,123355",

"1533315651,10.1.1.6,204.79.197.200,62387,80,T,O,A,B,,,,"

]

}

]

}

]

}

},

{

"time": "2018-08-03T17:01:54.5668880Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_DenyAllInBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315685,80.211.83.37,10.1.1.6,45321,81,T,I,D,B,,,,"

]

}

]

},

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315655,10.1.1.6,204.79.197.200,62385,80,T,O,A,E,20,1244,87,123079",

"1533315657,10.1.1.6,204.79.197.200,62388,80,T,O,A,B,,,,",

"1533315658,10.1.1.6,204.79.197.200,62386,80,T,O,A,E,26,1568,92,123355",

"1533315662,10.1.1.6,204.79.197.200,62389,80,T,O,A,B,,,,",

"1533315664,10.1.1.6,204.79.197.200,62387,80,T,O,A,E,15,974,88,123133",

"1533315667,10.1.1.6,204.79.197.200,62390,80,T,O,A,B,,,,",

"1533315670,10.1.1.6,204.79.197.200,62388,80,T,O,A,E,18,1136,88,123139",

"1533315673,10.1.1.6,204.79.197.200,62391,80,T,O,A,B,,,,",

"1533315676,10.1.1.6,204.79.197.200,62389,80,T,O,A,E,14,920,87,123079",

"1533315678,10.1.1.6,204.79.197.200,62392,80,T,O,A,B,,,,",

"1533315679,10.1.1.6,204.79.197.200,62390,80,T,O,A,E,31,1838,94,123739",

"1533315684,10.1.1.6,204.79.197.200,62393,80,T,O,A,B,,,,",

"1533315685,10.1.1.6,204.79.197.200,62391,80,T,O,A,E,28,1676,101,141199",

"1533315689,10.1.1.6,204.79.197.200,62394,80,T,O,A,B,,,,",

"1533315691,10.1.1.6,204.79.197.200,62392,80,T,O,A,E,21,1298,93,123409",

"1533315694,10.1.1.6,204.79.197.200,62395,80,T,O,A,B,,,,",

"1533315697,10.1.1.6,204.79.197.200,62393,80,T,O,A,E,26,1568,91,123393",

"1533315700,10.1.1.6,204.79.197.200,62396,80,T,O,A,B,,,,",

"1533315703,10.1.1.6,204.79.197.200,62394,80,T,O,A,E,14,920,89,123187",

"1533315705,10.1.1.6,204.79.197.200,62397,80,T,O,A,B,,,,",

"1533315706,10.1.1.6,204.79.197.200,62395,80,T,O,A,E,13,866,87,123079",

"1533315711,10.1.1.6,204.79.197.200,62398,80,T,O,A,B,,,,"

]

}

]

}

]

}

}

]

}
```

## <a name="version-1-sample"></a>Voorbeeld van versie 1

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

        } 

    ] 
}
```