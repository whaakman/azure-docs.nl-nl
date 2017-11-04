---
title: Actoren diagnostische gegevens en bewaking | Microsoft Docs
description: In dit artikel beschrijft de diagnostische gegevens en functies in de runtime voor Service Fabric Reliable Actors, met inbegrip van de gebeurtenissen en prestatiemeteritems die door het bewaken van de prestaties.
services: service-fabric
documentationcenter: .net
author: abhishekram
manager: timlt
editor: vturecek
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/26/2017
ms.author: abhisram
ms.openlocfilehash: 5fbef8a3fb32f4bc47856ef6c6b459ae389dd541
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnose- en prestatiecontrole voor betrouwbare actoren
De runtime Reliable Actors verzendt [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen en [prestatiemeteritems](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Deze biedt mogelijk inzicht in hoe de runtime functioneert en helpt bij het oplossen van problemen en de bewaking van toepassingsprestaties.

## <a name="eventsource-events"></a>EventSource gebeurtenissen
De providernaam EventSource voor Reliable Actors runtime is 'Microsoft-ServiceFabric-Actors'. Gebeurtenissen van de bron van deze gebeurtenissen worden weergegeven in de [diagnostische gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) venster wanneer de toepassing actor wordt [fouten worden opgespoord in Visual Studio](service-fabric-debugging-your-application.md).

Voorbeelden van hulpprogramma's en -technologieën die helpen bij het verzamelen en/of het bekijken van EventSource gebeurtenissen zijn [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), [semantische logboekregistratie](https://msdn.microsoft.com/library/dn774980.aspx), en de [ Bibliotheek van Microsoft TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Trefwoorden
Alle gebeurtenissen die deel uitmaken van het betrouwbare actoren EventSource zijn gekoppeld aan een of meer trefwoorden. Hierdoor kunnen voor het filteren van gebeurtenissen die worden verzameld. De volgende sleutelwoord bits worden gedefinieerd.

| bits | Beschrijving |
| --- | --- |
| 0x1 |Instellen van belangrijke gebeurtenissen die de werking van de runtime Fabric actoren samenvatten. |
| 0x2 |Verzameling van gebeurtenissen die actor methodeaanroepen beschrijven. Zie voor meer informatie de [inleidende informatie over actoren](service-fabric-reliable-actors-introduction.md). |
| 0x4 |Verzameling van gebeurtenissen met betrekking tot actorstatus. Zie voor meer informatie het onderwerp op [actor statusbeheer](service-fabric-reliable-actors-state-management.md). |
| 0x8 |Verzameling van gebeurtenissen met betrekking tot Schakel gebaseerde gelijktijdigheid van taken in de actor. Zie voor meer informatie het onderwerp op [gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency). |

## <a name="performance-counters"></a>Prestatiemeteritems
De runtime Reliable Actors definieert de volgende categorieën voor prestatiemeteritems.

| Category | Beschrijving |
| --- | --- |
| Service Fabric Actor |Items die specifiek bij Azure Service Fabric actoren, bijvoorbeeld de tijd die nodig is voor het opslaan van de actorstatus |
| Service Fabric-Actormethode |Prestatiemeteritems specifiek zijn voor methoden die worden geïmplementeerd door het Service Fabric actoren, bijvoorbeeld hoe vaak een actormethode wordt aangeroepen |

Elk van deze categorieën heeft een of meer items.

De [Windows Prestatiemeter](https://technet.microsoft.com/library/cc749249.aspx) toepassing die standaard beschikbaar in de Windows-besturingssysteem kan worden gebruikt voor het verzamelen en weergeven van gegevens van prestatiemeteritems. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens van prestatiemeteritems en uploaden naar Azure-tabellen.

### <a name="performance-counter-instance-names"></a>Namen van prestatiemeteritems exemplaar
Een cluster met een groot aantal actorservices of actor-partities heeft een groot aantal actor prestaties teller exemplaren. De namen van prestatiemeteritems exemplaar kunnen helpen bij het identificeren van de specifieke [partitie](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) en actormethode (indien van toepassing) die het exemplaar van prestatiemeteritem is gekoppeld.

#### <a name="service-fabric-actor-category"></a>Service Fabric Actor-categorie
Voor de categorie `Service Fabric Actor`, de teller exemplaarnamen worden in de volgende indeling:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* de tekenreeksweergave van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeksvoorstelling wordt gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ActorRuntimeInternalID* de tekenreeksweergave van een 64-bits geheel getal dat wordt gegenereerd door de actoren Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar om te controleren van de uniekheid en veroorzaken een conflict met andere namen van prestatiemeteritems exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

Hieronder volgt een voorbeeld van een naam van het exemplaar voor een item die hoort bij de `Service Fabric Actor` categorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

In het bovenstaande voorbeeld `2740af29-78aa-44bc-a20b-7e60fb783264` de tekenreeksweergave van de Service Fabric partitie-ID en `635650083799324046` is de 64-bits-ID die is gegenereerd voor de runtime interne's gebruiken.

#### <a name="service-fabric-actor-method-category"></a>Service Fabric-Actormethode categorie
Voor de categorie `Service Fabric Actor Method`, de teller exemplaarnamen worden in de volgende indeling:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* is de naam van de actormethode die het exemplaar van prestatiemeteritem is gekoppeld. De indeling van de naam van de methode wordt bepaald op basis van bepaalde logica in de Fabric actoren runtime compromis de leesbaarheid van de naam met beperkingen voor de maximale lengte van de namen van prestatiemeteritems exemplaar in Windows tussen.

*ActorsRuntimeMethodId* de tekenreeksweergave van een 32-bits geheel getal dat wordt gegenereerd door de actoren Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar om te controleren van de uniekheid en veroorzaken een conflict met andere namen van prestatiemeteritems exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

*ServiceFabricPartitionID* de tekenreeksweergave van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeksvoorstelling wordt gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ActorRuntimeInternalID* de tekenreeksweergave van een 64-bits geheel getal dat wordt gegenereerd door de actoren Fabric-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar om te controleren van de uniekheid en veroorzaken een conflict met andere namen van prestatiemeteritems exemplaar voorkomen. Gebruikers moeten niet proberen te interpreteren dit gedeelte van de naam van het exemplaar.

Hieronder volgt een voorbeeld van een naam van het exemplaar voor een item die hoort bij de `Service Fabric Actor Method` categorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

In het bovenstaande voorbeeld `ivoicemailboxactor.leavemessageasync` is de naam van de methode `2` is de 32-bits-ID gegenereerd voor intern gebruik door de runtime, `89383d32-e57e-4a9b-a6ad-57c6792aa521` de tekenreeksweergave van de Service Fabric partitie-ID en `635650083804480486` is de 64-bits-ID gegenereerd voor de de runtime intern gebruik.

## <a name="list-of-events-and-performance-counters"></a>Lijst met gebeurtenissen en prestatiemeters
### <a name="actor-method-events-and-performance-counters"></a>Acteur methode gebeurtenissen en prestatiemeteritems
De runtime Reliable Actors verzendt de volgende gebeurtenissen met betrekking tot [actor methoden](service-fabric-reliable-actors-introduction.md).

| De naam van gebeurtenis | Gebeurtenis-ID | Niveau | Sleutelwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Uitgebreide |0x2 |Actoren runtime is een actormethode worden aangeroepen. |
| ActorMethodStop |8 |Uitgebreide |0x2 |Uitvoering van een actormethode is voltooid. Dat wil zeggen, de runtime asynchrone aanroep van de actormethode heeft geretourneerd en de taak die is geretourneerd door de actormethode is voltooid. |
| ActorMethodThrewException |9 |Waarschuwing |0x3 |Er is een uitzondering opgetreden tijdens het uitvoeren van een actormethode van tijdens asynchrone aanroep van de runtime voor de actormethode of tijdens het uitvoeren van de taak geretourneerd door de actormethode. Deze gebeurtenis geeft aan dat sommige sorteren van de fout in de actor-code die onderzoek behoeften. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op de uitvoering van actor-methoden.

| Categorienaam | Naam van het meteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-Actormethode |Aanroepen per seconde |Aantal keren dat de actorservicemethode wordt aangeroepen per seconde |
| Service Fabric-Actormethode |Gemiddeld aantal milliseconden per aanroep |Gebruikte tijd in milliseconden voor het uitvoeren van de actorservicemethode |
| Service Fabric-Actormethode |Opgetreden uitzonderingen per seconde |Aantal keren dat de actorservicemethode uitzondering een gegenereerd per seconde |

### <a name="concurrency-events-and-performance-counters"></a>Gelijktijdigheid van gebeurtenissen en prestatiemeteritems
De runtime Reliable Actors verzendt de volgende gebeurtenissen met betrekking tot [gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency).

| De naam van gebeurtenis | Gebeurtenis-ID | Niveau | Sleutelwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorMethodCallsWaitingForLock |12 |Uitgebreide |0x8 |Deze gebeurtenis wordt aan het begin van elke nieuwe inschakelen in een actor geschreven. Het bevat het aantal in behandeling actoraanroepen die wachten op de afzonderlijke actorvergrendeling die wordt afgedwongen op basis van Schakel gelijktijdigheid verkrijgen. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op gelijktijdigheid van taken.

| Categorienaam | Naam van het meteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric Actor |Aantal actoraanroepen die wachten op een actorvergrendeling |Aantal in behandeling zijnde actoraanroepen die wachten op het verkrijgen van de afzonderlijke actorvergrendeling die wordt afgedwongen op basis van Schakel gelijktijdigheid van taken |
| Service Fabric Actor |Gemiddeld aantal milliseconden wachttijd per vergrendeling |Gebruikte tijd (in milliseconden) te verkrijgen van de afzonderlijke actorvergrendeling die wordt afgedwongen op basis van Schakel gelijktijdigheid van taken |
| Service Fabric Actor |Gemiddeld aantal milliseconden actorvergrendeling vastgehouden |Tijd (in milliseconden) waarvan de afzonderlijke actorvergrendeling wordt vastgehouden |

### <a name="actor-state-management-events-and-performance-counters"></a>Acteur status management gebeurtenissen en prestatiemeteritems
De runtime Reliable Actors verzendt de volgende gebeurtenissen met betrekking tot [actor statusbeheer](service-fabric-reliable-actors-state-management.md).

| De naam van gebeurtenis | Gebeurtenis-ID | Niveau | Sleutelwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorSaveStateStart |10 |Uitgebreide |0x4 |Actoren runtime is de actorstatus opslaan. |
| ActorSaveStateStop |11 |Uitgebreide |0x4 |Actoren runtime is klaar met het opslaan van de actorstatus. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op actor statusbeheer.

| Categorienaam | Naam van het meteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric Actor |Gemiddeld aantal milliseconden per bewerking voor het opslaan van de status |Gebruikte tijd in milliseconden voor het opslaan van de actorstatus |
| Service Fabric Actor |Gemiddeld aantal milliseconden per bewerking voor het laden van de status |Gebruikte tijd in milliseconden voor het laden van de actorstatus |

### <a name="events-related-to-actor-replicas"></a>Gebeurtenissen met betrekking tot actor replica 's
De runtime Reliable Actors verzendt de volgende gebeurtenissen met betrekking tot [actor replica's](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

| De naam van gebeurtenis | Gebeurtenis-ID | Niveau | Sleutelwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoleToPrimary |1 |Informatief |0x1 |Rol actor replica gewijzigd naar primaire. Dit betekent dat de actoren voor deze partitie binnen deze replica wordt gemaakt. |
| ReplicaChangeRoleFromPrimary |2 |Informatief |0x1 |Acteur replica gewijzigd in niet-primaire rol. Dit betekent dat de actoren voor deze partitie niet meer worden gemaakt binnen deze replica. Er geen nieuwe aanvragen worden geleverd aan actoren al in deze replica is gemaakt. De actoren wordt verwijderd nadat alle aanvragen in uitvoering zijn voltooid. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Acteur activering en deactivering van gebeurtenissen en prestatiemeteritems
De runtime Reliable Actors verzendt de volgende gebeurtenissen met betrekking tot [actor activering en deactivering](service-fabric-reliable-actors-lifecycle.md).

| De naam van gebeurtenis | Gebeurtenis-ID | Niveau | Sleutelwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorActivated |5 |Informatief |0x1 |Een actor is geactiveerd. |
| ActorDeactivated |6 |Informatief |0x1 |Een actor is gedeactiveerd. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op actor activering en deactivering.

| Categorienaam | Naam van het meteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric Actor |Gemiddeld aantal milliseconden op OnActivateAsync |Gebruikte tijd in milliseconden voor het uitvoeren van de methode OnActivateAsync |

### <a name="actor-request-processing-performance-counters"></a>Aanvraagverwerking actor-prestatiemeteritems
Wanneer een client een methode via een proxy actor object aanroept, leidt dit tot een aanvraagbericht via het netwerk worden verzonden naar de actor-service. De service verwerkt het request-bericht en stuurt een antwoord terug naar de client. De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op actor-aanvraagverwerking.

| Categorienaam | Naam van het meteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric Actor |Aantal openstaande aanvragen |Het aantal aanvragen dat in de service wordt verwerkt |
| Service Fabric Actor |Gemiddeld aantal milliseconden per aanvraag |Gebruikte tijd (in milliseconden) door de service een aanvraag te verwerken |
| Service Fabric Actor |Gemiddeld aantal milliseconden voor de deserialisatie van aanvragen |Gebruikte tijd (in milliseconden) voor het deserialiseren van aanvraagbericht actor wanneer het is ontvangen door de service |
| Service Fabric Actor |Gemiddeld aantal milliseconden voor de serialisatie van reacties |Gebruikte tijd (in milliseconden) voor het serialiseren van het antwoordbericht actor op de service voordat het antwoord wordt verzonden naar de client |

## <a name="next-steps"></a>Volgende stappen
* [Hoe Reliable Actors gebruiken voor het Service Fabric-platform](service-fabric-reliable-actors-platform.md)
* [Acteur API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Voorbeeldcode](https://github.com/Azure/servicefabric-samples)
* [EventSource providers op PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
