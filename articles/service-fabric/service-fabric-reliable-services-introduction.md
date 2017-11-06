---
title: Overzicht van het betrouwbare Service Fabric-Service-programmeermodel | Microsoft Docs
description: Informatie over Service Fabric van betrouwbare Service programmeermodel en schrijven van uw eigen services aan de slag.
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek; mani-ramaswamy
ms.assetid: 0c88a533-73f8-4ae1-a939-67d17456ac06
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: masnider;
ms.openlocfilehash: 601b1c7713c9785d949c1c72000ec7f3f63dd682
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="reliable-services-overview"></a>Overzicht van Reliable Services
Azure Service Fabric vereenvoudigt schrijven en staatloze en stateful Reliable Services beheren. Dit onderwerp wordt beschreven:

* Het Reliable Services programmeermodel voor staatloze en stateful services.
* De keuzes die u maken moet bij het schrijven van een betrouwbare Service.
* Sommige scenario's en voorbeelden van het gebruik van Reliable Services en hoe ze zijn geschreven.

Reliable Services is een van de programmeermodellen beschikbaar is op de Service Fabric. De andere is het programmeermodel betrouwbare acteur, waarmee u een virtuele Actor-programmeermodel boven op het model voor Reliable Services. Zie voor meer informatie over het model voor Reliable Actors [Inleiding tot Service Fabric Reliable Actors](service-fabric-reliable-actors-introduction.md).

Service Fabric de levensduur van de services van de inrichting en implementatie via bijwerken en verwijderen, beheert [Service Fabric-Toepassingsbeheer](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Wat zijn Reliable Services?
Reliable Services biedt een eenvoudige, krachtige, op het hoogste niveau programmeermodel kunt u express wat belangrijk is voor uw toepassing is. Met de Reliable Services programmeermodel gebruikt, beschikt u over:

* Toegang tot de rest van de Service Fabric programming API's. In tegenstelling tot Service Fabric-Services is gemodelleerd als [Gast uitvoerbare bestanden](service-fabric-deploy-existing-app.md), Reliable Services krijgen tot de rest van de Service Fabric-API's rechtstreeks gebruiken. Hierdoor kunnen services:
  * query op het systeem
  * rapport health over entiteiten in het cluster
  * meldingen ontvangen over configuratie-en code
  * zoeken en te communiceren met andere services
  * (optioneel) gebruiken de [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md)
  * ...en zodat ze toegang hebben tot veel andere mogelijkheden via een eerste-klas programmeermodel in diverse programmeertalen.
* Een eenvoudige model voor het uitvoeren van uw eigen code die lijkt op modellen die u om te gebruikt programmeren. Uw code is een goed gedefinieerde toegangspunt en de levenscyclus van eenvoudig te beheren.
* Een communicatiemodel pluggable. Gebruik het transport van uw keuze, zoals HTTP met [Web API](service-fabric-reliable-services-communication-webapi.md), WebSockets, aangepaste TCP-protocollen, of iets anders. Betrouwbare Services bieden sommige geweldige out-of-the-box-opties die u kunt gebruiken of u kunt uw eigen.
* Voor stateful services het programmeermodel Reliable Services kunt u voor het opslaan van uw status meteen in uw service met behulp van consistent en betrouwbaar [betrouwbare verzamelingen](service-fabric-reliable-services-reliable-collections.md). Betrouwbare verzamelingen zijn een eenvoudige reeks maximaal beschikbare en betrouwbare verzameling klassen die worden vertrouwd voor iedereen die is gebruikt C#-verzamelingen. Traditioneel vereist services externe systemen voor betrouwbare statusbeheer. Met betrouwbare verzamelingen kunt u uw status naast uw berekenings opslaan met de dezelfde hoge beschikbaarheid en betrouwbaarheid die u hebt zou verwachten van maximaal beschikbare externe stores. Dit model verbetert ook latentie omdat u zijn CO-locatie op de berekenings- en status moet werken.

Bekijk deze video van Microsoft Virtual Academy voor een overzicht van Reliable services:<center>
<a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=HhD9566yC_4106218965">
<img src="./media/service-fabric-reliable-services-introduction/ReliableServicesVid.png" WIDTH="360" HEIGHT="244" />
</a>
</center>

## <a name="what-makes-reliable-services-different"></a>Wat Reliable Services verschillende maakt?
Reliable Services in Service Fabric wijken af van de services die u hebt gemaakt voordat. Service Fabric biedt betrouwbaarheid, beschikbaarheid, consistentie en schaalbaarheid.

* **Betrouwbaarheid** - uw service blijft zelfs in omgevingen met onbetrouwbaar waar uw machines mislukken of druk op netwerkproblemen of in gevallen waarin de services zelf optreden fouten en vastlopen of mislukken. Voor stateful services, wordt de status behouden zelfs met netwerk of andere fouten.
* **Beschikbaarheid** -uw-service is bereikbaar is en reageert. Service Fabric onderhoudt het gewenste aantal actieve exemplaren.
* **Schaalbaarheid** - Services worden ontkoppeld van specifieke hardware en ze kunnen vergroten of verkleinen als nodig is via het toevoegen of verwijderen van de hardware of andere bronnen. Services gemakkelijk worden gepartitioneerd (met name in het geval stateful) om ervoor te zorgen dat de service kan worden geschaald en verwerken van gedeeltelijke fouten. Services kunnen worden gemaakt en verwijderd dynamisch via programmacode, waardoor meer exemplaren worden zo nodig worden ingezet spreken in reactie op aanvragen van klanten. Service Fabric raadt ten slotte services lightweight. Service Fabric kunt duizenden services in te richten in een enkel proces in plaats van dat of dat het volledige OS-exemplaren of processen tot één exemplaar van een service.
* **Consistentie** -gegevens zijn opgeslagen in deze service kan worden gegarandeerd consistent. Dit geldt zelfs in meerdere betrouwbare verzamelingen binnen een service. Wijzigingen in verzamelingen in een service kunnen worden gemaakt op een transactioneel atomic manier.

## <a name="service-lifecycle"></a>De levenscyclus van de service
Of uw service stateful of stateless, bieden Reliable Services de levensduur van een eenvoudige, waarmee u snel plug-in uw code en aan de slag.  Er zijn maar een of twee methoden die u implementeren wilt voor uw service up-to-date en worden uitgevoerd.

* **CreateServiceReplicaListeners/CreateServiceInstanceListeners** -deze methode is waar de service definieert voor de communicatie stack(s) die zij wil gebruiken. De communicatie-stack, zoals [Web API](service-fabric-reliable-services-communication-webapi.md), is wat definieert de luisterende eindpunt of de eindpunten voor de service (hoe clients bereiken voor de service). Het definieert ook de wisselwerking tussen de berichten die worden weergegeven met de rest van de servicecode.
* **RunAsync** -deze methode is waarin de zakelijke logica in uw service wordt uitgevoerd en waarbij deze zou ere van eventuele achtergrondtaken die moeten worden uitgevoerd voor de levensduur van de service. De annulering-token dat is opgegeven, is een signaal voor wanneer dat werk moet worden gestopt. Als de service moet berichten uit een betrouwbare wachtrij ophalen en deze te verwerken, is dit bijvoorbeeld waarbij die werken gebeurt.

Als u over reliable services voor de eerste keer leert, leest u op! Als u een gedetailleerd overzicht van de levenscyclus van betrouwbare services zoekt, kunt u Ga via naar [in dit artikel](service-fabric-reliable-services-lifecycle.md).

## <a name="example-services"></a>Voorbeeld van de services
Dit programmeermodel weet, gaat nu een kort overzicht van twee verschillende services om te zien hoe deze onderdelen in elkaar passen.

### <a name="stateless-reliable-services"></a>Staatloze Reliable Services
Een stateless service is een waar er is geen status die in de service wordt onderhouden tussen aanroepen. Geen status die aanwezig is, is volledig beschikbare en geen vereist synchronisatie, replicatie, persistentie of hoge beschikbaarheid.

Neem bijvoorbeeld een rekenmachine die heeft geen geheugen en alle voorwaarden en bewerkingen om uit te voeren in één keer ontvangt.

In dit geval de `RunAsync()` (C#) of `runAsync()` (Java) van de service kan niet leeg zijn, omdat er is geen taak-achtergrondverwerking die de service moet doen. Wanneer de Rekenmachine-service is gemaakt, wordt een `ICommunicationListener` (C#) of `CommunicationListener` (Java) (bijvoorbeeld [Web API](service-fabric-reliable-services-communication-webapi.md)) die een luisterende eindpunt op sommige poort wordt geopend. Dit eindpunt luisteren worden verbonden met de andere berekenings-methoden (voorbeeld: "Toevoegen (n1, n2)") die de Rekenmachine openbare API definiëren.

Wanneer er wordt een aanroep van een client, de juiste methode is aangeroepen en de Rekenmachine-service voert de bewerkingen van de gegevens die zijn opgegeven en retourneert het resultaat. Geen status worden niet opgeslagen.

Geen interne status niet opslaan vereenvoudigt de Rekenmachine van dit voorbeeld. Maar de meeste services worden niet echt staatloze. In plaats daarvan extern ze hun status naar een andere store. (Bijvoorbeeld een web-app die is afhankelijk van de sessiestatus behouden in een archief met back-ups maken of de cache is niet staatloze.)

Een voorbeeld van hoe stateless services worden gebruikt Service Fabric is als een front-end dat toegang biedt tot de openbare API voor een webtoepassing. De front-end-service wordt vervolgens gesproken stateful Services om de gebruikersaanvraag van een te voltooien. In dit geval worden aanroepen van clients omgeleid naar een bekende poort, bijvoorbeeld 80, waarbij de staatloze service luistert. Deze staatloze service ontvangt de oproep en bepaalt of de aanroep van een vertrouwde partij is en welke service het bestemd is voor.  Vervolgens de stateless-service stuurt de aanroep naar de juiste partitie van de stateful service en wordt gewacht op een antwoord. Wanneer de staatloze service een antwoord ontvangt, wordt geantwoord naar de oorspronkelijke client. Een voorbeeld van deze service is in onze voorbeelden [C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) / [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Actors/VisualObjectActor/VisualObjectWebService). Dit is slechts één voorbeeld van dit patroon in de voorbeelden, er zijn andere gebruikers in ook andere voorbeelden.

### <a name="stateful-reliable-services"></a>Stateful Reliable Services
Een stateful service is een die een gedeelte van de status behouden consistent en aanwezig is in de volgorde voor de service naar de functie nodig hebt. Houd rekening met een service die voortdurend berekent een oplopende gemiddelde van een bepaalde waarde op basis van updates die het ontvangt. U doet dit door moet de huidige set van inkomende aanvragen worden proces en de huidige gemiddelde moet hebben. Alle services die worden opgehaald, verwerkt en wordt informatie opgeslagen in een externe winkel (zoals een Azure-blob of tabel store vandaag) is stateful. De status blijven alleen in het externe gegevensarchief.

De meeste services opslaan vandaag hun status extern, omdat het externe archief wat biedt betrouwbaarheid, beschikbaarheid, schaalbaarheid en consistentie voor die status. Services worden niet in Service Fabric is vereist voor het opslaan van hun status extern. Service Fabric zorgt voor deze vereisten voor zowel de code van de status van de service.

> [!NOTE]
> Ondersteuning voor betrouwbare Stateful Services is niet beschikbaar op Linux nog (voor C# of Java).
>

Stel, dat we willen schrijven van een service waarmee afbeeldingen worden verwerkt. U doet dit door maakt de service in een afbeelding en de reeks conversies uitvoeren op die installatiekopie. Deze service retourneert een communicatie-listener (we Stel dat het is een WebAPI) dat gegarandeerd een API, zoals `ConvertImage(Image i, IList<Conversion> conversions)`. Wanneer een aanvraag ontvangt, de service wordt opgeslagen in een `IReliableQueue`, en retourneert een id voor de client zodat deze de aanvraag kunt bijhouden.

In deze service `RunAsync()` complexere kan worden. De service heeft een lus in de `RunAsync()` die aanvragen van haalt `IReliableQueue` en voert de gevraagde omzettingen. De resultaten ophalen opgeslagen in een `IReliableDictionary` zodat wanneer de client terugkomt krijgen ze hun geconverteerde installatiekopieën. Om ervoor te zorgen dat zelfs als er iets niet aan de installatiekopie niet verloren, deze betrouwbare Service zou ophalen uit de wachtrij de conversies uitvoeren en sla het resultaat allemaal in één transactie. In dit geval wordt het bericht is verwijderd uit de wachtrij en de resultaten worden opgeslagen in de woordenlijst resultaat alleen wanneer de conversies voltooid zijn. U kunt ook kan de service ophalen van de installatiekopie uit de wachtrij en direct op te slaan in een externe winkel. Dit reduceert de hoeveelheid staat die de service heeft voor het beheren van, maar de complexiteit toeneemt nadat de service heeft om de benodigde metagegevens voor het beheren van de externe store. Met de methode als er iets is mislukt in het midden blijft de aanvraag in de wachtrij moeten worden verwerkt.

Wat te weten over deze service is dat deze als een normale .NET-service klinkt. Het enige verschil is dat de gegevensstructuren wordt gebruikt (`IReliableQueue` en `IReliableDictionary`) worden geleverd door de Service Fabric en maximaal betrouwbare, beschikbare en consistent zijn.

## <a name="when-to-use-reliable-services-apis"></a>Het gebruik van betrouwbare API's voor Services
Als een van de volgende kenmerkend zijn voor de behoeften van uw toepassing-service, moet u betrouwbare Services-API's overwegen:

* U wilt dat uw service-code (en desgewenst-status) zijn de maximaal beschikbare en betrouwbare
* U moet transactionele garanties over meerdere van status (bijvoorbeeld orders en items op).
* De status van uw toepassing kan natuurlijk worden gemodelleerd als betrouwbare woordenlijsten en wachtrijen.
* Uw toepassingen, code of de status moet maximaal beschikbaar is met lage latentie lees- en schrijfbewerkingen.
* Uw toepassing moet voor het beheren van de gelijktijdigheid van taken of de granulatie van de transactiebewerkingen bij een of meer betrouwbare verzamelingen.
* U wilt beheren welke berichten of het partitioneringsschema voor uw service beheren.
* Uw code moet een gratis thread-runtime-omgeving.
* Uw toepassing moet worden dynamisch gemaakt of vernietigen betrouwbare woordenlijsten of wachtrijen of hele Services tijdens runtime.
* U moet programmatisch beheren van back-up van de geleverde Service Fabric en functies voor uw service-status te herstellen.
* Uw toepassing moet onderhouden wijzigingsoverzicht voor de eenheden van de status.
* U wilt ontwikkelen of gebruiken van de van de derde partij ontwikkeld, aangepaste statusproviders.

## <a name="next-steps"></a>Volgende stappen
* [Betrouwbare Services snel starten](service-fabric-reliable-services-quick-start.md)
* [Reliable Services geavanceerde gebruik](service-fabric-reliable-services-advanced-usage.md)
* [Het model voor Reliable Actors](service-fabric-reliable-actors-introduction.md)
