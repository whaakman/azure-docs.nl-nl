---
title: 'Service Fabric Cluster Resource Manager: Kosten voor gegevensverplaatsing | Microsoft Docs'
description: Overzicht van de kosten voor gegevensverplaatsing voor Service Fabric-services
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 58147f5c0c3c7203aa6978e284cf5bdc90fa6971
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737771"
---
# <a name="service-movement-cost"></a>Verplaatsingskosten van services
Een van meerdere factoren die de Service Fabric Cluster Resource Manager acht bij een poging om te bepalen welke wijzigingen aanbrengen in een cluster zijn de kosten van deze wijzigingen. Het concept van "kosten" wordt verhandeld uitschakelen op basis van hoeveel van het cluster kan worden verbeterd. Kosten zijn meeberekend bij het verplaatsen van services voor taakverdeling, defragmentatie en andere vereisten. Het doel is om te voldoen aan de vereisten in de minste verstorende of dure manier. 

Verplaatsen van services kosten CPU-tijd en ten minste de netwerkbandbreedte. Voor stateful services moet het kopiëren van de status van deze services, verbruikt meer geheugen en schijfruimte. Minimaliseren van de kosten van de oplossingen die de Azure Service Fabric Cluster Resource Manager weergegeven met wordt, zorgt ervoor dat de resources van het cluster niet worden besteed aan het onnodig. Echter, u ook niet wilt negeren oplossingen die u wilt de toewijzing van resources in het cluster aanzienlijk verbeteren.

Cluster Resource Manager heeft twee manieren om de kosten voor computing en ze beperken terwijl er wordt geprobeerd om het cluster te beheren. Het eerste mechanisme is elke beweging die u zou gewoon tellen. Als er twee mogelijke oplossingen worden gegenereerd met ongeveer hetzelfde saldo (score), en vervolgens met Cluster Resource Manager de voorkeur geeft aan dat met de laagste kosten (totaal aantal verplaatst).

Deze strategie werkt goed. Maar net als bij standaard- of statische belasting, is het onwaarschijnlijk dat in een complex systeem dat alle verplaatst gelijk zijn. Sommige zijn waarschijnlijk nog veel meer kostbaar zijn.

## <a name="setting-move-costs"></a>Kosten van instelling verplaatsen 
De standaardwaarde kosten voor een service verplaatsen wanneer deze is gemaakt, kunt u opgeven:

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

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Dynamisch verplaatsen kosten op te geven op basis van de per-replica

De voorgaande codefragmenten zijn alle voor het MoveCost opgeven voor een hele service in één keer van buiten de service zelf. Echter, verplaatsen kosten is vooral handig is als de kosten van het verplaatsen van een specifieke service-object wordt gewijzigd via de levensduur. Omdat de services zelf hebt waarschijnlijk het beste idee van hoe kostbare ze worden uitgevoerd op een bepaald moment verplaatsen, is er een API voor services aan hun eigen afzonderlijke kosten verplaatsen tijdens runtime rapport. 

C#:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Gevolgen van de kosten voor verplaatsen
MoveCost heeft vier niveaus: Nul, laag, gemiddeld en hoog. MoveCosts zijn ten opzichte van elkaar worden verbonden, met uitzondering van nul. Kosten verplaatsen betekent dat verkeer gratis is en moet niet meegeteld in de score van de oplossing. Instellen van uw kosten verbonden aan het hoge heeft verplaatsing *niet* garantie dat de replica op één plek blijft.

<center>
![Kosten verplaatsen als factor bij het selecteren van replica's voor gegevensverplaatsing][Image1]
</center>

MoveCost helpt u de oplossingen die ertoe leiden dat de minimale verstoring van de algehele en zijn het gemakkelijkst te controleren, terwijl u nog steeds die binnenkomen in equivalente balans te vinden. Begrip van de kosten van een service kan worden ten opzichte van een groot aantal dingen. De meest voorkomende factoren bij het berekenen van uw kosten verplaatsen zijn:

- De hoeveelheid van status- of gegevens die de service te verplaatsen.
- De kosten van ontkoppeling van clients. Het verplaatsen van een primaire replica is meestal duurder dan de kosten van het verplaatsen van een secundaire replica.
- De kosten van het onderbreken van een bewerking die onderweg zijn. Bepaalde bewerkingen op de gegevens opslaan niveau of bewerkingen die worden uitgevoerd in reactie op een client-aanroep kostbaar zijn. Na een bepaald moment u niet ze stopt als u niet te hoeft. Dus terwijl de bewerking wordt gebruikt op, verhoogt u de kosten verplaatsen van deze service-object te verminderen de kans dat wordt verplaatst. Wanneer de bewerking is voltooid, moet u de kosten op normaal instellen.

## <a name="enabling-move-cost-in-your-cluster"></a>Inschakelen van kosten in het cluster verplaatsen
Opdat de gedetailleerdere MoveCosts om te worden gehouden, moet MoveCost worden ingeschakeld in uw cluster. Zonder deze instelling is de standaardmodus van tellen verplaatst wordt gebruikt voor het berekenen van MoveCost en MoveCost rapporten worden genegeerd.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

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
- Service Fabric-Cluster Resource Manager gebruikt u metrische gegevens voor het beheren van gebruiks- en capaciteit in het cluster. Bekijk voor meer informatie over metrische gegevens en hoe u ze configureert, [resourceverbruik beheren en laden in Service Fabric met metrische gegevens over](service-fabric-cluster-resource-manager-metrics.md).
- Bekijk voor meer informatie over hoe met Cluster Resource Manager beheert en verdeelt de taken in het cluster, [taakverdeling van uw Service Fabric-cluster](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
