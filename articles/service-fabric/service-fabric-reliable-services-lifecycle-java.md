---
title: Levenscyclus van de Azure Service Fabric Reliable Services | Microsoft Docs
description: Meer informatie over de levenscyclus van gebeurtenissen in Service Fabric Reliable Services.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: ad4228ade68f4494e5be0454643752e742c1cc81
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/13/2018
---
# <a name="reliable-services-lifecycle"></a>Levenscyclus van Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java op Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Reliable Services is een van de programmeermodellen die beschikbaar zijn in Azure Service Fabric. Bij leren over de levenscyclus van Reliable Services, is het meest belangrijk om te begrijpen van de levenscyclus van basic-gebeurtenissen. De exacte ordening van gebeurtenissen, is afhankelijk van configuratie-informatie. 

In het algemeen bevat de levenscyclus van Reliable Services de volgende gebeurtenissen:

* Tijdens het opstarten:
  * Services worden samengesteld.
  * Services hebben een kans opbouwt en nul of meer listeners retourneren.
  * Elke geretourneerde luisteraars zijn geopend voor communicatie met de service.
  * De service `runAsync` methode wordt aangeroepen, zodat de service kunt langlopende doen of werk op de achtergrond.
* Tijdens het afsluiten:
  * De annulering-token dat is doorgegeven aan `runAsync` is geannuleerd, en de-listeners zijn gesloten.
  * Het serviceobject zelf is destructed.

De volgorde van gebeurtenissen in Reliable Services mogelijk enigszins verschillen, afhankelijk van de betrouwbare service stateless of stateful wijzigen. 

U moet ook de primaire wisselen scenario houden voor stateful services. Tijdens deze reeks de functie van primaire worden overgedragen naar een andere replica (of terugkomt) zonder dat de server afgesloten. 

Ten slotte moet u nadenken over de fout of storing voorwaarden.

## <a name="stateless-service-startup"></a>Staatloze service is gestart
De levenscyclus van een staatloze service is zeer eenvoudig. Dit is de volgorde van gebeurtenissen:

1. De service is samengesteld.
2. Deze gebeurtenissen plaatsvinden parallel:
    - `StatelessService.createServiceInstanceListeners()`wordt aangeroepen, en een geretourneerde luisteraars worden geopend. `CommunicationListener.openAsync()`voor elke listener wordt aangeroepen.
    - De service `runAsync` methode (`StatelessService.runAsync()`) wordt genoemd.
3. Indien aanwezig, van de service eigen `onOpenAsync` methode wordt aangeroepen. In het bijzonder `StatelessService.onOpenAsync()` wordt aangeroepen. Dit is een ongewoon onderdrukking, maar deze beschikbaar is.

Het is belangrijk dat u ziet dat er geen volgorde tussen de oproep maken en de listeners openen en de aanroep van `runAsync`. De listeners mogelijk geopend voordat `runAsync` wordt gestart. Op deze manier `runAsync` kan worden aangeroepen voordat de communicatielisteners zijn geopend, of ze ook zijn gemaakt. Als geen synchronisatie vereist is, moet het worden uitgevoerd door de implementeerder. Hier volgen enkele algemene oplossingen:

* Soms werkt listeners alleen als andere informatie is gemaakt of andere werk wordt uitgevoerd. Voor stateless services, die meestal kan worden gemaakt in de service-constructor. Het kan worden uitgevoerd tijdens de `createServiceInstanceListeners()` aanroepen, of als onderdeel van de opbouw van de listener zelf.
* Soms de code in `runAsync` start niet totdat de listeners geopend zijn. In dit geval is aanvullende coördinatie nodig. Een veelgebruikte oplossing is het toevoegen van een markering in de listeners. De vlag geeft aan wanneer de listeners hebt voltooid. De `runAsync` methode controleert dit voordat u doorgaat met het echte werk.

## <a name="stateless-service-shutdown"></a>Afsluiten van de staatloze service
Bij een stateless service wordt afgesloten, is het hetzelfde patroon gevolgd, maar in omgekeerde volgorde:

1. Deze gebeurtenissen plaatsvinden parallel:
    - Alle open listeners worden gesloten. `CommunicationListener.closeAsync()`voor elke listener wordt aangeroepen.
    - De annulering-token dat is doorgegeven aan `runAsync()` is geannuleerd. Controleren van de token van de annulering `isCancelled` resultaat van de eigenschap `true`, en als de naam van het token `throwIfCancellationRequested` methode er wordt een `CancellationException`.
2. Wanneer `closeAsync()` eindigt op elke listener en `runAsync()` ook is voltooid, van de service `StatelessService.onCloseAsync()` methode wordt aangeroepen, indien aanwezig. Dit wordt opnieuw, niet een algemene onderdrukking.
3. Na `StatelessService.onCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-startup"></a>Stateful service is gestart
Stateful services hebben een patroon dat is vergelijkbaar met stateless services, met enkele wijzigingen. Dit is de volgorde van gebeurtenissen voor het starten van een stateful service:

1. De service is samengesteld.
2. `StatefulServiceBase.onOpenAsync()`wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.
3. Deze gebeurtenissen plaatsvinden parallel:
    - `StatefulServiceBase.createServiceReplicaListeners()`wordt aangeroepen. 
      - Als de service een primaire service is, worden alle geretourneerde luisteraars geopend. `CommunicationListener.openAsync()`voor elke listener wordt aangeroepen.
      - Als de service een secundaire service is, alleen listeners gemarkeerd als `listenOnSecondary = true` worden geopend. -Listeners die geopend op de secundaire replica's zijn met is minder gangbaar.
    - Als de service momenteel een primaire, de service van is `StatefulServiceBase.runAsync()` methode wordt aangeroepen.
4. Nadat alle de replica-listener van `openAsync()` roept voltooien en `runAsync()` wordt aangeroepen, `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

Net als bij een stateless services, in de stateful service er is geen coördinatie tussen de volgorde waarin de listeners zijn gemaakt en geopend en wanneer `runAsync` wordt aangeroepen. Als u coördinatie moet, zijn de oplossingen grotendeels hetzelfde. Maar er is één extra case voor stateful service. Stel dat de oproepen die bij de communicatielisteners binnenkomen informatie bewaard binnen enkele vereisen [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). Omdat de communicatielisteners mogelijk geopend voordat de betrouwbare verzamelingen worden gelezen of beschrijfbaar, en voordat `runAsync` wordt gestart, een aantal extra coördinatie nodig is. De eenvoudigste en meest voorkomende oplossing is voor de communicatielisteners retourneren een foutcode. De client gebruikt de foutcode weten om de aanvraag opnieuw proberen.

## <a name="stateful-service-shutdown"></a>Afsluiten van de stateful service
Zoals stateless services de levenscyclus van gebeurtenissen tijdens het afsluiten zijn hetzelfde als tijdens het opstarten, maar omgekeerd. Wanneer een stateful service wordt afgesloten, gebeurt het volgende:

1. Deze gebeurtenissen plaatsvinden parallel:
    - Alle open listeners worden gesloten. `CommunicationListener.closeAsync()`voor elke listener wordt aangeroepen.
    - De annulering-token dat is doorgegeven aan `runAsync()` is geannuleerd. Een aanroep van de token van de annulering `isCancelled()` methode retourneert `true`, en als de naam van het token `throwIfCancellationRequested()` methode er wordt een `OperationCanceledException`.
2. Na `closeAsync()` eindigt op elke listener en `runAsync()` ook is voltooid, van de service `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

   > [!NOTE]  
   > Wachten op `runAsync` voltooid is alleen nodig als deze replica een primaire replica is.

3. Na de `StatefulServiceBase.onChangeRoleAsync()` methode is voltooid, de `StatefulServiceBase.onCloseAsync()` methode wordt aangeroepen. Deze aanroep is een ongewoon onderdrukking, maar deze beschikbaar is.
3. Na `StatefulServiceBase.onCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-primary-swaps"></a>Primaire worden verwisseld stateful service
Terwijl een stateful service wordt uitgevoerd, communicatielisteners zijn geopend en de `runAsync` methode wordt aangeroepen voor de primaire replica's van die stateful services alleen. Secundaire replica's zijn gebouwd, maar er worden geen verdere aanroepen zien. Terwijl een stateful service wordt uitgevoerd, de replica die momenteel is de primaire kunt wijzigen. De levenscyclus van gebeurtenissen die een stateful replica kunt zien, is afhankelijk van de replica wordt gedegradeerd of gepromoveerd tijdens het wisselen.

### <a name="for-the-demoted-primary"></a>Voor de gedegradeerde primaire
Service Fabric moet de primaire replica die wordt gedegradeerd om te stoppen verwerken van berichten en eventuele achtergrondwerk stoppen. Deze stap is vergelijkbaar met wanneer de service is afgesloten. Een verschil is dat de service is niet destructed of afgesloten, omdat een secundair blijft. De volgende gebeurtenissen vinden plaats:

1. Deze gebeurtenissen plaatsvinden parallel:
    - Alle open listeners worden gesloten. `CommunicationListener.closeAsync()`voor elke listener wordt aangeroepen.
    - De annulering-token dat is doorgegeven aan `runAsync()` is geannuleerd. Een controle van de annulering-token `isCancelled()` methode retourneert `true`. Als de naam van het token `throwIfCancellationRequested()` methode er wordt een `OperationCanceledException`.
2. Na `closeAsync()` eindigt op elke listener en `runAsync()` ook is voltooid, van de service `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

### <a name="for-the-promoted-secondary"></a>Voor de gepromoveerde secundaire
Service Fabric moet op dezelfde manier de secundaire replica die wordt gepromoveerd beginnen met luisteren op berichten op de kabel, en te starten elke achtergrondtaken die nodig is om te voltooien. Dit proces is vergelijkbaar met wanneer de service wordt gemaakt. Het verschil is dat de replica zelf al bestaat. De volgende gebeurtenissen vinden plaats:

1. Deze gebeurtenissen plaatsvinden parallel:
    - `StatefulServiceBase.createServiceReplicaListeners()`wordt aangeroepen en een geretourneerde luisteraars worden geopend. `CommunicationListener.openAsync()`voor elke listener wordt aangeroepen.
    - De service `StatefulServiceBase.runAsync()` methode wordt aangeroepen.
2. Nadat alle de replica-listener van `openAsync()` roept voltooien en `runAsync()` wordt aangeroepen, `StatefulServiceBase.onChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Veelvoorkomende problemen tijdens het afsluiten van de stateful service en de primaire degradatie
Service Fabric Hiermee wijzigt u de primaire van een stateful service om verschillende redenen. De meest voorkomende redenen zijn [cluster herverdeling](service-fabric-cluster-resource-manager-balancing.md) en [upgrade van de toepassing](service-fabric-application-upgrade.md). Tijdens deze bewerkingen is het belangrijk dat de service respecteert de `cancellationToken`. Dit geldt ook tijdens het afsluiten van de normale service, bijvoorbeeld als de service is verwijderd.

Services die door deze annulering niet correct verwerken kunnen enkele problemen optreden. Deze bewerkingen zijn trage omdat Service Fabric wacht tot de services te stoppen zonder problemen. Dit kan uiteindelijk leiden tot mislukte upgrades die time-out en het terugdraaien. Het token annulering inwilligen ook kunnen ertoe leiden dat imbalanced clusters. Clusters worden niet in balans omdat knooppunten hot ophalen. Echter, de services kunnen niet worden uitgevoerd omdat het te lang duurt voordat ze op een andere locatie verplaatsen. 

Omdat de services stateful, is het ook waarschijnlijk dat de services gebruiken [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). In Service Fabric wanneer een primaire wordt gedegradeerd, is een van de eerste dingen die plaatsvindt of schrijftoegang tot de status van de onderliggende is ingetrokken. Dit leidt tot een tweede set van problemen die mogelijk van invloed zijn op de levenscyclus van de service. De verzamelingen return uitzonderingen op basis van de timing en of de replica wordt verplaatst of afsluiten. Het is belangrijk dat u deze uitzonderingen correct wordt afgehandeld. 

Uitzonderingen in Service Fabric zijn permanente [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) of tijdelijke [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception). Permanente uitzonderingen moeten worden geregistreerd en worden gegenereerd. Tijdelijke uitzonderingen kunnen opnieuw worden geprobeerd op basis van Pogingslogica.

Een belangrijk onderdeel van de testen en valideren van Reliable Services verwerkt de uitzonderingen die afkomstig van het gebruik van zijn de `ReliableCollections` in combinatie met gebeurtenissen van de levenscyclus van de service. Het is raadzaam dat u altijd uitvoeren van uw service wordt belast. U moet ook upgrades uitvoeren en [chaos testen](service-fabric-controlled-chaos.md) voordat u implementeert naar productie. Deze eenvoudige stappen kunt u ervoor te zorgen dat uw service correct is uitgevoerd en waarop de levenscyclus van gebeurtenissen correct.

## <a name="notes-on-service-lifecycle"></a>Opmerkingen bij de levenscyclus van de service
* Zowel de `runAsync()` methode en de `createServiceInstanceListeners/createServiceReplicaListeners` aanroepen zijn optioneel. Mogelijk heeft een service een beide of geen van beide. Bijvoorbeeld, als de service alle zijn werk in reactie op gebruiker aanroepen doet, hoeft niet te implementeren `runAsync()`. Alleen de communicatielisteners en hun bijbehorende code zijn nodig. 

  Op deze manier is maken en het retourneren van communicatielisteners optioneel. De service mogelijk achtergrondwerk te doen, zodat alleen moet worden geïmplementeerd `runAsync()`.
* Is geldig voor een service te voltooien `runAsync()` is en dat het resultaat van deze. Dit wordt niet beschouwd als een voorwaarde is mislukt. Hiermee geeft u het achtergrondwerk van de service is voltooid. Voor betrouwbare stateful Services `runAsync()` zou worden opnieuw kan worden aangeroepen als de service wordt gedegradeerd van primaire en vervolgens terug naar de primaire gepromoveerd.
* Als een service afsluit `runAsync()` door er een onverwachte uitzondering is opgetreden, dit is een fout. Het service-object wordt afgesloten en een health-fout wordt gerapporteerd.
* Hoewel er geen tijdslimiet bij het retourneren van deze methoden, verliest u onmiddellijk de mogelijkheid om te schrijven. U kunt geen echte werk daarom niet voltooien. Het is raadzaam dat u zo snel mogelijk na ontvangst van de annuleringsaanvraag retourneren. Als uw service reageert niet op deze API-aanroepen in een redelijk tijdsbestek, kan uw service geforceerd beëindigd, Service Fabric. Meestal gebeurt dit alleen tijdens toepassingsupgrades of wanneer een service wordt verwijderd. Deze time-out is 15 minuten standaard.
* Fouten in de `onCloseAsync()` pad leiden tot een `onAbort()` wordt aangeroepen. Deze aanroep is een kans laatste kans, best-effort voor de service te opruimen en de vrijgave van alle bronnen die ze hebben aangevraagd.

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Reliable Services](service-fabric-reliable-services-introduction.md)
* [Betrouwbare Quick Start voor Services](service-fabric-reliable-services-quick-start-java.md)
* [Reliable Services geavanceerde gebruik](service-fabric-reliable-services-advanced-usage.md)
