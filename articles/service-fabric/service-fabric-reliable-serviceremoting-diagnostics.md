---
title: Azure Service fabric-diagnose en controle | Microsoft Docs
description: Dit artikel beschrijft de controlefuncties voor toepassingsprestaties in de betrouwbare ServiceRemoting van Service Fabric-runtime, zoals prestatiemeteritems die zijn gegenereerd door deze.
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: chackdan
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: suchiagicha
ms.openlocfilehash: e4c698dc5aa8a07d09835adeac39db6ee35f720c
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666151"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Diagnostische gegevens en prestatiecontrole voor betrouwbare Service voor externe toegang
De runtime betrouwbare ServiceRemoting verzendt [prestatiemeteritems](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Deze bieden inzicht in hoe de ServiceRemoting wordt uitgevoerd en u helpen bij het oplossen en bewaking van toepassingsprestaties.


## <a name="performance-counters"></a>Prestatiemeteritems
De runtime betrouwbare ServiceRemoting definieert de volgende categorieën voor prestatiemeteritems:

| Categorie | Description |
| --- | --- |
| Service Fabric Service |Items die specifiek zijn voor Azure Service Fabric-Service voor externe toegang, bijvoorbeeld, gemiddelde tijd die nodig is voor het verwerken van aanvraag |
| Service Fabric Service-methode |Items die specifiek zijn voor methoden geïmplementeerd door de Service Fabric Remoting Service, bijvoorbeeld hoe vaak een service-methode wordt aangeroepen |

Elk van de bovenstaande categorieën vallen heeft een of meer items.

De [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) toepassing die is standaard beschikbaar in het Windows-besturingssysteem kan worden gebruikt voor het verzamelen en weergeven van gegevens van prestatiemeteritems. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens van prestatiemeteritems en uploaden naar Azure-tabellen.

### <a name="performance-counter-instance-names"></a>Namen van prestatiemeteritems exemplaar
Een cluster met een groot aantal ServiceRemoting services of partities hebben een groot aantal exemplaren van prestaties teller. De namen van prestatiemeteritems exemplaar kunnen helpen bij het identificeren van de specifieke partitie en de Service-methode (indien van toepassing) dat het exemplaar van prestatiemeteritem gekoppeld is.

#### <a name="service-fabric-service-category"></a>Service Fabric Service-categorie
Voor de categorie `Service Fabric Service`, de namen van de teller-exemplaar zijn in de volgende indeling:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks is die worden gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ServiceReplicaOrInstanceId* is van de tekenreeksvoorstelling van de Replica/exemplaar-ID voor Service Fabric het exemplaar van prestatiemeteritem is gekoppeld.

*ServiceRuntimeInternalID* is van de tekenreeksvoorstelling van een 64-bits geheel getal zijn dat wordt gegenereerd door de Service Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat deze zijn uniek en te voorkomen veroorzaken een conflict met andere namen van prestatiemeteritems-exemplaar. Gebruikers moeten niet proberen te interpreteren in dit gedeelte van de naam exemplaar Prestatiemeter.

Hieronder volgt een voorbeeld van een naam van het exemplaar van een item die deel uitmaakt van de `Service Fabric Service` categorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

In het voorgaande voorbeeld `2740af29-78aa-44bc-a20b-7e60fb783264` is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID, `635650083799324046` tekenreeksweergave van Replica/InstanceId is en `5008379932` is de 64-bits-ID die is gegenereerd voor de runtime interne's gebruiken.

#### <a name="service-fabric-service-method-category"></a>Categorie van de service Fabric Service-methode
Voor de categorie `Service Fabric Service Method`, de namen van de teller-exemplaar zijn in de volgende indeling:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* is de naam van de servicemethode die het exemplaar van prestatiemeteritem is gekoppeld. De indeling van de naam van de methode wordt bepaald op basis van bepaalde logica in de Service Fabric-runtime die een van de leesbaarheid van de naam met beperkingen met betrekking tot de maximale lengte van de namen van prestatiemeteritems exemplaar van Windows.

*ServiceRuntimeMethodId* is van de tekenreeksvoorstelling van een 32-bits geheel getal zijn dat wordt gegenereerd door de Service Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat deze zijn uniek en te voorkomen veroorzaken een conflict met andere namen van prestatiemeteritems-exemplaar. Gebruikers moeten niet proberen te interpreteren in dit gedeelte van de naam exemplaar Prestatiemeter.

*ServiceFabricPartitionID* is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks is die worden gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ServiceReplicaOrInstanceId* is van de tekenreeksvoorstelling van de Replica/exemplaar-ID voor Service Fabric het exemplaar van prestatiemeteritem is gekoppeld.

*ServiceRuntimeInternalID* is van de tekenreeksvoorstelling van een 64-bits geheel getal zijn dat wordt gegenereerd door de Service Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat deze zijn uniek en te voorkomen veroorzaken een conflict met andere namen van prestatiemeteritems-exemplaar. Gebruikers moeten niet proberen te interpreteren in dit gedeelte van de naam exemplaar Prestatiemeter.

Hieronder volgt een voorbeeld van een naam van het exemplaar van een item die deel uitmaakt van de `Service Fabric Service Method` categorie:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

In het voorgaande voorbeeld `ivoicemailboxservice.leavemessageasync` is de naam van de methode `2` is de 32-bits-ID die is gegenereerd voor intern gebruik van de runtime, `89383d32-e57e-4a9b-a6ad-57c6792aa521` is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID,`635650083804480486` is van de tekenreeksvoorstelling van de Service Fabric-Replica-exemplaar-ID en `5008380` is de 64-bits-ID die is gegenereerd voor de runtime interne's gebruiken.

## <a name="list-of-performance-counters"></a>Lijst met prestatiemeteritems
### <a name="service-method-performance-counters"></a>Prestatiemeteritems voor service-methode

De betrouwbare Service runtime publiceert de volgende prestatiemeteritems met betrekking tot de uitvoering van de service-methoden.

| Categorienaam | Naam van het prestatiemeteritem | Description |
| --- | --- | --- |
| Service Fabric Service-methode |Aanroepen/sec |Aantal keren dat de servicemethode wordt aangeroepen per seconde |
| Service Fabric Service-methode |Gemiddeld aantal milliseconden per aanroep |Gebruikte tijd voor het uitvoeren van de servicemethode in milliseconden |
| Service Fabric Service-methode |Aantal opgetreden uitzonderingen/sec |Aantal keren dat de servicemethode een uitzondering per seconde heeft |

### <a name="service-request-processing-performance-counters"></a>Prestatiemeteritems voor service-aanvraag verwerken
Wanneer een client een methode via een proxy-serviceobject roept, resulteert dit in een bericht naar de service voor externe toegang via het netwerk worden verzonden. De service het aanvraagbericht verwerkt en stuurt een antwoord terug naar de client. De runtime betrouwbare ServiceRemoting publiceert de volgende prestatiemeteritems met betrekking tot de verwerking van de service-aanvraag.

| Categorienaam | Naam van het prestatiemeteritem | Description |
| --- | --- | --- |
| Service Fabric Service |Aantal openstaande aanvragen |Aantal aanvragen in de service wordt verwerkt |
| Service Fabric Service |Gemiddeld aantal milliseconden per aanvraag |Gebruikte tijd (in milliseconden) door de service om een aanvraag te verwerken |
| Service Fabric Service |Gemiddeld aantal milliseconden voor de deserialisatie van aanvragen |Gebruikte tijd (in milliseconden) voor het deserialiseren van het aanvraagbericht service wanneer het is ontvangen door de service |
| Service Fabric Service |Gemiddeld aantal milliseconden voor de serialisatie van reacties |Gebruikte tijd (in milliseconden) voor het serialiseren van het antwoordbericht service op de service voordat de reactie wordt verzonden naar de client |

## <a name="next-steps"></a>Volgende stappen
* [Voorbeeldcode](https://azure.microsoft.com/en-us/resources/samples/?service=service-fabric&sort=0)
* [Gebeurtenisbron providers in het voorbeeld](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
