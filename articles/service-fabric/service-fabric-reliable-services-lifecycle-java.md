---
title: Overzicht van de levenscyclus van Azure Service Fabric Reliable Services | Microsoft Docs
description: Meer informatie over de levenscyclus van andere gebeurtenissen in betrouwbare Service Fabric-services
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 80eb68346dd05c256c60725eb082aa0651fe7cbd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Overzicht van de levenscyclus van betrouwbare services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java op Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Wanneer u nadenkt over de levenscycli van Reliable Services, worden de basisprincipes van de levenscyclus van het belangrijkst zijn. In het algemeen:

* Tijdens het opstarten
  * Services zijn gebouwd
  * Ze hebben een kans opbouwt en nul of meer listeners retourneren
  * Elke geretourneerde luisteraars worden geopend, zodat de communicatie met de service
  * De Service runAsync-methode wordt aangeroepen, zodat de service lang actief of werk op de achtergrond
* Tijdens het afsluiten
  * De annulering-token dat is doorgegeven aan runAsync wordt geannuleerd en de-listeners zijn gesloten
  * Eenmaal is voltooid, wordt het serviceobject zelf destructed

Er zijn details om de exacte ordening van deze gebeurtenissen. De volgorde van gebeurtenissen kan met name enigszins verschillen, afhankelijk van de betrouwbare Service Stateless of Stateful wijzigen. Bovendien voor stateful services hebben we om te gaan met het primaire swap-scenario. Tijdens deze reeks de functie van primaire worden overgedragen naar een andere replica (of terugkomt) zonder dat de server afgesloten. Ten slotte, hebben we bij het nadenken over de fout of storing voorwaarden.

## <a name="stateless-service-startup"></a>Staatloze service is gestart
De levenscyclus van een staatloze service is zeer eenvoudig. Dit is de volgorde van gebeurtenissen:

1. De Service is samengesteld.
2. Klik in de parallelle twee dingen gebeuren:
    - `StatelessService.createServiceInstanceListeners()`wordt aangeroepen en elke geretourneerde luisteraars worden geopend (`CommunicationListener.openAsync()` is aangeroepen voor elke listener)
    - De service runAsync-methode (`StatelessService.runAsync()`) wordt genoemd
3. Indien aanwezig, wordt de methode van het service onOpenAsync aangeroepen (in het bijzonder `StatelessService.onOpenAsync()` wordt aangeroepen. Dit is een onderdrukking ongewoon maar deze beschikbaar is).

Het is belangrijk te weten dat er er geen volgorde tussen de aanroepen is voor het maken en open het listeners en runAsync. De listeners geopend voordat runAsync wordt gestart. Op deze manier runAsync mogelijk uiteindelijk aangeroepen voordat de communicatielisteners geopend zijn of zelfs is samengesteld. Als geen synchronisatie vereist is, is deze wijze van oefening maakt links naar de implementeerder. Algemene oplossingen:

* Soms werkt listeners alleen als andere informatie is gemaakt of gewerkte. Voor stateless services die werk kan doorgaans worden uitgevoerd in de constructor van de service, tijdens de `createServiceInstanceListeners()` aanroepen, of als onderdeel van de opbouw van de listener zelf.
* De code in runAsync wil soms niet worden gestart totdat de listeners geopend zijn. In dit geval is aanvullende coördinatie nodig. Een veelgebruikte oplossing is een aantal markering binnen de listeners die aangeeft wanneer ze hebt voltooid, die wordt gecontroleerd in runAsync voordat u doorgaat met het echte werk.

## <a name="stateless-service-shutdown"></a>Afsluiten van de staatloze service
Wanneer een stateless service wordt afgesloten, wordt hetzelfde patroon gevolgd, alleen in omgekeerde volgorde:

1. Parallel
    - Alle open listeners zijn gesloten (`CommunicationListener.closeAsync()` is aangeroepen voor elke listener)
    - Het token annulering doorgegeven aan `runAsync()` wordt geannuleerd (controleren van de token van de annulering `isCancelled` eigenschap true retourneert, en als de naam van het token `throwIfCancellationRequested` methode er wordt een `CancellationException`)
2. Eenmaal `closeAsync()` is voltooid op elke listener en `runAsync()` ook is voltooid, van de service `StatelessService.onCloseAsync()` methode wordt aangeroepen, indien aanwezig (opnieuw dit is een ongewoon onderdrukking).
3. Na `StatelessService.onCloseAsync()` is voltooid, het service-object is destructed

## <a name="notes-on-service-lifecycle"></a>Opmerkingen bij de levenscyclus van de service
* Zowel de `runAsync()` methode en de `createServiceInstanceListeners` aanroepen zijn optioneel. Een service kan één van deze beide of geen van beide hebben. Bijvoorbeeld, als de service alle zijn werk in reactie op gebruiker aanroepen doet, hoeft niet te implementeren `runAsync()`. Alleen de communicatielisteners en hun bijbehorende code zijn nodig. Op deze manier is maken en het retourneren van communicatielisteners optioneel, als de service heeft mogelijk alleen achtergrondwerk te doen en dus alleen moet worden geïmplementeerd`runAsync()`
* Is geldig voor een service te voltooien `runAsync()` is en dat het resultaat van deze. Dit wordt niet beschouwd als een voorwaarde is mislukt en het achtergrondwerk voltooien van de service zou vertegenwoordigen. Voor stateful betrouwbare services `runAsync()` opnieuw zou worden aangeroepen als de service zijn gedegradeerd van primaire en vervolgens terug naar de primaire gepromoveerd.
* Als een service afsluit `runAsync()` door er een onverwachte uitzondering is opgetreden, dit is een fout en het serviceobject wordt afgesloten en een statusfout gemeld.
* Hoewel er geen tijdslimiet bij het retourneren van deze methoden, moet u onmiddellijk de mogelijkheid verliezen om te schrijven en daarom echte werk niet voltooien. Het is raadzaam dat u zo snel mogelijk na ontvangst van de annuleringsaanvraag retourneren. Als uw service niet op deze API-aanroepen in een redelijk tijdsbestek reageert beëindigen Service Fabric geforceerd uw service. Meestal gebeurt dit alleen tijdens toepassingsupgrades of wanneer een service wordt verwijderd. Deze time-out is 15 minuten standaard.
* Fouten in de `onCloseAsync()` pad leiden tot een `onAbort()` wordt aangeroepen dit is een kans van de laatste kans best-effort voor de service op te schonen omhoog en laat alle bronnen die ze hebben aangevraagd.

> [!NOTE]
> Stateful betrouwbare services worden nog niet ondersteund in java.
>
>

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Reliable Services](service-fabric-reliable-services-introduction.md)
* [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start.md)
* [Reliable Services geavanceerde gebruik](service-fabric-reliable-services-advanced-usage.md)
