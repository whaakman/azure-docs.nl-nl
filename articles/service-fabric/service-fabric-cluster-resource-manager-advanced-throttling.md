---
title: Beperkingen in de Service Fabric-cluster resource manager | Microsoft Docs
description: Meer informatie over het configureren van de beperkingen die is opgegeven door de Service Fabric Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4c86655b650464f1debadab35fdd82611d17ad81
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55092336"
---
# <a name="throttling-the-service-fabric-cluster-resource-manager"></a>Beperking van de Service Fabric Cluster Resource Manager
Zelfs als u de Cluster Resource Manager correct hebt geconfigureerd, kan het cluster ophalen verstoord. Bijvoorbeeld, kunnen er gelijktijdige knooppunt en fouttolerantie domein fouten - wat er zou gebeuren als die is opgetreden tijdens een upgrade? Cluster Resource Manager probeert altijd om op te lossen alles, van het cluster-resources ordenen en oplossen van het cluster wilt gebruiken. Vertragingen te bieden een backstop zodat het cluster resources kunt te laten stabiliseren - terugkeren door de knooppunten, de netwerkpartities herstel knooppuntservice, gecorrigeerde bits geïmplementeerd.

Om te helpen met dit soort situaties, bevat het Service Fabric Cluster Resource Manager verschillende vertragingen in. Deze beperkingen zijn alle vrij groot hamers. Ze mag niet in het algemeen worden gewijzigd zonder een zorgvuldige planning en testen.

Als u de Cluster Resource Manager vertragingen wijzigt, moet u ze stemmen op uw verwachte werkelijke belasting. U kunt bepalen moet u enkele vertragingen zijn aanwezig is, zelfs als het langer duurt het cluster te laten stabiliseren in sommige gevallen betekent. Testen is vereist voor het bepalen van de juiste waarden voor vertragingen in. Vertragingen in moeten hoog genoeg zijn om toe te staan van het cluster om te reageren op wijzigingen in een redelijk tijdsbestek en laag genoeg is om daadwerkelijk te voorkomen dat te veel verbruik van databaseresources. 

De meeste van de tijd die we hebben gezien dat klanten gebruik vertragingen in die het is omdat ze al in een omgeving met beperkte resource waren. Enkele voorbeelden zijn beperkte netwerkbandbreedte voor afzonderlijke knooppunten of schijven die niet kunnen worden opgebouwd veel stateful replica's parallel vanwege beperkingen van de doorvoer. Zonder vertragingen, kunnen operations deze resources, waardoor bewerkingen mislukt of niet trage overbelasten. In deze situaties klanten gebruikt vertragingen en wist dat ze zijn uitbreiden van de hoeveelheid tijd zou het duren voordat het cluster geen stabiele status bereiken. Klanten ook begrepen dat ze kunnen terechtkomen op lagere betrouwbaarheid wordt uitgevoerd terwijl ze zijn beperkt.


## <a name="configuring-the-throttles"></a>De beperkingen configureren

Service Fabric heeft twee mechanismen voor de beperking van het aantal verplaatsingen van het type replica. Het standaardmechanisme voor die voordat de Service Fabric 5.7 bestonden vertegenwoordigt beperking als een absoluut aantal verplaatsingen toegestaan. Dit werkt niet voor clusters van elke omvang. Met name voor grote clusters is de standaardwaarde te klein, aanzienlijk vertragen balancing zelfs wanneer dit nodig is, terwijl hiervoor essentiële geen effect in kleinere clusters. Dit mechanisme voorafgaande is vervangen door het percentage op basis van beperking, die kan worden geschaald beter met dynamische clusters waarin het aantal services en knooppunten regelmatig gewijzigd.

De vertragingen zijn gebaseerd op een percentage van het aantal replica's in de clusters. Percentage op basis van vertragingen inschakelen uitdrukken van de regel: 'Verplaats niet meer dan 10% van de replica's in een interval van 10 minuten', bijvoorbeeld.

De configuratie-instellingen voor het percentage op basis van beperking zijn:

  - GlobalMovementThrottleThresholdPercentage - verplaatsingen van het type toegestaan in het cluster op elk gewenst moment het maximale aantal uitgedrukt als percentage van totaal aantal replica's in het cluster. 0 geeft aan dat er geen limiet. De standaardwaarde is 0. Als deze instelling en de GlobalMovementThrottleThreshold zijn opgegeven, wordt de limiet van meer conservatieve gebruikt.
  - GlobalMovementThrottleThresholdPercentageForPlacement - maximumaantal verplaatsingen van het type toegestaan tijdens de fase plaatsing, uitgedrukt als percentage van totale aantal replica's in het cluster. 0 geeft aan dat er geen limiet. De standaardwaarde is 0. Als deze instelling en de GlobalMovementThrottleThresholdForPlacement zijn opgegeven, wordt de limiet van meer conservatieve gebruikt.
  - GlobalMovementThrottleThresholdPercentageForBalancing - maximumaantal verplaatsingen van het type toegestaan tijdens de fase taakverdeling, uitgedrukt als percentage van totale aantal replica's in het cluster. 0 geeft aan dat er geen limiet. De standaardwaarde is 0. Als deze instelling en de GlobalMovementThrottleThresholdForBalancing zijn opgegeven, wordt de limiet van meer conservatieve gebruikt.

Wanneer u het percentage vertraging opgeeft, geeft u 5% als 0,05. Het interval op waarmee deze beperkingen gelden is de GlobalMovementThrottleCountingInterval, die is opgegeven in seconden.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

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

### <a name="default-count-based-throttles"></a>Standaard aantal op basis van vertragingen in
Deze informatie wordt verstrekt in het geval u oudere clusters hebt, of behoud van deze configuraties in clusters die sindsdien zijn bijgewerkt. In het algemeen is het aanbevolen dat deze worden vervangen door de bovenstaande beperkingen op basis van een percentage. Omdat het percentage op basis van beperking is standaard uitgeschakeld, blijven deze vertragingen in de standaard vertragingen in voor een cluster totdat ze zijn uitgeschakeld en door de beperkingen op basis van een percentage vervangen. 

  - GlobalMovementThrottleThreshold – met deze instelling bepaalt het totale aantal verplaatsingen van het type in het cluster gedurende een bepaalde periode. De hoeveelheid tijd is in seconden als de GlobalMovementThrottleCountingInterval opgegeven. De standaardwaarde voor de GlobalMovementThrottleThreshold is 1000 en de standaardwaarde voor de GlobalMovementThrottleCountingInterval is 600.
  - MovementPerPartitionThrottleThreshold – met deze instelling bepaalt het totale aantal verplaatsingen van het type voor elke servicepartitie gedurende een bepaalde periode. De hoeveelheid tijd is in seconden als de MovementPerPartitionThrottleCountingInterval opgegeven. De standaardwaarde voor de MovementPerPartitionThrottleThreshold is 50 en de standaardwaarde voor de MovementPerPartitionThrottleCountingInterval is 600.

De configuratie voor deze vertragingen volgt hetzelfde patroon als het percentage op basis van beperking.

## <a name="next-steps"></a>Volgende stappen
- Als u wilt weten over hoe met Cluster Resource Manager beheert en verdeelt de taken in het cluster, Zie het artikel op [taakverdeling](service-fabric-cluster-resource-manager-balancing.md)
- Cluster Resource Manager beschikt over veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
