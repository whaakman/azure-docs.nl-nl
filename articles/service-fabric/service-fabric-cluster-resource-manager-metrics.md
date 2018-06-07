---
title: Azure microservice load met metrische gegevens beheren | Microsoft Docs
description: Meer informatie over het configureren en gebruiken van metrische gegevens in Service Fabric service brongebruik te beheren.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7457a820d9179248eab976ceec64f6b7a4a38563
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643335"
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Het beheren van het brongebruik en de belasting in Service Fabric met metrische gegevens
*Metrische gegevens* zijn de resources die uw services voorzichtig over en die worden geleverd door de knooppunten in het cluster. Een metriek is alles wat u wilt beheren om te kunnen verbeteren of de prestaties van uw services controleren. U kunt bijvoorbeeld geheugenverbruik als u wilt weten of de service is overbelast bekijken. Er wordt een andere gebruikt om te achterhalen of de service elders waarbij geheugen dat minder beperkt verplaatsen kan is, om betere prestaties wenst.

Items zoals geheugen, schijf- en CPU-gebruik zijn voorbeelden van metrische gegevens. Deze metrische gegevens zijn fysieke metrische gegevens en bronnen die overeenkomen met de fysieke resources op het knooppunt dat moet worden beheerd. Metrische gegevens kan ook worden (en meestal zijn) logische metrische gegevens. Logische metrische gegevens zijn bijvoorbeeld 'MyWorkQueueDepth' of 'MessagesToProcess' of 'TotalRecords'. Logische metrische gegevens zijn toepassingsspecifieke en indirect overeen met het verbruik van bepaalde fysieke resource. Logische metrische gegevens zijn algemene omdat het soms moeilijk te meten en in het rapport verbruik van de fysieke resources op basis van de per-service. De complexiteit van het meten van en rapportage van uw eigen fysieke metrische gegevens is ook waarom Service Fabric sommige standaard metrische gegevens bevat.

## <a name="default-metrics"></a>Standaard metrische gegevens
Stel dat u wilt schrijven en implementeren van uw service aan de slag. Op dit moment weet niet welk fysieke of logische resources verbruikt. Dat is geen probleem! De Service Fabric Cluster Resource Manager maakt gebruik van bepaalde standaard metrische gegevens wanneer er geen andere waarden zijn opgegeven. Dit zijn:

  - PrimaryCount - telling van de primaire replica's op het knooppunt 
  - ReplicaCount - telling van de totale stateful replica's op het knooppunt
  - Aantal - telling van alle service-objecten (stateless en stateful) op het knooppunt

| Gegevens | Staatloze exemplaar laden | Stateful secundaire laden | Stateful primaire laden | Gewicht |
| --- | --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |0 |
| ReplicaCount |0 |1 |1 |0 |
| Count |1 |1 |1 |0 |


Voor algemene werkbelastingen geeft de standaard metrische gegevens een goede verdeling van het werk in het cluster. In het volgende voorbeeld gaan we kijken wat er gebeurt als er twee services en zijn afhankelijk van de standaard metrische gegevens voor netwerktaakverdeling. De eerste service is een stateful service met drie partities en een grootte van de drie doel replicaset. De tweede service is een staatloze service met één partitie en drie exemplaren.

Dit is wat u krijgt:

<center>
![Cluster-indeling met standaard metrische gegevens][Image1]
</center>

Een aantal zaken te weten:
  - Primaire replica's voor de stateful service zijn verdeeld over meerdere knooppunten
  - Replica's voor dezelfde partitie worden op verschillende knooppunten
  - Het totale aantal primaire en secundaire replica's wordt gedistribueerd in het cluster
  - Het totale aantal serviceobjecten worden gelijkmatig op elk knooppunt toegewezen

Goede!

De standaard metrische gegevens werken geweldig als een begin. Echter wordt de metrische gegevens die standaard alleen uitvoeren u tot nu toe. Bijvoorbeeld: Wat is de kans dat de partitionering schema u resultaten opgenomen in perfect zelfs-gebruik door alle partities? Wat is de kans dat de belasting voor een bepaalde service constant gedurende een bepaalde periode is, of zelfs dezelfde over meerdere partities nu?

U kunt uitvoeren met alleen de standaard metrische gegevens. Dit leidt meestal betekent echter dat het clustergebruik van uw is korter en meer ongelijke dan u wilt. Dit komt doordat de metrische gegevens die standaard zijn geen geavanceerde en wordt ervan uitgegaan dat alles is gelijk. Bijvoorbeeld: een primaire die bezig is en één die niet beide bijdragen "1" in de metriek PrimaryCount. In het ergste geval kan met alleen de standaard metrische gegevens ook leiden tot overscheduled knooppunten, wat leidt tot prestatieproblemen. Als u geïnteresseerd bent in het meeste uit uw cluster ophalen en prestatieproblemen te voorkomen, moet u aangepaste metrische gegevens en rapportage van de dynamische belasting bij gebruiken.

## <a name="custom-metrics"></a>Aangepaste metrische gegevens
Metrische gegevens zijn geconfigureerd op basis van de per-met de naam-service-exemplaar bij het maken van de service.

Een metriek heeft enkele eigenschappen beschrijving van het: een naam, een gewicht en een werklast (standaard).

* Metrische naam: De naam van de metrische gegevens. De naam van de meetwaarde is de unieke id voor de metriek binnen het cluster uit het perspectief van de Resource Manager.
* Gewicht: Metrische gewicht definieert hoe belangrijk deze waarde is ten opzichte van de andere metrische gegevens voor deze service.
* Werklast (standaard): De werklast (standaard) is anders, afhankelijk van de service stateless of stateful weergegeven.
  * Voor stateless services heeft elke metriek één eigenschap met de naam DefaultLoad
  * U definieert voor stateful services:
    * PrimaryDefaultLoad: De standaardhoeveelheid van deze metrische gegevens van deze service verbruikt wanneer het een primaire
    * SecondaryDefaultLoad: De standaardhoeveelheid van deze metrische gegevens van deze service verbruikt wanneer deze een secundair

> [!NOTE]
> Als u aangepaste metrische gegevens definiëren en u wilt _ook_ gebruiken de standaard metrische gegevens, moet u _expliciet_ de standaard metrische gegevens back- en gewicht en waarden definiëren voor hen toevoegen. Dit is omdat moet u de relatie tussen de standaard metrische gegevens en uw aangepaste metrische gegevens definiëren. Bijvoorbeeld, misschien u het belangrijkst ConnectionCount of WorkQueueDepth meer dan primaire distributie. Standaard is het gewicht van de metriek PrimaryCount hoog, zodat u beperken op Gemiddeld wilt wanneer u uw andere metrische gegevens om te controleren of dat ze voorrang toevoegt.
>

### <a name="defining-metrics-for-your-service---an-example"></a>Metrische gegevens voor uw service - een voorbeeld definiëren
Stel dat u wilt dat de volgende configuratie:

  - Een waarde met de naam 'ConnectionCount' uw service-rapporten
  - Wilt u ook de standaard metrische gegevens gebruiken 
  - U hebt gedaan sommige metingen en weten dat een primaire replica van die service gewoonlijk 20 eenheden van 'ConnectionCount duurt'
  - Secundaire replica's gebruiken "ConnectionCount" 5-eenheden
  - U weet dat 'ConnectionCount' is de belangrijkste metrische gegevens in termen van de prestaties van deze bepaalde service beheren
  - U wilt nog steeds primaire replica's met gelijke taakverdeling. Taakverdeling van de primaire replica's wordt doorgaans een goed idee ongeacht wat. Dit helpt voorkomen dat het verlies van een knooppunt of fault-domein die invloed hebben op het grootste deel van de primaire replica's samen met het. 
  - Anders is de standaard metrische gegevens er mis

Hier volgt de code die u schrijven wilt naar het maken van een service met deze configuratie:

Code:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription connectionMetric = new StatefulServiceLoadMetricDescription();
connectionMetric.Name = "ConnectionCount";
connectionMetric.PrimaryDefaultLoad = 20;
connectionMetric.SecondaryDefaultLoad = 5;
connectionMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(connectionMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ConnectionCount,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

> [!NOTE]
> De bovenstaande voorbeelden en de rest van dit document worden beschreven van het beheren van metrische gegevens op basis van de per-met de naam-service. Het is ook mogelijk om te definiëren van metrische gegevens voor uw services op de service _type_ niveau. Dit wordt bereikt door op te geven ze in de manifesten voor uw service. Type niveau metrische gegevens definiëren wordt niet aanbevolen om verschillende redenen. De eerste reden is dat de namen van de metrische vaak omgevingspecifiek zijn. Tenzij er een vast contract aanwezig is, mag u niet zeker dat de metriek 'Kernen' in een omgeving geen 'MiliCores' of 'Kernen' in andere gevallen. Als uw metrische gegevens zijn gedefinieerd in uw manifest moet u nieuwe manifesten per omgeving maken. Dit leidt meestal tot een verspreiding van verschillende manifesten met slechts kleine verschillen, wat tot problemen bij het beheer leiden kunnen.  
>
> Metrische belasting zijn vaak op basis van de per-met de naam-service-exemplaar toegewezen. Stel dat u één exemplaar van de service voor CustomerA die alleen licht gebruik wil maken. Stel ook dat u een andere maken voor CustomerB die een grotere werkbelasting heeft. In dit geval wilt u waarschijnlijk aanpassen van de belasting van de standaardwaarde voor deze services. Als u metrische gegevens laadt gedefinieerd via manifesten en u wilt ondersteunen in dit scenario, vereist deze verschillende groepen van toepassingen en servicetypen voor elke klant. De waarden die worden gedefinieerd tijdens het maken van een service overschrijven die zijn gedefinieerd in het manifest, zodat u die voor de specifieke standaardinstellingen kan gebruiken. Echter dat zorgt ervoor dat de waarden in de manifesten die niet overeenkomen met die de service daadwerkelijk wordt uitgevoerd met gedeclareerd. Dit kan leiden tot verwarring. 
>

Als een herinnering: als u alleen gebruiken de standaard metrische gegevens wilt, moet u niet de verzameling metrische gegevens op alle touch of iets speciale doen bij het maken van uw service. De standaard metrische gegevens ophalen automatisch gebruikt wanneer er geen andere worden bepaald. 

Nu gaan we door elk van deze instellingen in meer detail en hebben over het gedrag op dat dit van invloed is.

## <a name="load"></a>Belasting
De hele punt voor het definiëren van metrische gegevens is om aan te duiden sommige laden. *Load* hoeveel van een metriek is verbruikt door sommige service-exemplaar of de replica op een bepaald knooppunt is. Belasting kan worden geconfigureerd op bijna elk gewenst moment. Bijvoorbeeld:

  - Laden kan worden gedefinieerd als een service wordt gemaakt. Dit heet _standaard load_.
  - De metrische gegevens, met inbegrip van de standaardwaarde is geladen voor een service kunt bijgewerkt nadat de service is gemaakt. Dit heet _een service bijwerken_. 
  - De belasting voor een bepaalde partitie kunnen worden hersteld op de standaardwaarden voor de service. Dit heet _opnieuw instellen van belasting van partitie_.
  - Load worden gerapporteerd op een per per service object dynamisch tijdens runtime. Dit heet _reporting load_. 
  
Alle deze strategieën kan worden gebruikt binnen dezelfde service tijdens zijn levensduur. 

## <a name="default-load"></a>Werklast (standaard)
*Standaard load* is hoeveel van de metrische gegevens elk serviceobject (stateless exemplaar of stateful replica) van deze service verbruikt. De Cluster Resource Manager gebruikt dit nummer voor het laden van het service-object totdat het andere informatie, zoals een rapport van de dynamische belasting bij ontvangt. Voor eenvoudiger services is de belasting van de standaard een statische definitie. De belasting van de standaard nooit is bijgewerkt en wordt gebruikt voor de levensduur van de service. Standaard wordt geladen works ideaal voor eenvoudige scenario's waarbij bepaalde hoeveelheden resources zijn toegewezen aan verschillende werkbelastingen en niet wijzigen voor capaciteitsplanning.

> [!NOTE]
> Zie voor meer informatie over het capaciteitsbeheer en capaciteit voor de knooppunten in het cluster te definiëren, [in dit artikel](service-fabric-cluster-resource-manager-cluster-description.md#capacity).
> 

De Cluster Resource Manager kunt stateful services om op te geven van een andere standaard belasting voor hun primaire en secundaire replica's. Stateless services kunnen alleen een waarde opgeven die van toepassing op alle exemplaren. Voor stateful services, de standaard werkbelasting voor de primaire en secundaire verschillen replica's meestal omdat de replica's kunnen verschillende soorten werk in elke rol. Bijvoorbeeld: primaire meestal fungeren lees- en schrijfbewerkingen en verwerken van de meeste van de rekenkundige last en secundaire replica's niet. Meestal is de werklast (standaard) voor een primaire replica hoger dan de standaard belasting voor de secundaire replica's. De reële getallen moet afhankelijk van uw eigen metingen.

## <a name="dynamic-load"></a>Dynamische belasting bij
Stel dat u uw service een tijdje hebt actief. U hebt die opgemerkt met sommige bewaking:

1. Sommige partities of verschillende exemplaren van een bepaalde service meer bronnen dan andere gebruiken
2. Sommige services een belasting die gedurende een bepaalde periode varieert.

Er is een groot aantal dingen die dergelijke fluctuaties load kunnen veroorzaken. Bijvoorbeeld, zijn verschillende services of partities gekoppeld aan verschillende klanten met verschillende vereisten. Load kan ook wijzigen, omdat de hoeveelheid werk die de service beschikt in de loop van de dag varieert. Ongeacht de reden is er meestal geen enkel getal die u voor standaard gebruiken kunt. Dit geldt vooral als u wilt ophalen van het gebruik van de meeste buiten het cluster. Een waarde die u voor de werklast (standaard kiest) is onjuist enkele van de tijd. Onjuiste standaardinstellingen laadt resultaat in het Cluster Resource Manager boven of onder het toewijzen van resources. Als gevolg hiervan hebt u knooppunten die boven of onder benut worden ondanks dat de Cluster Resource Manager denkt dat het cluster wordt verdeeld. Standaard laadt zijn nog steeds goed, omdat ze bepaalde informatie voor de eerste positie bieden, maar ze nog niet voltooid verhaal voor echte werkbelastingen. Om vast te leggen nauwkeurig veranderende resourcevereisten, kan de Cluster Resource Manager elk serviceobject een eigen load bijwerken tijdens runtime. Dit de dynamische belasting bij reporting wordt genoemd.

Rapporten van de dynamische belasting bij toestaan replica's of exemplaren om aan te passen van hun toewijzing/gerapporteerd belasting van metrische gegevens gedurende hun levensduur. Meestal rapporteren dat het lage hoeveelheden een metriek is geïnstalleerd via een replica van de service of het exemplaar dat koude en niet doen. Een bezet replica of instance rapporteren dat ze meer gebruiken.

Reporting load per replica of het exemplaar, kunt de Cluster Resource Manager opnieuw indelen van de afzonderlijke service-objecten in het cluster. Opnieuw indelen van de services, kunt ervoor zorgen dat ze de resources die ze nodig hebben. Bezet services ophalen effectief ' vrijmaken ' bronnen van andere replica's of exemplaren die momenteel koude of minder werk te doen.

Binnen Reliable Services is de code voor het rapporteren over de belasting dynamisch ziet er als volgt:

Code:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

Een service kan rapporteren dat op een van de metrische gegevens gedefinieerd tijdens het maken. Als een service-rapporten belasting voor een waarde die niet is geconfigureerd voor gebruik, negeert Service Fabric die rapporteren. Als er andere metrische gegevens op hetzelfde moment gerapporteerd die geldig zijn, worden deze rapporten worden geaccepteerd. Service-code kunt meten en rapporteren van alle metrische gegevens die deze procedures, en de configuratie te gebruiken zonder dat de code wijzigen door operators kunnen opgeven. 

### <a name="updating-a-services-metric-configuration"></a>Configuratie van een service bijwerken
De lijst met metrische gegevens die zijn gekoppeld aan de service en de eigenschappen van deze metrische gegevens kunnen dynamisch worden bijgewerkt terwijl de service live wordt. Hiermee om te experimenteren en flexibiliteit. Enkele voorbeelden van wanneer dit handig is zijn:

  - een metriek met een buggy rapport voor een bepaalde service uitschakelen
  - het gewicht van de metrische gegevens op basis van het gewenste gedrag opnieuw configureren
  - inschakelen van een nieuwe waarde pas nadat de code al is geïmplementeerd en gevalideerd via andere methoden
  - het wijzigen van de belasting van de standaardwaarde voor een service op basis van waargenomen gedrag en het verbruik

De belangrijkste API's voor het wijzigen van de configuratie zijn `FabricClient.ServiceManagementClient.UpdateServiceAsync` in C# en `Update-ServiceFabricService` in PowerShell. Welke informatie u met deze API's opgeeft vervangt de bestaande metrische gegevens voor de service onmiddellijk. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>De combinatie van waarden van standaard load en rapporten van de dynamische belasting bij
Standaard-belasting en de dynamische belasting kunnen worden gebruikt voor dezelfde service. Wanneer een service maakt gebruik van zowel de werklast (standaard) en de dynamische belasting bij rapporten, wordt standaard load fungeert als een schatting totdat u dynamische rapporten worden weergegeven. Standaard load is goed, omdat dit de Cluster Resource Manager iets biedt werken met. De werklast (standaard) kunt de Cluster Resource Manager-service-objecten in goede locaties plaatsen wanneer ze worden gemaakt. Als er geen standaard-informatie laden is opgegeven, wordt een effectief willekeurige plaatsing van services. Als load rapporten later binnenkomen de initiële willekeurige plaatsing is vaak verkeerde en de Cluster Resource Manager te verplaatsen van services.

We nemen van het vorige voorbeeld en zien wat er gebeurt wanneer we enkele aangepaste metrische gegevens en de dynamische belasting bij reporting toevoegt. In dit voorbeeld gebruiken we 'MemoryInMb' als een voorbeeld van de meting.

> [!NOTE]
> Geheugen is een van de system metrische gegevens die Service Fabric kunt [resource reguleren](service-fabric-resource-governance.md), en uzelf reporting is doorgaans moeilijk. We niet daadwerkelijk verwacht dat u voor het rapporteren van geheugenverbruik; Geheugen wordt gebruikt als hulp bij het leren over de mogelijkheden van het Cluster Resource Manager hier.
>

We gaan ervan uit dat we in eerste instantie de stateful service met de volgende opdracht gemaakt:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Deze syntaxis is als een herinnering (MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad').

Kijken wat een mogelijke cluster indeling kan er als volgt uitzien:

<center>
![Cluster Gebalanceerd met zowel standaard als aangepaste metrische gegevens][Image2]
</center>

Een aantal zaken die opgemerkt zijn:

* Secundaire replica's binnen een partitie kunnen hebben hun eigen laden
* De metrische gegevens, zoek de algemene taakverdeling. Voor geheugen, is de verhouding tussen de maximale en minimale belasting 1,75 (het knooppunt met de meeste load N3, is de minst is N2 en 28/16 = 1,75).

Er zijn een aantal zaken die we nog steeds nodig om uit te leggen:

* Wat bepaald of een ratio van 1,75 redelijke of niet is? Hoe weet de Cluster Resource Manager als die voldoende is of als er meer werk te doen?
* Wanneer balancing treedt?
* Wat betekent dit dat geheugen 'Hoog' werd gewogen?

## <a name="metric-weights"></a>Metrische gewichten
Dezelfde metrische gegevens bijhouden over verschillende services is belangrijk. Globale weergave is wat de Cluster Resource Manager verbruik in het cluster volgen en zorg ervoor dat de knooppunten niet gaan over de capaciteit verbruik op alle knooppunten in balans is toegestaan. Services hebben echter verschillende weergaven over het belang van de dezelfde metrische gegevens. Ook in een cluster met veel metrische gegevens en partijen van services, perfect taakverdeling oplossingen bestaat mogelijk niet voor alle metrische gegevens. Hoe moet de Cluster Resource Manager in deze situaties verwerken?

Metrische gewichten toestaan dat de Cluster Resource Manager om te bepalen hoe u het cluster kan worden verdeeld wanneer er niet perfect opgenomen wordt. Metrische gewicht dat de Cluster Resource Manager voor een evenwichtige specifieke services anders. Metrische gegevens kunt laten vier verschillende gewicht niveaus: nul, laag, gemiddeld en hoog. Een metriek met een gewicht van nul bijdraagt niets wanneer u overweegt of dingen of niet evenwicht zijn. De belasting echter nog steeds dragen bij aan capaciteit management. Metrische gegevens met nul gewicht zijn nog steeds nuttig en worden vaak gebruikt als onderdeel van het servicegedrag en prestatiebewaking. [In dit artikel](service-fabric-diagnostics-event-generation-infra.md) bevat meer informatie over het gebruik van metrische gegevens voor controle en diagnostische gegevens van uw services. 

De werkelijke invloed van verschillende metrische gewichten in het cluster is dat de Cluster Resource Manager verschillende oplossingen genereert. Metrische gewichten geven de Cluster Resource Manager dat bepaalde metrische gegevens belangrijker dan andere zijn. Wanneer er geen ideale oplossing is de Cluster Resource Manager kunt geven de voorkeur oplossingen die de hogere gewogen metrische gegevens beter kan worden verdeeld. Als een service denkt een bepaalde waarde is niet belangrijk is dat, dat kan dit het gebruik van deze metrische gegevens van imbalanced vinden. Hierdoor kan een andere service een oneven distributie van sommige metrische gegevens die belangrijk voor het ophalen.

Bekijk een voorbeeld van sommige rapporten laden en hoe verschillende metriek gewicht resulteert in een andere toewijzingen aan in het cluster. In dit voorbeeld ziet u dat de Cluster Resource Manager voor het maken van verschillende regelingen van services overschakelen van het relatieve gewicht van de metrische gegevens veroorzaakt.

<center>
![Voorbeeld van de metrische gewicht en de invloed ervan op oplossingen Netwerktaakverdeling][Image3]
</center>

In dit voorbeeld zijn er vier verschillende services, alle reporting verschillende waarden voor twee verschillende maatstaven voor MetricA en MetricB. In één geval alle services definiëren MetricA is het belangrijkste instrument (gewicht = hoog) en MetricB als niet belangrijk (gewicht = laag). Als gevolg hiervan, ziet u dat de Cluster Resource Manager Hiermee plaatst u de services zodat MetricA is beter dan MetricB taakverdeling. 'Beter taakverdeling' betekent dat MetricA een lagere heeft heeft een lagere standaarddeviatie dan MetricB. In het tweede geval omkeren we de metrische gewichten. Hierdoor wordt de Cluster Resource Manager services A en B verzinnen van een toewijzing waar MetricB is een betere taakverdeling dan MetricA.

> [!NOTE]
> Metrische gewichten bepalen hoe de Cluster Resource Manager moet in evenwicht zijn, maar niet wanneer balancing moet gebeuren. Bekijk voor meer informatie over Netwerktaakverdeling, [in dit artikel](service-fabric-cluster-resource-manager-balancing.md)
>

### <a name="global-metric-weights"></a>Globale metrische gewichten
Stel dat ServiceA definieert MetricA zoals hoge gewicht en XPb stelt het gewicht voor MetricA laag of nul. Wat is het werkelijke gewicht dat belandt ophalen gebruikt?

Er zijn meerdere gewichten die worden voor elke metriek bijgehouden. De eerste dikte is gedefinieerd voor de metriek wanneer de service wordt gemaakt. Het andere gewicht is een globale gewicht, wordt automatisch berekend. De Cluster Resource Manager gebruikt deze beide gewichten wanneer score berekenen voor oplossingen. Het is belangrijk rekening houdend met beide gewichten. Hiermee wordt de Cluster Resource Manager voor elke service op basis van een eigen prioriteiten worden verdeeld en er ook voor zorgen dat de cluster als geheel correct wordt toegewezen.

Wat er zou gebeuren als de Cluster Resource Manager globale en lokale saldo niet interesseren? Het is ook eenvoudig bouwen van oplossingen die globaal in evenwicht zijn, maar die leiden tot slechte resourceverdeling voor afzonderlijke services. In het volgende voorbeeld gaan we kijken naar een service die is geconfigureerd met alleen de standaard metrische gegevens en kijk wat er gebeurt wanneer alleen globale saldo wordt beschouwd als:

<center>
![De Impact van een globale alleen oplossing][Image4]
</center>

In het bovenste voorbeeld op basis van de globale verdelen, wordt de cluster als geheel inderdaad verdeeld. Alle knooppunten hebben de dezelfde telling van de primaire en de hetzelfde aantal totaal aantal replica's. Echter, als u naar de werkelijke impact van deze toewijzing kijkt is het niet zo goed: het verlies van een willekeurig knooppunt van invloed is op een bepaalde belasting, omdat het duurt alle van de primaire voordat. Bijvoorbeeld, als het eerste knooppunt de drie primaire voor drie verschillende partities van de cirkel-service mislukt is alle verloren gegaan. Als u daarentegen hebben de driehoek en zeshoek services hun partities verliezen van een replica. Dit zorgt ervoor dat niet wordt onderbroken, behalve voor het herstellen van de replica omlaag.

De Cluster Resource Manager heeft in het voorbeeld onder de replica's op basis van het saldo globale en per-service worden gedistribueerd. Bij het berekenen van de score van de oplossing biedt het grootste deel van het gewicht globale oplossing en een (configureerbaar) deel aan afzonderlijke services. Globale verdelen voor een waarde wordt berekend op basis van het gemiddelde van de metrische gewicht van elke service. Elke service, wordt verdeeld volgens een eigen gedefinieerde metrische gewichten. Dit zorgt ervoor dat de services in binnen zelf volgens hun eigen behoeften evenwicht zijn. Als het eerste knooppunt de fout mislukt is als gevolg hiervan verdeeld over alle partities van alle services. De gevolgen voor elk is hetzelfde.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Defragmentatie metrische gegevens definiëren, is een manier om te consolideren belasting van de knooppunten in plaats van af te spreiden. Raadpleeg voor meer informatie over het configureren van defragmentatie, [in dit artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- Als u wilt weten over hoe de Cluster Resource Manager beheert en een compromis tussen de werklast van het cluster, Raadpleeg het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)
- Vanaf het begin starten en [Maak kennis met de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
- Verplaatsingskosten is één manier om de Cluster Resource Manager-signalering dat bepaalde services zijn duurder dan de andere verplaatsen. Raadpleeg voor meer informatie over verplaatsingskosten [in dit artikel](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
