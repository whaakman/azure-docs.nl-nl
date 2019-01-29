---
title: Overzicht van de levenscyclus van Azure Service Fabric Reliable Services | Microsoft Docs
description: Meer informatie over de van andere levenscyclusgebeurtenissen in Service Fabric Reliable Services
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek;
ms.assetid: ''
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 391fc493d642c260a10b74aa42b805ad055dd8b1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164551"
---
# <a name="reliable-services-lifecycle-overview"></a>Overzicht van de levenscyclus van Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java op Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Wanneer u erover over de levenscycli van Azure Service Fabric Reliable Services denkt, zijn de basisprincipes van de levenscyclus van de belangrijkste. In het algemeen omvat de levenscyclus het volgende:

- Tijdens het opstarten:
  - Services zijn samengesteld.
  - De services hebben een kans om te bouwen en nul of meer listeners retourneren.
  - Elke geretourneerde listeners zijn geopend, zodat de communicatie met de service.
  - Van de service **RunAsync** methode wordt aangeroepen, zodat de service wilt langlopende taken of achtergrondwerk.
- Tijdens het afsluiten:
  - Het token annulering doorgegeven aan **RunAsync** wordt geannuleerd, en de listeners zijn gesloten.
  - Nadat de listeners hebt gesloten, wordt de serviceobject zelf destructed.

Er zijn meer informatie over de exacte volgorde van deze gebeurtenissen. De volgorde van gebeurtenissen kan enigszins, afhankelijk van de betrouwbare Service stateless of stateful wijzigen. Bovendien moet hebben we voor stateful services te maken met het primaire swap-scenario. Tijdens deze volgorde hebben, de rol van de primaire wordt overgedragen naar een andere replica (of terugkomen) zonder dat de service wordt afgesloten. Ten slotte moet we denken over de fout of fout voorwaarden.

## <a name="stateless-service-startup"></a>Starten van de stateless service
De levenscyclus van een staatloze service is eenvoudig. Dit is de volgorde van gebeurtenissen:

1. De service is gebouwd.
2. Vervolgens, parallel, gebeuren er twee dingen:
    - `StatelessService.CreateServiceInstanceListeners()` wordt aangeroepen en een geretourneerd listeners zijn geopend. `ICommunicationListener.OpenAsync()` voor elke listener wordt aangeroepen.
    - Van de service `StatelessService.RunAsync()` methode wordt aangeroepen.
3. Indien aanwezig, van de service `StatelessService.OnOpenAsync()` methode wordt aangeroepen. Deze aanroep is een ongebruikelijk onderdrukking, maar deze beschikbaar is. Uitgebreide service initialisatietaken kunnen op dit moment worden gestart.

Houd er rekening mee dat er geen ordening tussen de aanroepen te maken en openen van de listeners en **RunAsync**. De listeners kunnen openen voordat **RunAsync** wordt gestart. Op deze manier kunt u aanroepen **RunAsync** voordat de communicatielisteners geopend of zelfs gebouwd zijn. Als geen synchronisatie vereist is, is het bij wijze van oefening links naar de implementeerder. Hier volgen enkele algemene oplossingen:

  - Soms werkt listeners alleen als andere informatie wordt gemaakt of het werk wordt uitgevoerd. Voor stateless services, die werk kan doorgaans worden uitgevoerd op andere locaties, zoals het volgende: 
    - In de constructor van de service.
    - Tijdens de `CreateServiceInstanceListeners()` aanroepen.
    - Als onderdeel van het samenstellen van de listener zelf.
  - Soms de code in **RunAsync** start niet totdat de listeners geopend zijn. In dit geval is extra coördinatie nodig. Een gangbare oplossing is dat er een vlag die aangeeft wanneer ze klaar zijn binnen de listeners. Met deze markering wordt vervolgens gecontroleerd in **RunAsync** voordat u verdergaat met de werkelijke hoeveelheid werk.

## <a name="stateless-service-shutdown"></a>Afsluiten van de stateless service
Voor een stateless service met afsluiten, wordt hetzelfde patroon gevolgd, alleen in omgekeerde volgorde:

1. Parallel:
    - Alle open listeners zijn gesloten. `ICommunicationListener.CloseAsync()` voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `RunAsync()` is geannuleerd. Een controle uit van de token van de annulering `IsCancellationRequested` eigenschap retourneert ' True ', en als ze worden aangeroepen, van het token `ThrowIfCancellationRequested` methode genereert een `OperationCanceledException`.
2. Na `CloseAsync()` is voltooid op elke listener en `RunAsync()` ook is voltooid, van de service `StatelessService.OnCloseAsync()` methode wordt aangeroepen, indien aanwezig.  OnCloseAsync wordt aangeroepen wanneer de stateless service-exemplaar zal worden zonder problemen wordt afgesloten. Dit kan gebeuren wanneer de code van de service wordt bijgewerkt, het service-exemplaar wordt verplaatst vanwege taakverdeling of een tijdelijke storing wordt gedetecteerd. Het is niet gebruikelijk om op te heffen `StatelessService.OnCloseAsync()`, maar deze kan worden gebruikt om veilig bronnen sluiten, verwerking op de achtergrond te stoppen, voltooien van de externe status is opgeslagen of bestaande verbindingen afgesloten.
3. Na `StatelessService.OnCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-startup"></a>Starten van de stateful service
Stateful services hebben een soortgelijk patroon voor stateless services, met een aantal wijzigingen. Voor het opstarten van een stateful service, is de volgorde van gebeurtenissen als volgt uit:

1. De service is gebouwd.
2. `StatefulServiceBase.OnOpenAsync()` wordt aangeroepen. Deze aanroep is niet vaak worden genegeerd in de service.
3. De volgende dingen gebeuren parallel:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wordt aangeroepen. 
      - Als de service een primaire service is, worden alle geretourneerde listeners geopend. `ICommunicationListener.OpenAsync()` voor elke listener wordt aangeroepen.
      - Als de service een secundaire service is, wordt alleen deze listeners gemarkeerd als `ListenOnSecondary = true` worden geopend. Met listeners die geopend op de secundaire replica's zijn is het minder gebruikelijk.
    - Als de service momenteel een primair, de service van is `StatefulServiceBase.RunAsync()` methode wordt aangeroepen.
4. Nadat alle de replica van de listener `OpenAsync()` roept voltooien en `RunAsync()` wordt aangeroepen, `StatefulServiceBase.OnChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

Net als bij stateless services, er is geen coördinatie tussen de volgorde waarin de listeners zijn gemaakt en geopend en wanneer **RunAsync** wordt genoemd. Als u coördinatie nodig hebt, wordt de oplossingen zijn bijna hetzelfde. Er is één extra case voor stateful service. Stel dat de aanroepen die bij de communicatielisteners binnenkomen vereist voor gegevens die zijn opgeslagen in een paar [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md).

   > [!NOTE]  
   > Omdat de communicatielisteners kunnen openen voordat de betrouwbare verzamelingen zijn kan worden gelezen of geschreven, en vóór **RunAsync** kan worden gestart, enkele aanvullende coördinatie nodig is. De eenvoudigste en meest voorkomende oplossing is voor de communicatielisteners om terug te keren een foutcode die de client gebruikt om te weten de aanvraag opnieuw uit te voeren.

## <a name="stateful-service-shutdown"></a>Afsluiten van de stateful service
Net als stateless services de van levenscyclusgebeurtenissen tijdens het afsluiten zijn gelijk aan die tijdens het opstarten, maar ongedaan gemaakt. Wanneer een stateful service wordt afgesloten, wordt de volgende gebeurtenissen plaats:

1. Parallel:
    - Alle open listeners zijn gesloten. `ICommunicationListener.CloseAsync()` voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `RunAsync()` is geannuleerd. Een controle uit van de token van de annulering `IsCancellationRequested` eigenschap retourneert ' True ', en als ze worden aangeroepen, van het token `ThrowIfCancellationRequested` methode genereert een `OperationCanceledException`.
2. Na `CloseAsync()` is voltooid op elke listener en `RunAsync()` ook is voltooid, van de service `StatefulServiceBase.OnChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

   > [!NOTE]  
   > De noodzaak om te wachten op **RunAsync** om te voltooien is alleen nodig als deze replica een primaire replica is.

3. Na de `StatefulServiceBase.OnChangeRoleAsync()` methode is voltooid, de `StatefulServiceBase.OnCloseAsync()` methode wordt aangeroepen. Deze aanroep is een ongebruikelijk onderdrukking, maar deze beschikbaar is.
3. Na `StatefulServiceBase.OnCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-primary-swaps"></a>Primaire swaps stateful service
Terwijl een stateful service wordt uitgevoerd, alleen de primaire replica's van die stateful services hebben hun communicatielisteners geopend en de bijbehorende **RunAsync** methode met de naam. Secundaire replica's zijn gebouwd, maar er is geen verdere aanroepen weergeven. Terwijl een stateful service wordt uitgevoerd, wordt de replica die momenteel is de primaire kunt wijzigen als gevolg van fouten of optimalisatie balancing-cluster. Wat betekent dit in termen van de van levenscyclusgebeurtenissen die een replica kan zien? Het gedrag van dat de stateful replica ziet, is afhankelijk van de replica wordt gedegradeerd of gepromoveerd tijdens het wisselen.

### <a name="for-the-primary-thats-demoted"></a>Voor de primaire die wordt gedegradeerd
Voor de primaire replica die wordt gedegradeerd, moet de Service Fabric deze replica verwerken van berichten stoppen en afsluiten van de achtergrondwerk bezig. Als gevolg hiervan, lijkt deze stap op wanneer de service wordt afgesloten. Een verschil is dat de service is niet destructed of gesloten omdat er een secundaire blijft. De volgende API's worden genoemd:

1. Parallel:
    - Alle open listeners zijn gesloten. `ICommunicationListener.CloseAsync()` voor elke listener wordt aangeroepen.
    - Het token annulering doorgegeven aan `RunAsync()` is geannuleerd. Een controle uit van de token van de annulering `IsCancellationRequested` eigenschap retourneert ' True ', en als ze worden aangeroepen, van het token `ThrowIfCancellationRequested` methode genereert een `OperationCanceledException`.
2. Na `CloseAsync()` is voltooid op elke listener en `RunAsync()` ook is voltooid, van de service `StatefulServiceBase.OnChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

### <a name="for-the-secondary-thats-promoted"></a>Voor de secundaire server die wordt gepromoveerd
Service Fabric moet op dezelfde manier de secundaire replica die wordt gepromoveerd als u wilt beginnen met het luisteren naar berichten op de kabel en alle achtergrondtaken nodig om te voltooien. Als gevolg hiervan lijkt dit proces op wanneer de service is gemaakt, behalve dat de replica zelf al bestaat. De volgende API's worden genoemd:

1. Parallel:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` wordt aangeroepen en een geretourneerd listeners zijn geopend. `ICommunicationListener.OpenAsync()` voor elke listener wordt aangeroepen.
    - Van de service `StatefulServiceBase.RunAsync()` methode wordt aangeroepen.
2. Nadat alle de replica van de listener `OpenAsync()` roept voltooien en `RunAsync()` wordt aangeroepen, `StatefulServiceBase.OnChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Veelvoorkomende problemen tijdens het afsluiten van de stateful service en de primaire degradatie
Service Fabric wordt de primaire van een stateful service voor een aantal redenen gewijzigd. De meest voorkomende reden zijn [cluster herverdeling](service-fabric-cluster-resource-manager-balancing.md) en [toepassingsupgrade](service-fabric-application-upgrade.md). Tijdens deze bewerkingen (en tijdens het afsluiten van de normale service, zoals u ziet als de service is verwijderd), is het belangrijk dat de service aansluiten bij de `CancellationToken`. 

Services die annulering niet correct verwerken kunnen verschillende problemen optreden. Deze bewerkingen zijn traag, omdat de Service Fabric wacht tot de services te stoppen zonder problemen. Uiteindelijk kan dit leiden tot mislukte upgrades die time-out en terug te draaien. Fout in acht neemt de token van de annulering kan ook leiden tot imbalanced clusters. Clusters niet in evenwicht zijn omdat knooppunten ' hot opvragen ', maar de services kunnen niet worden uitgevoerd omdat het duurt te lang is om ze te verplaatsen elders. 

Omdat de services stateful zijn, het is ook waarschijnlijk dat ze gebruiken de [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). In Service Fabric, wanneer een primaire wordt gedegradeerd, is een van de eerste dingen die plaatsvindt dat toegang voor schrijven naar de status van de onderliggende is ingetrokken. Dit leidt tot een tweede set met problemen die invloed kunnen zijn op de levenscyclus van de service. De verzamelingen geretourneerde uitzonderingen op basis van de timing en of de replica wordt verplaatst of afsluiten. Deze uitzonderingen moeten correct worden verwerkt. Uitzonderingen die door Service Fabric kunnen worden onderverdeeld in permanente [(`FabricException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) en tijdelijke [(`FabricTransientException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) categorieën. Permanente uitzonderingen moeten worden geregistreerd en opgetreden bij de tijdelijke uitzonderingen kunnen worden herhaald op basis van bepaalde logica voor opnieuw proberen.

Verwerken van de uitzonderingen die afkomstig van het gebruik van zijn de `ReliableCollections` in combinatie met gebeurtenissen van de levenscyclus van service is een belangrijk onderdeel van het testen en valideren van een betrouwbare Service. Het is raadzaam dat u uw service onder belasting altijd uitgevoerd tijdens het uitvoeren van upgrades en [chaos testen](service-fabric-controlled-chaos.md) voordat het in productie wilt nemen. Deze eenvoudige stappen kunt u ervoor te zorgen dat uw service correct is geïmplementeerd en gebeurtenissen in de levensduur correct worden verwerkt.


## <a name="notes-on-the-service-lifecycle"></a>Opmerkingen bij de levenscyclus van de service
  - Zowel de `RunAsync()` methode en de `CreateServiceReplicaListeners/CreateServiceInstanceListeners` aanroepen zijn optioneel. Een service kan een van deze, beide of geen hebben. Bijvoorbeeld, als de service al het werk in reactie op gebruiker-aanroepen is, er is niet nodig voor het implementeren van `RunAsync()`. Alleen de communicatielisteners en hun bijbehorende code zijn nodig. Op dezelfde manier het maken van en het retourneren van communicatielisteners is optioneel, als de service kan alleen achtergrondwerk te doen, en dus alleen behoeften voor het implementeren van `RunAsync()`.
  - De parameter is geldig voor een service om te voltooien `RunAsync()` is en het resultaat van deze. Voltooien is niet een foutomstandigheid. Voltooien van `RunAsync()` geeft aan dat het achtergrondwerk van de service is voltooid. Voor stateful reliable services moet `RunAsync()` opnieuw wordt aangeroepen als de replica is gedegradeerd van primaire naar secundaire en vervolgens terug naar de primaire gepromoveerd.
  - Als een service wordt afgesloten van `RunAsync()` door een onverwachte uitzondering, vormt dit een fout. Het service-object wordt afgesloten en er wordt een statusfout gerapporteerd.
  - Hoewel er geen beperking op het retourneren van deze methoden, moet u onmiddellijk de kans om te schrijven naar betrouwbare verzamelingen en daarom een echte werk niet voltooien. Het is raadzaam dat u zo snel mogelijk na ontvangst van de aanvraag voor annulering retourneren. Als uw service niet op deze API-aanroepen in een redelijk tijdsbestek reageert, kunt uw service geforceerd beëindigen door Service Fabric. Meestal gebeurt dit alleen tijdens upgrades van toepassingen of wanneer een service wordt verwijderd. Deze time-out is 15 minuten standaard.
  - Fouten in de `OnCloseAsync()` pad leiden `OnAbort()` die wordt aangeroepen, dit is een kans van de laatste kans best-effort voor de service op te schonen en de vrijgave van alle resources die ze hebben aangevraagd. Dit wordt meestal wanneer een permanente storing wordt gedetecteerd op het knooppunt, of wanneer de Service Fabric kan niet op betrouwbare wijze beheren van het service-exemplaar levensduur vanwege interne fouten genoemd.
  - `OnChangeRoleAsync()` wordt aangeroepen wanneer de stateful service-replica bijvoorbeeld rol in de primaire of secundaire wijzigen is. Primaire replica's krijgen schrijven (toegestaan om te maken en te schrijven naar betrouwbare verzamelingen). Secundaire replica's worden gelezen status (kan alleen worden gelezen uit bestaande betrouwbare verzamelingen) vermeld. De meeste werk in een stateful service wordt uitgevoerd op de primaire replica. Secundaire replica's kunnen uitvoeren voor validatie van alleen-lezen, rapport, gegevensanalyse of andere taken alleen-lezen.

## <a name="next-steps"></a>Volgende stappen
- [Kennismaking met betrouwbare Services](service-fabric-reliable-services-introduction.md)
- [Snel starten met betrouwbare Services](service-fabric-reliable-services-quick-start.md)
- [Replica's en exemplaren](service-fabric-concepts-replica-lifecycle.md)
