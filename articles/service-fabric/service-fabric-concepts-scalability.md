---
title: Schaalbaarheid van Service Fabric-services | Microsoft Docs
description: Hierin wordt beschreven hoe schalen Service Fabric-services
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 88c563876940da14f75d7ab30aa3f79a8f6dd870
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209369"
---
# <a name="scaling-in-service-fabric"></a>Schalen in Service Fabric
Azure Service Fabric kunt eenvoudig schaalbare toepassingen bouwen door het beheer van de services, partities en replica's op de knooppunten van een cluster. Veel werkbelastingen op dezelfde hardware uitgevoerd kunt maximale Resourcegebruik, maar ook biedt flexibiliteit in termen van hoe u kiest voor het schalen van uw werkbelastingen. Deze Channel 9 video wordt beschreven hoe u toepassingen voor schaalbare microservices kunt samenstellen:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T116/player]

In Service Fabric schalen, is het moet worden uitgevoerd op verschillende manieren:

1. Schalen door te maken of te verwijderen van staatloze service-exemplaren
2. Schalen door te maken of verwijderen van nieuwe services met de naam
3. Schalen door te maken of te verwijderen, nieuwe exemplaren van een toepassing met de naam
4. Met behulp van gepartitioneerde services schalen
5. Schalen door toe te voegen en knooppunten verwijderen uit het cluster 
6. Schalen met behulp van de Cluster Resource Manager metrische gegevens

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Schalen door te maken of te verwijderen van staatloze service-exemplaren
Een van de meest eenvoudige manieren schalen in Service Fabric werkt met stateless services. Als u een stateless service maakt, krijgt u de kans te definiëren een `InstanceCount`. `InstanceCount` Hiermee definieert u hoeveel actieve exemplaren van de code van de service worden gemaakt wanneer de service wordt gestart. Stel bijvoorbeeld dat er 100 knooppunten beschikbaar in het cluster zijn. Stel dat een service hebt gemaakt met een `InstanceCount` van 10. Tijdens runtime, die 10 actieve exemplaren van de code zou kunnen alle worden bezet (of kunnen niet genoeg bezet). Een manier schalen werkbelasting is het aantal exemplaren te wijzigen. Bijvoorbeeld, een blok van code voor bewaking of management die het bestaande aantal exemplaren van kunt wijzigen, 50 of 5, afhankelijk van of de werkbelasting moet in of uit te schalen op basis van de belasting. 

C#:

```csharp
StatelessServiceUpdateDescription updateDescription = new StatelessServiceUpdateDescription(); 
updateDescription.InstanceCount = 50;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

PowerShell:

```posh
Update-ServiceFabricService -Stateless -ServiceName $serviceName -InstanceCount 50
```
### <a name="using-dynamic-instance-count"></a>Met behulp van dynamische exemplaren
Specifiek voor stateless services biedt Service Fabric een automatische manier te wijzigen van het aantal exemplaren. Hierdoor kan de service te schalen dynamisch met het aantal knooppunten die beschikbaar zijn. De manier om te kiezen voor dit gedrag is het instellen van het aantal exemplaren = -1. InstanceCount = -1 is een instructie voor Service Fabric met de tekst 'Deze staatloze service uitvoeren op elk knooppunt'. Als het aantal knooppunten wordt gewijzigd, verandert Service Fabric automatisch het aantal exemplaren moet worden gezocht, ervoor te zorgen dat de service wordt uitgevoerd op alle geldige knooppunten. 

C#:

```csharp
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//Set other service properties necessary for creation....
serviceDescription.InstanceCount = -1;
await fc.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateless -PartitionSchemeSingleton -InstanceCount "-1"
```

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Schalen door te maken of verwijderen van nieuwe services met de naam
Een benoemd exemplaar van een specifiek exemplaar van een servicetype is (Zie [levenscyclus van de Service Fabric-toepassing](service-fabric-application-lifecycle.md)) binnen een aantal benoemde exemplaar in het cluster. 

Nieuwe benoemde service-exemplaren kunnen worden gemaakt (of verwijderd) als services komen meer of minder bezet. Dit kan aanvragen worden verdeeld over meer service-exemplaren, meestal waardoor de belasting van bestaande services te verlagen. Bij het maken van services, plaatst de resourcebeheer voor Service Fabric-Cluster de services in het cluster in een gedistribueerde wijze. De exacte beslissingen worden bepaald door de [metrische gegevens](service-fabric-cluster-resource-manager-metrics.md) in het cluster en andere plaatsingsregels. Services kunnen op verschillende manieren worden gemaakt, maar de meest voorkomende zijn via beheeracties als iemand het aanroepen van [ `New-ServiceFabricService` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps), of door code aanroepen [ `CreateServiceAsync` ](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet). `CreateServiceAsync` kan zelfs worden aangeroepen vanuit binnen andere services die worden uitgevoerd in het cluster.

Maken van services dynamisch kunnen worden gebruikt in allerlei scenario's en is een algemene patroon. Neem bijvoorbeeld een stateful service die staat voor een bepaalde workflow. Aanroepen werk die gaat worden weergegeven bij deze service en deze service wilt uitvoeren van de stappen voor die voortgang van de werkstroom en registreren. 

Hoe wilt u deze schaal bepaalde service maken De service kan multitenant in een vorm en accepteert aanroepen en stappen voor veel verschillende exemplaren van dezelfde werkstroom in één keer starten. Dat kan nog wel de code complexere sinds nu deze te hoeven maken over veel verschillende exemplaren van dezelfde werkstroom allemaal in verschillende stadia en van verschillende klanten heeft. Bovendien verwerken van meerdere werkstromen op hetzelfde moment het probleem oplost niet schaal. Dit is omdat op een bepaald moment verbruikt deze service te veel bronnen om aan te passen op een bepaalde computer. Veel services niet voor dit patroon is ingebouwd in eerste instantie ook problemen ondervindt als gevolg van sommige inherente knelpunt of vertraging in de code. Dit soort problemen zorgen dat de service niet werken ook wanneer het aantal gelijktijdige werkstromen, die is het bijhouden van grotere opgehaald.  

Er is een oplossing voor het maken van een exemplaar van deze service voor elk ander exemplaar van de werkstroom die u wilt traceren. Dit is een uitstekende patroon en of de service stateless of stateful is werkt. Voor dit patroon werken, moet u er meestal een andere service die als een 'van Workload Manager-Service fungeert' is. De taak van deze service is om aanvragen te ontvangen en te routeren die aanvragen bij andere services. De manager kan een exemplaar van de service werkbelasting dynamisch maken wanneer het bericht wordt ontvangen en geeft u op aanvragen aan deze services. De service manager kan ook retouraanroepen ontvangen wanneer een bepaalde workflow-service de taak is voltooid. Wanneer de manager de retouraanroepen ontvangt kan het verwijderen van dat exemplaar van de workflowservice of laat het als aanroepen meer verwacht. 

Advanced-versie van dit type manager kunnen maken van toepassingen van de services die deze beheert. De groep zorgt ervoor dat wanneer een nieuwe aanvraag afkomstig is deze heeft geen wachten tot de service met ronddraaien. In plaats daarvan kan de manager alleen kiezen van een werkstroomservice die niet uit de groep is momenteel bezet of routeren willekeurig. Een pool van services beschikbaar te houden kunt u nieuwe aanvragen voor de verwerking sneller, omdat het minder waarschijnlijk dat de aanvraag bevat de wachttijd voor een nieuwe service moet worden ingezet. Maken van nieuwe services is snel, maar niet beschikbaar of momentopname. De groep helpt te beperken van de hoeveelheid tijd die de aanvraag moet worden gewacht voordat de in behandeling heeft. Vaak ziet u deze manager en het patroon van de groep van toepassingen wanneer reactietijden maakt het meeste uit. De aanvraag Queuing en het maken van de service op de achtergrond en _vervolgens_ doorgeeft op is ook een patroon populaire manager is gemaakt en verwijderen van services op basis van bepaalde bijhouden van de hoeveelheid werk die service momenteel is in behandeling . 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Schalen door te maken of te verwijderen, nieuwe exemplaren van een toepassing met de naam
Maken en het verwijderen van de hele toepassingsexemplaren is vergelijkbaar met het patroon van het maken en -services verwijderen. Voor dit patroon is deel van de service manager die de beslissing op basis van de aanvragen die deze ziet en de informatie die dat het ontvangt van de andere services binnen het cluster te brengen. 

Wanneer u een nieuw toepassingsexemplaar van de benoemde moet worden gebruikt in plaats van een nieuwe benoemde service-exemplaren in een reeds bestaande toepassingen maken? Er is een enkele gevallen:

  * Het nieuwe toepassingsexemplaar is voor een klant waarvan de code moet worden uitgevoerd onder een bepaalde identiteit of beveiligingsinstellingen.
    * Service Fabric Hiermee kunt u pakketten van andere code worden uitgevoerd onder een bepaalde identiteiten definiëren. Als u wilt starten van het codepakket met dezelfde onder verschillende id's moeten de activeringen optreden in exemplaren van een andere toepassing. U kunt een aanvraag waarin het hebben van een bestaande klant werkbelastingen die zijn geïmplementeerd. Deze kunnen worden uitgevoerd onder een bepaalde identiteit zodat u kunt bewaken en beheren van toegang krijgen tot andere resources, zoals externe databases of andere systemen. In dit geval wanneer een nieuwe klant zich registreert u waarschijnlijk niet wilt dat de code in de context (procesruimte) activeren. Terwijl u kan wordt hierdoor het moeilijker voor uw servicecode binnen de context van een bepaalde identiteit. Normaal gesproken moet u meer beveiliging, isolatie- en identity management-code hebben. In plaats van een andere benoemde exemplaren van de service binnen hetzelfde toepassingsexemplaar en daarom wordt hetzelfde proces ruimte, kunt u verschillende benoemde exemplaren van Service Fabric-toepassing. Dit vergemakkelijkt het definiëren van de identiteit van de verschillende contexten.
  * Het nieuwe toepassingsexemplaar fungeert ook als een middel van configuratie
    * Standaard wordt alle van de benoemde service-exemplaren van een bepaalde servicetype in een toepassingsexemplaar in hetzelfde proces uitgevoerd op een bepaald knooppunt. Dit betekent dat terwijl u elk service-exemplaar anders configureren kunt, orde ingewikkeld is. Sommige token die ze gebruiken voor het opzoeken van hun config binnen een configuratiepakket hebben. Meestal is dit alleen de naam van de service. Dit werkt prima, maar de configuratie van de namen van de afzonderlijke benoemde service-exemplaren binnen het toepassingsexemplaar van deze worden gekoppeld. Dit kan verwarrend en moeilijk te beheren omdat de configuratie normaal gesproken een artefact ontwerp tijd met specifieke waarden van toepassing exemplaar is zijn. Meer services maakt altijd, meer toepassingsupgrades om de informatie in de config-pakketten te wijzigen of nieuwe te implementeren, zodat de nieuwe services kunnen worden opgezocht hun specifieke informatie. Het is vaak gemakkelijker te maken van een geheel nieuw benoemde exemplaar. Vervolgens kunt u de parameters voor de toepassing in te stellen welke configuratie is noodzakelijk voor de services. Op deze manier alle services die in die zijn gemaakt met de naam toepassingsexemplaar kan bepaalde configuratie-instellingen overnemen. Bijvoorbeeld, in plaats van een enkel configuratiebestand met de instellingen en aanpassingen voor elke klant, zoals geheimen of per klant limieten, hebt in plaats daarvan u een exemplaar van de verschillende groepen van toepassingen voor elke klant met deze instellingen overschreven. 
  * De nieuwe toepassing fungeert als een grens voor upgrade
    * In Service Fabric fungeren exemplaren van een andere benoemde toepassing als grenzen voor de upgrade. Een upgrade van een benoemd exemplaar heeft geen invloed op de code die een ander toepassingsexemplaar van de benoemde wordt uitgevoerd. De verschillende toepassingen uiteindelijk verschillende versies van dezelfde code wordt uitgevoerd op de dezelfde knooppunten. Dit kan een factor zijn wanneer u een beslissing vergroten/verkleinen moet omdat u kiezen kunt of de nieuwe code met de dezelfde upgrades als een andere service of niet volgen moet. Stel dat een aanroep van de manager-service die verantwoordelijk is voor het schalen van een bepaalde klant werkbelastingen door te maken en verwijderen van services dynamisch aankomt. In dit geval de aanroep is echter voor een werkbelasting die is gekoppeld aan een _nieuwe_ klant. De meeste klanten dat ze niet alleen omwille van de beveiliging en configuratie van elkaar zijn afgezonderd dat eerder is vermeld, maar omdat deze biedt meer flexibiliteit in termen van met een specifieke versie van de software en kiezen wanneer ze ophalen bijgewerkt. U kunt ook een nieuw exemplaar van de toepassing en het bedrag van de services die elk één upgrade blijven maken van de service er gewoon aan verdere partitie. Exemplaren van een afzonderlijke toepassing groter granulariteit bieden bij het uitvoeren van toepassingsupgrades en ook inschakelen A / B-tests en blauw/groen-implementaties. 
  * Het bestaande toepassingsexemplaar is vol
    * In Service Fabric [toepassing capaciteit](service-fabric-cluster-resource-manager-application-groups.md) een concept is die u gebruiken kunt om te bepalen welke resources beschikbaar voor exemplaren van een bepaalde toepassing. U kunt bijvoorbeeld besluiten dat een bepaalde service moet zijn van een ander exemplaar gemaakt om te schalen. Dit toepassingsexemplaar valt echter buiten capaciteit voor een bepaalde waarde. Als deze specifieke klant of de werkbelasting moet nog steeds worden toegekend meer resources, kan klikt u vergroot de capaciteit van de bestaande voor die toepassing of een nieuwe toepassing maken. 

## <a name="scaling-at-the-partition-level"></a>Schalen op het niveau van de partitie
Service Fabric ondersteunt partitionering. Partitioneren van splitst een service in meerdere logische en fysieke secties, die elk onafhankelijk werkt. Dit is handig voor stateful services, omdat er geen ingesteld van replica's heeft verwerkt alle aanroepen en alle van de status tegelijk bewerken. De [overzicht partitioneren](service-fabric-concepts-partitioning.md) bevat informatie over de soorten partitionering schema's die worden ondersteund. De replica's van elke partitie worden verdeeld over de knooppunten in een cluster, het distribueren van die service laden en ervoor te zorgen dat geen van beide de service als geheel of elke partitie een potentieel risico heeft. 

Houd rekening met een service die gebruikmaakt van een ranged partitieschema met een lage sleutel van 0, een hoge sleutel 99, en een aantal partities van 4. In een cluster met drie knooppunten, kan de service worden ingedeeld met vier replica's die de bronnen op elk knooppunt delen, zoals hier wordt weergegeven:

<center>
![Partitie-indeling met drie knooppunten](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Als u het aantal knooppunten verhoogt, wordt het Service Fabric enkele van de bestaande replica's er verplaatsen. Bijvoorbeeld, gaan we dat het aantal knooppunten toeneemt tot vier en de replica's ophalen opnieuw gedistribueerd. De service heeft nu nu drie replica's die worden uitgevoerd op elk knooppunt elk die behoren tot verschillende partities. Hiermee kunt beter brongebruik omdat het nieuwe knooppunt wordt niet koude. Normaal gesproken ook verbetert de prestaties als elke service meer beschikbare bronnen heeft.

<center>
![Partitie-indeling met vier knooppunten](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-the-service-fabric-cluster-resource-manager-and-metrics"></a>Schalen met behulp van de Service Fabric-Cluster Resource Manager en metrische gegevens
[Metrische gegevens](service-fabric-cluster-resource-manager-metrics.md) hoe services hun brongebruik naar Service Fabric express zijn. Met metrische gegevens, geeft de Cluster Resource Manager een kans opnieuw indelen en optimaliseren van de indeling van het cluster. Bijvoorbeeld, kunnen er voldoende bronnen in het cluster, maar ze kunnen niet worden toegewezen aan de services die momenteel werk doen. Met metrische gegevens, kunt de Cluster Resource Manager opnieuw indelen van het cluster om ervoor te zorgen dat services toegang tot de beschikbare bronnen hebben. 


## <a name="scaling-by-adding-and-removing-nodes-from-the-cluster"></a>Schalen door toe te voegen en knooppunten verwijderen uit het cluster 
Er is een andere optie voor schaling mogelijk met Service Fabric om de grootte van het cluster te wijzigen. Het wijzigen van de grootte van het cluster betekent toevoegen of verwijderen van knooppunten voor een of meer van de knooppunttypen in het cluster. Neem bijvoorbeeld een aanvraag waarin alle knooppunten in het cluster hot zijn. Dit betekent dat de cluster-resources bijna alle verbruikt zijn. In dit geval meer knooppunten toevoegen aan het cluster is de beste manier om te schalen. Zodra nieuwe knooppunten toegevoegd aan het cluster de resourcebeheer voor Service Fabric-Cluster wordt verplaatst services, wat resulteert in minder totale belasting van de bestaande knooppunten. Voor stateless services met het aantal exemplaren =-1, meer service exemplaren automatisch worden gemaakt. Hierdoor kunnen sommige aanroepen van de bestaande knooppunten naar de nieuwe knooppunten verplaatsen. 

Zie voor meer informatie [clusterschaling](service-fabric-cluster-scaling.md).

## <a name="putting-it-all-together"></a>Kort samengevat
Laten we de ideeën die we hier besproken en communiceren via een voorbeeld. Houd rekening met de volgende service: u probeert te maken van een service die als een adresboek naar namen fungeert op te houden en contactgegevens. 

Rechts vooraf hebt u een aantal vragen met betrekking tot schaal: hoeveel gebruikers gaat u hebben? Hoeveel contactpersonen slaat elke gebruiker? Wilt dit allemaal uit afbeelding is wanneer u dat de service voor het eerst permanent moeilijk. Stel dat u wilt gaan met een enkele statische service met een specifieke partitie telling. De gevolgen van het aantal partities verkeerde verzamelen kunnen ervoor zorgen dat u later scale problemen hebben. Op dezelfde manier, zelfs als u het juiste aantal u geen hebt mogelijk kiest alle informatie moet u. Bijvoorbeeld, hebt u ook om te bepalen van de grootte van het cluster vooraf, zowel op het gebied van het aantal knooppunten en de grootte. Het is doorgaans moeilijk te voorspellen, hoeveel resources een service wilt gebruiken tijdens zijn levensduur. Het kan ook zijn moeilijk te weten tevoren het verkeer patroon dat de service daadwerkelijk ziet. Zo mogelijk gebruikers toevoegen en verwijderen van hun contactpersonen alleen eerst te beginnen in de ochtend of mogelijk deze wordt evenredig verdeeld in de loop van de dag. Op basis hiervan u dynamisch moet mogelijk en schalen. Mogelijk vindt u meer te voorspellen wanneer u moeten gaat worden uit- en uitgebreid, maar in beide gevallen u waarschijnlijk gaat nodig hebt om te reageren op het brongebruik wijzigen door de service. Dit kan betrekking hebben op het wijzigen van de grootte van het cluster om te kunnen bieden meer bronnen bij het gebruik van bestaande resources opnieuw indelen niet voldoende. 

Maar waarom zelfs probeer naar een enkele partitieschema pikken voor alle gebruikers? Waarom zelf beperken tot een service en een statische cluster? De werkelijke situatie is meestal meer dynamische. 

Wanneer u voor schaal, houd rekening met het volgende patroon voor dynamische. Mogelijk moet u deze aanpassen aan uw situatie:

1. In plaats van een partitieschema voor iedereen vooraf verzamelen, bouwt u een 'manager-service'.
2. De taak van de service manager is om te kijken naar klantgegevens wanneer ze zich registreren voor uw service. Vervolgens, afhankelijk van de informatie die de service manager geen exemplaar maken van uw _werkelijke_ Neem contact op met storage-service _alleen voor die klant_. Als ze specifieke configuratie-, isolatie- of upgrades vereist, kunt u ook bepalen met ronddraaien van instantie van een toepassing voor de klant. 

Het maken van deze dynamische patroon veel voordelen:

  - U probeert niet te raden het aantal juiste partities voor alle gebruikers van tevoren of een service waarmee is oneindig schaalbare allemaal op een eigen bouwen. 
  - Andere gebruikers hebben geen hebben de dezelfde partitie aantal, aantal replica's, plaatsingsbeperkingen, metrische gegevens, standaard laadt, servicenamen, DNS-instellingen of een van de andere eigenschappen die zijn opgegeven op het niveau van de service of toepassing. 
  - Krijgt u extra gegevens segmentering. Elke klant heeft een eigen kopie van de service
    - Elke klantenservice worden anders geconfigureerd en meer of minder bronnen, met meer of minder partities of replica's zo nodig op basis van hun verwachte schaal verleend.
      - Bijvoorbeeld aannemen dat de klant betaald voor de laag 'Gold' - kunnen krijgen ze meer replica's of groter aantal van de partities en mogelijk resources toegewezen aan hun services via de capaciteit van metrische gegevens en toepassingen.
      - Of spreken die zijn opgegeven informatie geeft het aantal contacten die deze nodig is 'Klein' - ze krijgt alleen enkele partities of zelfs in een pool gedeelde service met andere klanten kunnen worden geplaatst.
  - Een aantal service-exemplaren of replica's zijn niet worden uitgevoerd terwijl u wacht voor klanten om te worden weergegeven
  - Als een klant ooit verlaat, is hun gegevens van uw service verwijderen net zo eenvoudig als bestanden met de manager verwijderen die service of toepassing dat is gemaakt.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

* [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
* [Partitionering Service Fabric-services](service-fabric-concepts-partitioning.md)
