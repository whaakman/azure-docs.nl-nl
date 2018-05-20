---
title: 'Service Fabric Cluster Resource Manager: gegevensverplaatsing kosten | Microsoft Docs'
description: Overzicht van verplaatsingskosten voor Service Fabric-services
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 74b61967a796fca22ab86918235f1def27a22f91
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="service-movement-cost"></a>Service verplaatsingskosten
Een factor die de Service Fabric Cluster Resource Manager overweegt wanneer u probeert om te bepalen welke wijzigingen aanbrengen in een cluster is de kosten van deze wijzigingen. Het begrip 'kosten' wordt verhandeld uitschakelen op basis van hoeveel van het cluster kan worden verbeterd. Kosten zijn meeberekend bij het verplaatsen van services voor taakverdeling, defragmentatie en andere vereisten. Het doel is om te voldoen aan de vereisten in de minste verstoren of dure manier. 

Services kosten CPU-tijd verplaatst, en ten minste de netwerkbandbreedte. Voor stateful services vereist voor het kopiëren van de status van deze services, verbruikt meer geheugen en schijfruimte. De kosten van oplossingen die de Azure Service Fabric-Cluster Resource Manager wanneer voordoet minimaliseren, zorgt ervoor dat de cluster-resources zijn niet bezig is geweest onnodig. Echter, u niet wilt ook voor het negeren van oplossingen die zou de toewijzing van resources in het cluster aanzienlijk verbeteren.

De Cluster Resource Manager heeft twee manieren om de kosten computing en ze beperken terwijl wordt geprobeerd om het cluster te beheren. Het eerste mechanisme met elke verplaatsing die zou gewoon tellen. Als twee oplossingen zijn gegenereerd met ongeveer hetzelfde saldo (score), en vervolgens de Cluster Resource Manager geeft de voorkeur aan één met de laagste kosten (totaal aantal verplaatst).

Deze strategie werkt goed. Maar met standaard of statische belasting is het onwaarschijnlijk dat in een complex systeem dat alle verplaatst gelijk zijn. Sommige zijn waarschijnlijk veel duurder.

## <a name="setting-move-costs"></a>Kosten voor instelling verplaatsen 
De standaardwaarde verplaatsen kosten voor een service wanneer deze wordt gemaakt, kunt u opgeven:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C#: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

U kunt ook opgeven of MoveCost dynamisch bijwerken voor een service nadat de service is gemaakt: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Dynamisch geven verplaatsen kosten op basis van de per-replica

De voorgaande codefragmenten zijn alle voor het MoveCost opgeven voor een hele service tegelijk van buiten de service zelf. Echter verplaatsen kosten is vooral handig is als de kosten van het verplaatsen van een specifieke service-object verandert gedurende de levensduur. Aangezien de services zelf waarschijnlijk het beste idee hoe kostbaar zijn om te verplaatsen van een bepaald moment hebben, is er een API voor services om te rapporteren die hun eigen afzonderlijke kosten verplaatsen tijdens runtime. 

C#:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Gevolgen van het verplaatsen kosten
MoveCost heeft vier niveaus: nul, laag, gemiddeld en hoog. MoveCosts zijn ten opzichte van elkaar worden verbonden, met uitzondering van nul. Nul kosten verplaatsen betekent dat verkeer gratis is en mag niet in mindering gebracht op de score van de oplossing. Instellen van uw kosten om deze te hoog biedt verplaatsen *niet* garanderen dat de replica op één plek blijft.

<center>
![Kosten verplaatsen als een factor bij het selecteren van replica's voor gegevensverplaatsing][Image1]
</center>

MoveCost kunt u de oplossingen die ervoor zorgen dat de algehele minimaal onderbreking en zijn het gemakkelijkst te bereiken nog steeds dat binnenkomt bij gelijkwaardige balans vinden. Een service begrip van kosten mag ten opzichte van een groot aantal dingen. De meest voorkomende factoren bij het berekenen van de verplaatsing kosten zijn:

- De hoeveelheid status- of gegevens die de service te verplaatsen.
- De kosten van het verbreken van de verbinding van clients. Het verplaatsen van een primaire replica is meestal duurder dan de kosten van het verplaatsen van een secundaire replica.
- De kosten van een onderweg bewerking te onderbreken. Bepaalde bewerkingen op de gegevens opslaan niveau of bewerkingen die worden uitgevoerd in reactie op een client-aanroep kostbaar zijn. Na een bepaald moment u niet ze stopt als u niet te hoeft. Tijdens de bewerking, verhoogt u dus de kosten van het verplaatsen van deze service-object te verminderen de kans dat het wordt verplaatst. Als de bewerking is voltooid, stelt u de kosten in normale tekst.

## <a name="enabling-move-cost-in-your-cluster"></a>Inschakelen van kosten in uw cluster verplaatsen
De gedetailleerdere MoveCosts rekening worden gehouden als MoveCost worden ingeschakeld in uw cluster. Zonder deze instelling wordt de standaardmodus tellen verplaatsen wordt gebruikt voor het berekenen van MoveCost en MoveCost rapporten worden genegeerd.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen
- Service Fabric-Cluster Resource Manager maakt gebruik van metrische gegevens voor het beheren van gebruiks- en capaciteit in het cluster. Bekijk voor meer informatie over metrische gegevens en het configureren ervan [brongebruik beheren en de belasting in Service Fabric met metrische gegevens](service-fabric-cluster-resource-manager-metrics.md).
- Bekijk voor meer informatie over hoe de Cluster Resource Manager beheert en een compromis tussen de werklast van het cluster, [Balancing Service Fabric-cluster](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
