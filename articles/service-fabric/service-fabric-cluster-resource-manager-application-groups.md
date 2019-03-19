---
title: Service Fabric Cluster Resource Manager - toepassingsgroepen | Microsoft Docs
description: Overzicht van de functionaliteit van de groep van toepassingen in de Service Fabric Cluster Resource Manager
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 94ae233f8591c43afa1bb73c3e17964922967d36
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123025"
---
# <a name="introduction-to-application-groups"></a>Inleiding tot toepassingsgroepen
De service Fabric Cluster Resource Manager beheert doorgaans de clusterbronnen door de belasting te spreiden (vertegenwoordigd [metrische gegevens](service-fabric-cluster-resource-manager-metrics.md)) gelijkmatig in het cluster. Service Fabric beheert de capaciteit van de knooppunten in het cluster en het cluster als geheel via [capaciteit](service-fabric-cluster-resource-manager-cluster-description.md). Metrische gegevens en -capaciteit werken ideaal voor veel werkbelastingen, maar de patronen die intensief gebruik van verschillende exemplaren van Service Fabric-toepassing soms binnenhalen van de aanvullende vereisten. U kunt bijvoorbeeld op:

- Sommige capaciteit op de knooppunten in het cluster voor de services binnen een benoemde toepassingsexemplaar reserveren
- Het totale aantal knooppunten die de services binnen een benoemde toepassingsexemplaar uitvoeren op (in plaats van ze uit te spreiden over het hele cluster)
- Capaciteit op het benoemde exemplaar zelf om te beperken het nummer van services of totaal resourceverbruik van de services binnen definiëren

Om te voldoen aan deze vereisten, de Service Fabric Cluster Resource Manager biedt ondersteuning voor een functie met de naam van toepassingsgroepen.

## <a name="limiting-the-maximum-number-of-nodes"></a>Het maximum aantal knooppunten te beperken
De eenvoudigste use-case voor capaciteit van een toepassing is wanneer een exemplaar van de toepassing moet worden beperkt tot een bepaalde maximum aantal knooppunten. Hiermee worden alle services in de toepassingsinstantie van die naar een bepaald aantal machines samengevoegd. Consolidatie is nuttig wanneer u probeert te voorspellen of fysieke resource gebruiken door de services binnen die met de naam toepassingsexemplaar cap. 

De volgende afbeelding ziet het exemplaar van een toepassing met en zonder een maximum aantal knooppunten dat is gedefinieerd:

<center>

![Maximum aantal knooppunten definiëren toepassingsexemplaar][Image1]
</center>

De toepassing beschikt niet over een maximum aantal knooppunten dat is gedefinieerd in het voorbeeld naar links en er drie services. Cluster Resource Manager heeft van alle replica's verdeeld over de zes beschikbare knooppunten om de beste balans in het cluster (de standaardinstelling). In de juiste voorbeeld ziet u dezelfde toepassing beperkt tot drie knooppunten.

De parameter die het gedrag van deze bepaalt wordt MaximumNodes genoemd. Deze parameter kan worden ingesteld tijdens het maken van de toepassing of bijgewerkt in verband met het exemplaar van een toepassing die al is uitgevoerd.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –ApplicationName fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

In de set knooppunten, worden met Cluster Resource Manager biedt geen garantie welke serviceobjecten samen worden geplaatst of welke knooppunten ophalen gebruikt.

## <a name="application-metrics-load-and-capacity"></a>Metrische toepassingsgegevens, laden en capaciteit
Toepassingsgroepen kunnen ook u metrische gegevens die zijn gekoppeld aan een bepaalde naam toepassingsexemplaar en capaciteit voor deze metrische gegevens van de toepassingsinstantie van die definiëren. Metrische toepassingsgegevens kunt u bijhouden, reserveren en het brongebruik van de services binnen die toepassingsexemplaar beperken.

Er zijn twee waarden die kunnen worden ingesteld voor elke toepassingsmetrieken:

- **Totale capaciteit van een toepassing** : deze instelling geeft de totale capaciteit van de toepassing voor een bepaalde meetwaarde. Cluster Resource Manager is niet toegestaan door het maken van nieuwe services in deze toepassingsinstantie die ertoe kan leiden dat de totale werklast groter zijn dan deze waarde. Stel dat bijvoorbeeld exemplaar van de toepassing heeft een maximale capaciteit van 10 en al had load van vijf. Het maken van een service met een totale standaard belasting van 10 zou zijn niet toegestaan.
- **Maximale capaciteit van het knooppunt** : deze instelling geeft u de maximale totale belasting voor de toepassing op een enkel knooppunt. Als load via deze capaciteit gaat, verplaatst met Cluster Resource Manager replica's naar andere knooppunten, zodat de belasting afneemt.


PowerShell:

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

C#:

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a>Capaciteit reserveren
Een ander algemeen gebruik voor toepassingsgroepen is om ervoor te zorgen dat bronnen binnen het cluster zijn gereserveerd voor een bepaalde toepassing-exemplaar. De ruimte wordt altijd gereserveerd wanneer het exemplaar wordt gemaakt.

Reserveren van ruimte in het cluster voor de toepassing onmiddellijk gebeurt ook wanneer:
- exemplaar van de toepassing is gemaakt, maar geen services binnen het nog
- het aantal services binnen het toepassingsexemplaar verandert telkens wanneer 
- de services aanwezig zijn, maar worden niet de resources verbruikt 

Reserveren van resources voor het exemplaar van een toepassing, moet twee extra parameters op te geven: *MinimumNodes* en *NodeReservationCapacity*

- **MinimumNodes** -definieert het minimum aantal knooppunten dat exemplaar van de toepassing moet worden uitgevoerd op.  
- **NodeReservationCapacity** -deze instelling is per metrisch gegeven voor de toepassing. De waarde is de hoeveelheid die gereserveerd voor de toepassing op een willekeurig knooppunt metriek waar die de services in die toepassing worden uitgevoerd.

Combineren **MinimumNodes** en **NodeReservationCapacity** garandeert een minimale belasting reservering voor de toepassing binnen het cluster. Als er minder resterende capaciteit in het cluster dan de totale reservering vereist, mislukt het maken van de toepassing. 

We bekijken een voorbeeld van de capaciteitsreservering:

<center>

![Exemplaren van een toepassing reservecapaciteit definiëren][Image2]
</center>

In het voorbeeld links hoeft toepassingen niet elke capaciteit van een toepassing gedefinieerd. Cluster Resource Manager saldi alles volgens normale regels.

In het voorbeeld aan de rechterkant, laten we zeggen dat Toepassing1 is gemaakt met de volgende instellingen:

- Ingesteld op twee MinimumNodes
- Een toepassing waarde die is gedefinieerd met
  - NodeReservationCapacity van 20

PowerShell

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

C#

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

Service Fabric capaciteit op twee knooppunten reserveert voor Toepassing1 en services van Toepassing2 die capaciteit gebruiken, zelfs als er zijn dat geen load wordt verbruikt door de services binnen Toepassing1 op het moment niet toestaan. Deze toepassing gereserveerde capaciteit wordt beschouwd als verbruikt en geteld op basis van de resterende capaciteit op dat knooppunt en binnen het cluster.  De reservering wordt in mindering gebracht op de resterende capaciteit van het cluster onmiddellijk, maar het gereserveerde verbruik wordt afgetrokken van de capaciteit van een specifiek knooppunt alleen wanneer ten minste één service-object wordt geplaatst op het. Deze reservering hoger kunt u flexibiliteit en beter brongebruik omdat alleen de bronnen worden gereserveerd op knooppunten wanneer dat nodig is.

## <a name="obtaining-the-application-load-information"></a>Het ophalen van de toepassing-informatie laden
Voor elke toepassing heeft die een capaciteit van een toepassing gedefinieerd voor een of meer waarden die u kunt de informatie over de cumulatieve load gerapporteerd door replica's van de services te verkrijgen.

PowerShell:

``` posh
Get-ServiceFabricApplicationLoadInformation –ApplicationName fabric:/MyApplication1
```

C#

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

De query ApplicationLoad retourneert de algemene informatie over de capaciteit van een toepassing die is opgegeven voor de toepassing. Deze informatie omvat de gegevens van de knooppunten van de Minimum en maximum aantal knooppunten en het getal dat de toepassing is momenteel bezet. Het bevat ook informatie over metrische gegevens laden van elke toepassing met inbegrip van:

* Naam van de metrische gegevens: De naam van de metrische gegevens.
* Capaciteit van de reservering: Clustercapaciteit die in het cluster is gereserveerd voor deze toepassing.
* Belasting van toepassing: Totale belasting van deze toepassing onderliggende replica's.
* Capaciteit van een toepassing: Maximale toegestane waarde van de belasting van toepassing.

## <a name="removing-application-capacity"></a>Capaciteit van een toepassing verwijderen
Wanneer de capaciteit van een toepassing-parameters zijn ingesteld voor een toepassing, kunnen ze worden verwijderd met behulp van de Update-toepassing-API's of PowerShell-cmdlets. Bijvoorbeeld:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Met deze opdracht verwijdert u alle capaciteit management toepassingsparameters exemplaar van de toepassing. Dit omvat MinimumNodes, MaximumNodes en metrische gegevens van de toepassing, indien van toepassing. Het effect van de opdracht is direct. Nadat u deze opdracht is voltooid, met Cluster Resource Manager maakt gebruik van het standaardgedrag voor het beheren van toepassingen. Capaciteit parameters voor de toepassing opnieuw kunnen worden opgegeven `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Beperkingen van de capaciteit van een toepassing
Er zijn enkele beperkingen met betrekking tot de capaciteit van een toepassing parameters die in acht worden genomen. Als er validatiefouten zijn plaatsvinden geen wijzigingen.

- Alle parameters van geheel getal moet niet-negatieve getallen.
- Nooit moet MinimumNodes groter zijn dan MaximumNodes.
- Als de capaciteit voor metrische gegevens laden zijn gedefinieerd, moeten ze deze regels volgen:
  - Reservering Knooppuntcapaciteit mag niet groter zijn dan de maximale capaciteit van het knooppunt zijn. U kunt geen bijvoorbeeld de capaciteit voor de metriek 'Processor' op het knooppunt naar twee eenheden beperken en probeert te reserveren drie eenheden op elk knooppunt.
  - Als MaximumNodes is opgegeven, klikt u vervolgens moet het product van MaximumNodes en de maximale capaciteit van het knooppunt niet groter zijn dan de totale capaciteit van een toepassing. Stel bijvoorbeeld de maximale capaciteit van knooppunt voor de load metrische waarde 'Processor' is ingesteld op acht. Ook Stel dat u het maximumaantal knooppunten ingesteld op 10. In dit geval moet totale capaciteit van een toepassing groter zijn dan 80 voor deze load metrische gegevens.

De beperkingen gelden voor zowel tijdens het maken van de toepassing en updates.

## <a name="how-not-to-use-application-capacity"></a>Over niet het gebruik van de capaciteit van een toepassing
- Probeer niet de groep van toepassingen om functies te gebruiken om te beperken van de toepassing naar een _specifieke_ subset knooppunten. Met andere woorden, u kunt opgeven dat de toepassing wordt uitgevoerd op maximaal vijf knooppunten, maar niet welke specifieke vijf knooppunten in het cluster. Beperken van specifieke knooppunten van een toepassing kan worden bereikt met behulp van plaatsingsbeperkingen voor services.
- Probeer niet de capaciteit van een toepassing gebruiken om ervoor te zorgen dat de twee services van dezelfde toepassing op de dezelfde knooppunten zijn geplaatst. Gebruik in plaats daarvan [affiniteit](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) of [plaatsingsbeperkingen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Als u wilt weten over hoe met Cluster Resource Manager beheert en verdeelt de taken in het cluster, Zie het artikel op [taakverdeling](service-fabric-cluster-resource-manager-balancing.md)
- Vanaf het begin starten en [een inleiding tot de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
- Lees voor meer informatie over de werking metrische gegevens over het algemeen, van op [meetwaarden van Service Fabric](service-fabric-cluster-resource-manager-metrics.md)
- Cluster Resource Manager beschikt over veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
