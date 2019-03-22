---
title: Uw Azure Service Fabric-cluster in balans brengen | Microsoft Docs
description: Een inleiding tot de taakverdeling van uw cluster met de Service Fabric Cluster Resource Manager.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9a124bd9a52e22c359fb771e4d4c8714bd1dbe2c
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58123229"
---
# <a name="balancing-your-service-fabric-cluster"></a>Taakverdeling van uw service fabric-cluster
De Service Fabric Cluster Resource Manager biedt ondersteuning voor dynamische belasting verandert, reageren op toevoegingen of verwijderingen van knooppunten of services. Het wordt ook automatisch gecorrigeerd schendingen van plaatsingsbeperkingen en proactief rebalances de cluster. Maar hoe vaak deze acties worden uitgevoerd, en wat ze activeert?

Er zijn drie verschillende categorieën van werk dat de Cluster Resource Manager wordt uitgevoerd. Dit zijn:

1. Plaatsing: deze fase worden behandeld met een stateful replica's of stateless exemplaren die ontbreken. Plaatsing bevat nieuwe services en verwerken van stateful replica's of stateless exemplaren die zijn mislukt. Verwijderen en het verwijderen van replica's of -exemplaren worden hier verwerkt.
2. Beperking wordt gecontroleerd: deze fase controleert en corrigeert schendingen van de verschillende plaatsingsbeperkingen (regels) binnen het systeem. Voorbeelden van regels zijn dingen als u ervoor te zorgen dat knooppunten niet overbelast zijn en dat de plaatsingsbeperkingen van een service is voldaan.
3. Netwerktaakverdeling: deze fase wordt gecontroleerd om te zien of herverdeling nodig op basis van het geconfigureerde gewenste niveau saldo voor verschillende metrische gegevens. Als dit het geval wordt geprobeerd een rangschikking vinden in het cluster dat is meer met gelijke taakverdeling.

## <a name="configuring-cluster-resource-manager-timers"></a>Cluster Resource Manager-Timers configureren
De eerste set besturingselementen om netwerktaakverdeling zijn een set van timers. Deze timers bepalen hoe vaak met Cluster Resource Manager onderzoekt van het cluster en voert corrigerende acties uit.

Elk van deze soorten correcties met Cluster Resource Manager maken kunt wordt bepaald door een andere timer die geldt voor de frequentie ervan. Wanneer de timer wordt geactiveerd, wordt de taak is gepland. Standaard de Resource Manager:

* controleert de status en past updates (zoals de opnamen die een knooppunt niet beschikbaar is) op elke 1/10e van een seconde
* Hiermee stelt u de plaatsing van controle-vlag 
* Hiermee stelt u de beperking check-vlag per seconde
* Hiermee stelt u de vlag voor taakverdeling om de vijf seconden.

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

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

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

Vandaag nog met Cluster Resource Manager voert alleen uit een van deze acties op een tijdstip sequentieel worden verwerkt. Dit is de reden waarom we naar deze timers als 'minimale intervallen' en de acties die u uitgevoerd verwijzen wanneer de timers uitschakelen als 'vlaggen voor instelling gaat'. Bijvoorbeeld, zorgt met Cluster Resource Manager in behandeling zijnde aanvragen voor het maken van services voordat u Netwerktaakverdeling van het cluster. Zoals u door de standaard-tijdsintervallen die is opgegeven ziet, wordt de Cluster Resource Manager scant voor alles wat nodig is om u te vaak doen. Dit betekent doorgaans dat de set wijzigingen tijdens elke stap klein is. Kleine wijzigingen vaak maken, kunt de Cluster Resource Manager niet meer reageert wanneer dingen in het cluster gebeuren. De standaard-timers bieden via batchverwerking uitvoeren omdat er veel van dezelfde typen gebeurtenissen meestal tegelijk worden uitgevoerd. 

Bijvoorbeeld, als knooppunten uitvallen, ze kunnen dus hele domeinen met fouten doen op een tijdstip. Al deze fouten worden vastgelegd tijdens de volgende status bijwerken nadat u hebt de *PLBRefreshGap*. De correcties zijn vastgelegd tijdens de volgende plaatsing, check-beperking, en taakverdeling wordt uitgevoerd. Cluster Resource Manager is niet standaard scannen via de uren van wijzigingen in het cluster en probeert om alle wijzigingen in één keer. In dat geval zou leiden tot pieken van het verloop.

Cluster Resource Manager moet ook aanvullende informatie om te bepalen of het cluster imbalanced. Voor die we hebben twee andere onderdelen van de configuratie: *BalancingThresholds* en *ActivityThresholds*.

## <a name="balancing-thresholds"></a>Drempelwaarden voor netwerktaakverdeling
Een drempel voor netwerktaakverdeling is het belangrijkste besturingselement voor het activeren van herverdelen. De drempelwaarde voor taakverdeling voor een metrische waarde is een _verhouding_. Als de belasting voor metrische gegevens op het meest geladen knooppunt gedeeld door de hoeveelheid belasting op het minste geladen knooppunt groter is dan van dat metriek *BalancingThreshold*, en vervolgens het cluster imbalanced is. Als gevolg hiervan taakverdeling, wordt de volgende keer dat de Cluster Resource Manager controleert geactiveerd. De *MinLoadBalancingInterval* timer wordt gedefinieerd hoe vaak met Cluster Resource Manager moet controleren als herverdeling nodig is. Controle van betekent niet dat er iets gebeurt. 

Taakverdeling drempelwaarden zijn gedefinieerd op basis van per metrische gegevens als onderdeel van de clusterdefinitie van de. Bekijk voor meer informatie over metrische gegevens [in dit artikel](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

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

![Voorbeeld van de drempelwaarde voor netwerktaakverdeling][Image1]
</center>

In dit voorbeeld wordt elke service sommige metrische gegevens voor één eenheid verbruikt. In het bovenste voorbeeld wordt de maximale belasting van een knooppunt is vijf en de minimumwaarde is twee. Stel de drempelwaarde voor taakverdeling voor deze metrische gegevens is drie. Omdat de verhouding in het cluster is 5/2 = 2,5 en dat is minder dan de opgegeven drempelwaarde van drie, het cluster is taakverdeling. Er zijn geen taakverdeling wordt geactiveerd wanneer de Cluster Resource Manager controleert.

In het voorbeeld onder is de maximale belasting van een knooppunt 10, terwijl de minimumwaarde twee is, wat resulteert in een ratio van vijf. Vijf is groter dan de aangewezen drempelwaarde voor taakverdeling van drie voor deze metrische gegevens. Als gevolg hiervan zijn een herverdeling uitvoeren geplande zodra de taakverdeling timer wordt geactiveerd. In een dergelijke situatie wordt meestal enkele load gedistribueerd naar Knooppunt3. Omdat de Service Fabric Cluster Resource Manager biedt geen een greedy benadering gebruiken, kan sommige load ook worden gedistribueerd naar knooppunt2. 

<center>

![Taakverdeling drempelwaarde voorbeeld acties][Image2]
</center>

> [!NOTE]
> "Balancing" twee verschillende strategieën voor het beheren van laden in het cluster worden verwerkt. De standaardstrategie die gebruikmaakt van Cluster Resource Manager is verdelen over de knooppunten in het cluster. De andere strategie bestaat uit [defragmentatie](service-fabric-cluster-resource-manager-defragmentation-metrics.md). Defragmentatie wordt uitgevoerd tijdens de dezelfde balancing. De taakverdeling en defragmentatie strategieën kunnen worden gebruikt voor verschillende metrische gegevens binnen hetzelfde cluster. Een service kan metrische gegevens over taakverdeling en defragmentatie hebben. Voor defragmentatie metrische gegevens, de verhouding van het laden in het cluster wordt geactiveerd wanneer het herverdelen _hieronder_ de drempel voor taakverdeling. 
>

Onder de drempel van taakverdeling ophalen is niet een expliciete doel. Taakverdeling drempelwaarden zijn slechts een *trigger*. De taakverdeling wordt uitgevoerd, bepaalt de Cluster Resource Manager welke verbeteringen die kan worden gemaakt, indien van toepassing. Alleen omdat een taakverdeling zoekopdracht wordt gestart betekent niet dat alles wordt verplaatst. Het cluster is soms imbalanced, maar ook beperkte om op te lossen. U kunt ook de verbeteringen verplaatsingen die te zijn vereisen [kostbare](service-fabric-cluster-resource-manager-movement-cost.md)).

## <a name="activity-thresholds"></a>Activiteit drempelwaarden
Soms, hoewel er knooppunten zijn relatief imbalanced de *totale* hoeveelheid laden in het cluster is laag. Het ontbreken van load kan een tijdelijke dip, of omdat het cluster is er nieuw en alleen ophalen er wordt opnieuw opgestart. In beide gevallen moet u mogelijk niet wilt taakverdeling van het cluster, omdat er weinig worden verkregen. Als het cluster twee taakverdeling, zou u uitgaven netwerk en rekenresources zaken herinrichten zonder dat een grote *absolute* verschil. Om te voorkomen dat onnodige wordt verplaatst, wordt er een ander besturingselement bekend als activiteit drempelwaarden. Drempelwaarden voor activiteit kunt u sommige absolute ondergrens voor activiteit opgeven. Als er geen knooppunt boven deze drempelwaarde, wordt niet taakverdeling geactiveerd, zelfs als de drempelwaarde voor netwerktaakverdeling wordt voldaan.

Stel dat we onze drempelwaarde Balancing drie voor deze metrische gegevens bewaren. Ook Stel dat we hebben een drempel voor de activiteit van 1536. In het eerste geval is, terwijl de cluster is imbalanced per de netwerktaakverdeling drempelwaarde er kan geen knooppunt voor voldoet aan deze drempelwaarde activiteit, zodat er niets gebeurt. In het voorbeeld onder is knooppunt1 hoger dan de drempelwaarde voor de activiteit. Omdat zowel de drempel voor taakverdeling en de activiteit drempelwaarde voor de metrische gegevens zijn overschreden, is taakverdeling gepland. Een voorbeeld: Bekijk het volgende diagram: 

<center>

![Voorbeeld van de activiteit drempelwaarde][Image3]
</center>

Net zoals Netwerktaakverdeling drempelwaarden, drempels activiteit per-rekenmodel via de clusterdefinitie van de zijn:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

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

Drempelwaarden voor taakverdeling en activiteit zijn beide gekoppeld aan een bepaalde meetwaarde - balancing alleen als de drempelwaarde voor taakverdeling en de activiteit drempelwaarde is overschreden voor de dezelfde metrische gegevens wordt geactiveerd.

> [!NOTE]
> Als niet is opgegeven, is de drempel voor taakverdeling voor een metrische waarde 1, en de drempelwaarde voor de activiteit is ingesteld op 0. Dit betekent dat Cluster Resource Manager wordt geprobeerd om te houden die perfect evenwicht voor een elke belasting door bepaalde meetwaarde. Als u aangepaste metrische gegevens is het aanbevolen dat u uw eigen drempelwaarden voor taakverdeling en activiteit expliciet voor uw metrische gegevens definieert. 
>

## <a name="balancing-services-together"></a>Samen Balancing-services
Of het cluster imbalanced is of niet is een beslissing brede, door het cluster. De manier waarop we gaat over het oplossen van het is echter afzonderlijke service-replica's en exemplaren rond verplaatsen. Dit klinkt logisch, toch? Als geheugen is op één knooppunt van gestapeld, meerdere replica's of exemplaren kunnen een bijdrage leveren aan deze. De imbalance vaststelling nodig verplaatsen van een van de stateful replica's of stateless exemplaren die gebruikmaken van de imbalanced metrische gegevens.

Af en toe als een service die is niet van zichzelf imbalanced opgehaald verplaatst (Houd er rekening mee de discussie van lokale en globale gewicht eerder). Waarom zou een service ophalen verplaatst wanneer alle metrische gegevens van de service zijn verdeeld? We bekijken een voorbeeld:

- Stel dat er zijn vier services, Service1, Service2, Service3 en Service4. 
- Metrische gegevens Metric1 en Metric2 Service1-rapporten. 
- Metrische gegevens Metric2 en Metric3 Service2-rapporten. 
- Metrische gegevens Metric3 en Metric4 Service3-rapporten.
- Service4 metrische Metric99 rapporteert. 

U kunt staren zien waar we hier gaan: Er is een keten. Er zijn geen echt vier onafhankelijke services, er zijn drie services die zijn gekoppeld en dat is uitgeschakeld op een eigen.

<center>

![Samen Balancing-Services][Image4]
</center>

Vanwege deze keten is het mogelijk dat een uit balans zijn metrische gegevens over 1-4 ervoor zorgen dat replica's of -exemplaren die behoren tot de services 1-3 om te navigeren. Ook weten dat een uit balans zijn metrische gegevens over 1, 2 of 3 verplaatsingen van het type niet kan in Service4 veroorzaken. Er is geen punt sinds het verplaatsen van de replica's of instanties die behoren tot Service4 rond helemaal geen invloed hebben op het saldo van metrische gegevens over 1-3 kunnen doen.

Cluster Resource Manager wordt automatisch zoekt uit welke services zijn gerelateerd. Toe te voegen, te verwijderen of wijzigen van de metrische gegevens voor services kan invloed hebben op hun relaties. Bijvoorbeeld, tussen twee uitvoeringen van netwerktaakverdeling Service2 mogelijk zijn bijgewerkt als u wilt verwijderen van Metric2. Hiermee wordt de keten tussen Service1 en Service2 verbroken. In plaats van twee groepen met gerelateerde services zijn er nu drie:

<center>

![Samen Balancing-Services][Image5]
</center>

## <a name="next-steps"></a>Volgende stappen
* Metrische gegevens zijn hoe gebruiks- en capaciteit in het cluster worden beheerd door de Service Fabric-Cluster Resource Manager. Bekijk voor meer informatie over metrische gegevens en hoe u ze configureert, [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
* Kosten voor gegevensverplaatsing is één manier om naar de Cluster Resource Manager-signalering dat bepaalde services zijn duurder dan de andere verplaatsen. Raadpleeg voor meer informatie over de kosten voor gegevensverplaatsing, [in dit artikel](service-fabric-cluster-resource-manager-movement-cost.md)
* Cluster Resource Manager heeft verschillende beperkingen die u configureren kunt voor het verloop in het cluster vertragen. Ze zijn niet normaal gesproken nodig, maar als u deze nodig hebt vindt u informatie over deze [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
