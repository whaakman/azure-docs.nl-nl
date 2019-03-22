---
title: Azure Service Fabric-app laden met metrische gegevens beheren | Microsoft Docs
description: Meer informatie over het configureren en gebruiken van metrische gegevens in Service Fabric voor het beheren van service resourceverbruik.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 0d622ea6-a7c7-4bef-886b-06e6b85a97fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 642f479aba62e5cc9dde63aed7c30de39b513a5e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58093346"
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Beheren van resourceverbruik en laden in Service Fabric met metrische gegevens
*Metrische gegevens* zijn de resources die uw care services over en die worden geleverd door de knooppunten in het cluster. Een metrische waarde is alles wat u wilt beheren om te verbeteren of de prestaties van uw services controleren. U kunt bijvoorbeeld geheugenverbruik als u wilt weten als uw service is overbelast bekijken. Een andere toepassing is om te achterhalen of de service elders waar geheugen dat minder beperkt is om betere prestaties kan verplaatsen.

Items zoals geheugen, schijf en de CPU-gebruik zijn voorbeelden van metrische gegevens. Deze metrische gegevens zijn fysieke metrische gegevens en resources die overeenkomen met de fysieke resources op het knooppunt dat moet worden beheerd. Metrische gegevens kan ook worden (en vaak zijn) logische metrische gegevens. Logische metrische gegevens zijn dingen als 'MyWorkQueueDepth' of 'MessagesToProcess' of 'TotalRecords'. Logische metrische gegevens worden door de toepassing worden gedefinieerd en indirect komen overeen met het gebruik van bepaalde fysieke resources. Logische metrische gegevens zijn algemene omdat het kan moeilijk te meten en het rapport verbruik van fysieke resources op basis van de per-service. De complexiteit van het meten en rapporteren van uw eigen fysieke metrische gegevens is ook waarom Service Fabric biedt enkele standaard metrische gegevens.

## <a name="default-metrics"></a>Standaard metrische gegevens
Stel dat u wilt aan de slag schrijven en implementeren van uw service. Op dit moment weet niet welke fysieke of logische resources verbruikt. Dat is prima! De Service Fabric Cluster Resource Manager maakt gebruik van bepaalde standaard metrische gegevens wanneer er geen andere metrische gegevens zijn opgegeven. Dit zijn:

  - PrimaryCount - telling van de primaire replica's op het knooppunt 
  - ReplicaCount - telling van de totale stateful replica's op het knooppunt
  - Aantal - telling van alle service-objecten (staatloze en stateful) op het knooppunt

| Gegevens | Stateless exemplaar laden | Stateful secundaire laden | Stateful primaire laden | Gewicht |
| --- | --- | --- | --- | --- |
| PrimaryCount |0 |0 |1 |Hoog |
| ReplicaCount |0 |1 |1 |Middelgroot |
| Count |1 |1 |1 |Laag |


Voor algemene werkbelastingen bieden de standaard metrische gegevens over een goede verdeling van het werk in het cluster. In het volgende voorbeeld, moet u gaan we kijken wat er gebeurt wanneer we twee services en zijn afhankelijk van de standaard metrische gegevens voor taakverdeling. De eerste service is een stateful service met drie partities en een doel replicaset grootte van drie. De tweede service is een staatloze service met één partitie en een aantal exemplaren van drie.

Dit is wat u krijgt:

<center>

![Cluster-indeling met standaard metrische gegevens][Image1]
</center>

Dingen om te weten:
  - Primaire replica's voor de stateful service worden verdeeld over meerdere knooppunten
  - Replica's voor dezelfde partitie worden op verschillende knooppunten
  - Het totale aantal primaire en secundaire replica's wordt gedistribueerd in het cluster
  - Het totale aantal service-objecten zijn gelijkmatig verdeeld op elk knooppunt

Goede!

De standaard metrische gegevens werkt uitstekend als een begin. Echter, de standaard metrische gegevens alleen hebben u tot nu toe. Bijvoorbeeld: Wat is de kans dat het partitioneren van u-schema opgenomen resultaten in perfect zelfs-gebruik door alle partities? Wat is de kans dat de belasting voor een bepaalde service constant na verloop van tijd is, of zelfs dezelfde over meerdere partities nu?

U kunt uitvoeren met alleen de standaard metrische gegevens. Echter meestal doen betekent dat uw clustergebruik lager en meer ongelijke dan u wilt. Dit is omdat de standaard metrische gegevens worden niet adaptieve en wordt ervan uitgegaan dat alles is gelijk. Bijvoorbeeld, bijdragen een primaire die bezig is en één die niet beide "1" in de metriek PrimaryCount. In het ergste geval, met behulp van alleen de standaard metrische gegevens kan ook leiden tot overscheduled knooppunten leidt tot prestatieproblemen. Als u geïnteresseerd bent in het cluster optimaal te benutten en prestatieproblemen voorkomen, moet u het gebruik van aangepaste metrische gegevens en dynamische werkbelastingsrapportage.

## <a name="custom-metrics"></a>Aangepaste metrische gegevens
Metrische gegevens zijn geconfigureerd op basis van de per-met de naam-service-exemplaar, wanneer u de service maakt.

Elke meetwaarde heeft enkele eigenschappen die het beschrijven: een naam, een gewicht en een standaard-belasting.

* Naam van de metrische gegevens: De naam van de metrische gegevens. De naam van de meetwaarde is een unieke id voor de metrische gegevens in het cluster op basis van de Resource Manager-perspectief.
* Gewicht: Gewicht van de metrische definieert hoe belangrijk met deze metriek is ten opzichte van de andere metrische gegevens voor deze service.
* Werklast (standaard): De standaard-belasting is anders, afhankelijk van de service stateless of stateful weergegeven.
  * Voor stateless services heeft alle gegevens één eigenschap met de naam DefaultLoad
  * U definieert voor stateful services:
    * PrimaryDefaultLoad: De standaardinstelling van deze metrische gegevens van deze service verbruikt wanneer het een primaire
    * SecondaryDefaultLoad: De standaardinstelling van deze metrische gegevens van deze service verbruikt wanneer deze een secundair

> [!NOTE]
> Als u aangepaste metrische gegevens definieert en u wilt _ook_ gebruiken de standaard metrische gegevens, moet u _expliciet_ de standaard metrische gegevens back-ups maken en gewicht en -waarden definiëren voor deze toevoegen. Dit komt doordat moet u de relatie tussen de standaard metrische gegevens en uw aangepaste metrische gegevens definiëren. Bijvoorbeeld, wellicht u het belangrijkst ConnectionCount of WorkQueueDepth meer dan primaire distributie. Standaard is het gewicht van de metriek PrimaryCount hoog, zodat u beperken op Gemiddeld wilt, wanneer u uw andere metrische gegevens om te controleren of dat ze voorrang toevoegt.
>

### <a name="defining-metrics-for-your-service---an-example"></a>Metrische gegevens voor uw service, een voorbeeld definiëren
Stel dat u wilt dat de volgende configuratie:

  - Uw service rapporteert een metrische waarde met de naam "ConnectionCount"
  - Wilt u ook de standaard metrische gegevens gebruiken 
  - U enkele metingen hebben gedaan en u weet dat normaal gesproken een primaire replica van de betreffende service maximaal 20 eenheden van "ConnectionCount duurt"
  - Secundaire replica's gebruiken "ConnectionCount" 5-eenheden
  - U weet dat 'ConnectionCount' is de belangrijkste metrische gegevens in termen van de prestaties van deze bepaalde service beheren
  - U wordt nog steeds primaire replica's met gelijke taakverdeling. Taakverdeling van de primaire replica's is doorgaans een goed idee, ongeacht wat. Dit helpt te voorkomen dat het verlies van een domein-knooppunt of fouten die invloed hebben op een meerderheid van de primaire replica's samen met het. 
  - Anders wordt zijn de standaard metrische gegevens prima

Dit is de code die u schrijft om te maken van een service met deze configuratie:

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
> De bovenstaande voorbeelden en de rest van dit document wordt beschreven beheren metrische gegevens op basis van de per-met de naam-service. Het is ook mogelijk om te definiëren van metrische gegevens voor uw services van de service _type_ niveau. Dit wordt bereikt door ze in de servicemanifesten op te geven. Het definiëren van metingen op het niveau van type wordt niet aanbevolen om verschillende redenen. De eerste reden is dat metrische namen vaak omgevingsspecifieke zijn. Tenzij er een contract vast aanwezig is, kan u ervoor dat de metriek "Kernen" in een omgeving 'MiliCores' of "Kernen" niet in andere niet. Als uw metrische gegevens zijn gedefinieerd in uw manifest die u wilt maken van nieuwe manifesten per omgeving. Dit leidt meestal tot een toename van verschillende manifesten met alleen kleine verschillen, wat tot problemen bij het beheer leiden kunnen.  
>
> Metrische gegevens geladen zijn vaak toegewezen op basis van de per-met de naam-service-exemplaar. Stel dat u één exemplaar van de service voor CustomerA die alleen licht gebruik wil maken. Ook Stel dat u nog een voor CustomerB die een grotere werkbelasting heeft maakt. In dit geval wilt u waarschijnlijk de belasting voor de standaardwaarde voor deze services aanpassen. Als u metrische gegevens en belasting gedefinieerd via manifesten en u wilt ondersteunen in dit scenario, is er andere toepassing en servicetypen voor elke klant vereist. De waarden die worden gedefinieerd tijdens de aanmaak van de service overschrijven die zijn gedefinieerd in het manifest, zodat u die gebruiken kunt om in te stellen van de specifieke standaardwaarden. Echter, dat zorgt ervoor dat de waarden in de manifesten die niet overeenkomt met die de service daadwerkelijk wordt uitgevoerd met gedeclareerd. Dit kan leiden tot verwarring. 
>

Als een herinnering: als u alleen gebruiken de standaard metrische gegevens wilt, moet u niet de metrische gegevens verzameling helemaal touch of niets te doen bij het maken van uw service. De standaard metrische gegevens ophalen automatisch gebruikt wanneer er geen andere zijn gedefinieerd. 

Nu we gaan door elk van deze instellingen in meer detail en praten over het gedrag op dat dit van invloed op.

## <a name="load"></a>Belasting
Het hele punt van het definiëren van metrische gegevens is voor sommige laden. *Load* welk deel van een bepaalde waarde wordt gebruikt door sommige service-exemplaar of de replica op een bepaald knooppunt is. Belasting kan worden geconfigureerd op bijna elk gewenst moment. Bijvoorbeeld:

  - Belasting kan worden gedefinieerd als een service wordt gemaakt. Dit heet _standaard load_.
  - De metrische gegevens, met inbegrip van de standaard is geladen voor een service kan worden bijgewerkt nadat de service is gemaakt. Dit heet _bijwerken van een service_. 
  - De belasting voor een bepaalde partitie kunnen opnieuw worden ingesteld op de standaardwaarden voor de service. Dit heet _opnieuw instellen van partitiebelasting_.
  - Laden kan worden gerapporteerd op een per service-object gefactureerd dynamisch tijdens runtime. Dit heet _reporting load_. 
  
Al deze strategieën kan worden gebruikt binnen dezelfde service tijdens hun levensduur. 

## <a name="default-load"></a>Standaardwaarde laden
*Standaard load* is hoeveel van de metrische gegevens die elke serviceobject (exemplaar stateless of stateful replica's) van deze service worden verbruikt. Cluster Resource Manager gebruikt dit nummer voor de belasting van het serviceobject totdat het andere informatie, zoals een rapport van de dynamische belasting ontvangt. Voor eenvoudiger services is de standaard-belasting een statische definitie. De standaard-belasting nooit is bijgewerkt en wordt gebruikt voor de levensduur van de service. Standaard wordt geladen werkt goed voor eenvoudige scenario's waarbij bepaalde hoeveelheden van resources zijn toegewezen aan verschillende werkbelastingen en niet wijzigen voor capaciteitsplanning.

> [!NOTE]
> Zie voor meer informatie over het capaciteitsbeheer van de en het definiëren van capaciteit voor de knooppunten in uw cluster, [in dit artikel](service-fabric-cluster-resource-manager-cluster-description.md#capacity).
> 

Cluster Resource Manager kunnen stateful services om op te geven van een andere standaard belasting voor de primaire en secundaire replica's. Stateless services kunnen alleen een waarde opgeven die van toepassing op alle exemplaren. Voor stateful services, het laden van de standaard voor primaire en secundaire verschillen replica's meestal omdat de replica's kunnen verschillende soorten werk in elke rol. Bijvoorbeeld, primaire meestal fungeren zowel lees- en schrijfbewerkingen en de meeste van de rekenkundige belasting te verwerken en secundaire replica's niet. Meestal is de standaard-belasting voor een primaire replica hoger dan de standaard-belasting voor secundaire replica's. De reële getallen moet afhankelijk van uw eigen metingen.

## <a name="dynamic-load"></a>Dynamische belasting
Stel dat u uw service een tijdje hebt uitgevoerd. Met enige controle u hebt al opgemerkt dat:

1. Sommige partities of verschillende exemplaren van een bepaalde service meer bronnen dan andere gebruiken
2. Sommige services hebben belasting gedurende een periode varieert.

Er zijn talloze dingen die deze typen load fluctuaties kunnen veroorzaken. Bijvoorbeeld, zijn verschillende services of partities gekoppeld aan verschillende klanten met verschillende vereisten. Belasting kan ook wijzigen, omdat in de loop van de dag is afhankelijk van de hoeveelheid werk die de service beschikt. Ongeacht de reden is het doorgaans geen enkel getal dat u voor de standaardwaarde gebruiken kunt. Dit geldt met name als u wilt ophalen van het gebruik van de meeste buiten het cluster. Een waarde die u voor de werklast (standaard kiest) is onjuist deel van de tijd. Onjuiste standaard geladen resultaat in het Cluster Resource Manager boven of onder het toewijzen van resources. Als gevolg hiervan hebt u knooppunten die boven of onder benut worden ondanks dat de Cluster Resource Manager als dat het cluster is verdeeld. Standaard geladen zijn nog steeds prima geschikt omdat deze bepaalde informatie voor de eerste plaatsing opgeven, maar ze niet een volledige verhaal voor echte workloads zijn. Om vast te leggen nauwkeurig veranderende resourcevereisten, kunnen met Cluster Resource Manager elk serviceobject om bij te werken van een eigen laden tijdens runtime. Dit heet dynamische werkbelastingsrapportage.

Dynamische rapporten kunnen replica's of instanties om aan te passen van hun toewijzing/gerapporteerd laden van metrische gegevens gedurende hun levensduur. Meestal rapporteren dat is het gebruik van lage hoeveelheden een metriek een service-replica of het exemplaar dat is koude en niet doen. Een bezet doelreplica of rapporteren dat ze meer gebruiken.

Belasting per doelreplica of Reporting kunt met Cluster Resource Manager opnieuw indelen van de afzonderlijke serviceobjecten in het cluster. Opnieuw indelen van de services, kunt ervoor zorgen dat ze de resources die ze nodig hebben. Bezet services ophalen effectief "vrij" resources uit andere replica's of exemplaren die momenteel koude of minder werk te maken.

In Reliable Services is de code voor het rapporteren over de belasting dynamisch ziet er als volgt:

Code:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

Een service kan rapporteren dat op een van de metrische gegevens voor deze gedefinieerd tijdens het maken. Als een service-rapporten laden voor metrische gegevens die niet is geconfigureerd voor gebruik, negeert Service Fabric dat rapport. Als er andere metrische gegevens gerapporteerd op hetzelfde moment die geldig zijn, worden deze rapporten worden geaccepteerd. Service-code kunt meten en rapporteren van alle metrische gegevens die deze handleidingen, en operators kunnen geeft u de configuratie van de metrische gegevens gebruiken zonder dat de servicecode te wijzigen. 

### <a name="updating-a-services-metric-configuration"></a>Configuratie van een service bijwerken
De lijst met metrische gegevens die zijn gekoppeld aan de service en de eigenschappen van deze metrische gegevens kunnen dynamisch worden bijgewerkt terwijl de service actief is. Hiermee om te experimenteren en flexibiliteit. Enkele voorbeelden van wanneer dit handig is zijn:

  - een metrische waarde met een buggy rapport voor een bepaalde service uitschakelen
  - het gewicht van de metrische gegevens op basis van het gewenste gedrag opnieuw configureren
  - een nieuwe metrische gegevens inschakelen nadat de code al is geïmplementeerd en geverifieerd via andere methoden
  - wijzigen van de belasting van de standaardwaarde voor een service op basis van waargenomen gedrag en verbruik

De belangrijkste API's voor het wijzigen van de configuratie zijn `FabricClient.ServiceManagementClient.UpdateServiceAsync` in C# en `Update-ServiceFabricService` in PowerShell. De informatie die u met deze API's opgeeft vervangt de bestaande metrische gegevens voor de service onmiddellijk. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Met een combinatie van standaardwaarden laden en dynamische rapporten
Standaard laden en dynamische belasting kunnen worden gebruikt voor dezelfde service. Wanneer een service maakt gebruik van zowel standaard laden en dynamische rapporten, wordt standaard load fungeert als een schatting totdat u dynamische rapporten weergegeven. Standaard load is goed, omdat het Cluster Resource Manager iets om te werken biedt met. De standaard-belasting kan met Cluster Resource Manager de serviceobjecten in goede locaties plaatsen wanneer ze worden gemaakt. Als er geen standaard-informatie laden is opgegeven, wordt een effectief willekeurige plaatsing van services. Bij het laden rapporten later ontvangen van de plaatsing van de eerste willekeurige is vaak verkeerde en met Cluster Resource Manager heeft diensten verplaatsen.

Laten we het vorige voorbeeld nemen en kijken wat er gebeurt wanneer we enkele aangepaste metrische gegevens en dynamische werkbelastingsrapportage toevoegt. In dit voorbeeld gebruiken we 'MemoryInMb' als een voorbeeld van de metrische gegevens.

> [!NOTE]
> Geheugen is een van de-metrische systeemmeetgegevens die Service Fabric kan [resource beheren](service-fabric-resource-governance.md), en zelf reporting is doorgaans moeilijk. Er niet daadwerkelijk wordt verwacht dat u voor het rapporteren van geheugenverbruik; Geheugen wordt gebruikt als hulpmiddel om te leren over de mogelijkheden van het Cluster Resource Manager.
>

We gaan ervan uit dat we de stateful service oorspronkelijk is gemaakt met de volgende opdracht:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Deze syntaxis is als een herinnering ("MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad').

We gaan nu kijken welke een mogelijke cluster indeling kan er als volgt uitzien:

<center>

![Cluster Gebalanceerd met zowel standaard als aangepaste metrische gegevens][Image2]
</center>

Dingen die vermelden waard zijn:

* Secundaire replica's binnen een partitie kunnen hebben elk hun eigen laden
* De metrische gegevens zoeken algemene met gelijke taakverdeling. Voor geheugen, is de verhouding tussen de maximale en minimale belasting 1,75 (het knooppunt met de meeste load N3, is het minst is N2 en 28/16 = 1,75).

Er zijn enkele dingen die we nog steeds nodig hebben om uit te leggen:

* Wat bepaald of een ratio van 1,75 redelijke of niet is? Hoe weet met Cluster Resource Manager als die goed genoeg is, of als er meer werk te doen?
* Wanneer wordt balancing uitgevoerd?
* Wat betekent dat geheugen 'Hoog' werd gewogen?

## <a name="metric-weights"></a>Gewicht van metrische gegevens
Dezelfde metrische gegevens bijhouden over verschillende services is belangrijk. Globale weergave is wat de Cluster Resource Manager bijhouden van gebruik door in het cluster en ervoor te zorgen dat knooppunten niet via capaciteit lopen gebruik op alle knooppunten in balans is toegestaan. Services hebben echter verschillende weergaven over het belang van de dezelfde metrische gegevens. Ook in een cluster met veel metrische gegevens en veel services perfect met gelijke taakverdeling oplossingen bestaat mogelijk niet voor alle metrische gegevens. Hoe moet deze situaties verwerken met Cluster Resource Manager?

Gewicht van metrische gegevens kunnen met Cluster Resource Manager om te bepalen hoe het cluster in balans brengen wanneer er geen perfecte antwoord. Gewicht van metrische gegevens kunnen ook met Cluster Resource Manager anders afwegen van specifieke services. Metrische gegevens kunt laten vier verschillende gewicht niveaus: Nul, laag, gemiddeld en hoog. Een metrische waarde met een gewicht van nul bijdraagt niets wanneer u overweegt of dingen worden verdeeld, of niet. De belasting echter nog steeds dragen bij aan capaciteit management. Metrische gegevens met nul gewicht zijn nog steeds nuttig en worden vaak gebruikt als onderdeel van service-gedrag en de bewaking van toepassingsprestaties. [In dit artikel](service-fabric-diagnostics-event-generation-infra.md) vindt u meer informatie over het gebruik van metrische gegevens voor bewaking en diagnostische gegevens van uw services. 

De werkelijke invloed van verschillende metrische gewichten in het cluster is dat de Cluster Resource Manager verschillende oplossingen genereert. Metrische waarden geven met Cluster Resource Manager dat bepaalde metrische gegevens belangrijker dan andere zijn. Wanneer er geen perfecte oplossing is met Cluster Resource Manager kunt geven de voorkeur oplossingen die de hogere gewogen metrische gegevens beter in balans brengen. Als een service als dat een bepaalde meetwaarde is niet belangrijk is, dat kan deze het gebruik van deze metrische gegevens van imbalanced vinden. Dit kan een andere service een gelijkmatige verdeling van sommige metrische gegevens die belangrijk is voor het ophalen.

We bekijken een voorbeeld van sommige rapporten laden en hoe verschillende metrische gegevens gewicht resultaten in andere toewijzingen in het cluster. In dit voorbeeld ziet u dat het relatieve gewicht van de metrische gegevens over te schakelen met Cluster Resource Manager te maken van verschillende structuren van services veroorzaakt.

<center>

![Gewicht van de metrische voorbeeld en de invloed ervan op taakverdelingsoplossingen][Image3]
</center>

In dit voorbeeld zijn er vier verschillende services, alle reporting verschillende waarden voor twee verschillende metrische gegevens, MetricA en MetricB. Voor alle services definiëren MetricA is de belangrijkste (gewicht = hoog) en MetricB als onbelangrijk (gewicht = laag). Als gevolg hiervan, ziet u dat de Cluster Resource Manager de services plaatst, zodat MetricA beter dan MetricB wordt verdeeld. 'Beter met gelijke taakverdeling' betekent dat MetricA een lagere heeft heeft een lagere standaarddeviatie dan MetricB. In het tweede geval omkeren we de metrische waarden. Als gevolg hiervan is met Cluster Resource Manager worden verwisseld services A en B te komen met een toewijzing waarbij MetricB is beter dan MetricA met gelijke taakverdeling.

> [!NOTE]
> Metrische waarden te bepalen hoe met Cluster Resource Manager moet in evenwicht zijn, maar niet wanneer taakverdeling moet gebeuren. Bekijk voor meer informatie over Netwerktaakverdeling, [in dit artikel](service-fabric-cluster-resource-manager-balancing.md)
>

### <a name="global-metric-weights"></a>Algemene metrische gewicht
Stel ServiceA definieert MetricA als hoge gewicht en XPb stelt het gewicht voor MetricA op laag of nul. Wat is het werkelijke gewicht die eindigt ophalen gebruikt?

Er zijn meerdere gewichten die worden voor elke metrische gegevens bijgehouden. Het eerste gewicht is gedefinieerd voor de metrische gegevens wanneer de service is gemaakt. Het gewicht van andere is een algemene gewicht, die automatisch wordt berekend. Cluster Resource Manager gebruikt deze beide gewichten wanneer scoring-oplossingen. Het is belangrijk rekening houdend met beide gewicht. Hiermee wordt de Cluster Resource Manager voor elke service op basis van een eigen prioriteiten in balans brengen en ook voor zorgen dat het cluster als geheel correct is toegewezen.

Wat er zou gebeuren als met Cluster Resource Manager is niet het belangrijkst globale en lokale saldo? Goed, is het eenvoudig om oplossingen die wereldwijd worden verdeeld, maar die leiden tot slechte resourceverdeling voor afzonderlijke services samen te stellen. In het volgende voorbeeld gaan we kijken naar een service die is geconfigureerd met alleen de standaard metrische gegevens en kijken wat er gebeurt wanneer alleen globale taakverdeling wordt beschouwd als:

<center>

![De Impact van een globale oplossing voor alleen][Image4]
</center>

In het bovenste voorbeeld alleen op basis van algemene saldo, wordt het cluster als geheel inderdaad verdeeld. Alle knooppunten hebben de dezelfde aantal primaire en de hetzelfde aantal totaal aantal replica's. Als u de werkelijke impact van deze toewijzing bekijkt het is echter niet zo goed waren: het verlies van gegevens van een willekeurig knooppunt van invloed is op een bepaalde belasting, omdat het duurt u alle van de voorverkiezingen uit te brengen voordat. Bijvoorbeeld, als het eerste knooppunt, de drie primaire voor de drie verschillende partities van de cirkel service mislukt zou alle verloren. De driehoek en zeshoek services hebben echter hun partities verloren gaan van een replica. Dit zorgt ervoor dat er geen onderbreking, behalve de omlaag replica herstellen.

Cluster Resource Manager heeft in het voorbeeld onder de replica's op basis van het saldo global en per-service worden gedistribueerd. Bij het berekenen van de score van de oplossing geeft het gewicht de meeste tot het globale oplossing en een gedeelte (configureerbaar) tot afzonderlijke services. Globale saldo voor een metrische waarde wordt berekend op basis van het gemiddelde van de metrische waarden van elke service. Elke service is met gelijke taakverdeling op basis van een eigen gedefinieerde metrische gewicht. Dit zorgt ervoor dat de services worden verdeeld in zelf op basis van hun eigen behoeften. Als gevolg hiervan als de fout is het eerste knooppunt mislukt wordt verdeeld over alle partities van alle services. De impact op elk is hetzelfde.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Defragmentatie metrische gegevens definiëren is één manier om te laden op knooppunten in plaats van deze uitspreiden consolideren. Raadpleeg voor informatie over het configureren van de defragmentatie, [in dit artikel](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
- Als u wilt weten over hoe met Cluster Resource Manager beheert en verdeelt de taken in het cluster, Zie het artikel op [taakverdeling](service-fabric-cluster-resource-manager-balancing.md)
- Vanaf het begin starten en [een inleiding tot de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
- Kosten voor gegevensverplaatsing is één manier om naar de Cluster Resource Manager-signalering dat bepaalde services zijn duurder dan de andere verplaatsen. Raadpleeg voor meer informatie over de kosten voor gegevensverplaatsing, [in dit artikel](service-fabric-cluster-resource-manager-movement-cost.md)

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
