---
title: Actoren diagnose en controle | Microsoft Docs
description: Dit artikel beschrijft de diagnostische gegevens en functies in de runtime Service Fabric Reliable Actors, met inbegrip van de gebeurtenissen en prestatiemeteritems die zijn gegenereerd door deze bewaking van de prestaties.
services: service-fabric
documentationcenter: .net
author: abhishekram
manager: timlt
editor: vturecek
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/26/2017
ms.author: abhisram
ms.openlocfilehash: 61c01e8ea3b4cbe7b5f7ab83ab35383d74df3105
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234921"
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnose- en prestatiecontrole voor betrouwbare actoren
De Reliable Actors-runtime verzendt [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) gebeurtenissen en [prestatiemeteritems](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Deze bieden inzicht in hoe de runtime wordt uitgevoerd en u te helpen bij het oplossen van problemen en bewaking van toepassingsprestaties.

## <a name="eventsource-events"></a>Gebeurtenisbron gebeurtenissen
De naam van de gebeurtenisbron-provider voor de Reliable Actors-runtime is 'Microsoft-service fabric-actoren'. Gebeurtenissen uit deze gebeurtenisbron worden weergegeven in de [diagnostische gebeurtenissen](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) venster wanneer de actortoepassing wordt [foutopsporing in Visual Studio](service-fabric-debugging-your-application.md).

Voorbeelden van hulpprogramma's en technologieën die hulp bieden bij het verzamelen en/of EventSource gebeurtenissen bekijken zijn [voorbeeld](https://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), [semantische logboekregistratie](https://msdn.microsoft.com/library/dn774980.aspx), en de [ Bibliotheek van Microsoft TraceEvent](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Trefwoorden
Alle gebeurtenissen die deel uitmaken van de betrouwbare actoren EventSource zijn gekoppeld aan een of meer trefwoorden. Hierdoor is het filteren van gebeurtenissen die worden verzameld. De volgende sleutelwoord bits zijn gedefinieerd.

| bits | Beschrijving |
| --- | --- |
| 0x1 |Instellen van belangrijke gebeurtenissen die de werking van de Fabric-actoren runtime samenvatten. |
| 0x2 |Verzameling van gebeurtenissen die de actor methodeaanroepen beschrijven. Zie voor meer informatie de [inleidende onderwerp op actoren](service-fabric-reliable-actors-introduction.md). |
| 0x4 |Het aantal gebeurtenissen met betrekking tot de actorstatus. Zie het onderwerp voor meer informatie over [actor statusbeheer](service-fabric-reliable-actors-state-management.md). |
| 0x8 |Het aantal gebeurtenissen met betrekking tot inschakelen op basis van gelijktijdigheid van taken in de actor. Zie het onderwerp voor meer informatie over [gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency). |

## <a name="performance-counters"></a>Prestatiemeteritems
De runtime Reliable Actors definieert de volgende categorieën voor prestatiemeteritems.

| Categorie | Beschrijving |
| --- | --- |
| Service Fabric-actor |Prestatiemeteritems specifiek voor Azure Service Fabric-actoren, bijvoorbeeld tijd genomen om op te slaan van de actorstatus |
| Service Fabric-actormethode |Prestatiemeteritems specifiek zijn voor methoden die worden geïmplementeerd door de Service Fabric-actoren, bijvoorbeeld hoe vaak een actormethode wordt aangeroepen |

Elk van de bovenstaande categorieën heeft een of meer items.

De [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) toepassing die is standaard beschikbaar in het Windows-besturingssysteem kan worden gebruikt voor het verzamelen en weergeven van gegevens van prestatiemeteritems. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) is een andere optie voor het verzamelen van gegevens van prestatiemeteritems en uploaden naar Azure-tabellen.

### <a name="performance-counter-instance-names"></a>Namen van prestatiemeteritems exemplaar
Een cluster met een groot aantal actorservices of acteur-partities heeft een groot aantal actor prestaties teller exemplaren. De namen van prestatiemeteritems exemplaar kunnen helpen bij het identificeren van de specifieke [partitie](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) en de actormethode (indien van toepassing) dat het exemplaar van prestatiemeteritem is gekoppeld aan.

#### <a name="service-fabric-actor-category"></a>Service Fabric-Actor-categorie
Voor de categorie `Service Fabric Actor`, de namen van de teller-exemplaar zijn in de volgende indeling:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks is die worden gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ActorRuntimeInternalID* is van de tekenreeksvoorstelling van een 64-bits geheel getal zijn dat wordt gegenereerd door de Fabric-actoren-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat deze zijn uniek en te voorkomen veroorzaken een conflict met andere namen van prestatiemeteritems-exemplaar. Gebruikers moeten niet proberen te interpreteren in dit gedeelte van de naam exemplaar Prestatiemeter.

Hieronder volgt een voorbeeld van een naam van het exemplaar van een item die deel uitmaakt van de `Service Fabric Actor` categorie:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

In het bovenstaande voorbeeld `2740af29-78aa-44bc-a20b-7e60fb783264` is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID, en `635650083799324046` is de 64-bits-ID die is gegenereerd voor de runtime interne's gebruiken.

#### <a name="service-fabric-actor-method-category"></a>Service Fabric-Actormethode categorie
Voor de categorie `Service Fabric Actor Method`, de namen van de teller-exemplaar zijn in de volgende indeling:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* is de naam van de actormethode die het exemplaar van prestatiemeteritem is gekoppeld. De indeling van de naam van de methode wordt bepaald op basis van bepaalde logica in de Fabric-actoren-runtime die een van de leesbaarheid van de naam met beperkingen met betrekking tot de maximale lengte van de namen van prestatiemeteritems exemplaar van Windows.

*ActorsRuntimeMethodId* is van de tekenreeksvoorstelling van een 32-bits geheel getal zijn dat wordt gegenereerd door de Fabric-actoren-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat deze zijn uniek en te voorkomen veroorzaken een conflict met andere namen van prestatiemeteritems-exemplaar. Gebruikers moeten niet proberen te interpreteren in dit gedeelte van de naam exemplaar Prestatiemeter.

*ServiceFabricPartitionID* is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID die het exemplaar van prestatiemeteritem is gekoppeld. De partitie-ID is een GUID en de tekenreeks is die worden gegenereerd door de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) methode met indelingsopgave "D".

*ActorRuntimeInternalID* is van de tekenreeksvoorstelling van een 64-bits geheel getal zijn dat wordt gegenereerd door de Fabric-actoren-runtime voor intern gebruik. Dit is opgenomen in de naam van het exemplaar te zorgen dat deze zijn uniek en te voorkomen veroorzaken een conflict met andere namen van prestatiemeteritems-exemplaar. Gebruikers moeten niet proberen te interpreteren in dit gedeelte van de naam exemplaar Prestatiemeter.

Hieronder volgt een voorbeeld van een naam van het exemplaar van een item die deel uitmaakt van de `Service Fabric Actor Method` categorie:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

In het bovenstaande voorbeeld `ivoicemailboxactor.leavemessageasync` is de naam van de methode `2` is de 32-bits-ID die is gegenereerd voor intern gebruik van de runtime, `89383d32-e57e-4a9b-a6ad-57c6792aa521` is van de tekenreeksvoorstelling van de Service Fabric-partitie-ID, en `635650083804480486` is de 64-bits-ID die is gegenereerd voor de de runtime intern gebruik.

## <a name="list-of-events-and-performance-counters"></a>Lijst met gebeurtenissen en prestatiemeters
### <a name="actor-method-events-and-performance-counters"></a>Actor-methode gebeurtenissen en prestatiemeters
De Reliable Actors-runtime verzendt de volgende gebeurtenissen met betrekking tot [actor methoden](service-fabric-reliable-actors-introduction.md).

| Gebeurtenisnaam | Gebeurtenis-id | Niveau | Trefwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Uitgebreid |0x2 |Actoren runtime is bijna een actormethode aanroepen. |
| ActorMethodStop |8 |Uitgebreid |0x2 |Uitvoering van een actormethode is voltooid. Dat wil zeggen, de asynchrone aanroep van de runtime voor de actormethode heeft geretourneerd en de taak die wordt geretourneerd door de actormethode is voltooid. |
| ActorMethodThrewException |9 |Waarschuwing |0x3 |Er is een uitzondering opgetreden tijdens het uitvoeren van een actormethode van tijdens asynchrone aanroep naar de actormethode van de runtime of tijdens het uitvoeren van de taak die wordt geretourneerd door de actormethode. Deze gebeurtenis geeft aan dat een soort fout in de actor-code die onderzoek nodig heeft. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems met betrekking tot de uitvoering van de actor-methoden.

| Categorienaam | Naam van het prestatiemeteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-actormethode |Aanroepen/sec |Aantal keren dat de actorservicemethode wordt aangeroepen per seconde |
| Service Fabric-actormethode |Gemiddeld aantal milliseconden per aanroep |Gebruikte tijd in milliseconden voor het uitvoeren van de actorservicemethode |
| Service Fabric-actormethode |Aantal opgetreden uitzonderingen/sec |Aantal keren dat de actorservicemethode een uitzondering per seconde is opgetreden |

### <a name="concurrency-events-and-performance-counters"></a>Gelijktijdigheid van taken, gebeurtenissen en prestatiemeters
De Reliable Actors-runtime verzendt de volgende gebeurtenissen met betrekking tot [gelijktijdigheid](service-fabric-reliable-actors-introduction.md#concurrency).

| Gebeurtenisnaam | Gebeurtenis-id | Niveau | Trefwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorMethodCallsWaitingForLock |12 |Uitgebreid |0x8 |Deze gebeurtenis is geschreven aan het begin van elke nieuwe inschakelen in een actor. Het bevat het aantal in behandeling zijnde actoraanroepen die wachten op het verkrijgen van de per-actorvergrendeling die schakelt op basis van gelijktijdigheid afdwingt. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op de gelijktijdigheid van taken.

| Categorienaam | Naam van het prestatiemeteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-actor |Aantal actoraanroepen die wachten op een actorvergrendeling |Aantal in behandeling zijnde actoraanroepen die wachten op het verkrijgen van de per-actorvergrendeling die inschakelen op basis van gelijktijdigheid afdwingt |
| Service Fabric-actor |Gemiddeld aantal milliseconden wachttijd per vergrendeling |Gebruikte tijd (in milliseconden) voor het verkrijgen van de per-actorvergrendeling die inschakelen op basis van gelijktijdigheid afdwingt |
| Service Fabric-actor |Gemiddeld aantal milliseconden actorvergrendeling vastgehouden |Tijd (in milliseconden) waarvoor de afzonderlijke actorvergrendeling wordt vastgehouden |

### <a name="actor-state-management-events-and-performance-counters"></a>Actor-staat management gebeurtenissen en prestatiemeters
De Reliable Actors-runtime verzendt de volgende gebeurtenissen met betrekking tot [actor statusbeheer](service-fabric-reliable-actors-state-management.md).

| Gebeurtenisnaam | Gebeurtenis-id | Niveau | Trefwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorSaveStateStart |10 |Uitgebreid |0x4 |Actoren runtime is bijna de actorstatus opslaan. |
| ActorSaveStateStop |11 |Uitgebreid |0x4 |Actoren runtime is voltooid de actorstatus opslaan. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems met betrekking tot statusbeheer actor.

| Categorienaam | Naam van het prestatiemeteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-actor |Gemiddeld aantal milliseconden per bewerking voor het opslaan van de status |Gebruikte tijd in milliseconden voor het opslaan van de actorstatus |
| Service Fabric-actor |Gemiddeld aantal milliseconden per bewerking voor het laden van de status |Gebruikte tijd in milliseconden voor het laden van de actorstatus |

### <a name="events-related-to-actor-replicas"></a>Gebeurtenissen met betrekking tot actorreplica 's
De Reliable Actors-runtime verzendt de volgende gebeurtenissen met betrekking tot [actorreplica's](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

| Gebeurtenisnaam | Gebeurtenis-id | Niveau | Trefwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoleToPrimary |1 |Informatief |0x1 |Actor-replica gewijzigd primaire rol. Dit betekent dat de actoren voor deze partitie binnen deze replica wordt gemaakt. |
| ReplicaChangeRoleFromPrimary |2 |Informatief |0x1 |Actor-replica gewijzigd in niet-primaire rol. Dit betekent dat de actoren voor deze partitie niet meer worden gemaakt binnen deze replica. Er zijn geen nieuwe aanvragen geleverd aan actoren al binnen deze replica wordt gemaakt. De actoren wordt verwijderd nadat alle aanvragen in uitvoering zijn voltooid. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Actor-activering en deactivering gebeurtenissen en prestatiemeters
De Reliable Actors-runtime verzendt de volgende gebeurtenissen met betrekking tot [actor activering en deactivering](service-fabric-reliable-actors-lifecycle.md).

| Gebeurtenisnaam | Gebeurtenis-id | Niveau | Trefwoord | Beschrijving |
| --- | --- | --- | --- | --- |
| ActorActivated |5 |Informatief |0x1 |Een actor is geactiveerd. |
| ActorDeactivated |6 |Informatief |0x1 |Een actor is gedeactiveerd. |

De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op de actor-activering en deactivering.

| Categorienaam | Naam van het prestatiemeteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-actor |Gemiddeld aantal milliseconden voor OnActivateAsync |Gebruikte tijd in milliseconden voor het uitvoeren van de methode OnActivateAsync |

### <a name="actor-request-processing-performance-counters"></a>Actor-aanvraagverwerking-prestatiemeteritems
Wanneer een client een methode via een proxy actorobject roept, resulteert dit in een bericht naar de actorservice worden verzonden via het netwerk. De service het aanvraagbericht verwerkt en stuurt een antwoord terug naar de client. De runtime Reliable Actors publiceert de volgende prestatiemeteritems die betrekking hebben op de actor-aanvraagverwerking.

| Categorienaam | Naam van het prestatiemeteritem | Beschrijving |
| --- | --- | --- |
| Service Fabric-actor |Aantal openstaande aanvragen |Aantal aanvragen in de service wordt verwerkt |
| Service Fabric-actor |Gemiddeld aantal milliseconden per aanvraag |Gebruikte tijd (in milliseconden) door de service om een aanvraag te verwerken |
| Service Fabric-actor |Gemiddeld aantal milliseconden voor de deserialisatie van aanvragen |Gebruikte tijd (in milliseconden) voor het deserialiseren van het aanvraagbericht actor wanneer het is ontvangen door de service |
| Service Fabric-actor |Gemiddeld aantal milliseconden voor de serialisatie van reacties |Gebruikte tijd (in milliseconden) voor het serialiseren van het antwoordbericht actor bij de service voordat de reactie wordt verzonden naar de client |

## <a name="next-steps"></a>Volgende stappen
* [Hoe Reliable Actors het Service Fabric-platform gebruiken](service-fabric-reliable-actors-platform.md)
* [Actor-API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Gebeurtenisbron providers in het voorbeeld](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
