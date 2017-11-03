---
title: Azure ServiceFabric diagnostische gegevens en bewaking | Microsoft Docs
description: Dit artikel worden de prestaties bewakingsfuncties in de runtime voor Service Fabric betrouwbare ServiceRemoting, zoals die door deze prestatiemeteritems.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: suchiagicha
ms.openlocfilehash: f54e157654fb15d2f7ff48ddc666c6c8803c75a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Diagnostische gegevens en Prestatiebewaking voor betrouwbare Service voor externe toegang
De runtime betrouwbare ServiceRemoting verzendt [prestatiemeteritems](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Deze biedt mogelijk inzicht in hoe de ServiceRemoting functioneert en helpt bij het oplossen van problemen en de bewaking van toepassingsprestaties.


## <a name="performance-counters"></a>Prestatiemeteritems
De runtime betrouwbare ServiceRemoting definieert de volgende categorieën voor prestatiemeteritems:

| Category | Beschrijving |
| --- | --- |
| Service Fabric-Service |Tellers die specifiek zijn voor Azure Service Fabric-Service voor externe toegang, bijvoorbeeld, gemiddelde tijd die nodig is voor het verwerken van aanvraag |
| Service Fabric-Service-methode |Items die specifiek zijn voor methoden geïmplementeerd door de Service Fabric Remoting Service, bijvoorbeeld hoe vaak een servicemethode wordt aangeroepen |

Elk van de voorgaande categorieën heeft een of meer items.

De [Windows Prestatiemeter](https://technet.microsoft.com/library/cc749249.aspx) toepassing die standaard beschikbaar in de Windows-besturingssysteem kan worden gebruikt voor het verzamelen en weergeven van gegevens van prestatiemeteritems. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens van prestatiemeteritems en uploaden naar Azure-tabellen.

### <a name="performance-counter-instance-names"></a>Namen van prestatiemeteritems exemplaar
Een cluster met een groot aantal ServiceRemoting services of partities hebben een groot aantal exemplaren van prestaties teller. De namen van prestatiemeteritems exemplaar kunnen helpen bij het identificeren van de specifieke partitie en de Service-methode (indien van toepassing) of het exemplaar van prestatiemeteritem is gekoppeld aan.

#### <a name="service-fabric-service-category"></a>Service Fabric-Service-categorie
Voor de categorie `Service Fabric Service`, de teller exemplaarnamen worden in de volgende indeling:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* de tekenreeksweergave van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeksvoorstelling wordt gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ServiceReplicaOrInstanceId* de tekenreeksweergave van de Replica-exemplaar-ID voor Service Fabric exemplaar van het prestatiemeteritem is gekoppeld.

*ServiceRuntimeInternalID* de tekenreeksweergave van een 64-bits geheel getal dat wordt gegenereerd door de Service Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar om te controleren van de uniekheid en veroorzaken een conflict met andere namen van prestatiemeteritems exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

Hieronder volgt een voorbeeld van een naam van het exemplaar voor een item die hoort bij de `Service Fabric Service` categorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

In het voorgaande voorbeeld `2740af29-78aa-44bc-a20b-7e60fb783264` de tekenreeksweergave van de Service Fabric-partitie-ID, `635650083799324046` tekenreeksweergave van Replica/exemplaar-id en `5008379932` is de 64-bits-ID die is gegenereerd voor de runtime interne's gebruiken.

#### <a name="service-fabric-service-method-category"></a>Methode voor de Service Fabric-categorie
Voor de categorie `Service Fabric Service Method`, de teller exemplaarnamen worden in de volgende indeling:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* is de naam van de servicemethode die het exemplaar van prestatiemeteritem is gekoppeld. De indeling van de naam van de methode wordt bepaald op basis van bepaalde logica in de Service Fabric-runtime die een compromis de leesbaarheid van de naam met beperkingen voor de maximale lengte van de namen van prestatiemeteritems exemplaar in Windows tussen.

*ServiceRuntimeMethodId* de tekenreeksweergave van een 32-bits geheel getal dat wordt gegenereerd door de Service Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar om te controleren van de uniekheid en veroorzaken een conflict met andere namen van prestatiemeteritems exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

*ServiceFabricPartitionID* de tekenreeksweergave van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeksvoorstelling wordt gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ServiceReplicaOrInstanceId* de tekenreeksweergave van de Replica-exemplaar-ID voor Service Fabric exemplaar van het prestatiemeteritem is gekoppeld.

*ServiceRuntimeInternalID* de tekenreeksweergave van een 64-bits geheel getal dat wordt gegenereerd door de Service Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar om te controleren van de uniekheid en veroorzaken een conflict met andere namen van prestatiemeteritems exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

Hieronder volgt een voorbeeld van een naam van het exemplaar voor een item die hoort bij de `Service Fabric Service Method` categorie:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

In het voorgaande voorbeeld `ivoicemailboxservice.leavemessageasync` is de naam van de methode `2` is de 32-bits-ID gegenereerd voor intern gebruik door de runtime, `89383d32-e57e-4a9b-a6ad-57c6792aa521` de tekenreeksweergave van de Service Fabric-partitie-ID,`635650083804480486` de tekenreeksweergave van de Service Fabric-Replica-exemplaar-ID en `5008380` is de 64-bits-ID die is gegenereerd voor de runtime interne's gebruiken.

## <a name="list-of-performance-counters"></a>Lijst met prestatiemeteritems
### <a name="service-method-performance-counters"></a>Prestatiemeteritems voor service-methode

De runtime betrouwbare Service publiceert de volgende prestatiemeteritems die betrekking hebben op de uitvoering van de methoden.

| Categorienaam | Naam van het meteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-Service-methode |Aanroepen per seconde |Aantal keren dat de servicemethode wordt aangeroepen per seconde |
| Service Fabric-Service-methode |Gemiddeld aantal milliseconden per aanroep |Tijd uitvoering van de servicemethode in milliseconden |
| Service Fabric-Service-methode |Opgetreden uitzonderingen per seconde |Aantal keren dat de servicemethode een uitzondering gegenereerd per seconde heeft |

### <a name="service-request-processing-performance-counters"></a>Prestatiemeteritems voor service-aanvraag verwerken
Wanneer een client een methode via een proxy serviceobject aanroept, leidt dit tot een aanvraagbericht naar de service voor externe toegang via het netwerk wordt verzonden. De service verwerkt het request-bericht en stuurt een antwoord terug naar de client. De runtime betrouwbare ServiceRemoting publiceert de volgende prestatiemeteritems die betrekking hebben op de verwerking van de service-aanvraag.

| Categorienaam | Naam van het meteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-Service |Aantal openstaande aanvragen |Het aantal aanvragen dat in de service wordt verwerkt |
| Service Fabric-Service |Gemiddeld aantal milliseconden per aanvraag |Gebruikte tijd (in milliseconden) door de service een aanvraag te verwerken |
| Service Fabric-Service |Gemiddeld aantal milliseconden voor de deserialisatie van aanvragen |Gebruikte tijd (in milliseconden) voor het deserialiseren van aanvraagbericht service wanneer deze is ontvangen door de service |
| Service Fabric-Service |Gemiddeld aantal milliseconden voor de serialisatie van reacties |Gebruikte tijd (in milliseconden) voor het serialiseren van het antwoordbericht van de service op de service voordat het antwoord wordt verzonden naar de client |

## <a name="next-steps"></a>Volgende stappen
* [Voorbeeldcode](https://github.com/Azure/servicefabric-samples)
* [EventSource providers op PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
