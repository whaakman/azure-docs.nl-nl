---
title: Eenheidstesten stateful services in Azure Service Fabric | Microsoft Docs
description: Meer informatie over de concepten en procedures van Eenheidstesten Service Fabric Stateful Services.
services: service-fabric
documentationcenter: .net
author: charleszipp
manager: timlt
editor: vturecek
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/04/2018
ms.author: ryanwi
ms.openlocfilehash: c2d98316b81b3d908ebbe6147fe40f231e94c142
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703641"
---
# <a name="unit-testing-stateful-services-in-service-fabric"></a>Eenheidstesten stateful services in Service Fabric

In dit artikel bevat informatie over de concepten en procedures van Eenheidstesten Service Fabric Stateful Services. Eenheidstesten in Service Fabric verdient een eigen overwegingen vanwege het feit dat de code van de toepassing actief wordt uitgevoerd onder meerdere verschillende contexten. In dit artikel beschrijft de procedures die worden gebruikt om ervoor te zorgen toepassingscode wordt gedekt onder elk van de verschillende contexten die kan worden uitgevoerd.

## <a name="unit-testing-and-mocking"></a>Eenheid testen en simuleren
Moduletests uitvoeren in de context van dit artikel is geautomatiseerd testen die kunnen worden uitgevoerd binnen de context van een test-runner zoals MSTest of NUnit. De eenheidstests in dit artikel, voeren geen bewerkingen op een externe bron, zoals een database of een RESTFul-API. Deze externe resources moeten worden simulatie. Nabootsend in de context van dit artikel wordt valse, record, en de retourwaarden voor de externe bronnen beheren.

### <a name="service-fabric-considerations"></a>Overwegingen met betrekking tot service Fabric
Een stateful Service Fabric-service testen eenheid heeft verschillende overwegingen. De servicecode uitgevoerd in de eerste plaats op meerdere knooppunten hebben, maar in verschillende rollen. Eenheidstests moeten de code onder elke rol voor een volledige dekking evalueren. De verschillende rollen zijn primaire, secundaire actieve niet-actieve secundaire en onbekend. De geen rol normaal gesproken hoeft niet elke speciale dekking als Service Fabric rekening gehouden met deze rol zijn ongeldig of null-service. Ten tweede, wordt elk knooppunt de rol op elk gewenst moment wijzigen. Voor het bereiken van volledige dekking moet code-uitvoeringspad van worden getest met de rol van wijzigingen moet worden uitgevoerd.

## <a name="why-unit-test-stateful-services"></a>Waarom testen eenheid stateful services? 
Stateful services Eenheidstesten kan helpen om enkele veelvoorkomende fouten die zijn aangebracht die zou niet per se worden opgepikt door conventionele toepassing of domeinspecifieke Eenheidstesten. Bijvoorbeeld, als de stateful service een status in het geheugen heeft, dit type testen kunt controleren of dat deze status in het geheugen worden bewaard gesynchroniseerd via elke replica. Dit type testen kunt ook controleren of dat een stateful service op annuleringstokens die zijn doorgegeven door de Service Fabric-indeling op de juiste wijze reageert. Wanneer annuleringen worden geactiveerd, moet de service uitgevoerd en/of lang asynchrone bewerkingen te stoppen.  

## <a name="common-practices"></a>Algemene procedures

De volgende sectie wordt op de meest voorkomende procedures voor het testen van een stateful service eenheid. Het geeft ook aan wat een antwoordnabootsing laag zodat deze aansluiten op de Service Fabric-indeling en statusbeheer moet hebben. Antwoordnabootsing bibliotheken bestaan bibliotheken die deze functionaliteit bieden. [ServiceFabric.Mocks](https://www.nuget.org/packages/ServiceFabric.Mocks/) vanaf 3.3.0 of hoger is een dergelijke bibliotheek de antwoordnabootsing functionaliteit aanbevolen en de onderstaande procedures volgt.

### <a name="arrangement"></a>Rangschikking

#### <a name="use-multiple-service-instances"></a>Meerdere exemplaren van de service gebruiken
Eenheidstests meerdere exemplaren van een stateful service moeten worden uitgevoerd. Dit wordt gesimuleerd wat er werkelijk gebeurt op het cluster waar Service Fabric bepalingen meerdere replica's die uw service op verschillende knooppunten worden uitgevoerd. Elk van deze exemplaren wordt uitgevoerd onder een andere context echter. Wanneer de test wordt uitgevoerd, moet elke instantie gereed worden gemaakt met de configuratie van de functie verwacht op het cluster. Bijvoorbeeld, als de service wordt verwacht dat doel voor replica's van 3, Service Fabric worden ingericht drie replica's op verschillende knooppunten. Waarbij een van wordt de primaire en de andere twee wordt actief en secundair van.

De service-uitvoeringspad variëren in de meeste gevallen enigszins voor elk van deze rollen. Bijvoorbeeld, als de service niet van aanvragen van een actieve secundaire accepteren moet, mogelijk de service een controle voor deze aanvraag inzetten weer een informatieve uitzondering dat geeft aan een aanvraag dat is uitgevoerd op een secundaire. Met meerdere exemplaren, kunnen deze situatie moet worden getest.

Met meerdere exemplaren kan bovendien de tests uit om over te schakelen van de functies van elk van deze instanties om te controleren of dat de antwoorden zijn ondanks de wijzigingen van de rol.

#### <a name="mock-the-state-manager"></a>Model van de status manager
De status Manager moet worden beschouwd als een externe bron en daarom simulatie. Wanneer de status manager simuleren, moet er een onderliggende opslag in het geheugen voor het bijhouden van wat wordt opgeslagen in de status manager zodat deze kan worden gelezen en gecontroleerd. Er is een eenvoudige manier om dit te bereiken om mock exemplaren van elk van de typen van betrouwbare verzamelingen te maken. Binnen deze mocks gebruiken een gegevenstype dat goed wordt uitgelijnd met de bewerkingen die worden uitgevoerd op basis van die verzameling. Hier volgen enkele suggesties gegevenstypen voor elke betrouwbare verzameling

- IReliableDictionary < TKey, TValue >-System.Collections.Concurrent.ConcurrentDictionary < TKey, TValue > >
- IReliableQueue<T> System.Collections.Generic.Queue -><T>
- IReliableConcurrentQueue<T> System.Collections.Concurrent.ConcurrentQueue -><T>

#### <a name="many-state-manager-instances-single-storage"></a>Veel status Manager instanties, één opslag
Zoals al eerder vermeld, moeten de status Manager en de betrouwbare verzamelingen worden behandeld als een externe bron. Daarom deze resources moeten en wordt binnen de eenheidstests worden simulatie. Bij het uitvoeren van meerdere exemplaren van een stateful service wordt het een uitdaging voor elke manager gesimuleerd status gesynchroniseerd houden tussen verschillende stateful service-exemplaren zijn. Wanneer de stateful service wordt uitgevoerd op het cluster, zorgt de Service Fabric te houden dat elke secundaire replica de status manager consistent zijn met de primaire replica. Daarom moeten de tests werken op dezelfde manier zodat ze rol wijzigingen kunnen simuleren.

Een eenvoudige manier deze synchronisatie kan worden bereikt, is het gebruik van een singleton-patroon voor het onderliggende object die de gegevens geschreven naar elke betrouwbare verzameling opslaat. Bijvoorbeeld, als een stateful service gebruikmaakt van een `IReliableDictionary<string, string>`. Het model de status manager moet retourneren een nagebootste van `IReliableDictionary<string, string>`. Deze nagebootste mag gebruiken een `ConcurrentDictionary<string, string>` voor het bijhouden van de sleutel/waarde-paren die zijn geschreven. De `ConcurrentDictionary<string, string>` moet worden gebruikt door alle exemplaren van de status managers singleton doorgegeven aan de service.

#### <a name="keep-track-of-cancellation-tokens"></a>Bijhouden van annuleringstokens
Annuleringstokens zijn dat een belangrijk nog vaak over het hoofd gezien aspect van stateful services. Wanneer Service Fabric een primaire replica voor een stateful service wordt gestart, wordt een annulering-token opgegeven. Dit token annulering is bedoeld om aan te geven aan de service wanneer deze wordt verwijderd of naar een andere rol gedegradeerd. De stateful service moet lang actief of asynchrone bewerkingen worden gestopt zodat Service Fabric de rol wijzigen-workflow kunt uitvoeren.

Wanneer het uitvoeren van de eenheid tests, eventuele annuleringstokens die zijn opgegeven voor RunAsync, moeten u ChangeRoleAsync en OpenAsync, CloseAsync blijven tijdens het uitvoeren van de test. Bedrijf op deze tokens kunnen de test voor het simuleren van een service wordt uitgeschakeld of degradatie en de service reageert op de juiste wijze controleren.

#### <a name="test-end-to-end-with-mocked-remote-resources"></a>Testen van end-to-end met gesimuleerd externe bronnen
Eenheidstests moeten worden uitgevoerd als veel van de toepassingscode die de status van de stateful service mogelijk kunt wijzigen. Het verdient aanbeveling dat de tests meer end-to-end van aard zijn. De enige mocks die aanwezig zijn om te registreren, simuleren en/of Controleer of de externe resource interacties. Dit omvat interacties met de status Manager en de betrouwbare verzamelingen. Het volgende codefragment is een voorbeeld van gherkin te testen die u laat end-to-end testen zien:

```
    Given stateful service named "fabric:/MyApp/MyService" is created
    And a new replica is created as "Primary" with id "111"
    And a new replica is created as "IdleSecondary" with id "222"
    And a new replica is created as "IdleSecondary" with id "333"
    And all idle secondary replicas are promoted to active secondary
    When a request is made to add the an employee "John Smith"
    And the active secondary replica "222" is promoted to primary
    And a request is made to get all employees
    Then the request should should return the "John Smith" employee
```

Deze test beschouwt dat de gegevens die wordt vastgelegd op een van de replica beschikbaar voor een secundaire replica, is wanneer deze wordt gepromoveerd tot primaire. Ervan uitgaande dat een betrouwbare verzameling het archief met back-ups maken voor de gegevens van werknemers is, Aa eventuele fouten die kan worden opgepikt met deze test wordt als de code van de toepassing niet Receive heeft `CommitAsync` voor de transactie op te slaan van de nieuwe werknemer. De tweede aanvraag voor het ophalen van werknemers retourneerde in dat geval niet werknemer toegevoegd door de eerste aanvraag.

### <a name="acting"></a>Fungeert
#### <a name="mimic-service-fabric-replica-orchestration"></a>Service Fabric-replica orchestration nabootsen
Bij het beheren van meerdere exemplaren van de service, wordt dit door de tests worden geïnitialiseerd en deze services verbreken op dezelfde manier als de Service Fabric-indeling. Bijvoorbeeld, wanneer een service op een nieuwe primaire replica wordt gemaakt, roept Service Fabric CreateServiceReplicaListener, OpenAsync, ChangeRoleAsync en RunAsync. De van levenscyclusgebeurtenissen worden beschreven in de volgende artikelen:

- [Starten van de stateful Service](service-fabric-reliable-services-lifecycle.md#stateful-service-startup)
- [Afsluiten van de stateful Service](service-fabric-reliable-services-lifecycle.md#stateful-service-shutdown)
- [Primaire Swaps stateful Service](service-fabric-reliable-services-lifecycle.md#stateful-service-primary-swaps)

#### <a name="run-replica-role-changes"></a>Replica rol wijzigingen worden uitgevoerd
De eenheidstests moeten de functies van de service-exemplaren op dezelfde manier als de Service Fabric-indeling wijzigen. De rol toestandsmachine wordt beschreven in het volgende artikel:

[De Rolstatus replica-Machine](service-fabric-concepts-replica-lifecycle.md#replica-role)

Simulatie van wijzigingen van de rol is een van de meer belangrijke aspecten van het testen en krijgt meteen problemen waar de status van de replica niet consistent zijn met elkaar zijn. Inconsistent replicastatus kan optreden vanwege het opslaan van de status in het geheugen in statisch of klasse serviceniveau-instantievariabelen. Voorbeelden van deze mogelijk annuleringstokens, enums en configuratiewaarden objecten. Hiermee zorgt u ervoor dat de service inachtneming van de annuleringstokens die is opgegeven tijdens RunAsync waarmee het wijzigen van een rol worden uitgevoerd. Wijzigingen van de rol te simuleren, krijgt u problemen die zich voordoen kunnen als de code niet geschreven om toe te staan een aanroep van RunAsync meerdere keren ook meteen.

#### <a name="cancel-cancellation-tokens"></a>Annuleringstokens annuleren
Er moet zijn aangelegd eenheidstests waar de annulering-token dat is opgegeven voor RunAsync is geannuleerd. Hierdoor wordt de test om te controleren dat de service zonder problemen wordt afgesloten. Tijdens deze afsluiten lang actief of asynchrone bewerkingen moet worden gestopt. Voorbeeld van een langdurige procedure die mogelijk aanwezig zijn op een service is een die naar berichten in een betrouwbare wachtrij luistert. Dit bestaat mogelijk rechtstreeks in RunAsync of een achtergrond-thread. De implementatie moet opnemen logica voor het afsluiten van de bewerking als dit token annulering is geannuleerd.

Als de stateful services maken gebruik van een cache of in het geheugen staat die alleen op de primaire moet aanwezig zijn, moet er op dit moment worden verwijderd. Dit is om ervoor te zorgen dat deze status consistent is als het knooppunt een primaire het later opnieuw wordt. Annulering testen kunt de test om te controleren of dat deze status goed is verwijderd.

#### <a name="execute-requests-against-multiple-replicas"></a>Uitvoeren van aanvragen op basis van meerdere replica 's
Assert tests moeten dezelfde aanvraag op basis van andere replica worden uitgevoerd. In combinatie met de rol van wijzigingen, kunnen problemen met de consistentiecontrole kunnen worden gezien. Een voorbeeld van de test kan de volgende stappen uitvoeren:
1. Een aanvraag schrijven op basis van de huidige primaire uitvoeren
2. Een leesaanvraag die de gegevens die zijn geschreven in stap 1 op basis van de huidige primaire retourneert uitvoeren
3. Promoveren van een secundaire naar primaire. Dit moet ook de huidige primaire naar secundaire verlagen
4. De dezelfde leesaanvraag van stap 2 op basis van de nieuwe secundaire uitvoeren.

In de vorige stap, kan de test bevestigen dat de geretourneerde gegevens consistent is. Een mogelijk probleem die dit kan onthullen is dat de gegevens worden geretourneerd door de service mogelijk in het geheugen, maar back-ups uiteindelijk door een betrouwbare verzameling. Deze gegevens in het geheugen kan niet worden bewaard gesynchroniseerd goed met wat in de betrouwbare verzameling bestaat.

In-memory-gegevens wordt doorgaans gebruikt voor het maken van secundaire indexen of aggregaties van gegevens in een betrouwbare verzameling.

### <a name="asserting"></a>Aantonen
#### <a name="ensure-responses-match-across-replicas"></a>Zorg ervoor dat de antwoorden op deze in de replica 's
Eenheidstests moeten bevestigen dat een antwoord voor een bepaalde aanvraag consistent voor meerdere replica's is wanneer deze worden overgezet naar primaire. Kan dit potentiële problemen waar gegevens die zijn opgegeven in het antwoord wordt niet ondersteund door een betrouwbare verzameling of in het geheugen blijven surface zonder een mechanisme om te synchroniseren die gegevens voor replica's. Dit zorgt ervoor dat de service weer consistent antwoorden verzonden nadat de Service Fabric rebalances of overgeschakeld naar een nieuwe primaire replica.

#### <a name="verify-service-respects-cancellation"></a>Controleer of de service respecteert annuleren
Langlopende of asynchrone processen die moeten worden beëindigd wanneer de token van een annulering wordt geannuleerd moeten worden geverifieerd dat ze daadwerkelijk beëindigd na annulering. Dit zorgt ervoor dat ondanks de replica verandert rollen, processen die niet zijn bedoeld om te blijven uitvoeren op niet-primaire replica stoppen voordat de overgang is voltooid. Dit kan ook problemen waar deze een proces een aanvraag voor rol wijzigen of afsluiten van Service Fabric uitvoeren blokkeert onthullen.

#### <a name="verify-which-replicas-should-serve-requests"></a>Controleren welke replica-aanvragen moeten verwerken
De tests moeten het verwachte gedrag assert als een aanvraag wordt gerouteerd naar een niet-primaire-replica. Service Fabric biedt de mogelijkheid om secundaire replica's aanvragen verwerken. Schrijfbewerkingen naar betrouwbare verzamelingen kunnen echter alleen optreden van de primaire replica. Als uw toepassing zal voor alleen primaire replica's voor het verzenden van aanvragen of alleen een subset van aanvragen kan worden verwerkt door een secundaire, moeten het verwachte gedrag voor de positieve en negatieve gevallen assert door de tests. Het negatief geval wordt een aanvraag wordt doorgestuurd naar een replica die u moet niet worden verwerkt de aanvraag en de positieve wordt het tegenovergestelde.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [eenheid test stateful services](service-fabric-how-to-unit-test-stateful-services.md).