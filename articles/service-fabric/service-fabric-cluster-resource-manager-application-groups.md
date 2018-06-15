---
title: Service Fabric Cluster Resource Manager - toepassingsgroepen | Microsoft Docs
description: Overzicht van de functionaliteit van de groep toepassingen in de Service Fabric Cluster Resource Manager
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 215efc1f0597f5199dd37baf4b109d7e76040aae
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212990"
---
# <a name="introduction-to-application-groups"></a>Inleiding tot toepassingsgroepen
Service-Fabric Cluster Resource Manager beheert doorgaans clusterbronnen door te spreiden van de belasting (vertegenwoordigd [metrische gegevens](service-fabric-cluster-resource-manager-metrics.md)) gelijkmatig in het cluster. Service Fabric beheert de capaciteit van de knooppunten in het cluster en het cluster als geheel via [capaciteit](service-fabric-cluster-resource-manager-cluster-description.md). Metrische gegevens en capaciteit werken geweldig voor veel werkbelastingen, maar de patronen die maken intensief gebruik van verschillende exemplaren van Service Fabric-toepassing soms Breng in aanvullende vereisten. U kunt bijvoorbeeld naar:

- Sommige capaciteit op de knooppunten in het cluster voor de services in een aantal benoemde toepassingsexemplaar reserveren
- Beperkt het totale aantal knooppunten dat de services in een benoemd exemplaar op (in plaats van ze uit te spreiden over het hele cluster) uitgevoerd.
- Capaciteit op het benoemde exemplaar zelf beperkt het aantal services of verbruik van de services in het totaal aantal bronnen definiëren

Om te voldoen aan deze vereisten, ondersteunt de Service Fabric Cluster Resource Manager de functie toepassingsgroepen.

## <a name="limiting-the-maximum-number-of-nodes"></a>Het maximum aantal knooppunten beperken
De eenvoudigste gebruiksvoorbeeld capaciteit van de toepassing is wanneer een toepassingsexemplaar moet worden beperkt tot een bepaalde maximum aantal knooppunten. Dit consolideert alle services in dat toepassingsexemplaar naar een bepaald aantal machines. Consolidatie is nuttig wanneer u probeert te voorspellen of cap fysieke resource gebruik door de services binnen die benoemde exemplaar. 

De volgende afbeelding ziet u de instantie van een toepassing met en zonder een maximum aantal knooppunten gedefinieerd:

<center>
![Toepassingsexemplaar Maximum aantal knooppunten definiëren][Image1]
</center>

In het linkerdeelvenster voorbeeld wordt de toepassing geen een maximum aantal knooppunten dat is gedefinieerd en hieraan de drie services. De Cluster Resource Manager heeft alle replica's over verspreid zes beschikbare knooppunten voor de beste balans in het cluster (de standaardinstelling). In het juiste voorbeeld zien we dezelfde toepassing beperkt tot drie knooppunten.

De parameter waarmee dit gedrag wordt MaximumNodes genoemd. Deze parameter worden ingesteld tijdens het maken van de toepassing of bijgewerkt in verband met de instantie van een toepassing die al wordt uitgevoerd.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
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

In de set met knooppunten, worden de Cluster Resource Manager geen garantie welke serviceobjecten samen worden geplaatst of welke knooppunten ophalen gebruikt.

## <a name="application-metrics-load-and-capacity"></a>Metrische gegevens van toepassing, laden en capaciteit
Toepassingsgroepen kunnen u metrische gegevens die zijn gekoppeld aan een bepaalde toepassing van de benoemde exemplaar, en die van het toepassingsexemplaar capaciteit voor deze metrische gegevens definiëren. Toepassing metrische gegevens kunt u volgen, reserveren en het brongebruik van de services in dat toepassingsexemplaar te beperken.

Er zijn twee waarden kunnen worden ingesteld voor elke metriek toepassing:

- **Totale capaciteit van de toepassing** – deze instelling geeft de totale capaciteit van de toepassing voor een bepaalde waarde. Het maken van de nieuwe services binnen deze toepassingsexemplaar waardoor totale belasting zijn dan deze waarde niet toegestaan door de Cluster Resource Manager. Stel dat bijvoorbeeld het toepassingsexemplaar had een capaciteit van 10 en laden van vijf al had. Het maken van een service met een totale standaard belasting van 10 zou niet worden toegestaan.
- **Maximale capaciteit van het knooppunt** – deze instelling geeft u de maximale totale belasting voor de toepassing op één knooppunt. Als de belasting toeneemt via deze capaciteit, verplaatst de Cluster Resource Manager replica's naar andere knooppunten, zodat de belasting afneemt.


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
Er wordt een andere vaak gebruikt voor toepassingsgroepen om ervoor te zorgen dat resources binnen het cluster zijn gereserveerd voor het exemplaar van een bepaalde toepassing. De ruimte wordt altijd gereserveerd wanneer het exemplaar wordt gemaakt.

Ruimte in het cluster te reserveren voor de toepassing onmiddellijk gebeurt ook wanneer:
- exemplaar van de toepassing is gemaakt, maar geen binnen deze services nog
- het aantal services binnen het toepassingsexemplaar verandert telkens wanneer 
- de services bestaan, maar de bronnen worden niet gebruiken 

Resources worden gereserveerd voor een toepassingsexemplaar vereist twee extra parameters opgeven: *MinimumNodes* en *NodeReservationCapacity*

- **MinimumNodes** -definieert het minimale aantal knooppunten dat exemplaar van de toepassing moet worden uitgevoerd op.  
- **NodeReservationCapacity** -deze instelling is per metrische gegevens voor de toepassing. De waarde is de hoeveelheid die gereserveerd voor de toepassing op een willekeurig knooppunt metriek waar waarop de services in de toepassing wordt uitgevoerd.

Combineren **MinimumNodes** en **NodeReservationCapacity** wordt gegarandeerd dat een minimale belasting reservering voor de toepassing binnen het cluster. Als er minder resterende capaciteit in het cluster dan de totale reservering vereist, mislukt het maken van de toepassing. 

Bekijk een voorbeeld van capaciteitsreservering:

<center>
![Exemplaren van een toepassing gereserveerde capaciteit definiëren][Image2]
</center>

In het linkerdeelvenster voorbeeld hoeft toepassingen niet elke toepassing capaciteit gedefinieerd. De Cluster Resource Manager een compromis tussen alles volgens de normale regels.

Stel de of Toepassing1 is gemaakt met de volgende instellingen in het voorbeeld aan de rechterkant:

- Ingesteld op twee MinimumNodes
- Een waarde die is gedefinieerd met de van toepassing
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

Service Fabric reserveert capaciteit op twee knooppunten voor Toepassing1 en services van Toepassing2 die capaciteit gebruiken, zelfs als er dat geen load wordt verbruikt door de services in Toepassing1 op het moment dat niet is toegestaan. Deze toepassing gereserveerde capaciteit wordt beschouwd als verbruikt en wordt in mindering gebracht op de resterende capaciteit op dat knooppunt en binnen het cluster.  De reservering wordt afgetrokken van de resterende cluster capaciteit onmiddellijk, maar het gereserveerde verbruik wordt afgetrokken van de capaciteit van een specifiek knooppunt alleen als er ten minste één service-object is geplaatst op het. Deze reservering hoger kunt u flexibiliteit en beter brongebruik omdat alleen de bronnen worden gereserveerd op de knooppunten wanneer deze nodig is.

## <a name="obtaining-the-application-load-information"></a>Het verkrijgen van de informatie over de toepassing werklast
Voor elke toepassing heeft die de capaciteit van een toepassing gedefinieerd voor een of meer waarden die kunt u de informatie over de werklast die zijn gerapporteerd door de replica's van de services.

PowerShell:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
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

De query ApplicationLoad retourneert de basisinformatie over de capaciteit van de toepassingen die is opgegeven voor de toepassing. Deze informatie omvat de gegevens van de knooppunten van de Minimum en maximum aantal knooppunten en het getal dat de toepassing is op dit moment bepaald dat zich bevindt. Dit omvat ook informatie over elke toepassing werklast metriek, met inbegrip van:

* Metrische naam: Naam van de metrische gegevens.
* Reserveringscapaciteit: Cluster de capaciteit die is gereserveerd in het cluster voor deze toepassing.
* Laden van toepassingen: Totale belasting van deze toepassing onderliggende replica's.
* Capaciteit van de toepassing: De maximaal toegestane waarde van het laden van toepassingen.

## <a name="removing-application-capacity"></a>Capaciteit van de toepassing verwijderen
Zodra de capaciteit van de toepassing-parameters zijn ingesteld voor een toepassing, kunnen ze worden verwijderd met behulp van de Update toepassing API's of PowerShell-cmdlets. Bijvoorbeeld:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Deze opdracht verwijdert u alle parameters van toepassing capaciteit management exemplaar van de toepassing. Dit omvat MinimumNodes, MaximumNodes en metrische gegevens voor de toepassing, indien van toepassing. Het effect van de opdracht is direct. Nadat u deze opdracht is voltooid, gebruikt de Cluster Resource Manager het standaardgedrag voor het beheren van toepassingen. Capaciteit parameters voor de toepassing opnieuw kunnen worden opgegeven `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Beperkingen voor de capaciteit van toepassingen
Er zijn enkele beperkingen van toepassing capaciteit parameters die moeten worden voldaan. Als er validatiefouten zijn plaatsvinden geen wijzigingen.

- Alle parameters van geheel getal moet niet-negatieve getallen.
- Nooit moet MinimumNodes groter zijn dan MaximumNodes.
- Als de capaciteit voor een load-metriek zijn gedefinieerd, moeten ze deze regels volgen:
  - Knooppunt Reserveringscapaciteit mag niet groter zijn dan de maximale capaciteit van het knooppunt zijn. U kan bijvoorbeeld de capaciteit voor de metriek 'CPU' op het knooppunt aan twee eenheden beperken en probeert te reserveren drie eenheden op elk knooppunt.
  - Als MaximumNodes is opgegeven, klikt u vervolgens moet het product van MaximumNodes en de maximale capaciteit van het knooppunt niet groter dan de totale capaciteit van de toepassing. Stel bijvoorbeeld de maximale capaciteit van het knooppunt voor load metriek 'CPU' is ingesteld op acht. Stel ook dat u het maximum aantal knooppunten tot en met 10. In dit geval moet totale capaciteit van de toepassing groter zijn dan 80 voor deze metriek laden.

Zowel tijdens het maken van de toepassing en updates worden de beperkingen afgedwongen.

## <a name="how-not-to-use-application-capacity"></a>Het niet het gebruik van de capaciteit van toepassingen
- Probeer niet de functies van de groep toepassingen gebruiken om te beperken van de toepassing naar een _specifieke_ deelverzameling met knooppunten. Met andere woorden, u kunt opgeven dat de toepassing wordt uitgevoerd op maximaal vijf knooppunten, maar niet welke specifieke vijf knooppunten in het cluster. Een toepassing op bepaalde knooppunten met beperkingen kan worden bereikt met behulp van plaatsingsbeperkingen voor services.
- Probeer niet de capaciteit van de toepassing gebruiken om ervoor te zorgen dat de twee services van dezelfde toepassing op de dezelfde knooppunten zijn geplaatst. Gebruik in plaats daarvan [affiniteit](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) of [plaatsingsbeperkingen](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Als u wilt weten over hoe de Cluster Resource Manager beheert en een compromis tussen de werklast van het cluster, Raadpleeg het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)
- Vanaf het begin starten en [Maak kennis met de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
- Voor meer informatie over de werking metrische gegevens over het algemeen tot lezen op [Service Fabric Load metrische gegevens](service-fabric-cluster-resource-manager-metrics.md)
- De Cluster Resource Manager bevat veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
