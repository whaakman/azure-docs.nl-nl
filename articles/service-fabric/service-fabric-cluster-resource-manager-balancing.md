---
title: Een balans vinden tussen uw Azure Service Fabric-cluster | Microsoft Docs
description: Een inleiding tot het cluster met Service Fabric Cluster Resource Manager netwerktaakverdeling.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5d2f195c50750a5c7685f62c909f77b2960613e6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34213143"
---
# <a name="balancing-your-service-fabric-cluster"></a>Taakverdeling van uw service fabric-cluster
De Service Fabric Cluster Resource Manager biedt ondersteuning voor wijzigingen van de dynamische belasting, reageren op toevoegingen of verwijderingen van knooppunten of services. Deze functie ook automatisch gecorrigeerd schendingen van plaatsingsbeperkingen en proactief rebalances het cluster. Maar hoe vaak deze acties worden uitgevoerd en wat ze activeert?

Er zijn drie verschillende categorieën van het werk dat de Cluster Resource Manager uitvoert. Dit zijn:

1. Plaatsing: deze fase omgaat met de plaatsing van een stateful replica's of stateless exemplaren die ontbreken. Plaatsing bevat nieuwe services en de afhandeling van stateful replica's of stateless exemplaren die zijn mislukt. Verwijderen en het verwijderen van replica's of exemplaren worden hier afgehandeld.
2. Beperking controleert: deze fase controleert en corrigeert schendingen van de verschillende plaatsingsbeperkingen (regels) in het systeem. Voorbeelden van regels zijn bijvoorbeeld ervoor te zorgen dat knooppunten niet overbelast zijn en dat de plaatsingsbeperkingen voor een service is voldaan.
3. Netwerktaakverdeling: deze fase wordt gecontroleerd om te zien als herverdeling nodig op basis van het geconfigureerde gewenste niveau van verdelen voor een andere metrische gegevens is. Als dit het geval probeert een rangschikking in het cluster dat is meer in evenwicht te vinden.

## <a name="configuring-cluster-resource-manager-timers"></a>Cluster Resource Manager Timers configureren
De eerste set besturingselementen rond de taakverdeling van een reeks timers zijn. Deze timers bepalen hoe vaak de Cluster Resource Manager onderzoekt het cluster en neemt corrigerende maatregelen.

Elk van deze andere typen van de Cluster Resource Manager kunt aanbrengen correcties wordt beheerd door een andere timer die de frequentie bepaalt. Wanneer elke timer wordt gestart, wordt de taak is gepland. Standaard de Resource Manager:

* de status wordt gescand en past updates (zoals opname die een knooppunt niet actief is) elke 1/10 van een seconde
* stelt de vlag voor plaatsing selectievakje 
* stelt de vlag voor het controleren van beperking per seconde
* stelt de vlag voor taakverdeling om de vijf seconden.

Voorbeelden van de configuratie van deze timers bestuur zijn hieronder:

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

Vandaag voert de Cluster Resource Manager alleen een van deze acties op een tijdstip sequentieel worden verwerkt. Daarom is deze timers als 'minimale intervallen' en de acties die ophalen uitgevoerd wanneer de timers uitschakelen als 'instelling vlaggen gaat'. Bijvoorbeeld, zorgt de Cluster Resource Manager voor in behandeling zijnde aanvragen voor het maken van services voordat het cluster verdelen. Zoals u door de standaard tijdsintervallen opgegeven zien kunt, wordt de Cluster Resource Manager voor alles moet worden regelmatig scant. Dit betekent doorgaans dat de set wijzigingen die zijn aangebracht tijdens elke stap klein is. Kleine wijzigingen vaak maken, kunt de Cluster Resource Manager worden reageren als er dingen optreden in het cluster. De standaard timers bieden sommige batchverwerking omdat veel van dezelfde typen gebeurtenissen meestal tegelijkertijd optreden. 

Bijvoorbeeld, wanneer knooppunten uitvallen ze dus hele domeinen met fouten per keer kunnen doen. Alle deze fouten worden vastgelegd tijdens de volgende status bijwerken nadat u hebt de *PLBRefreshGap*. De correcties zijn vastgelegd tijdens de plaatsing van volgende check-beperking, en Netwerktaakverdeling wordt uitgevoerd. De Cluster Resource Manager is niet standaard scannen via uren van wijzigingen in het cluster en probeert om alle wijzigingen in één keer. In dat geval zou leiden tot lichtflitsen verloop.

De Cluster Resource Manager moet ook aanvullende informatie om te bepalen of het cluster imbalanced. Voor die we hebben twee andere delen van configuratie: *BalancingThresholds* en *ActivityThresholds*.

## <a name="balancing-thresholds"></a>Drempelwaarden voor netwerktaakverdeling
Een drempelwaarde Balancing is het belangrijkste besturingselement voor activering van herverdeling. De drempelwaarde voor taakverdeling voor een metriek is een _verhouding_. Als de belasting voor een metriek op het meest geladen knooppunt gedeeld door de hoeveelheid belasting op het knooppunt minste geladen groter is dan deze metriek *BalancingThreshold*, dan is het cluster imbalanced. Als gevolg hiervan balancing, wordt de volgende keer dat de Cluster Resource Manager controleert geactiveerd. De *MinLoadBalancingInterval* timer definieert hoe vaak de Cluster Resource Manager moet controleren als herverdeling nodig is. Controleren betekent niet dat iets gebeurt. 

Drempelwaarden voor taakverdeling worden gedefinieerd op basis van per metriek als onderdeel van de definitie van de cluster. Bekijk voor meer informatie over metrische gegevens [in dit artikel](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<center>
![Voorbeeld van taakverdeling drempelwaarde][Image1]
</center>

In dit voorbeeld wordt elke service één eenheid van sommige metriek verbruikt. In het bovenste voorbeeld wordt de maximale belasting van een knooppunt is vijf en de minimale is twee. Stel de drempelwaarde voor taakverdeling voor deze metriek is drie. Aangezien de verhouding in het cluster is 5/2 = 2,5 en dat is kleiner dan de opgegeven drempelwaarde van drie, het cluster is taakverdeling. Er is geen balancing wordt geactiveerd wanneer de Cluster Resource Manager controleert.

De maximale belasting van een knooppunt is in het voorbeeld onder 10, terwijl de minimaal twee, wat resulteert in een ratio van vijf. Vijf is groter dan de drempelwaarde voor aangewezen taakverdeling van drie voor deze metrische gegevens. Als gevolg hiervan zijn een herverdeling uitvoeren geplande zodra de taakverdeling timer wordt geactiveerd. In een dergelijke situatie wordt meestal sommige load gedistribueerd naar Knooppunt3. Omdat de Service Fabric Cluster Resource Manager biedt geen een greedy benadering gebruiken, kan sommige load ook worden gedistribueerd naar knooppunt2. 

<center>
![Taakverdeling drempelwaarde voorbeeld acties][Image2]
</center>

> [!NOTE]
> Twee verschillende strategieën voor het beheren van laden in het cluster 'Balancing' worden verwerkt. De standaard-strategie die gebruikmaakt van de Cluster Resource Manager is om te verdelen over de knooppunten in het cluster. De andere strategie is [defragmentatie](service-fabric-cluster-resource-manager-defragmentation-metrics.md). Defragmentatie wordt uitgevoerd tijdens de dezelfde verdeling. De strategieën taakverdeling en defragmentatie kunnen worden gebruikt voor andere metrische gegevens in hetzelfde cluster. Een service kunt taakverdeling en defragmentatie metrische gegevens hebben. Voor defragmentatie metrische gegevens en de verhouding van de belasting in het cluster activeert wanneer deze herverdeling _hieronder_ de drempelwaarde voor taakverdeling. 
>

Onder de drempel van taakverdeling ophalen is niet een expliciete doel. Taakverdeling drempelwaarden zijn slechts een *trigger*. De taakverdeling wordt uitgevoerd, bepaalt de Cluster Resource Manager welke verbeteringen in deze maken kan, indien van toepassing. Omdat een taakverdeling zoekopdracht is gestarte betekent niet dat alles wordt verplaatst. Soms is het cluster imbalanced maar te beperkte om op te lossen. U kunt ook de verbeteringen verplaatsingen die te zijn vereisen [kostbare](service-fabric-cluster-resource-manager-movement-cost.md)).

## <a name="activity-thresholds"></a>Drempelwaarden voor de activiteit
Soms, hoewel de knooppunten zijn relatief imbalanced de *totale* hoeveelheid laden in het cluster is laag. Het ontbreken van de belasting kan een tijdelijke dip, of omdat het cluster nieuw en net ophalen is bootstrap uitvoert. In beide gevallen kunt u niet wilt laten Netwerktaakverdeling van het cluster, omdat er te weinig tijd te besteden aan. Als het cluster twee balancing, zou u netwerk hoeven te besteden aan en rekenresources objecten verplaatsen zonder dat een grote *absolute* verschil. Om te voorkomen dat onnodige wordt verplaatst, er is een ander besturingselement activiteit drempelwaarden genoemd. Drempelwaarden voor de activiteit kunt u sommige absolute ondergrens voor de activiteit opgeven. Als er geen knooppunt boven deze drempelwaarde, is niet balancing geactiveerd, zelfs als de drempelwaarde Balancing wordt voldaan.

Stel dat we onze drempelwaarde Balancing van drie voor deze metrische gegevens behouden. Stel ook dat we hebben een drempel voor de activiteit van 1536. In het eerste geval terwijl het cluster is imbalanced per de drempelwaarde Balancing er is geen knooppunt voldoet aan deze drempelwaarde activiteit zodat er niets gebeurt. In het voorbeeld onder is knooppunt1 hoger dan de drempelwaarde voor de activiteit. Omdat zowel de drempelwaarde voor netwerktaakverdeling en de activiteit drempelwaarde voor de metriek zijn overschreden, is balancing gepland. Een voorbeeld: Bekijk het volgende diagram: 

<center>
![Voorbeeld van de activiteit drempelwaarde][Image3]
</center>

Net als de drempelwaarden voor netwerktaakverdeling zijn activiteit drempelwaarden gedefinieerde per-meetwaarde via de definitie van het cluster:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

Taakverdeling en activiteit drempelwaarden zijn beide gekoppeld aan een specifieke metric - balancing alleen als de drempelwaarde voor netwerktaakverdeling en de activiteit drempelwaarde is overschreden voor dezelfde metric wordt geactiveerd.

> [!NOTE]
> Wanneer niet wordt opgegeven, is de netwerktaakverdeling drempelwaarde voor een metriek 1 en de drempelwaarde voor de activiteit is 0. Dit betekent dat de Cluster Resource Manager wordt geprobeerd te houden die metriek perfect met gelijke taakverdeling voor een bepaalde belasting. Als u van aangepaste metrische gegevens gebruikmaakt is het raadzaam om uw eigen drempelwaarden voor taakverdeling en activiteit expliciet te definiëren voor de metrische gegevens van uw. 
>

## <a name="balancing-services-together"></a>Services Netwerktaakverdeling samen
Hiermee wordt aangegeven of het cluster imbalanced is of niet is een beslissing gehele cluster. De manier waarop wij gaat over het oplossen van het is echter afzonderlijke service replica's en -exemplaren rond verplaatsen. Dit klinkt logisch, toch? Als het geheugen is up gestapelde op één knooppunt, meerdere replica's of exemplaren kunnen een bijdrage leveren aan het. De onbalans is hersteld nodig zijn de stateful-replica's of stateless exemplaren die gebruikmaken van de imbalanced metrische gegevens te verplaatsen.

Tijd tot tijd al een service die is niet van zichzelf imbalanced opgehaald verplaatst (Vergeet niet de bespreking van de lokale en globale gewicht eerder). Waarom zou een service verplaatst wanneer alle metrische gegevens van de service zijn verdeeld? We bekijken een voorbeeld:

- Stel dat er zijn vier services, Service1, Service2 Service3 en Service4. 
- Service1 rapporten metrische gegevens Metric1 en Metric2. 
- Service2 rapporten metrische gegevens Metric2 en Metric3. 
- Service3 rapporteert metrische gegevens Metric3 en Metric4.
- Service4 metrische Metric99 rapporteert. 

U kunt nietwaar zien waar gaan we hier: Er is een keten! Er zijn momenteel geen echt vier onafhankelijke services, hebben we drie services die zijn gerelateerd en één die is uitgeschakeld op zichzelf.

<center>
![Services Netwerktaakverdeling samen][Image4]
</center>

Vanwege deze keten is het mogelijk dat een onbalans in metrics 1-4 ertoe leiden dat replica's of exemplaren die horen bij services 1-3 om te navigeren. We ook weten dat een onbalans in Metrics 1, 2 of 3 verplaatsingen van het type kan niet in Service4 veroorzaken. Er zijn geen punt sinds het verplaatsen van de replica's of exemplaren die horen bij Service4 rond helemaal geen invloed op het saldo van metrische gegevens 1-3 kunnen doen.

De Cluster Resource Manager automatisch weten welke services zijn gerelateerd te achterhalen. Toevoegen, verwijderen of wijzigen van de metrische gegevens voor services kan invloed hebben op hun relaties. Bijvoorbeeld, tussen twee uitvoert balancing Service2 mogelijk zijn bijgewerkt voor het verwijderen van Metric2. Hiermee wordt de keten tussen Service1 en Service2 verbroken. In plaats van twee groepen van gerelateerde services zijn er nu drie:

<center>
![Services Netwerktaakverdeling samen][Image5]
</center>

## <a name="next-steps"></a>Volgende stappen
* Metrische gegevens zijn hoe de Service Fabric-Cluster Resource Manager beheert gebruiks- en capaciteit in het cluster. Bekijk voor meer informatie over metrische gegevens en het configureren ervan [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
* Verplaatsingskosten is één manier om de Cluster Resource Manager-signalering dat bepaalde services zijn duurder dan de andere verplaatsen. Raadpleeg voor meer informatie over verplaatsingskosten, [in dit artikel](service-fabric-cluster-resource-manager-movement-cost.md)
* De Cluster Resource Manager bevat verschillende vertragingen die u configureren kunt voor het verloop in het cluster vertragen. Ze dat niet normaal gesproken nodig zijn, maar als u deze nodig hebt vindt u meer informatie hierover [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
