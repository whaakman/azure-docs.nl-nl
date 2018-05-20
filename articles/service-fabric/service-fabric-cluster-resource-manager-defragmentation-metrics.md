---
title: Defragmentatie van metrische gegevens in Azure Service Fabric | Microsoft Docs
description: Een overzicht van defragmentatie gebruiken of als een strategie voor metrische gegevens in Service Fabric verpakken
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: a35ae5933729615d634359e64e31d43536d81431
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentatie van metrische gegevens en de belasting in Service Fabric
Service Fabric Cluster Resource Manager standaard strategie voor het beheren van metrische gegevens laden in het cluster is de verdelen. Ervoor te zorgen dat de knooppunten gelijkmatig worden gebruikt, voorkomt warme en koude plaatsen die tot conflicten zowel verspilde resources leiden. Distributie van werkbelastingen in het cluster is ook de veiligste in termen van functionerende fouten omdat Hiermee zorgt u ervoor dat een fout wordt pas uit een groot percentage van een bepaalde werkbelasting. 

De Service Fabric Cluster Resource Manager biedt ondersteuning voor een andere strategie voor het beheren van belasting, namelijk defragmentatie. Defragmentatie betekent dat in plaats van het gebruik van een metriek verdelen over het cluster, het geconsolideerd. Consolidatie is slechts een omkering van de standaard strategie – in plaats van voor het minimaliseren van de gemiddelde standaardafwijking van metrische load balancing, de Cluster Resource Manager probeert te verhogen.

## <a name="when-to-use-defragmentation"></a>Wanneer gebruikt u Defragmentatie
Sommige van de bronnen op elk knooppunt verbruikt laden in het cluster verdelen. Sommige werkbelastingen maken van services die uitzonderlijk groot en gebruiken van de meeste of alle van een knooppunt. In dergelijke gevallen is het mogelijk dat wanneer er grote werkbelastingen ophalen gemaakt dat er onvoldoende ruimte op een willekeurig knooppunt is uitvoeren. Grote werkbelastingen zijn niet van een probleem in Service Fabric; in dergelijke gevallen bepaalt de Cluster Resource Manager dat deze moeten opnieuw indelen van het cluster ruimte te maken voor deze grote werkbelasting. In de tussentijd heeft werkbelasting echter moet worden gewacht in het cluster moeten worden gepland.

Als er veel services en status om te navigeren, kan klikt u vervolgens het lang duren voor grote werkbelasting in het cluster worden geplaatst. Dit is het waarschijnlijker als andere werkbelastingen in het cluster ook grote zijn en dus langer duren te ordenen. Het Service Fabric-team gemeten maken keer simulaties van dit scenario. We vinden dat grote services maken veel langer duurde zodra clustergebruik kreeg hierboven tussen 30 en 50%. Voor het afhandelen van dit scenario geïntroduceerd we defragmentatie als een strategie voor taakverdeling. We vinden dat voor grote werkbelastingen, met name zijn waarop Aanmaaktijd belangrijk is, defragmentatie echt geholpen bij deze nieuwe werkbelastingen ophalen gepland in het cluster.

U kunt Defragmentatie metrische gegevens om de Cluster Resource Manager om proactief te verkleinen van de belasting van de services in minder knooppunten te laten configureren. Dit zorgt ervoor dat er bijna altijd ruimte voor grote services is zonder opnieuw indelen van het cluster. Niet met opnieuw indelen van het cluster, kunt snel grote werkbelastingen maken.

De meeste mensen hoeft niet defragmentatie. Services zijn meestal worden klein, zodat deze niet is moeilijk te vinden plaats ze in het cluster. Wanneer de reorganisatie mogelijk is, gaat snel, omdat de meeste services klein zijn en snel en parallel kunnen worden verplaatst. Als u grote services hebt en deze snel gemaakt vervolgens de strategie defragmentatie nodig is echter voor u. We bespreken de voor-en nadelen van het gebruik van defragmentatie vervolgens. 

## <a name="defragmentation-tradeoffs"></a>Defragmentatie-en nadelen
Defragmentatie kunt impactfulness van fouten, verhogen omdat meer services die worden uitgevoerd op knooppunten die niet voldoen aan. Defragmentatie kan ook kosten, verhogen omdat resources in het cluster deel van reserve uitmaken moeten, wachten op het maken van een grote werkbelastingen.

Het volgende diagram biedt een visuele representatie van twee clusters, één dat wordt gedefragmenteerd en één die niet. 

<center>
![Vergelijken met gelijke taakverdeling en gedefragmenteerd Clusters][Image1]
</center>

In het geval is taakverdeling, kunt u het aantal verplaatsingen die noodzakelijk zijn om een van de grootste serviceobjecten plaatsen. In het cluster gedefragmenteerde, kan de grote werkbelasting op knooppunten vier of vijf worden geplaatst zonder te wachten op andere diensten te verplaatsen.

## <a name="defragmentation-pros-and-cons"></a>Defragmentatie voor- en nadelen
Wat zijn zodat deze conceptuele optimalisatie? Dit is een snelle tabel dingen om na te denken over:

| Defragmentatie-professionals | Defragmentatie nadelen |
| --- | --- |
| Hiermee kunt sneller maken van grote services |Concentraten op minder knooppunten, verhogen conflicten laden |
| Hiermee verlagen verplaatsing van gegevens tijdens het maken van |Fouten kunnen invloed meer services en meer verloop veroorzaken |
| Hiermee maakt u uitgebreide beschrijving van de vereisten en het vrijmaken van ruimte |Complexere algehele Resource Management-configuratie |

U kunt combineren gedefragmenteerde en normaal metrische gegevens in hetzelfde cluster. De Cluster Resource Manager probeert de defragmentatie metrische gegevens zo veel mogelijk tijdens het spreiden van de andere consolideren. De resultaten van de combinatie van defragmentatie en netwerktaakverdeling strategieën, is afhankelijk van verschillende factoren, waaronder:
  - het aantal balancing metrische gegevens vergeleken met het aantal defragmentatie metrische gegevens
  - Hiermee wordt aangegeven of alle services maakt gebruik van beide typen metrische gegevens 
  - de metrische gewichten
  - huidige metrische gegevens worden geladen
  
Experimenteren is vereist voor het bepalen van de exacte configuratie nodig. Het is raadzaam grondige meting van de werkbelasting van uw voordat u Defragmentatie metrische gegevens in de productieomgeving inschakelen. Dit is vooral van toepassing wanneer de combinatie van defragmentatie en taakverdeling metrische gegevens binnen dezelfde service. 

## <a name="configuring-defragmentation-metrics"></a>Defragmentatie metrische gegevens configureren
Een globale beslissing in het cluster configureren defragmentatie metrische gegevens is en afzonderlijke metrische gegevens voor defragmentatie kan worden geselecteerd. De volgende configuratie codefragmenten laten zien hoe metrische gegevens voor defragmentatie configureren. In dit geval 'Metric1' geconfigureerd als een metriek defragmentatie 'Metric2' wordt verder normaal worden verdeeld. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Metric1",
          "value": "true"
      },
      {
          "name": "Metric2",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a>Volgende stappen
- De Cluster Resource Manager heeft man-opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
- Metrische gegevens zijn hoe de Service Fabric-Cluster Resource Manager beheert gebruiks- en capaciteit in het cluster. Bekijk voor meer informatie over metrische gegevens en het configureren ervan [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
