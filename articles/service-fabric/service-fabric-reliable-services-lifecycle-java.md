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
ms.openlocfilehash: 93fd003ff5ba7673e5a719fb1ced0cbb97034610
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
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

## <a name="stateful-service-startup"></a>Stateful service is gestart
Stateful services hebben een vergelijkbaar patroon naar stateless services, met enkele wijzigingen. Voor het opstarten van een stateful service, is de volgorde van gebeurtenissen als volgt uit:

1. De service is samengesteld.
2. `StatefulServiceBase.onOpenAsync()`wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.
3. De volgende zaken gebeuren parallel:
    - `StatefulServiceBase.createServiceReplicaListeners()`wordt aangeroepen. 
      - Als de service een primaire service is, worden alle geretourneerde luisteraars geopend. `CommunicationListener.openAsync()`voor elke listener wordt aangeroepen.
      - Als de service een secundaire service is, alleen deze listeners gemarkeerd als `listenOnSecondary = true` worden geopend. -Listeners die geopend op de secundaire replica's zijn met is minder gangbaar.
    - Als de service momenteel een primaire, de service van is `StatefulServiceBase.runAsync()` methode wordt aangeroepen.
4. Nadat alle de replica-listener van `openAsync()` roept voltooien en `runAsync()` wordt aangeroepen, `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

Net als bij een stateless services, er is geen coördinatie tussen de volgorde waarin de listeners zijn gemaakt en geopend en wanneer **runAsync** wordt aangeroepen. Als u coördinatie moet, zijn de oplossingen grotendeels hetzelfde. Er is één extra case voor stateful service. Stel dat de oproepen die bij de communicatielisteners binnenkomen informatie bewaard binnen enkele vereisen [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). Omdat de communicatielisteners kunnen worden geopend voordat de betrouwbare verzamelingen worden gelezen of geschreven en vóór **runAsync** kan worden gestart, een aantal extra coördinatie nodig is. De eenvoudigste en meest voorkomende oplossing is voor de communicatielisteners retourneren een foutcode die de client gebruikt om te weten om de aanvraag opnieuw proberen.

## <a name="stateful-service-shutdown"></a>Afsluiten van de stateful service
Zoals stateless services de levenscyclus van gebeurtenissen tijdens het afsluiten zijn hetzelfde als tijdens het opstarten, maar omgekeerd. Wanneer een stateful service wordt afgesloten, gebeurt het volgende:

1. Parallel:
    - Alle open listeners worden gesloten. `CommunicationListener.closeAsync()`voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `runAsync()` is geannuleerd. Een aanroep van de token van de annulering `isCancelled()` methode retourneert de waarde true is, en als de naam van het token `throwIfCancellationRequested()` methode er wordt een `OperationCanceledException`.
2. Na `closeAsync()` eindigt op elke listener en `runAsync()` ook is voltooid, van de service `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

   > [!NOTE]  
   > De noodzaak om te wachten op **runAsync** voltooid is alleen nodig als deze replica een primaire replica is.

3. Na de `StatefulServiceBase.onChangeRoleAsync()` methode is voltooid, de `StatefulServiceBase.onCloseAsync()` methode wordt aangeroepen. Deze aanroep is een ongewoon onderdrukking, maar deze beschikbaar is.
3. Na `StatefulServiceBase.onCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-primary-swaps"></a>Primaire worden verwisseld stateful service
Terwijl een stateful service wordt uitgevoerd, alleen de primaire replica's van die stateful services hebben hun communicatielisteners geopend en hun **runAsync** methode aangeroepen. Secundaire replica's zijn gebouwd, maar er worden geen verdere aanroepen zien. Terwijl een stateful service wordt uitgevoerd, de replica die momenteel is de primaire kunt wijzigen. Wat betekent dit in termen van de levenscyclus van gebeurtenissen die een replica kunt zien? Het gedrag dat de stateful replica ziet, is afhankelijk van de replica wordt gedegradeerd of gepromoveerd tijdens het wisselen.

### <a name="for-the-primary-thats-demoted"></a>Voor de primaire die wordt gedegradeerd
Voor de primaire replica die wordt gedegradeerd, moet Service Fabric deze replica stoppen verwerken van berichten en eventuele achtergrondwerk hiervan is afgesloten. Als gevolg hiervan lijkt deze stap op wanneer de service wordt afgesloten. Een verschil is dat de service is niet destructed of gesloten omdat er een secundair blijft. De volgende API's worden genoemd:

1. Parallel:
    - Alle open listeners worden gesloten. `CommunicationListener.closeAsync()`voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `runAsync()` is geannuleerd. Een controle van de annulering-token `isCancelled()` methode retourneert de waarde true is, en als de naam van het token `throwIfCancellationRequested()` methode er wordt een `OperationCanceledException`.
2. Na `closeAsync()` eindigt op elke listener en `runAsync()` ook is voltooid, van de service `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

### <a name="for-the-secondary-thats-promoted"></a>Voor de secundaire die wordt gepromoveerd
Service Fabric moet op dezelfde manier de secundaire replica die wordt gepromoveerd om te beginnen met luisteren op berichten op de kabel en start een achtergrondtaken moet worden voltooid. Als gevolg hiervan lijkt dit proces op wanneer de service is gemaakt, behalve dat de replica zelf al bestaat. De volgende API's worden genoemd:

1. Parallel:
    - `StatefulServiceBase.createServiceReplicaListeners()`wordt aangeroepen en een geretourneerde luisteraars worden geopend. `CommunicationListener.openAsync()`voor elke listener wordt aangeroepen.
    - De service `StatefulServiceBase.runAsync()` methode wordt aangeroepen.
2. Nadat alle de replica-listener van `openAsync()` roept voltooien en `runAsync()` wordt aangeroepen, `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.


### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Veelvoorkomende problemen tijdens het afsluiten van de stateful service en de primaire degradatie
Service Fabric Hiermee wijzigt u de primaire van een stateful service voor een aantal redenen. Zijn de meest voorkomende [cluster herverdeling](service-fabric-cluster-resource-manager-balancing.md) en [upgrade van de toepassing](service-fabric-application-upgrade.md). Tijdens deze bewerkingen (en tijdens het afsluiten van de normale service, zoals u ziet u of de service is verwijderd), is het belangrijk dat de service wordt de `cancellationToken`. 

Services die door deze annulering niet correct verwerken, kunnen enkele problemen optreden. Deze bewerkingen zijn trage omdat Service Fabric wacht tot de services te stoppen zonder problemen. Uiteindelijk kan dit leiden tot mislukte upgrades die time-out en terug te draaien. Imbalanced clusters kan ook worden veroorzaakt door niet voldoen aan het token voor annulering. Clusters worden niet in balans omdat knooppunten hot ophalen, maar de services kunnen niet worden uitgevoerd omdat het te lang duurt voordat ze op een andere locatie verplaatsen. 

Omdat de services stateful, is het ook waarschijnlijk dat ze gebruiken de [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). In Service Fabric wanneer een primaire wordt gedegradeerd, is een van de eerste dingen die plaatsvindt of schrijftoegang tot de status van de onderliggende is ingetrokken. Dit leidt tot een tweede set van problemen met de levenscyclus van de service. De verzamelingen return uitzonderingen op basis van de timing en of de replica wordt verplaatst of afsluiten. Deze uitzonderingen moeten correct worden verwerkt. Uitzonderingen in Service Fabric vallen in de permanente [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) en tijdelijke [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception) categorieën. Permanente uitzonderingen moeten worden geregistreerd en opgetreden tijdens de tijdelijke uitzonderingen kunnen opnieuw worden geprobeerd op basis van bepaalde Pogingslogica.

Afhandeling van de uitzonderingen die afkomstig van het gebruik van zijn de `ReliableCollections` in combinatie met gebeurtenissen van de levenscyclus van de service is een belangrijk onderdeel van de testen en valideren van een betrouwbare Service. Het is raadzaam dat u uw service wordt belast altijd worden uitgevoerd tijdens het uitvoeren van upgrades en [chaos testen](service-fabric-controlled-chaos.md) voordat u implementeert naar productie. Deze eenvoudige stappen kunt u ervoor te zorgen dat uw service correct is geïmplementeerd en de levenscyclus van gebeurtenissen correct worden verwerkt.

## <a name="notes-on-service-lifecycle"></a>Opmerkingen bij de levenscyclus van de service
* Zowel de `runAsync()` methode en de `createServiceInstanceListeners/createServiceReplicaListeners` aanroepen zijn optioneel. Een service kan één van deze beide of geen van beide hebben. Bijvoorbeeld, als de service alle zijn werk in reactie op gebruiker aanroepen doet, hoeft niet te implementeren `runAsync()`. Alleen de communicatielisteners en hun bijbehorende code zijn nodig. Op deze manier is maken en het retourneren van communicatielisteners optioneel, als de service heeft mogelijk alleen achtergrondwerk te doen en dus alleen moet worden geïmplementeerd`runAsync()`
* Is geldig voor een service te voltooien `runAsync()` is en dat het resultaat van deze. Dit wordt niet beschouwd als een voorwaarde is mislukt en het achtergrondwerk voltooien van de service zou vertegenwoordigen. Voor stateful betrouwbare services `runAsync()` opnieuw zou worden aangeroepen als de service zijn gedegradeerd van primaire en vervolgens terug naar de primaire gepromoveerd.
* Als een service afsluit `runAsync()` door er een onverwachte uitzondering is opgetreden, dit is een fout en het serviceobject wordt afgesloten en een statusfout gemeld.
* Hoewel er geen tijdslimiet bij het retourneren van deze methoden, moet u onmiddellijk de mogelijkheid verliezen om te schrijven en daarom echte werk niet voltooien. Het is raadzaam dat u zo snel mogelijk na ontvangst van de annuleringsaanvraag retourneren. Als uw service niet op deze API-aanroepen in een redelijk tijdsbestek reageert beëindigen Service Fabric geforceerd uw service. Meestal gebeurt dit alleen tijdens toepassingsupgrades of wanneer een service wordt verwijderd. Deze time-out is 15 minuten standaard.
* Fouten in de `onCloseAsync()` pad leiden tot een `onAbort()` wordt aangeroepen dit is een kans van de laatste kans best-effort voor de service op te schonen omhoog en laat alle bronnen die ze hebben aangevraagd.

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Reliable Services](service-fabric-reliable-services-introduction.md)
* [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start-java.md)
* [Reliable Services geavanceerde gebruik](service-fabric-reliable-services-advanced-usage.md)
