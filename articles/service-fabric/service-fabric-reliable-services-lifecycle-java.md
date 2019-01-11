---
title: Levenscyclus van de Azure Service Fabric Reliable Services | Microsoft Docs
description: Meer informatie over de van levenscyclusgebeurtenissen in Service Fabric Reliable Services.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 3254b29ed380b526be6d5fe5f671adeccbd8ea46
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54196702"
---
# <a name="reliable-services-lifecycle"></a>Levenscyclus van Reliable Services
> [!div class="op_single_selector"]
> * [C# op Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java op Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Reliable Services is een van de programmeermodellen die beschikbaar zijn in Azure Service Fabric. Bij het leren over de levenscyclus van Reliable Services, is het meest belangrijk om te begrijpen van de van basic-levenscyclusgebeurtenissen. De exacte volgorde van gebeurtenissen, is afhankelijk van informatie over de configuratie. 

In het algemeen bevat de levenscyclus van Reliable Services de volgende gebeurtenissen:

* Tijdens het opstarten:
  * Services zijn samengesteld.
  * Services hebben een kans om te bouwen en nul of meer listeners retourneren.
  * Elke geretourneerde listeners zijn geopend voor communicatie met de service.
  * Van de service `runAsync` methode wordt aangeroepen, zodat de service kunt langlopende doen of werk op de achtergrond.
* Tijdens het afsluiten:
  * De annulering-token dat is doorgegeven aan `runAsync` wordt geannuleerd, en de listeners zijn gesloten.
  * Het serviceobject zelf is destructed.

De volgorde van gebeurtenissen in Reliable Services kan enigszins afwijken afhankelijk van of de betrouwbare service stateless of stateful is. 

Voor stateful services, moet u ook het wisselen van primaire scenario oplossen. Tijdens deze volgorde hebben, de rol van de primaire wordt overgedragen naar een andere replica (of terugkomen) zonder dat de service wordt afgesloten. 

Ten slotte hebt om na te denken over de fout of fout voorwaarden.

## <a name="stateless-service-startup"></a>Starten van de stateless service
De levenscyclus van een staatloze service is redelijk eenvoudig. Dit is de volgorde van gebeurtenissen:

1. De service is gebouwd.
2. Deze gebeurtenissen optreden parallel:
    - `StatelessService.createServiceInstanceListeners()` wordt aangeroepen, en een geretourneerd listeners zijn geopend. `CommunicationListener.openAsync()` voor elke listener wordt aangeroepen.
    - Van de service `runAsync` methode (`StatelessService.runAsync()`) wordt genoemd.
3. Indien aanwezig, van de service zelf `onOpenAsync` methode wordt aangeroepen. Met name `StatelessService.onOpenAsync()` wordt genoemd. Dit is een ongebruikelijk onderdrukking, maar deze beschikbaar is.

Het is belangrijk te weten dat er geen ordening tussen de aanroep is te maken en de listeners te openen en de aanroep van `runAsync`. De listeners kunnen openen voordat `runAsync` wordt gestart. Op deze manier `runAsync` kan worden aangeroepen voordat de communicatielisteners zijn geopend, of ze zelfs is gebouwd. Als geen synchronisatie vereist is, moet deze worden uitgevoerd door de implementeerder. Hier volgen enkele algemene oplossingen:

* Soms werkt listeners alleen als andere gegevens is gemaakt of andere werk wordt uitgevoerd. Voor stateless services, die meestal kan gebruik worden gemaakt in de constructor van de service. Het kan worden uitgevoerd tijdens de `createServiceInstanceListeners()` aanroepen, of als onderdeel van het samenstellen van de listener zelf.
* Soms de code in `runAsync` pas gestart nadat de listeners geopend zijn. In dit geval is extra coördinatie nodig. Een gangbare oplossing is het toevoegen van een markering in de listeners. De vlag geeft aan wanneer de listeners zijn voltooid. De `runAsync` methode controleert dit voordat u doorgaat met het echte werk.

## <a name="stateless-service-shutdown"></a>Afsluiten van de stateless service
Als een stateless service wordt afgesloten, wordt hetzelfde patroon wordt gevolgd, maar in omgekeerde volgorde:

1. Deze gebeurtenissen optreden parallel:
    - Alle open listeners zijn gesloten. `CommunicationListener.closeAsync()` voor elke listener wordt aangeroepen.
    - De annulering-token dat is doorgegeven aan `runAsync()` is geannuleerd. Controleren van de token van de annulering `isCancelled` resultaat van de eigenschap `true`, en als ze worden aangeroepen, van het token `throwIfCancellationRequested` methode genereert een `CancellationException`.
2. Wanneer `closeAsync()` is voltooid op elke listener en `runAsync()` ook is voltooid, van de service `StatelessService.onCloseAsync()` methode wordt aangeroepen, indien aanwezig. Nogmaals, dit is geen gemeenschappelijke override, maar deze kan worden gebruikt om veilig bronnen sluiten, verwerking op de achtergrond te stoppen, voltooien van de externe status is opgeslagen of bestaande verbindingen afgesloten.
3. Na `StatelessService.onCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-startup"></a>Starten van de stateful service
Stateful services hebben een patroon dat vergelijkbaar is met stateless services, met een aantal wijzigingen.  Dit is de volgorde van gebeurtenissen voor het starten van een stateful service:

1. De service is gebouwd.
2. `StatefulServiceBase.onOpenAsync()` wordt aangeroepen. Deze aanroep is niet vaak worden genegeerd in de service.
3. Deze gebeurtenissen optreden parallel:
    - `StatefulServiceBase.createServiceReplicaListeners()` wordt aangeroepen. 
      - Als de service een primaire service is, worden alle geretourneerde listeners geopend. `CommunicationListener.openAsync()` voor elke listener wordt aangeroepen.
      - Als de service een secundaire service is, alleen listeners gemarkeerd als `listenOnSecondary = true` worden geopend. Met listeners die geopend op de secundaire replica's zijn is het minder gebruikelijk.
    - Als de service momenteel een primair, de service van is `StatefulServiceBase.runAsync()` methode wordt aangeroepen.
4. Nadat alle de replica van de listener `openAsync()` roept voltooien en `runAsync()` wordt aangeroepen, `StatefulServiceBase.onChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

Vergelijkbaar met stateless services in de stateful service, er is geen coördinatie tussen de volgorde waarin de listeners zijn gemaakt en geopend en wanneer `runAsync` wordt genoemd. Als u coördinatie nodig hebt, wordt de oplossingen zijn bijna hetzelfde. Maar er is één extra case voor stateful service. Stel dat de aanroepen die bij de communicatielisteners binnenkomen vereist voor gegevens die zijn opgeslagen in een paar [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). Omdat de communicatielisteners mogelijk openen voordat de betrouwbare verzamelingen zijn kan worden gelezen of geschreven, en vóór `runAsync` wordt gestart, enkele aanvullende coördinatie nodig is. De eenvoudigste en meest voorkomende oplossing is voor de communicatielisteners om terug te keren een foutcode. De client gebruikt de foutcode om te weten de aanvraag opnieuw uit te voeren.

## <a name="stateful-service-shutdown"></a>Afsluiten van de stateful service
Net als stateless services de van levenscyclusgebeurtenissen tijdens het afsluiten zijn gelijk aan die tijdens het opstarten, maar ongedaan gemaakt. Wanneer een stateful service wordt afgesloten, wordt de volgende gebeurtenissen plaats:

1. Deze gebeurtenissen optreden parallel:
    - Alle open listeners zijn gesloten. `CommunicationListener.closeAsync()` voor elke listener wordt aangeroepen.
    - De annulering-token dat is doorgegeven aan `runAsync()` is geannuleerd. Een aanroep van de token van de annulering `isCancelled()` methode retourneert `true`, en als ze worden aangeroepen, van het token `throwIfCancellationRequested()` methode genereert een `OperationCanceledException`.
2. Na `closeAsync()` is voltooid op elke listener en `runAsync()` ook is voltooid, van de service `StatefulServiceBase.onChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

   > [!NOTE]  
   > Wachten op `runAsync` om te voltooien is alleen nodig als deze replica een primaire replica is.

3. Na de `StatefulServiceBase.onChangeRoleAsync()` methode is voltooid, de `StatefulServiceBase.onCloseAsync()` methode wordt aangeroepen. Deze aanroep is een ongebruikelijk onderdrukking, maar deze beschikbaar is.
3. Na `StatefulServiceBase.onCloseAsync()` is voltooid, het service-object is destructed.

## <a name="stateful-service-primary-swaps"></a>Primaire swaps stateful service
Terwijl een stateful service wordt uitgevoerd, communicatielisteners worden geopend en de `runAsync` methode wordt aangeroepen voor de primaire replica's van die stateful services. Secundaire replica's zijn gebouwd, maar er is geen verdere aanroepen weergeven. Terwijl een stateful service wordt uitgevoerd, de replica die momenteel is de primaire kunt wijzigen. De van levenscyclusgebeurtenissen die een stateful replica kunt zien, is afhankelijk van de replica wordt gedegradeerd of gepromoveerd tijdens het wisselen.

### <a name="for-the-demoted-primary"></a>Voor de gedegradeerde primaire
Service Fabric moet de primaire replica die wordt gedegradeerd voor het verwerken van berichten te stoppen en achtergrondwerk stoppen. Deze stap is vergelijkbaar met wanneer de service wordt afgesloten. Een verschil is dat de service is niet destructed of afgesloten, omdat deze als een secundaire blijft. De volgende gebeurtenissen vinden plaats:

1. Deze gebeurtenissen optreden parallel:
    - Alle open listeners zijn gesloten. `CommunicationListener.closeAsync()` voor elke listener wordt aangeroepen.
    - De annulering-token dat is doorgegeven aan `runAsync()` is geannuleerd. Een controle uit van de token van de annulering `isCancelled()` methode retourneert `true`. Als ze worden aangeroepen, van het token `throwIfCancellationRequested()` methode genereert een `OperationCanceledException`.
2. Na `closeAsync()` is voltooid op elke listener en `runAsync()` ook is voltooid, van de service `StatefulServiceBase.onChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

### <a name="for-the-promoted-secondary"></a>Voor de gepromoveerde secundaire
Service Fabric moet op dezelfde manier de secundaire replica die wordt gepromoveerd om te luisteren naar berichten op de kabel starten en alle achtergrondtaken die nodig is om te voltooien. Dit proces is vergelijkbaar met wanneer de service is gemaakt. Het verschil is dat de replica zelf al bestaat. De volgende gebeurtenissen vinden plaats:

1. Deze gebeurtenissen optreden parallel:
    - `StatefulServiceBase.createServiceReplicaListeners()` wordt aangeroepen en een geretourneerd listeners zijn geopend. `CommunicationListener.openAsync()` voor elke listener wordt aangeroepen.
    - Van de service `StatefulServiceBase.runAsync()` methode wordt aangeroepen.
2. Nadat alle de replica van de listener `openAsync()` roept voltooien en `runAsync()` wordt aangeroepen, `StatefulServiceBase.onChangeRoleAsync()` wordt genoemd. Deze aanroep is niet vaak worden genegeerd in de service.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Veelvoorkomende problemen tijdens het afsluiten van de stateful service en de primaire degradatie
Service Fabric verandert de primaire van een stateful service om verschillende redenen. De meest voorkomende oorzaken zijn [cluster herverdeling](service-fabric-cluster-resource-manager-balancing.md) en [toepassingsupgrade](service-fabric-application-upgrade.md). Tijdens deze bewerkingen is het belangrijk dat de service respecteert de `cancellationToken`. Dit geldt ook tijdens het afsluiten van de normale service, bijvoorbeeld als de service is verwijderd.

Services die annulering niet correct verwerken kunnen verschillende problemen optreden. Deze bewerkingen zijn traag, omdat de Service Fabric wacht tot de services te stoppen zonder problemen. Dit kan uiteindelijk leiden tot mislukte upgrades die time-out en het ongedaan maken. Fout bij het token annulering in acht neemt ook kan leiden tot imbalanced clusters. Clusters niet in evenwicht zijn omdat knooppunten ' hot opvragen '. Echter, de services kunnen niet worden uitgevoerd omdat het duurt te lang is om ze te verplaatsen elders. 

Omdat de services stateful zijn, het is ook waarschijnlijk dat de services gebruiken [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). In Service Fabric, wanneer een primaire wordt gedegradeerd, is een van de eerste dingen die plaatsvindt dat toegang voor schrijven naar de status van de onderliggende is ingetrokken. Dit leidt tot een tweede set met problemen die invloed kunnen hebben op de levenscyclus van de service. De verzamelingen geretourneerde uitzonderingen op basis van de timing en of de replica wordt verplaatst of afsluiten. Het is belangrijk dat u deze uitzonderingen correct worden verwerkt. 

Uitzonderingen die door Service Fabric zijn permanente [(`FabricException`)](https://docs.microsoft.com/java/api/system.fabric.exception) of tijdelijke [(`FabricTransientException`)](https://docs.microsoft.com/java/api/system.fabric.exception.fabrictransientexception). Permanente uitzonderingen moeten worden geregistreerd en worden gegenereerd. Tijdelijke uitzonderingen kunnen opnieuw worden uitgevoerd op basis van de logica voor opnieuw proberen.

Een belangrijk onderdeel van het testen en valideren van Reliable Services verwerkt de uitzonderingen die afkomstig uit met behulp van zijn de `ReliableCollections` in combinatie met gebeurtenissen van levenscyclus van de service. Het wordt aangeraden om uw service onder belasting altijd uit te voeren. U moet ook upgrades uitvoeren en [chaos testen](service-fabric-controlled-chaos.md) voordat het in productie wilt nemen. Deze eenvoudige stappen kunt u ervoor te zorgen dat uw service juist is geïmplementeerd, en dat deze gebeurtenissen in de levensduur correct worden verwerkt.

## <a name="notes-on-service-lifecycle"></a>Opmerkingen bij de levenscyclus van de service
* Zowel de `runAsync()` methode en de `createServiceInstanceListeners/createServiceReplicaListeners` aanroepen zijn optioneel. Een service mogelijk een, beide of geen van beide. Bijvoorbeeld, als de service al het werk in reactie op gebruiker-aanroepen is, er is niet nodig voor het implementeren van `runAsync()`. Alleen de communicatielisteners en hun bijbehorende code zijn nodig.  Op dezelfde manier is het maken van en het retourneren van communicatielisteners optioneel. De service mogelijk alleen achtergrondwerk te doen, dus hij alleen moet voor het implementeren van `runAsync()`.
* De parameter is geldig voor een service om te voltooien `runAsync()` is en het resultaat van deze. Dit wordt niet beschouwd als een foutomstandigheid. Hiermee geeft u het achtergrondwerk van de service is voltooid. Voor stateful Reliable Services `runAsync()` zou worden opnieuw wordt aangeroepen als de service wordt gedegradeerd van primaire en vervolgens terug naar de primaire gepromoveerd.
* Als een service wordt afgesloten van `runAsync()` door een onverwachte uitzondering, dit is een fout. Het service-object wordt afgesloten en er wordt een statusfout gerapporteerd.
* Hoewel er geen beperking op het retourneren van deze methoden, verliest u onmiddellijk de mogelijkheid om te schrijven. U kunt daarom geen echte werk niet voltooien. Het is raadzaam dat u zo snel mogelijk na ontvangst van de aanvraag voor annulering retourneren. Als uw service niet op deze API-aanroepen in een redelijk tijdsbestek reageert, mogelijk uw service geforceerd beëindigen door Service Fabric. Dit gebeurt normaal gesproken alleen tijdens upgrades van toepassingen of wanneer een service wordt verwijderd. Deze time-out is 15 minuten standaard.
* Fouten in de `onCloseAsync()` pad leiden `onAbort()` die wordt aangeroepen. Deze aanroep is een laatste kans, best-effort kans voor de service op te schonen en de vrijgave van alle resources die ze hebben aangevraagd. Dit wordt meestal wanneer een permanente storing wordt gedetecteerd op het knooppunt, of wanneer de Service Fabric kan niet op betrouwbare wijze beheren van het service-exemplaar levensduur vanwege interne fouten genoemd.
* `OnChangeRoleAsync()` wordt aangeroepen wanneer de stateful service-replica bijvoorbeeld rol in de primaire of secundaire wijzigen is. Primaire replica's krijgen schrijven (toegestaan om te maken en te schrijven naar betrouwbare verzamelingen). Secundaire replica's worden gelezen status (kan alleen worden gelezen uit bestaande betrouwbare verzamelingen) vermeld. De meeste werk in een stateful service wordt uitgevoerd op de primaire replica. Secundaire replica's kunnen uitvoeren voor validatie van alleen-lezen, rapport, gegevensanalyse of andere taken alleen-lezen.

## <a name="next-steps"></a>Volgende stappen
* [Kennismaking met betrouwbare Services](service-fabric-reliable-services-introduction.md)
* [Snelstartgids voor betrouwbare Services](service-fabric-reliable-services-quick-start-java.md)

