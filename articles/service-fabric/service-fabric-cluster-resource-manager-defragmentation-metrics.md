---
title: Defragmentatie van metrische gegevens in Azure Service Fabric | Microsoft Docs
description: Een overzicht van de defragmentatie gebruiken of als een strategie voor metrische gegevens in Service Fabric verpakken
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: a50d65940ec12f9883f8991307c3cc5692d96f06
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56738076"
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentatie van metrische gegevens en belasting in Service Fabric
Service Fabric Cluster Resource Manager van standaardstrategie voor het beheren van metrische gegevens laden in het cluster bestaat uit het verdelen van de belasting. Ervoor te zorgen dat knooppunten gelijkmatig worden benut voorkomt hot en cold punten die tot conflicten zowel verspilde resources leiden. Distributie van workloads in het cluster is ook de veiligste in termen van functionerende fouten omdat Hiermee zorgt u ervoor dat er een fout niet u een groot percentage van een bepaalde workload neemt. 

De Service Fabric Cluster Resource Manager biedt ondersteuning voor een andere strategie voor het beheren van de werkbelasting, dit is de defragmentatie. Defragmentatie betekent dat in plaats van voor de distributie van het gebruik van metrische gegevens over het cluster, het is samengevoegd. Consolidatie is slechts een omkering van de standaard strategie – in plaats van het minimaliseren van de gemiddelde standaardafwijking van metrische load balancing, met Cluster Resource Manager probeert te verhogen.

## <a name="when-to-use-defragmentation"></a>Wanneer u Defragmentatie
Distribueren van laden in het cluster, verbruikt enkele van de bronnen op elk knooppunt. Sommige werkbelastingen maken van services die uitzonderlijk groot zijn en de meeste of alle van een knooppunt in beslag nemen. In dergelijke gevallen is het mogelijk dat wanneer er grote workloads die worden gemaakt die er is onvoldoende ruimte op een willekeurig knooppunt om ze te voeren. Grote werkbelastingen worden niet een probleem in Service Fabric; in dergelijke gevallen met Cluster Resource Manager bepaalt dat het moet opnieuw indelen van het cluster om ruimte voor deze grote workload te maken. In de tussentijd heeft die werkbelasting echter moet worden gewacht voordat in het cluster worden gepland.

Als er veel services en de status om te verplaatsen, kan klikt u vervolgens het lang duren voor de grote workload in het cluster worden geplaatst. Dit is waarschijnlijk als andere werkbelastingen in het cluster ook groot zijn en dus het langer duren voordat opnieuw indelen. De Service Fabric-team gemeten maken tijden in simulaties van dit scenario. Er is aangetroffen dat grote services maken veel langer duurden zodra clustergebruik boven tussen 30 en 50 is %. Voor het afhandelen van dit scenario hebben we defragmentatie geïntroduceerd als een strategie voor taakverdeling. Wij vinden dat voor grote workloads, met name die waar Aanmaaktijd belangrijk is, defragmentatie echt geholpen bij deze nieuwe werkbelastingen ophalen gepland in het cluster.

U kunt metrische gegevens defragmentatie als u wilt dat de Cluster Resource Manager om proactief te verkleinen van de belasting van de services in minder knooppunten kunt configureren. Dit zorgt ervoor dat er bijna altijd ruimte voor grote services is zonder opnieuw indelen van het cluster. Omdat niet hoeft te ordenen van het cluster, kunt snel grote werkbelastingen maken.

De meeste mensen hoeft niet defragmenteren. Services zijn meestal worden kleine, dus het is niet moeilijk te vinden van ruimte voor deze in het cluster. Bij het opnieuw rangschikken mogelijk is, gaat snel, opnieuw omdat de meeste services klein zijn en snel en parallel kunnen worden verplaatst. Echter, als u grote services en ze snel gemaakt vervolgens de defragmentatie-strategie nodig is voor u. We bespreken de nadelen van het gebruik van defragmentatie vervolgens. 

## <a name="defragmentation-tradeoffs"></a>Defragmentatie compromissen
Defragmentatie kunt impactfulness van fouten, verhogen omdat meer services worden uitgevoerd op knooppunten die fungeren als. Defragmentatie kunt ook kosten, vergroten, omdat de resources in het cluster moeten worden ondergebracht in reserve, wachten op het maken van grote werkbelastingen.

Het volgende diagram biedt een visuele representatie van twee clusters, één dat wordt gedefragmenteerd en één die niet. 

<center>
![Vergelijken met gelijke taakverdeling en defragmenteren van Clusters][Image1]
</center>

In het geval is met gelijke taakverdeling, kunt u het aantal verplaatsingen van het type die noodzakelijk zijn om een van de grootste serviceobjecten plaatsen. In het cluster gedefragmenteerde, kan de grote werkbelasting worden geplaatst op knooppunten vier of vijf zonder te wachten op andere services te verplaatsen.

## <a name="defragmentation-pros-and-cons"></a>Defragmentatie voor- en nadelen
Dus wat zijn de conceptuele compromissen? Hier volgt een snelle tabel dingen om na te denken over:

| Defragmentatie-professionals | Defragmentatie nadelen |
| --- | --- |
| Hiermee kunnen sneller worden gemaakt van grote services |Concentraten op minder knooppunten, verhogen conflicten laden |
| Hiermee lagere verplaatsing van gegevens tijdens het maken van |Fouten kunnen invloed hebben op meer services en ervoor zorgen dat meer verloop |
| Uitgebreide beschrijving van vereisten en het vrijmaken van ruimte |Complexere algehele Resource Management-configuratie |

U kunt combineren gedefragmenteerde en normale metrische gegevens in hetzelfde cluster. Cluster Resource Manager probeert de defragmentatie metrische gegevens zo veel mogelijk tijdens het spreiden van de andere consolideren. De resultaten van een combinatie van defragmentatie en strategieën voor taakverdeling, is afhankelijk van verschillende factoren, waaronder:
  - het aantal metrische gegevens vergeleken met het aantal defragmentatie metrische gegevens over taakverdeling
  - Elke service die u gebruikt of beide typen metrische gegevens 
  - de metrische waarden
  - huidige metrische gegevens worden geladen
  
Experimenten is vereist om te bepalen van de exacte configuratie die nodig zijn. Uitgebreide meting van uw workloads wordt aangeraden voordat u Defragmentatie metrische gegevens in de productieomgeving inschakelen. Dit is vooral van toepassing wanneer een combinatie van defragmentatie en met gelijke taakverdeling metrische gegevens binnen dezelfde service. 

## <a name="configuring-defragmentation-metrics"></a>Configureren van defragmentatie metrische gegevens
Configureren van defragmentatie metrische gegevens is een algemene beslissing in het cluster en afzonderlijke metrische gegevens voor defragmentatie kan worden geselecteerd. De volgende configuratie-codefragmenten laten zien hoe het configureren van metrische gegevens voor defragmentatie. In dit geval worden 'Metric1' is geconfigureerd als een metriek defragmentatie terwijl 'Metric2' blijft normaal gesproken worden verdeeld. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

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
- Cluster Resource Manager beschikt over man-opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
- Metrische gegevens zijn hoe gebruiks- en capaciteit in het cluster worden beheerd door de Service Fabric-Cluster Resource Manager. Bekijk voor meer informatie over metrische gegevens en hoe u ze configureert, [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
