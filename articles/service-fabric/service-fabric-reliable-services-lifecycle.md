---
title: Overzicht van de levenscyclus van Azure Service Fabric Reliable Services | Microsoft Docs
description: Meer informatie over de levenscyclus van andere gebeurtenissen in Service Fabric Reliable Services
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek;
ms.assetid: ''
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9cb017997c528c987403186097599a721ee591bc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="reliable-services-lifecycle-overview"></a>Overzicht van de levenscyclus van betrouwbare Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java op Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Wanneer u de levenscycli van Azure Service Fabric Reliable Services overweegt, worden de basisprincipes van de levenscyclus van het belangrijkst zijn. In het algemeen omvat de levenscyclus het volgende:

- Tijdens het opstarten:
  - Services worden samengesteld.
  - De services hebben een kans opbouwt en nul of meer listeners retourneren.
  - Elke geretourneerde luisteraars worden geopend, zodat de communicatie met de service.
  - De service **RunAsync** methode wordt aangeroepen, zodat de service wilt langlopende taken of achtergrondwerk.
- Tijdens het afsluiten:
  - Het token annulering doorgegeven aan **RunAsync** is geannuleerd, en de-listeners zijn gesloten.
  - Nadat de listeners sluit, wordt het serviceobject zelf destructed.

Er zijn details om de exacte ordening van deze gebeurtenissen. De volgorde van gebeurtenissen kan enigszins verschillen, afhankelijk van de betrouwbare Service stateless of stateful wijzigen. Daarnaast moet er voor stateful services te maken met het primaire swap-scenario. Tijdens deze reeks de functie van primaire worden overgedragen naar een andere replica (of terugkomt) zonder dat de server afgesloten. Tot slot moeten we Denk na over de fout of storing voorwaarden.

## <a name="stateless-service-startup"></a>Staatloze service is gestart
De levenscyclus van een staatloze service is eenvoudig. Dit is de volgorde van gebeurtenissen:

1. De service is samengesteld.
2. Vervolgens, parallel gebeuren twee dingen:
    - `StatelessService.CreateServiceInstanceListeners()` wordt aangeroepen en een geretourneerde luisteraars worden geopend. `ICommunicationListener.OpenAsync()` voor elke listener wordt aangeroepen.
    - De service `StatelessService.RunAsync()` methode wordt aangeroepen.
3. Indien aanwezig, van de service `StatelessService.OnOpenAsync()` methode wordt aangeroepen. Deze aanroep is een ongewoon onderdrukking, maar deze beschikbaar is. Initialisatie van uitgebreide servicetaken kunnen op dit moment worden gestart.

Houd er rekening mee dat er geen ordening tussen de aanroepen maken en openen van de listeners en **RunAsync**. De listeners kunnen openen voordat **RunAsync** is gestart. U kunt op deze manier aanroepen **RunAsync** voordat de communicatielisteners geopend of zelfs gebouwd zijn. Als geen synchronisatie vereist is, is deze wijze van oefening maakt links naar de implementeerder. Hier volgen enkele algemene oplossingen:

  - Soms werkt listeners alleen als andere informatie is gemaakt of werk wordt uitgevoerd. Voor stateless services die werk kan doorgaans worden uitgevoerd op andere locaties, zoals het volgende: 
    - In de constructor van de service.
    - Tijdens de `CreateServiceInstanceListeners()` aanroepen.
    - Als onderdeel van de opbouw van de listener zelf.
  - Soms de code in **RunAsync** start niet totdat de listeners geopend zijn. In dit geval is aanvullende coördinatie nodig. Een veelgebruikte oplossing, is er een vlag die aangeeft wanneer ze klaar zijn binnen de listeners. Deze markering wordt vervolgens ingecheckt **RunAsync** voordat u doorgaat met het echte werk.

## <a name="stateless-service-shutdown"></a>Afsluiten van de staatloze service
Voor een stateless service wordt afgesloten, hetzelfde patroon gevolgd, alleen in omgekeerde volgorde:

1. Parallel:
    - Alle open listeners worden gesloten. `ICommunicationListener.CloseAsync()` voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `RunAsync()` is geannuleerd. Een controle van de annulering-token `IsCancellationRequested` eigenschap true retourneert, en als de naam van het token `ThrowIfCancellationRequested` methode er wordt een `OperationCanceledException`.
2. Na `CloseAsync()` eindigt op elke listener en `RunAsync()` ook is voltooid, van de service `StatelessService.OnCloseAsync()` methode wordt aangeroepen, indien aanwezig.  OnCloseAsync wordt aangeroepen wanneer het staatloze service-exemplaar gaat naar de juiste manier worden afgesloten. Dit kan gebeuren wanneer de code van de service wordt bijgewerkt, het service-exemplaar wordt verplaatst als gevolg van taakverdeling of een tijdelijke fout wordt gedetecteerd. Het is ongebruikelijk voor het onderdrukken van `StatelessService.OnCloseAsync()`, maar deze kan worden gebruikt voor het veilig bronnen sluiten, achtergrondverwerking stoppen, voltooien externe status opslaan of bestaande verbindingen afgesloten.
3. Na `StatelessService.OnCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-startup"></a>Stateful service is gestart
Stateful services hebben een vergelijkbaar patroon naar stateless services, met enkele wijzigingen. Voor het opstarten van een stateful service, is de volgorde van gebeurtenissen als volgt uit:

1. De service is samengesteld.
2. `StatefulServiceBase.OnOpenAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.
3. De volgende zaken gebeuren parallel:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wordt aangeroepen. 
      - Als de service een primaire service is, worden alle geretourneerde luisteraars geopend. `ICommunicationListener.OpenAsync()` voor elke listener wordt aangeroepen.
      - Als de service een secundaire service is, alleen deze listeners gemarkeerd als `ListenOnSecondary = true` worden geopend. -Listeners die geopend op de secundaire replica's zijn met is minder gangbaar.
    - Als de service momenteel een primaire, de service van is `StatefulServiceBase.RunAsync()` methode wordt aangeroepen.
4. Nadat alle de replica-listener van `OpenAsync()` roept voltooien en `RunAsync()` wordt aangeroepen, `StatefulServiceBase.OnChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

Net als bij een stateless services, er is geen coördinatie tussen de volgorde waarin de listeners zijn gemaakt en geopend en wanneer **RunAsync** wordt aangeroepen. Als u coördinatie moet, zijn de oplossingen grotendeels hetzelfde. Er is één extra case voor stateful service. Stel dat de oproepen die bij de communicatielisteners binnenkomen informatie bewaard binnen enkele vereisen [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). Omdat de communicatielisteners kunnen worden geopend voordat de betrouwbare verzamelingen worden gelezen of geschreven en vóór **RunAsync** kan worden gestart, een aantal extra coördinatie nodig is. De eenvoudigste en meest voorkomende oplossing is voor de communicatielisteners retourneren een foutcode die de client gebruikt om te weten om de aanvraag opnieuw proberen.

## <a name="stateful-service-shutdown"></a>Afsluiten van de stateful service
Zoals stateless services de levenscyclus van gebeurtenissen tijdens het afsluiten zijn hetzelfde als tijdens het opstarten, maar omgekeerd. Wanneer een stateful service wordt afgesloten, gebeurt het volgende:

1. Parallel:
    - Alle open listeners worden gesloten. `ICommunicationListener.CloseAsync()` voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `RunAsync()` is geannuleerd. Een controle van de annulering-token `IsCancellationRequested` eigenschap true retourneert, en als de naam van het token `ThrowIfCancellationRequested` methode er wordt een `OperationCanceledException`.
2. Na `CloseAsync()` eindigt op elke listener en `RunAsync()` ook is voltooid, van de service `StatefulServiceBase.OnChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

   > [!NOTE]  
   > De noodzaak om te wachten op **RunAsync** voltooid is alleen nodig als deze replica een primaire replica is.

3. Na de `StatefulServiceBase.OnChangeRoleAsync()` methode is voltooid, de `StatefulServiceBase.OnCloseAsync()` methode wordt aangeroepen. Deze aanroep is een ongewoon onderdrukking, maar deze beschikbaar is.
3. Na `StatefulServiceBase.OnCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-primary-swaps"></a>Primaire worden verwisseld stateful service
Terwijl een stateful service wordt uitgevoerd, alleen de primaire replica's van die stateful services hebben hun communicatielisteners geopend en hun **RunAsync** methode aangeroepen. Secundaire replica's zijn gebouwd, maar er worden geen verdere aanroepen zien. Terwijl een stateful service wordt uitgevoerd, de replica die momenteel is de primaire kunt wijzigen. Wat betekent dit in termen van de levenscyclus van gebeurtenissen die een replica kunt zien? Het gedrag dat de stateful replica ziet, is afhankelijk van de replica wordt gedegradeerd of gepromoveerd tijdens het wisselen.

### <a name="for-the-primary-thats-demoted"></a>Voor de primaire die wordt gedegradeerd
Voor de primaire replica die wordt gedegradeerd, moet Service Fabric deze replica stoppen verwerken van berichten en eventuele achtergrondwerk hiervan is afgesloten. Als gevolg hiervan lijkt deze stap op wanneer de service wordt afgesloten. Een verschil is dat de service is niet destructed of gesloten omdat er een secundair blijft. De volgende API's worden genoemd:

1. Parallel:
    - Alle open listeners worden gesloten. `ICommunicationListener.CloseAsync()` voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `RunAsync()` is geannuleerd. Een controle van de annulering-token `IsCancellationRequested` eigenschap true retourneert, en als de naam van het token `ThrowIfCancellationRequested` methode er wordt een `OperationCanceledException`.
2. Na `CloseAsync()` eindigt op elke listener en `RunAsync()` ook is voltooid, van de service `StatefulServiceBase.OnChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

### <a name="for-the-secondary-thats-promoted"></a>Voor de secundaire die wordt gepromoveerd
Service Fabric moet op dezelfde manier de secundaire replica die wordt gepromoveerd om te beginnen met luisteren op berichten op de kabel en start een achtergrondtaken moet worden voltooid. Als gevolg hiervan lijkt dit proces op wanneer de service is gemaakt, behalve dat de replica zelf al bestaat. De volgende API's worden genoemd:

1. Parallel:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wordt aangeroepen en een geretourneerde luisteraars worden geopend. `ICommunicationListener.OpenAsync()` voor elke listener wordt aangeroepen.
    - De service `StatefulServiceBase.RunAsync()` methode wordt aangeroepen.
2. Nadat alle de replica-listener van `OpenAsync()` roept voltooien en `RunAsync()` wordt aangeroepen, `StatefulServiceBase.OnChangeRoleAsync()` wordt aangeroepen. Deze aanroep is doorgaans niet overschreven in de service.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Veelvoorkomende problemen tijdens het afsluiten van de stateful service en de primaire degradatie
Service Fabric Hiermee wijzigt u de primaire van een stateful service voor een aantal redenen. Zijn de meest voorkomende [cluster herverdeling](service-fabric-cluster-resource-manager-balancing.md) en [upgrade van de toepassing](service-fabric-application-upgrade.md). Tijdens deze bewerkingen (en tijdens het afsluiten van de normale service, zoals u ziet u of de service is verwijderd), is het belangrijk dat de service wordt de `CancellationToken`. 

Services die door deze annulering niet correct verwerken, kunnen enkele problemen optreden. Deze bewerkingen zijn trage omdat Service Fabric wacht tot de services te stoppen zonder problemen. Uiteindelijk kan dit leiden tot mislukte upgrades die time-out en terug te draaien. Imbalanced clusters kan ook worden veroorzaakt door niet voldoen aan het token voor annulering. Clusters worden niet in balans omdat knooppunten hot ophalen, maar de services kunnen niet worden uitgevoerd omdat het te lang duurt voordat ze op een andere locatie verplaatsen. 

Omdat de services stateful, is het ook waarschijnlijk dat ze gebruiken de [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). In Service Fabric wanneer een primaire wordt gedegradeerd, is een van de eerste dingen die plaatsvindt of schrijftoegang tot de status van de onderliggende is ingetrokken. Dit leidt tot een tweede set van problemen met de levenscyclus van de service. De verzamelingen return uitzonderingen op basis van de timing en of de replica wordt verplaatst of afsluiten. Deze uitzonderingen moeten correct worden verwerkt. Uitzonderingen in Service Fabric vallen in de permanente [(`FabricException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) en tijdelijke [(`FabricTransientException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) categorieën. Permanente uitzonderingen moeten worden geregistreerd en opgetreden tijdens de tijdelijke uitzonderingen kunnen opnieuw worden geprobeerd op basis van bepaalde Pogingslogica.

Afhandeling van de uitzonderingen die afkomstig van het gebruik van zijn de `ReliableCollections` in combinatie met gebeurtenissen van de levenscyclus van de service is een belangrijk onderdeel van de testen en valideren van een betrouwbare Service. Het is raadzaam dat u uw service wordt belast altijd worden uitgevoerd tijdens het uitvoeren van upgrades en [chaos testen](service-fabric-controlled-chaos.md) voordat u implementeert naar productie. Deze eenvoudige stappen kunt u ervoor te zorgen dat uw service correct is geïmplementeerd en de levenscyclus van gebeurtenissen correct worden verwerkt.


## <a name="notes-on-the-service-lifecycle"></a>Opmerkingen bij de levenscyclus van de service
  - Zowel de `RunAsync()` methode en de `CreateServiceReplicaListeners/CreateServiceInstanceListeners` aanroepen zijn optioneel. Een service kan een van deze beide of geen van beide hebben. Bijvoorbeeld, als de service alle zijn werk in reactie op gebruiker aanroepen doet, hoeft niet te implementeren `RunAsync()`. Alleen de communicatielisteners en hun bijbehorende code zijn nodig. Op deze manier maken en het retourneren van communicatielisteners is optioneel, want de service hebben kan alleen achtergrondwerk te doen en dus alleen moet worden geïmplementeerd `RunAsync()`.
  - Is geldig voor een service te voltooien `RunAsync()` is en dat het resultaat van deze. Voltooien is niet een voorwaarde is mislukt. Voltooien van `RunAsync()` geeft aan dat het achtergrondwerk van de service is voltooid. Voor betrouwbare stateful services `RunAsync()` opnieuw wordt aangeroepen als de replica van primaire naar secundaire gedegradeerd en vervolgens terug naar de primaire gepromoveerd.
  - Als een service afsluit `RunAsync()` door er een onverwachte uitzondering is opgetreden, betekent dit een fout. Het service-object wordt afgesloten en een health-fout wordt gerapporteerd.
  - Hoewel er geen tijdslimiet bij het retourneren van deze methoden, moet u onmiddellijk de mogelijkheid verliezen om te schrijven naar betrouwbare verzamelingen en daarom echte werk niet voltooien. Het is raadzaam dat u zo snel mogelijk na ontvangst van de annuleringsaanvraag retourneren. Als uw service niet op deze API-aanroepen in een redelijk tijdsbestek reageert, kunnen uw service geforceerd beëindigd, Service Fabric. Meestal gebeurt dit alleen tijdens toepassingsupgrades of wanneer een service wordt verwijderd. Deze time-out is 15 minuten standaard.
  - Fouten in de `OnCloseAsync()` pad leiden tot een `OnAbort()` wordt aangeroepen, dit is een kans van de laatste kans best-effort voor de service te opruimen en de vrijgave van alle bronnen die ze hebben aangevraagd. Dit wordt doorgaans als een permanente storing wordt gedetecteerd op het knooppunt, of wanneer het Service Fabric niet betrouwbaar beheren van de levenscyclus van het service-exemplaar als gevolg van interne fouten genoemd.
  - `OnChangeRoleAsync()` wordt aangeroepen wanneer de replica stateful service rol, bijvoorbeeld primair of secundair wijzigen. Primaire replica's zijn opgegeven schrijven (toegestaan te maken en schrijven naar betrouwbare verzamelingen). Secundaire replica's zijn opgegeven leesstatus (kan alleen worden gelezen van bestaande betrouwbare verzamelingen). De meeste werk in een stateful service wordt uitgevoerd op de primaire replica. Secundaire replica's kunnen uitvoeren voor validatie van alleen-lezen, rapport genereren, gegevensanalyse of andere taken alleen-lezen.

## <a name="next-steps"></a>Volgende stappen
- [Inleiding tot Reliable Services](service-fabric-reliable-services-introduction.md)
- [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start.md)
- [Replica's en exemplaren](service-fabric-concepts-replica-lifecycle.md)
