---
title: Schaalbaarheid van Service Fabric-services | Microsoft Docs
description: Beschrijft hoe u voor het schalen van Service Fabric-services
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: ed324f23-242f-47b7-af1a-e55c839e7d5d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 14a7389fe562b5f3206b81411d2224257051c636
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666644"
---
# <a name="scaling-in-service-fabric"></a>Schalen in Service Fabric
Azure Service Fabric kunt eenvoudig schaalbare toepassingen kunt maken met het beheren van de services, partities en replica's op de knooppunten van een cluster. Veel werkbelastingen die wordt uitgevoerd op dezelfde hardware kunt maximale Resourcegebruik, maar ook beschikt u over flexibele hoe u kiest voor het schalen van uw workloads. Deze video voor Channel 9 wordt beschreven hoe u schaalbare microservices-toepassingen kunt ontwikkelen:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T116/player]

Schalen in Service Fabric is op verschillende manieren doen:

1. Door het maken of verwijderen van exemplaren van de stateless service schalen
2. Met de services schalen door te maken of verwijderen van nieuwe naam
3. Schalen met het maken of verwijderen van nieuwe exemplaren van een toepassing met de naam
4. Schalen met behulp van gepartitioneerde services
5. Door toe te voegen en knooppunten verwijderen uit het cluster schalen 
6. Schalen met behulp van Cluster Resource Manager-metrische gegevens

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Door het maken of verwijderen van exemplaren van de stateless service schalen
Een van de eenvoudigste manieren om te schalen in Service Fabric werkt met stateless services. Wanneer u een stateless service maakt, krijgt u een mogelijkheid voor het definiëren van een `InstanceCount`. `InstanceCount` bepaalt hoeveel actieve exemplaren van de code van de service worden gemaakt wanneer de service wordt gestart. Stel, bijvoorbeeld, er zijn 100 knooppunten in het cluster. Stel dat een service wordt gemaakt met een `InstanceCount` van 10. Tijdens runtime, die 10 actieve exemplaren van de code kunnen alle worden bezet (of kunnen zijn niet genoeg bezet). Een manier voor het schalen van die werkbelasting is het aantal exemplaren wijzigen. Sommige stukje controle of beheer code kunt bijvoorbeeld het bestaande aantal exemplaren wijzigen 50, of 5, afhankelijk van of de werkbelasting moet worden geschaald in- of uitschalen op basis van de belasting. 

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
Specifiek voor stateless services biedt Service Fabric een automatische manier te wijzigen van het aantal instanties. Dit kan de service dynamisch schalen met het aantal knooppunten die beschikbaar zijn. De manier om te kiezen voor dit gedrag is het instellen van het aantal instanties =-1. InstanceCount = -1 is een instructie naar Service Fabric met de tekst "Dit stateless service uitvoeren op elk knooppunt." Als het aantal knooppunten wordt gewijzigd, verandert Service Fabric automatisch het aantal exemplaren moet overeenkomen ervoor te zorgen dat de service wordt uitgevoerd op alle geldige knooppunten. 

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

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Met de services schalen door te maken of verwijderen van nieuwe naam
Een benoemd service-exemplaar een specifiek exemplaar van een servicetype is (Zie [levenscyclus van de Service Fabric-toepassingen](service-fabric-application-lifecycle.md)) binnen een benoemde toepassingsexemplaar in het cluster. 

Nieuwe exemplaren van de benoemde service kunnen worden gemaakt (of verwijderd) als services komen meer of minder bezet. Dit kan aanvragen worden verdeeld over meer service-exemplaren, zodat meestal laden op bestaande services om te verlagen. Bij het maken van services, plaatst het Cluster Resource Manager van Service Fabric de services in het cluster in een gedistribueerde manier. De exacte beslissingen vallen onder de [metrische gegevens](service-fabric-cluster-resource-manager-metrics.md) in het cluster en andere regels voor de plaatsing. Services kunnen op verschillende manieren worden gemaakt, maar de meest voorkomende zijn via de beheerderstaken iemand van het aanroepen van [ `New-ServiceFabricService` ](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps), of door code aanroepen [ `CreateServiceAsync` ](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet). `CreateServiceAsync` kan ook worden aangeroepen vanuit andere services die in het cluster.

Het maken van het services dynamisch kunnen worden gebruikt in allerlei scenario's en is een algemeen patroon. Bijvoorbeeld, kunt u overwegen een stateful service die een specifieke werkstroom vertegenwoordigt. Aanroepen die werk gaat bij deze service wordt weergegeven en gaat deze service voor het uitvoeren van de stappen voor die werkstroom en -record wordt uitgevoerd. 

Hoe maakt u de schaal van deze bepaalde service? De service kan worden meerdere tenants in een vorm en accepteert aanroepen en stappen voor veel verschillende exemplaren van dezelfde werkstroom in één keer starten. Dat kan nog wel de code complexer worden, nadat nu deze zorgen maken over veel verschillende exemplaren van dezelfde werkstroom, allemaal in verschillende stadia van en naar verschillende klanten heeft. Bovendien verwerken van meerdere werkstromen op hetzelfde moment het probleem oplost niet schaal. Dit is omdat op een bepaald moment gebruikt deze service te veel resources om aan te passen op een bepaalde virtuele machine. Veel services niet voor dit patroon is ingebouwd problemen in de eerste plaats ook vanwege een bepaalde inherente knelpunt of vertraging in de code. Dit soort problemen ertoe leiden dat de service niet te werken ook wanneer het aantal gelijktijdige werkstromen, die is het bijhouden van grotere opgehaald.  

Een oplossing is het maken van een exemplaar van deze service voor elk ander exemplaar van de werkstroom die u wilt bijhouden. Dit is een geweldige patroon en of de service stateless of stateful is werkt. Voor dit patroon om te werken, is er meestal een andere service die als een 'van Workload Manager-Service fungeert'. De taak van deze service is om aanvragen te ontvangen en het routeren van deze aanvragen bij andere services. De manager kan dynamisch gemaakt door een exemplaar van de workload-service wanneer deze het bericht ontvangt, en vervolgens op aanvragen zijn doorgegeven aan deze services. De service manager kan ook callbacks ontvangen wanneer een bepaalde workflow-service de taak is voltooid. Wanneer de manager de retouraanroepen ontvangt kan het verwijderen van deze instantie van de werkstroomservice of laat het als aanroepen meer verwacht zijn. 

Advanced-versie van dit type manager kunnen pools van de services die ermee wordt beheerd, zelfs maken. De groep zorgt ervoor dat wanneer een nieuwe aanvraag binnenkomt dit hoeft te wachten op de service om te activeren. In plaats daarvan kan de manager alleen kiezen van een werkstroomservice die niet uit de pool is momenteel bezet of willekeurig routeren. Een pool van services beschikbaar te houden kunt u nieuwe aanvragen voor de verwerking sneller, omdat het minder waarschijnlijk dat de aanvraag moet worden gewacht op een nieuwe service om te worden ingezette heeft. Het maken van nieuwe services is een snelle, maar niet gratis of momentopname. De pool te minimaliseren van de hoeveelheid tijd die de aanvraag heeft moet worden gewacht voordat een onderhoudsbeurt. Vaak ziet u deze manager en de pool patroon wanneer reactietijden het belangrijkst. De aanvraag Queuing en het maken van de service op de achtergrond en _vervolgens_ deze op door te geven is ook een patroon populaire manager, zoals het maken van is en verwijderen van services op basis van bepaalde bijhouden van de hoeveelheid werk die service die momenteel is in behandeling . 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Schalen met het maken of verwijderen van nieuwe exemplaren van een toepassing met de naam
Maken en verwijderen van exemplaren van de hele toepassing is vergelijkbaar met het patroon voor het maken en verwijderen van services. Voor dit patroon is een service manager dat ervoor zorgt dat het besluit op basis van de aanvragen die deze zien en de informatie die dat het ontvangt van de andere services binnen het cluster. 

Wanneer u een nieuw toepassingsexemplaar met de naam moet worden gebruikt in plaats van het maken van een nieuwe naam service-exemplaren in een bestaande toepassing? Er is een aantal gevallen:

  * Het nieuwe toepassingsexemplaar is voor een klant waarvan de code moet worden uitgevoerd onder een bepaalde identiteit of beveiligingsinstellingen.
    * Service Fabric kunt verschillende codepakketten om uit te voeren onder bepaalde identiteiten te definiëren. Als u wilt starten de dezelfde codepakket onder verschillende identiteiten, moeten de activeringen zich voordoen in andere toepassingsexemplaren. Houd rekening met een aanvraag waarin u werkt met een bestaande klant-werkbelastingen die zijn geïmplementeerd. Deze kunnen worden uitgevoerd onder een bepaalde identiteit, zodat u kunt bewaken en beheren van hun toegang tot andere resources, zoals externe databases of andere systemen. In dit geval, wanneer een nieuwe klant zich aanmeldt, wilt u waarschijnlijk niet activeren van hun code binnen dezelfde context (procesruimte). Terwijl u kan wordt hierdoor het moeilijker voor uw servicecode om op te treden in de context van een bepaalde identiteit. U moet hebben meestal meer beveiliging, isolatie en identity management-code. In plaats van verschillende met de naam service-exemplaren binnen hetzelfde exemplaar van de toepassing en daarom wordt hetzelfde proces ruimte, kunt u verschillende benoemde exemplaren van Service Fabric-toepassing. Dit maakt het gemakkelijker voor het definiëren van andere identiteit contexten.
  * Het exemplaar van de nieuwe fungeert ook als een van de configuratie
    * Standaard wordt alle van de benoemde service-exemplaren van een bepaalde servicetype binnen een exemplaar van een toepassing in hetzelfde proces uitgevoerd op een bepaald knooppunt. Dit betekent dat terwijl u elk service-exemplaar anders configureren kunt, als dit zo ingewikkeld is. Services moeten een token die ze gebruiken om te controleren of de configuratie in een configuratiepakket hebben. Dit is meestal alleen de naam van de service. Dit werkt prima, maar de configuratie van de namen van de afzonderlijke benoemde service-exemplaren in dat toepassingsexemplaar worden gekoppeld. Dit kan verwarrend zijn en moeilijk te beheren omdat de configuratie is doorgaans een artefact ontwerp tijd met specifieke waarden van toepassing exemplaar zijn. Het maken van meer services altijd betekent meer upgrades van toepassingen om de informatie in de config-pakketten te wijzigen of nieuwe labels te implementeren zodat de nieuwe services kunnen controleren of hun specifieke informatie. Vaak is het gemakkelijker om een geheel nieuwe met de naam toepassingsexemplaar te maken. Vervolgens kunt u de parameters voor de toepassing om in te stellen welke configuratie is noodzakelijk voor de services. Op deze manier alle services die binnen die zijn gemaakt met de naam van het exemplaar van toepassing kan bepaalde configuratie-instellingen overnemen. Bijvoorbeeld in plaats van een enkel configuratiebestand met de instellingen en aanpassingen voor elke klant, zoals geheimen of per klant resourcelimieten, dat moet u in plaats daarvan het exemplaar van een andere toepassing voor elke klant met deze instellingen overschreven. 
  * De nieuwe toepassing fungeert als grens van een upgrade
    * Service fabric fungeren andere benoemde exemplaren als grenzen voor de upgrade. Een upgrade van een benoemde toepassingsexemplaar heeft geen gevolgen voor de code die een ander toepassingsexemplaar van de benoemde wordt uitgevoerd. De verschillende toepassingen wordt van verschillende versies van dezelfde code uitvoeren op dezelfde knooppunten beëindigd. Dit kan een factor zijn als u beslissen vergroten/verkleinen wilt omdat u kiezen kunt of de nieuwe code met de dezelfde updates als een andere service of niet volgen moet. Stel dat een aanroep van de manager-service die verantwoordelijk is voor een bepaalde klant-werkbelastingen schalen door te maken en verwijderen van services dynamisch aankomt. In dit geval de aanroep is echter voor een werkbelasting die is gekoppeld aan een _nieuwe_ klant. De meeste klanten, zoals wordt geïsoleerd van elkaar niet alleen voor de redenen voor beveiliging en configuratie die eerder vermeld, maar omdat dit biedt meer flexibiliteit wat betreft waarop specifieke versies van de software en te kiezen wanneer ze een upgrade uitgevoerd. U kunt ook een nieuw toepassingsexemplaar maken en het bedrag van de services die elk één upgrade blijven van de service er gewoon naar verdere partitie maken. Exemplaren van een afzonderlijke toepassing groter granulariteit bieden bij het uitvoeren van upgrades van toepassingen en schakelt u tevens A / B-tests en Blue/Green implementaties. 
  * Het exemplaar van de bestaande is vol
    * In Service Fabric, [capaciteit van een toepassing](service-fabric-cluster-resource-manager-application-groups.md) een concept, dat u gebruiken kunt voor het beheren van de hoeveelheid resources die beschikbaar zijn voor exemplaren van een bepaalde toepassing is. U kunt bijvoorbeeld besluiten dat een bepaalde service moet zijn van een ander exemplaar gemaakt om te schalen. Dit toepassingsexemplaar is echter beperkt voor een bepaalde metrische gegevens. Als deze specifieke klant of de werkbelasting moet nog steeds worden verleend meer resources, kan vervolgens u verhogen van de bestaande capaciteit voor de toepassing of een nieuwe toepassing maken. 

## <a name="scaling-at-the-partition-level"></a>Schalen op het niveau van de partitie
Service Fabric ondersteunt partitionering. Partitioneren van splitst een service in meerdere logische en fysieke secties, die elk onafhankelijk van elkaar werkt. Dit is handig voor stateful services, omdat er niemand van replica's hebt ingesteld is om te verwerken van alle aanroepen en alle van de status in één keer bewerken. De [overzicht partitioneren](service-fabric-concepts-partitioning.md) bevat informatie over de typen partitioneringsschema's die worden ondersteund. De replica's van elke partitie worden verdeeld over de knooppunten in een cluster, het verdelen van de service belasting en ervoor te zorgen dat geen van beide de service als geheel of elke partitie een single point of failure heeft. 

Houd rekening met een service die gebruikmaakt van een ranged partitieschema met een lage sleutelwaarde van 0, een hoge sleutel 99, en een aantal partities van 4. De service in een cluster met drie knooppunten kan worden weergegeven met vier replica's die de resources op elk knooppunt delen, zoals hier wordt weergegeven:

<center>

![Partitie-indeling met drie knooppunten](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Als u het aantal knooppunten verhoogt, wordt Service Fabric enkele van de bestaande replica's er verplaatsen. Bijvoorbeeld, laten we zeggen dat het aantal knooppunten toeneemt vier en de replica's ophalen opnieuw gedistribueerd. De service heeft nu nu drie replica's die worden uitgevoerd op elk knooppunt, elk die behoren tot verschillende partities. Hiermee kunt beter Resourcegebruik omdat het nieuwe knooppunt zich niet koud. Normaal gesproken ook verbetert de prestaties omdat elke service meer beschikbare bronnen heeft.

<center>

![Partitie-indeling met vier knooppunten](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-the-service-fabric-cluster-resource-manager-and-metrics"></a>Schalen met behulp van de Service Fabric-Cluster Resource Manager en metrische gegevens
[Metrische gegevens](service-fabric-cluster-resource-manager-metrics.md) zijn hoe services hun resourceverbruik naar Service Fabric express. Het gebruik van metrische gegevens biedt met Cluster Resource Manager een verkoopkans opnieuw indelen en optimaliseren van de indeling van het cluster. Bijvoorbeeld, kunnen er voldoende bronnen in het cluster, maar ze kunnen niet worden toegewezen aan de services die momenteel werk doen. Met metrische gegevens kan met Cluster Resource Manager opnieuw indelen van het cluster om ervoor te zorgen dat services toegang tot de beschikbare resources hebben. 


## <a name="scaling-by-adding-and-removing-nodes-from-the-cluster"></a>Door toe te voegen en knooppunten verwijderen uit het cluster schalen 
Een andere optie voor het schalen met Service Fabric is de grootte van het cluster wijzigen. De grootte van het cluster te wijzigen, betekent toevoegen of verwijderen van knooppunten voor een of meer van de knooppunttypen in het cluster. Neem bijvoorbeeld een aanvraag waarin alle knooppunten in het cluster ' hot ' zijn. Dit betekent dat de resources van het cluster bijna alle verbruikt zijn. In dit geval meer knooppunten toevoegen aan het cluster is de beste manier om te schalen. Zodra de nieuwe knooppunten die zijn toegevoegd aan het cluster verplaatst de Cluster Resource Manager van Service Fabric services, wat resulteert in minder totale belasting van de bestaande knooppunten. Voor stateless services met het aantal exemplaren =-1, meer service exemplaren automatisch worden gemaakt. Hiermee kunt enkele aanroepen van de bestaande knooppunten naar de nieuwe knooppunten verplaatsen. 

Zie voor meer informatie, [clusterschaling](service-fabric-cluster-scaling.md).

## <a name="putting-it-all-together"></a>Alles samenvoegen
We gaan de ideeën die we hebben hier besproken en communiceren via een voorbeeld. Houd rekening met de volgende service: u probeert te maken van een service die als een adresboek fungeert, bedrijf namen en contactgegevens. 

Rechts van voorgrond beschikt u over een aantal vragen met betrekking tot schaal: Hoeveel gebruikers gaan u dat? Hoeveel contactpersonen slaat elke gebruiker? Dit alles uit afbeelding wilt is wanneer u uw service voor het eerst permanent moeilijk. Stel dat u met één statische service met een aantal specifieke partities zijn gaan. De gevolgen van het verzamelen van het aantal verkeerd partities kunnen ervoor zorgen dat u later schaal problemen hebben. Op dezelfde manier, zelfs als u het juiste aantal dat kunt u niet wellicht alle informatie verzamelen nodig u hebt. Bijvoorbeeld, hebt u ook om te bepalen van de grootte van het cluster van voorgrond, zowel wat betreft het aantal knooppunten en de grootte. Het is doorgaans moeilijk te voorspellen, hoeveel bronnen een service wilt gebruiken tijdens hun levensduur. Kan ook het moeilijk zijn om te weten tevoren het verkeer-patroon dat de service daadwerkelijk ziet. Bijvoorbeeld, misschien personen toevoegen en verwijderen van hun contactpersonen alleen eerst te beginnen in de ochtend of misschien het wordt evenredig verdeeld in de loop van de dag. Op basis hiervan moet u mogelijk in- en uitschalen dynamisch schalen. Misschien kunt u leren om te voorspellen wanneer u verder wilt in- en uitschalen schalen, maar in beide gevallen u waarschijnlijk gaat nodig hebt om te reageren op het gebruik van resources wijzigen door uw service. Dit kan betrekking hebben op de grootte van het cluster te wijzigen om te kunnen bieden meer bronnen bij het gebruik van bestaande resources ordenen niet voldoende. 

Maar waarom zelfs op een enkele partitieschema pikken voor alle gebruikers proberen? Waarom zelf beperken tot een service en één statische cluster? De werkelijke situatie is doorgaans meer dynamische. 

Bij het bouwen van de schaal te vergroten, houd rekening met de volgende dynamische patroon. Mogelijk moet u deze aanpassen aan uw situatie:

1. In plaats van het kiezen van een partitieschema voor iedereen een, een 'manager-service' te bouwen.
2. De taak van de service manager is om te kijken naar klantgegevens wanneer ze zich registreren voor uw service. Afhankelijk van die informatie maken de manager-service een exemplaar van uw _werkelijke_ contact op met de storage-service _alleen voor die klant_. Als ze nodig hebt voor specifieke configuratie-, isolatie- of upgrades worden uitgevoerd, kunt u ook bepalen exemplaar van een toepassing instellen voor de klant. 

Deze dynamische maken van het patroon van tal van voordelen:

  - U probeert niet te raden het juiste aantal voor alle gebruikers een of een enkele service oneindig schaalbaar allemaal op een eigen bouwen. 
  - Verschillende gebruikers hoeft te hebben van het hetzelfde aantal, aantal replica's, plaatsingsbeperkingen, metrische gegevens, standaard geladen, servicenamen, dns-instellingen, of een van de andere eigenschappen die zijn opgegeven op het niveau van de service of toepassing. 
  - Krijgt u extra gegevens segmentering. Elke klant heeft een eigen kopie van de service
    - Elke klantenservice kan anders geconfigureerd en meer of minder bronnen, met meer of minder partities of replica's indien nodig op basis van hun verwachte schaal verleend.
      - Bijvoorbeeld, zegt de klant betaald voor de 'Gold' laag - kan krijgen meer replica's of groter aantal van de partities, en mogelijk resources toegewezen aan hun services via capaciteiten van metrische gegevens en toepassingen.
      - Of Stel dat ze opgegeven informatie geeft het aantal benodigde contactpersonen is 'Kleine' - deze krijgt alleen een aantal partities of zelfs in een gedeelde service-toepassingen met andere klanten kunnen worden geplaatst.
  - Een groot aantal service-exemplaren of replica's nu niet worden uitgevoerd terwijl u voor klanten wacht worden weergegeven
  - Als een klant ooit verlaat, is klikt u vervolgens de gegevens verwijderen uit uw service net zo eenvoudig als met de manager verwijderen die service of toepassing dat is gemaakt.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over Service Fabric-concepten:

* [Beschikbaarheid van Service Fabric-services](service-fabric-availability-services.md)
* [Service Fabric-services partitioneren](service-fabric-concepts-partitioning.md)
