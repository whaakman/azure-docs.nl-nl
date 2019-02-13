---
title: Overzicht van het programmeermodel van betrouwbare Service Fabric-Service | Microsoft Docs
description: Meer informatie over Reliable Service van Service Fabric-programmeermodel en aan de slag schrijven van uw eigen services.
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek; mani-ramaswamy
ms.assetid: 0c88a533-73f8-4ae1-a939-67d17456ac06
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: masnider
ms.openlocfilehash: bc4e4c9137003dad9cfd96772b7ebe231cd6eace
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171720"
---
# <a name="reliable-services-overview"></a>Overzicht van Reliable Services
Azure Service Fabric wordt het schrijven en beheren van staatloze en stateful Reliable Services. In dit onderwerp bevat informatie over:

* Het programmeermodel van Reliable Services voor staatloze en stateful services.
* De keuzes die u hebt om bij het schrijven van een betrouwbare Service.
* Sommige scenario's en voorbeelden van het gebruik van Reliable Services en hoe ze zijn geschreven.

Reliable Services is een van de programmeermodellen die beschikbaar zijn in Service Fabric. De andere is het programmeermodel Reliable Actor, waarmee u een virtuele Actor-programmeermodel boven op het Reliable Services-model. Zie voor meer informatie over het programmeermodel van Reliable Actors [Inleiding tot Service Fabric Reliable Actors](service-fabric-reliable-actors-introduction.md).

Service Fabric beheert de levensduur van services, van inrichting en implementatie via bijwerken en verwijderen, via [Service Fabric-Toepassingsbeheer](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Wat zijn Reliable Services?
Betrouwbare Services biedt u een eenvoudige, krachtige, op het hoogste niveau programmeermodel om u te helpen u express wat belangrijk is voor uw toepassing is. Met het programmeermodel voor Reliable Services, profiteert u van:

* Toegang tot de rest van de Service Fabric-programmeermodellen API's. In tegenstelling tot Service Fabric-Services is gemodelleerd als [Gastbestanden](service-fabric-guest-executables-introduction.md), Reliable Services ophalen voor rechtstreeks gebruik van de rest van de Service Fabric-API's. Dit kan services:
  * query op het systeem
  * de health rapport over entiteiten in het cluster
  * meldingen ontvangen over wijzigingen van configuratie en de code
  * zoeken en communiceren met andere services
  * (optioneel) gebruik van de [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)
  * .. .en zodat ze toegang hebben tot veel andere mogelijkheden, geheel vanuit een eersteklas programmeermodel in diverse programmeertalen.
* Een eenvoudig model voor het uitvoeren van uw eigen code die lijkt op programmeermodellen u die gewend bent. Uw code is een goed gedefinieerde toegangspunt en de levenscyclus van eenvoudig te beheren.
* Een communicatiemodel pluggable. Gebruik het transport van uw keuze, zoals HTTP met [Web-API](service-fabric-reliable-services-communication-webapi.md), WebSockets, aangepaste TCP-protocollen of iets anders. Reliable Services bieden een uitstekende out-of-the-box-opties die u kunt gebruiken, of u kunt uw eigen.
* Voor stateful services, het programmeermodel van Reliable Services kunt u voor het opslaan van uw staat rechtstreeks in uw service met behulp van consistent en betrouwbaar [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). Betrouwbare verzamelingen zijn een eenvoudige set van maximaal beschikbare en betrouwbare verzamelingsklassen die bekend bij iedereen die is gebruikt C# verzamelingen. Services nodig traditioneel externe systemen voor het statusbeheer van betrouwbare. Met betrouwbare verzamelingen, kunt u uw staat naast uw compute opslaan met de dezelfde hoge beschikbaarheid en betrouwbaarheid die u hebt gaan verwachten van maximaal beschikbare externe winkels. Dit model worden de latentie ook verbeterd omdat u dezelfde zoeken naar de reken- en staat nodig is om te werken.

## <a name="what-makes-reliable-services-different"></a>Wat is het verschil Reliable Services?
Betrouwbare Services in Service Fabric verschillen van de services die u hebt gemaakt voordat u. Service Fabric biedt betrouwbaarheid, beschikbaarheid, consistentie en schaalbaarheid.

* **Betrouwbaarheid** - de service blijft u zelfs in omgevingen met onbetrouwbare waar uw machines mislukken of druk op netwerkproblemen of in gevallen waarin de services zelf fouten en crashes voorkomen of mislukt. Voor stateful services de status blijft behouden, zelfs bij netwerk- of andere fouten.
* **Beschikbaarheid** -uw-service is bereikbaar is en reageert. Service Fabric houdt het gewenste aantal actieve exemplaren.
* **Schaalbaarheid** - Services zijn ontkoppeld van specifieke hardware, en ze kunnen groeien of krimpen zo nodig tot en met het toevoegen of verwijderen van de hardware of andere bronnen. Services eenvoudig (met name in de stateful geval) worden gepartitioneerd om ervoor te zorgen dat de service kan worden geschaald en gedeeltelijke fouten afhandelen. Services kunnen worden gemaakt en verwijderd dynamisch via code, waardoor meer exemplaren van ingezette indien nodig, bijvoorbeeld in reactie op aanvragen van klanten. Ten slotte moedigt Service Fabric services moet lichtgewicht. Service Fabric kunt duizenden services in te richten in een enkel proces, in plaats van dat of uitsluitend volledige OS-exemplaren of processen naar één exemplaar van een service.
* **Consistentie** -alle informatie die zijn opgeslagen in deze service kan worden gegarandeerd consistent. Dit geldt zelfs in meerdere betrouwbare verzamelingen in een service. Wijzigingen in verzamelingen in een service kunnen worden gemaakt in een transactioneel atomic manier.

## <a name="service-lifecycle"></a>Levenscyclus van de service
Of uw service stateful of stateless is, bieden Reliable Services de levensduur van een eenvoudige, waarmee u snel plug-in uw code en aan de slag.  Er zijn maar een of twee methoden die u nodig hebt om uw service operationeel te implementeren.

* **CreateServiceReplicaListeners/CreateServiceInstanceListeners** -deze methode is waar de service definieert voor de communicatie stack(s) die het bedrijf wil gebruiken. De communicatie-stack, zoals [Web-API](service-fabric-reliable-services-communication-webapi.md), wordt bepaald de luisterende eindpunt of de eindpunten voor de service (hoe clients bereiken voor de service). Het definieert ook de interactie van de berichten die worden weergegeven met de rest van de servicecode.
* **RunAsync** -deze methode is waarop de bedrijfslogica in uw service wordt uitgevoerd en waarbij deze zou een vliegende start alle achtergrondtaken die moeten worden uitgevoerd voor de levensduur van de service. De annulering-token dat is opgegeven, is een signaal voor wanneer dat werk moet worden stopgezet. Bijvoorbeeld, als de service nodig heeft voor het ophalen van berichten uit een wachtrij voor betrouwbare en ze te verwerken, is dit waar dat werk plaatsvindt.

Als u over betrouwbare services voor de eerste keer leren wilt, lees dan verder! Als u een gedetailleerd overzicht van de levenscyclus van reliable services zoekt, gaat u naar [in dit artikel](service-fabric-reliable-services-lifecycle.md).

## <a name="example-services"></a>Voorbeeld van de services
Wetenschap dat deze programmeermodel, eens een beknopt overzicht van twee verschillende services om te zien hoe deze onderdelen in elkaar passen.

### <a name="stateless-reliable-services"></a>Stateless Reliable Services
Een stateless service is waar er is geen status die wordt onderhouden in de service tussen aanroepen. Elke status die aanwezig is volledig beschikbaar is en vereist geen synchronisatie, replicatie, persistentie of hoge beschikbaarheid.

Neem bijvoorbeeld een rekenmachine die geen geheugen heeft en alle voorwaarden en bewerkingen moeten worden uitgevoerd in één keer ontvangt.

In dit geval de `RunAsync()` (C#) of `runAsync()` (Java) van de service kan niet leeg zijn, omdat er geen taak-verwerking op de achtergrond die de service moet doen. Wanneer de Rekenmachine-service is gemaakt, wordt een `ICommunicationListener` (C#) of `CommunicationListener` (Java) (bijvoorbeeld [Web-API](service-fabric-reliable-services-communication-webapi.md)) die wordt het eindpunt van een luistert op sommige poort geopend. Dit eindpunt luisteren hooks omhoog in de van de verschillende berekeningsmethoden (voorbeeld: 'Toevoegen (n1, n2)') die van de Rekenmachine openbare API definiëren.

Wanneer er wordt een aanroep uitgevoerd vanaf een client, de juiste methode wordt aangeroepen en de Rekenmachine-service voert de bewerkingen op de gegevens die zijn opgegeven en retourneert het resultaat. Elke status worden niet opgeslagen.

Een interne status niet opslaan is in dit voorbeeld van de Rekenmachine eenvoudig. Maar de meeste services zijn niet echt stateless. In plaats daarvan extern ze hun status naar een andere store. (Bijvoorbeeld een web-app die is gebaseerd op de status van de sessie bijhouden in een back-ups archief of cache is niet stateless.)

Een algemeen voorbeeld van hoe de stateless services worden gebruikt Service Fabric is als een front-end dat de openbare API voor een web-App beschikbaar stelt. De front-end-service wordt vervolgens praat met stateful services om de gebruikersaanvraag van een te voltooien. In dit geval worden aanroepen van clients omgeleid naar een bekende poort, zoals 80, waar de stateless service luistert. Deze stateless service ontvangt de oproep en bepaalt u of de oproep afkomstig is van een vertrouwde partij en welke service het bestemd is voor.  De stateless service wordt vervolgens de aanroep doorstuurt naar de juiste partitie van de stateful service en wachten op antwoord. Wanneer de stateless service een antwoord ontvangt, wordt geantwoord naar de oorspronkelijke client. Een voorbeeld van een dergelijke service is in onze voorbeelden [ C# ](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)  /  [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started). Dit is slechts één voorbeeld van dit patroon in de voorbeelden, anderen er zijn in andere voorbeelden ook.

### <a name="stateful-reliable-services"></a>Stateful Reliable Services
Een stateful service is een die over een gedeelte van de status consistent en aanwezig zijn in volgorde van de service blijven functioneren beschikken. Houd rekening met een service die voortdurend berekent een oplopende gemiddelde van een waarde op basis van updates die wordt ontvangen. U doet dit door moet de huidige set van inkomende aanvragen verwerken en de huidige gemiddelde moet hebben. Elke service die wordt opgehaald, verwerkt en worden gegevens opgeslagen in een externe opslag (zoals een Azure blob of table store vandaag) is stateful. Alleen houdt u de status in het externe gegevensarchief.

De meeste services opslaan vandaag nog de status extern, omdat de externe opslag is wat de betrouwbaarheid, beschikbaarheid, schaalbaarheid en consistentie voor die staat biedt. Services worden niet in Service Fabric vereist voor het opslaan van hun status extern. Service Fabric zorgt dat deze vereisten voor zowel de code van de status van de service.

Stel dat we willen schrijven van een service waarmee afbeeldingen worden verwerkt. U doet dit door maakt de service in een afbeelding en de reeks conversies om uit te voeren op die installatiekopie. Deze service retourneert een communicatielistener (we Stel dit is een WebAPI) dat toont een API, zoals `ConvertImage(Image i, IList<Conversion> conversions)`. Wanneer een aanvraag wordt ontvangen, de service slaat deze op in een `IReliableQueue`, en sommige id geretourneerd naar de client zodat deze de aanvraag kunt bijhouden.

In deze service `RunAsync()` kan complexer worden. De service heeft een lus binnen de `RunAsync()` die aanvragen van haalt `IReliableQueue` en voert de gevraagde omzettingen. De resultaten ophalen die zijn opgeslagen in een `IReliableDictionary` zodat wanneer de client terugkomen ze geconverteerde afbeeldingen kunnen krijgen. Om ervoor te zorgen dat zelfs als er een storing in de afbeelding optreedt niet verloren gaan, deze betrouwbare Service zou ophalen uit de wachtrij, de-conversies uitvoeren en sla het resultaat allemaal op één transactie. In dit geval wordt het bericht is verwijderd uit de wachtrij en de resultaten worden opgeslagen in de woordenlijst resultaat alleen wanneer de conversies voltooid zijn. De service kan ook, haal de installatiekopie uit de wachtrij en onmiddellijk opslaan in een externe opslag. Dit vermindert het bedrag van de status van die de service heeft voor het beheren van, maar de complexiteit toeneemt nadat de service heeft dat de vereiste metagegevens voor het beheren van de externe opslag. Met beide benaderingen als iets verkeerd gegaan in het midden is blijft de aanvraag in de wachtrij om te worden verwerkt.

Eén ding te weten over deze service is dat je zou, een normale .NET-service denken. Het enige verschil is dat de gegevensstructuren die wordt gebruikt (`IReliableQueue` en `IReliableDictionary`) worden geleverd door de Service Fabric en uiterst betrouwbare, beschikbaar en consistent.

## <a name="when-to-use-reliable-services-apis"></a>Wanneer u betrouwbare Services-API 's
Als een van de volgende de behoeften van uw toepassing service kenmerkt, moet u Reliable Services-API's overwegen:

* U wilt dat de code van uw service (en eventueel status) zijn zeer beschikbare en betrouwbare
* Moet u transactionele garanties voor meerdere eenheden van status (bijvoorbeeld orders en regelitems volgorde).
* De status van uw toepassing kan op een natuurlijke manier worden gemodelleerd als betrouwbare woordenboeken en wachtrijen.
* Uw toepassingen, code of de status moet maximaal beschikbaar met lage latentie lees- en schrijfbewerkingen.
* De toepassing nodig heeft voor het beheren van de gelijktijdigheid of de granulatie van transactionele bewerkingen in een of meer betrouwbare verzamelingen.
* Wilt u beheren welke berichten of het partitioneringsschema voor uw service beheren.
* Uw code moet een gratis-threaded runtime-omgeving.
* Uw toepassing nodig heeft voor het dynamisch maken of vernietigen betrouwbare woordenlijsten of wachtrijen of hele Services tijdens runtime.
* U moet programmatisch beheren van de opgegeven Service Fabric-back-up en herstellen van de functies voor de status van uw service.
* Uw toepassing moet wijzigingsoverzicht voor de eenheden van de status behouden.
* U wilt ontwikkelen of derde partij ontwikkeld, aangepaste statusproviders in beslag nemen.

## <a name="next-steps"></a>Volgende stappen
* [Snel starten met betrouwbare Services](service-fabric-reliable-services-quick-start.md)
* [Betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)
* [Het model voor Reliable Actors](service-fabric-reliable-actors-introduction.md)
