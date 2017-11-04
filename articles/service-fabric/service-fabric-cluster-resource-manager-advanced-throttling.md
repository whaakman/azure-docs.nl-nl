---
title: Beperking in de Service Fabric-cluster resourcemanager | Microsoft Docs
description: Informatie over het configureren van de vertragingen die is opgegeven door de Service Fabric Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 22223923f6783f11178b44c3c3087f17eb8dc41b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="throttling-the-service-fabric-cluster-resource-manager"></a>Beperking van de Service Fabric Cluster Resource Manager
Zelfs als u de Cluster Resource Manager correct hebt geconfigureerd, kan het cluster ophalen verstoord. Bijvoorbeeld, kan er gelijktijdige knooppunt en fouttolerantie domein fouten - wat er zou gebeuren als die is opgetreden tijdens een upgrade? De Cluster Resource Manager probeert altijd om op te lossen alles, de clusterbronnen opnieuw indelen en los van het cluster wilt gebruiken. Vertragingen zorgt voor een backstop zodat het cluster resources gebruiken kan stabiel - knooppunten terugkeren, de netwerkpartities retoucheren, gecorrigeerde bits geïmplementeerd.

Om te helpen met dit soort situaties, de Service Fabric Cluster Resource Manager bevat verschillende vertragingen. Deze vertragingen zijn alle behoorlijk groot hamers. Ze mag niet in het algemeen worden gewijzigd zonder een zorgvuldige planning en testen.

Als u de Cluster Resource Manager vertragingen wijzigt, moet u ze aan uw verwachte daadwerkelijke laadbewerking afstemmen. U kunt bepalen moet u beschikken over een aantal vertragingen, zelfs als betekent dit dat het cluster duurt langer regulering in sommige situaties. Testen is vereist voor het bepalen van de juiste waarden voor vertragingen. Vertragingen moeten hoog genoeg zijn voor het toestaan van het cluster om te reageren op wijzigingen in een redelijk tijdsbestek en laag genoeg is om daadwerkelijk te voorkomen dat te veel resourceverbruik. 

De meeste gevallen die we klanten hebt gezien vertragingen die is nu omdat ze al aanwezig in een omgeving met beperkte resource waren gebruiken. Voorbeelden zijn beperkte netwerkbandbreedte voor afzonderlijke knooppunten of schijven die niet kunnen worden opgebouwd veel stateful replica's parallel door beperkingen van de doorvoer. Zonder vertragingen, kunnen operations deze resources, waardoor bewerkingen mislukken of traag worden overbelast. In deze situaties klanten vertragingen gebruikt en zijn ze uitbreiden van de hoeveelheid tijd op de hoogte zou het duren voordat het cluster een stabiele status bereiken. Klanten ook begrepen dat ze kunnen terechtkomen op lagere algehele betrouwbaarheid actief, terwijl ze zijn beperkt.


## <a name="configuring-the-throttles"></a>De vertragingen configureren

Service Fabric heeft twee mechanismen voor beperking van het aantal verplaatsingen van de replica. Het standaardmechanisme die bestonden voordat de Service Fabric 5.7 vertegenwoordigt beperking als een absoluut aantal verplaatst toegestaan. Dit werkt niet voor clusters van elke grootte. Met name voor grote clusters is de standaardwaarde te klein, aanzienlijk vertragen balancing zelfs wanneer deze nodig is, terwijl hiervoor geen effect in kleinere clusters. Dit mechanisme voorafgaande is vervangen door beperking percentage is gebaseerd, die betere met dynamische clusters waarin het aantal services en de knooppunten regelmatig wijzigen schaalt.

De vertragingen zijn gebaseerd op een percentage van het aantal replica's in de clusters. Op basis van Percetage vertragingen inschakelen uitdrukken van de regel: 'niet verplaatsen meer dan 10% van de replica's in een interval van 10 minuten', bijvoorbeeld.

De configuratie-instellingen voor bandbreedtebeperking op basis van een percentage zijn:

  - GlobalMovementThrottleThresholdPercentage - verplaatsingen toegestaan in het cluster op elk gewenst moment het maximale aantal uitgedrukt als percentage van totaal aantal replica's in het cluster. 0 geeft geen limiet. De standaardwaarde is 0. Als deze instelling en de GlobalMovementThrottleThreshold worden opgegeven, wordt de limiet voor meer conservatief gebruikt.
  - GlobalMovementThrottleThresholdPercentageForPlacement - maximumaantal verplaatsingen van het type toegestaan tijdens de fase plaatsing uitgedrukt als percentage van totaal aantal replica's in het cluster. 0 geeft geen limiet. De standaardwaarde is 0. Als deze instelling en de GlobalMovementThrottleThresholdForPlacement worden opgegeven, wordt de limiet voor meer conservatief gebruikt.
  - GlobalMovementThrottleThresholdPercentageForBalancing - maximumaantal verplaatsingen van het type toegestaan tijdens de fase taakverdeling, uitgedrukt als percentage van totaal aantal replica's in het cluster. 0 geeft geen limiet. De standaardwaarde is 0. Als deze instelling en de GlobalMovementThrottleThresholdForBalancing worden opgegeven, wordt de limiet voor meer conservatief gebruikt.

Wanneer u het versnellingspercentage opgeeft, geeft u 5% als 0,05. Het interval waarop deze vertragingen zijn bepaald is de GlobalMovementThrottleCountingInterval die is opgegeven in seconden.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThresholdPercentage",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForPlacement",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForBalancing",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

### <a name="default-count-based-throttles"></a>Standaard op basis van aantal vertragingen
Deze informatie wordt geboden voor het geval u oudere clusters hebt, of behoud van deze configuraties in clusters die sindsdien zijn bijgewerkt. In het algemeen wordt aangeraden dat deze worden vervangen door de bovenstaande vertragingen op basis van een percentage. Aangezien bandbreedtebeperking op basis van het percentage is standaard uitgeschakeld, blijven deze vertragingen de standaard vertragingen voor een cluster totdat deze zijn uitgeschakeld en door de vertragingen op basis van een percentage vervangen. 

  - GlobalMovementThrottleThreshold – deze instelling bepaalt het totale aantal verplaatsingen van het type in het cluster gedurende een bepaalde periode. De hoeveelheid tijd is in seconden als de GlobalMovementThrottleCountingInterval opgegeven. De standaardwaarde voor de GlobalMovementThrottleThreshold is 1000 en de standaardwaarde voor de GlobalMovementThrottleCountingInterval 600 is.
  - MovementPerPartitionThrottleThreshold – deze instelling bepaalt het totale aantal verplaatsingen van het type voor elke partitie service gedurende een bepaalde periode. De hoeveelheid tijd is in seconden als de MovementPerPartitionThrottleCountingInterval opgegeven. De standaardwaarde voor de MovementPerPartitionThrottleThreshold is 50 en de standaardwaarde voor de MovementPerPartitionThrottleCountingInterval 600 is.

De configuratie voor deze vertragingen volgt hetzelfde patroon als bandbreedtebeperking op basis van een percentage.

## <a name="next-steps"></a>Volgende stappen
- Als u wilt weten over hoe de Cluster Resource Manager beheert en een compromis tussen de werklast van het cluster, Raadpleeg het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)
- De Cluster Resource Manager bevat veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
