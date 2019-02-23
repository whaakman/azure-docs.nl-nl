---
title: Service Fabric Cluster Resource Manager - integratie van Onkostenbeheer | Microsoft Docs
description: Een overzicht van de integratiepunten tussen de Cluster Resource Manager en Service Fabric-beheer.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: a51593753cab8a6b07d99df46560808de5400047
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737923"
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Cluster resource manager-integratie met beheer van Service Fabric-clusters
De Service Fabric Cluster Resource Manager biedt geen upgrades station in Service Fabric, maar het is betrokken. De eerste manier die met Cluster Resource Manager met management is door bij te houden van de gewenste status van het cluster en de services binnen het. Cluster Resource Manager verzendt statusrapporten wanneer deze het cluster kan niet in de gewenste configuratie. Bijvoorbeeld, als er onvoldoende capaciteit is verzendt met Cluster Resource Manager waarschuwingen en fouten die wijzen op het probleem. Een ander deel van de integratie van heeft te maken met de werking van upgrades. Cluster Resource Manager verandert het gedrag van het iets tijdens upgrades.  

## <a name="health-integration"></a>Health-integratie
Cluster Resource Manager houdt voortdurend de regels die u hebt gedefinieerd voor het plaatsen van uw services. Deze ook houdt de resterende capaciteit voor alle gegevens op de knooppunten en in het cluster en in het cluster als geheel. Als deze niet voldoen aan deze regels of als er onvoldoende capaciteit, worden status, waarschuwingen en fouten uitgezonden. Bijvoorbeeld, als een knooppunt over capaciteit en de Cluster Resource Manager wordt geprobeerd de situatie oplossen door het verplaatsen van services. Als deze niet de situatie oplossen verzendt deze een status-waarschuwing die aangeeft welk knooppunt is dan de capaciteit en voor welke metrische gegevens.

Een ander voorbeeld van de Resource Manager-waarschuwingen is schendingen van plaatsingsbeperkingen. Bijvoorbeeld, als u een beperking voor de plaatsing hebt gedefinieerd (zoals `“NodeColor == Blue”`) en de Resource Manager een schending van deze beperking wordt gedetecteerd, wordt deze een status-waarschuwing verzendt. Dit geldt voor aangepaste beperkingen en de default-beperkingen (zoals de beperkingen voor Foutdomein en het upgraden van domein).

Hier volgt een voorbeeld van een dergelijke statusrapport. In dit geval is het statusrapport voor een van de partities van de systeemservice. De health-bericht geeft aan dat de replica's van de betreffende partitie tijdelijk zijn verpakt in te weinig Upgrade-domeinen.

```posh
PS C:\Users\User > Get-ServiceFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


PartitionId           : 00000000-0000-0000-0000-000000000001
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB', Property='ReplicaConstraintViolation_UpgradeDomain', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 130766528804733380
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577821
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528854889931
                        AggregatedHealthState : Ok

                        ReplicaId             : 130766528804577822
                        AggregatedHealthState : Ok

                        ReplicaId             : 130837073190680024
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.PLB
                        Property              : ReplicaConstraintViolation_UpgradeDomain
                        HealthState           : Warning
                        SequenceNumber        : 130837100116930204
                        SentAt                : 8/10/2015 7:53:31 PM
                        ReceivedAt            : 8/10/2015 7:53:33 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer has detected a Constraint Violation for this Replica: fabric:/System/FailoverManagerService Secondary Partition 00000000-0000-0000-0000-000000000001 is
                        violating the Constraint: UpgradeDomain Details: UpgradeDomain ID -- 4, Replica on NodeName -- Node.8 Currently Upgrading -- false Distribution Policy -- Packing
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/10/2015 7:13:02 PM, LastError = 1/1/0001 12:00:00 AM
```

Dit is wat deze health-bericht is laat ons weten dat is:

1. Alle replica's zelf zijn in orde: Elk heeft AggregatedHealthState: OK
2. De beperking van het upgraden van domein distributie wordt op dit moment wordt geschonden. Dit betekent dat een bepaald domein voor Upgrade heeft meer replica's van deze partitie dan nodig.
3. Welk knooppunt bevat de replica de schending veroorzaakt. In dit geval is het knooppunt met de naam "Node.8"
4. Of een upgrade op dat moment plaatsvindt voor deze partitie ("momenteel upgraden--onwaar")
5. Het distributiebeleid voor deze service: 'Distributiebeleid--verpakken'. Dit is onderhevig aan de `RequireDomainDistribution` [plaatsing beleid](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). 'Verpakking' geeft aan dat in dit geval DomainDistribution _niet_ vereist, zodat we weten dat het beleid voor plaatsing is niet opgegeven voor deze service. 
6. Wanneer het rapport is er gebeurd - 8-10-2015 19:13:02 uur

Informatie, zoals deze bevoegdheden waarschuwingen die worden geactiveerd in productie te laten u weten dat er iets is iets verkeerd gegaan en wordt ook gebruikt om te detecteren en onjuiste upgrades stoppen. In dit geval gaan we bekijken als we waarom de Resource Manager achterhalen kunt om de replica's in het domein Upgrade had. Meestal verpakken is tijdelijk omdat de knooppunten in de andere Upgrade domeinen, bijvoorbeeld zijn.

Stel met Cluster Resource Manager wordt geprobeerd om bepaalde services, maar er zijn oplossingen die werken niet. Als services kunnen niet worden geplaatst, komt dit meestal voor een van de volgende redenen:

1. Sommige tijdelijke toestand is het moeilijk geworden onmogelijk is om deze service-exemplaar of de replica correct plaatsen
2. Vereisten voor de plaatsing van de service zijn ongeldig.

In dergelijke gevallen kunnen statusrapporten van Cluster Resource Manager u bepalen waarom de service kan niet worden geplaatst. We noemen dit proces de volgorde van de opheffing beperking. Tijdens deze helpt het systeem via de geconfigureerde beperkingen die betrekking hebben op de service en -records wat ze te elimineren. Deze manier als services niet kunnen worden geplaatst, kunt u zien welke knooppunten zijn uitgesloten en waarom.

## <a name="constraint-types"></a>Beperkingstypen
Laten we eens over elk van de verschillende beperkingen in deze health-rapporten. Hier ziet u de gezondheid van berichten met betrekking tot deze beperkingen als replica's kunnen niet worden geplaatst.

* **ReplicaExclusionStatic** en **ReplicaExclusionDynamic**: Deze beperkingen geeft aan dat een oplossing is geweigerd, omdat twee serviceobjecten van dezelfde partitie zou hebben op hetzelfde knooppunt worden geplaatst. Dit is niet toegestaan omdat en uitval van dat knooppunt overmatig gevolgen zou hebben voor deze partitie. ReplicaExclusionStatic en ReplicaExclusionDynamic zijn bijna dezelfde regel en de verschillen niet echt toe doen. Als er een beperking opheffing reeks die de ReplicaExclusionStatic of ReplicaExclusionDynamic beperking bevat, met Cluster Resource Manager u denkt dat er niet voldoende knooppunten. Hiervoor moet de resterende oplossingen voor het gebruik van deze ongeldige plaatsingen die zijn niet toegestaan. De andere beperkingen in de reeks wordt meestal Vertel ons waarom knooppunten in de eerste plaats worden worden geëlimineerd.
* **PlacementConstraint**: Als u dit bericht ziet, betekent dit dat we sommige knooppunten verwijderd, omdat ze niet overeenkomen met de plaatsingsbeperkingen van de service. We traceren van de momenteel geconfigureerde plaatsingsbeperkingen als onderdeel van dit bericht. Dit is normaal als er een plaatsing-beperking gedefinieerd. Echter, als de beperking van de plaatsing van onjuist wordt veroorzaakt door te veel knooppunten die moeten worden uitgesloten dit is hoe u ziet.
* **NodeCapacity**: Deze beperking betekent dat de Cluster Resource Manager de replica's op de aangegeven knooppunten kan niet worden plaatsen omdat dat ze over capaciteit plaatst.
* **Affiniteit**: Deze beperking geeft aan dat we de replica kan niet plaats op de betrokken knooppunten omdat dit leiden een schending van de beperking affiniteit tot zou. Meer informatie over de affiniteit is in [in dit artikel](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** en **UpgradeDomain**: Deze beperking wordt voorkomen dat knooppunten als het plaatsen van de replica op de aangegeven knooppunten zou veroorzaken verpakken in een specifieke fout of een upgradedomein. Enkele voorbeelden bespreken van deze beperking worden weergegeven in het onderwerp op [fout- en beperkingen van domein en het resulterende gedrag](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: U mag niet normaal gesproken deze beperking verwijderen van knooppunten van de oplossing, omdat deze wordt uitgevoerd als een optimalisatie standaard zien. De beperking van de gewenste locatie is ook aanwezig zijn tijdens de upgrade. Tijdens de upgrade wordt gebruikt voor services verplaatsen naar waar ze zijn wanneer de upgrade wordt gestart.

## <a name="blocklisting-nodes"></a>Blocklisting knooppunten
Een andere health-bericht is de Cluster Resource Manager-rapporten wanneer knooppunten zijn blocklisted. U kunt blocklisting beschouwen als een tijdelijke beperking die automatisch voor u is toegepast. Knooppunten ontvangen blocklisted wanneer ze herhaalde fouten optreden bij het starten van exemplaren van het type van die service. Knooppunten zijn blocklisted op basis van de per-service-type. Een knooppunt is mogelijk blocklisted voor één servicetype maar niet nog een. 

U ziet blocklisting worden gehouden, trappen vaak tijdens de ontwikkeling: bepaalde bug zorgt ervoor dat de ServiceHost vastloopt tijdens het opstarten. Service Fabric probeert te maken van de ServiceHost een paar keer, en de fout blijft optreden. Het knooppunt opgehaald blocklisted na een paar pogingen, en met Cluster Resource Manager wordt geprobeerd om de service elders te maken. Als deze fout op meerdere knooppunten plaatsvinden blijft, is het mogelijk dat alle knooppunten in de cluster-end van geldige geblokkeerd. Blocklisting kan ook zo veel knooppunten dat kan niet voldoende is Start de service om te voldoen aan de gewenste schaal verwijderen. U ziet doorgaans fouten of waarschuwingen van de Cluster Resource Manager die aangeeft dat de service lager is dan de gewenste replica of aantal exemplaren, evenals de gezondheid van berichten die aangeven wat de fout is dat leidt tot de blocklisting in de eerste plaats.

Blocklisting is niet een permanente voorwaarde. Het knooppunt wordt verwijderd uit de blokkeringslijst na een paar minuten en Service Fabric kunnen de services op dat knooppunt opnieuw activeren. Services kunnen blijven mislukken, dan is het knooppunt opnieuw blocklisted voor die service. 

### <a name="constraint-priorities"></a>Beperking prioriteiten

> [!WARNING]
> Veranderende prioriteiten in uw beperking wordt niet aangeraden en mogelijk aanzienlijke nadelige gevolgen hebben voor uw cluster. De onderstaande informatie is opgegeven voor de verwijzing naar de standaard beperking prioriteiten en hun gedrag. 
>

Met al deze beperkingen kan hebben is denkt u 'Hallo, ik denk dat dat fouten domein beperkingen het belangrijkste in mijn systeem zijn. Om ervoor te zorgen voor dat de beperking van fout met betrekking tot domein niet wordt geschonden, ik wil andere beperkingen schenden."

Beperkingen kunnen worden geconfigureerd met verschillende prioriteitsniveaus. Dit zijn:

   - "vaste" (0)
   - 'soft' (1)
   - "optimalisatie' (2)
   - 'uit' (-1). 
   
De meeste van de beperkingen zijn standaard geconfigureerd als harde beperkingen.

De prioriteit van beperkingen wijzigen is niet gebruikelijk. Er zijn tijden wanneer beperking prioriteiten nodig om te wijzigen, meestal als tijdelijke oplossing voor sommige andere fout of het gedrag is van invloed op de omgeving. In het algemeen de flexibiliteit van de beperking prioriteit infrastructuur zeer goed heeft gewerkt, maar het is niet vaak nodig. De meeste gevallen alles bevindt zich op hun Standaardprioriteiten. 

De prioriteitsniveaus niet betekenen dat een bepaalde beperking _wordt_ worden geschonden, noch wordt altijd voldaan. Beperking prioriteiten definieert een waarin u beperkingen gelden. Prioriteiten definiëren afwegingen wanneer het is niet mogelijk om te voldoen aan alle beperkingen. Alle beperkingen kunnen doorgaans worden voldaan, tenzij er is iets anders in de omgeving gebeurt. Enkele voorbeelden van scenario's die tot schendingen van plaatsingsbeperkingen leiden zijn conflicterende beperkingen of grote aantallen gelijktijdige fouten.

U kunt de prioriteiten beperking wijzigen in geavanceerde situaties. Stel dat u wilt om ervoor te zorgen dat affiniteit altijd zouden worden geschonden, wanneer die nodig zijn voor het oplossen van problemen met knooppunt-capaciteit. U kunt hiervoor de prioriteit van de beperking affiniteit 'soft' (1) instellen en laat u de capaciteit beperking is ingesteld op 'harde' (0).

De standaardwaarden van de prioriteit voor de verschillende beperkingen die zijn opgegeven in de volgende configuratie:

ClusterManifest.xml

```xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PlacementConstraintPriority" Value="0" />
            <Parameter Name="CapacityConstraintPriority" Value="0" />
            <Parameter Name="AffinityConstraintPriority" Value="0" />
            <Parameter Name="FaultDomainConstraintPriority" Value="0" />
            <Parameter Name="UpgradeDomainConstraintPriority" Value="1" />
            <Parameter Name="PreferredLocationConstraintPriority" Value="2" />
        </Section>
```

via ClusterConfig.json voor implementaties met zelfstandige of Template.json voor Azure die worden gehost clusters:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PlacementConstraintPriority",
          "value": "0"
      },
      {
          "name": "CapacityConstraintPriority",
          "value": "0"
      },
      {
          "name": "AffinityConstraintPriority",
          "value": "0"
      },
      {
          "name": "FaultDomainConstraintPriority",
          "value": "0"
      },
      {
          "name": "UpgradeDomainConstraintPriority",
          "value": "1"
      },
      {
          "name": "PreferredLocationConstraintPriority",
          "value": "2"
      }
    ]
  }
]
```

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Beperkingen voor fouttolerantie domein- en upgrade-domein
Cluster Resource Manager wil dat services die zijn verdeeld over fout- en upgradedomeinen te behouden. Dit bepaalt als een beperking in de Cluster Resource Manager-engine. Voor meer informatie over hoe ze worden gebruikt en hun specifieke gedrag, Zie het artikel op [clusterconfiguratie](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior).

Cluster Resource Manager mogelijk om een aantal replica's om u te bekommeren om upgrades, fouten of andere schendingen van plaatsingsbeperkingen in een upgradedomein. Normaal gesproken verpakken in domeinen met fouten of upgrade gebeurt alleen als er zijn meerdere fouten of andere verloop in het systeem te voorkomen dat de juiste plaatsing. Als u wenst te verpakken, zelfs tijdens deze situaties voorkomen, u kunt gebruikmaken van de `RequireDomainDistribution` [plaatsing beleid](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). Houd er rekening mee dat dit kan mogelijk van invloed op servicebeschikbaarheid en betrouwbaarheid als een neveneffect, dus zorgvuldig overwegen.

Als de omgeving goed is geconfigureerd, alle beperkingen zijn volledig in acht genomen, zelfs tijdens upgrades. Het belangrijkste is dat de Cluster Resource Manager is Let op de beperkingen. Wanneer er een fout wordt gedetecteerd het onmiddellijk gerapporteerd en probeert het probleem te corrigeren.

## <a name="the-preferred-location-constraint"></a>De beperking van de gewenste locatie
De beperking PreferredLocation is enigszins anders, omdat er twee verschillende manieren worden gebruikt. Er is een gebruik van deze beperking tijdens upgrades van toepassingen. Cluster Resource Manager beheert automatisch deze beperking tijdens upgrades. Het wordt gebruikt om ervoor te zorgen dat wanneer upgrades voltooid zijn of replica's terug naar hun oorspronkelijke locatie. Het gebruik van de beperking PreferredLocation is voor de [ `PreferredPrimaryDomain` plaatsing beleid](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md). Beide zijn optimalisaties en kan daarom de PreferredLocation-beperking is de enige beperking is standaard ingesteld op 'Optimalisatie'.

## <a name="upgrades"></a>Upgrades
Cluster Resource Manager helpt ook bij de toepassing en cluster, upgrades, waarin deze twee taken heeft:

* Zorg ervoor dat de regels van het cluster niet worden gecompromitteerd
* Probeer te helpen bij de upgrade soepel verloopt

### <a name="keep-enforcing-the-rules"></a>Houd de regels afdwingen
Het belangrijkste dat u moet weten is dat de regels – de strikte beperkingen, zoals plaatsingsbeperkingen en capaciteiten - nog steeds tijdens upgrades afgedwongen wordt. Plaatsingsbeperkingen ervoor te zorgen dat uw workloads alleen worden uitgevoerd waar ze zijn toegestaan, zelfs tijdens upgrades. Als services zeer beperkte zijn, kunnen dit upgrades langer duren. Wanneer de service of het knooppunt waarop het wordt uitgevoerd is verbroken voor een update is het mogelijk dat er enkele opties voor waar het gaat.

### <a name="smart-replacements"></a>Slimme vervangingen
Wanneer een upgrade wordt gestart, wordt de Resource Manager een momentopname van de huidige indeling van het cluster. Omdat elk domein bijwerken is voltooid, wordt geprobeerd om te retourneren van de services die in dat domein een upgrade uitvoert naar hun oorspronkelijke indeling zijn. Op deze manier zijn er maximaal twee overgangen voor een service tijdens de upgrade. Er is één verplaatsen uit van de betrokken knooppunt en een terug in. Retourneren van het cluster of de service aan hoe voordat de upgrade was zorgt er ook voor de upgrade geen invloed heeft op de indeling van het cluster. 

### <a name="reduced-churn"></a>Verminderde verloop
Een andere wat er tijdens upgrades gebeurt is die de Cluster Resource Manager schakelt netwerktaakverdeling. Zo wordt voorkomen dat balancing voorkomt dat onnodige reacties naar de upgrade zelf, zoals het verplaatsen van services in knooppunten die u voor de upgrade verwijderd. Als de upgrade in kwestie een clusterupgrade van een is, klikt u vervolgens het volledige cluster is niet in evenwicht tijdens de upgrade. Beperking controles blijven actief, alleen verkeer op basis van de proactieve verdeling van metrische gegevens is uitgeschakeld.

### <a name="buffered-capacity--upgrade"></a>Gebufferde capaciteit & Upgrade
Doorgaans wilt u de upgrade te voltooien, zelfs als het cluster wordt beperkt of dicht bij volledige. Beheren van de capaciteit van het cluster is zelfs belangrijker tijdens upgrades dan normaal. Afhankelijk van het aantal upgradedomeinen, moeten tussen 5 en 20 procent van de capaciteit worden gemigreerd als de upgrade via het cluster wordt. Dit werk heeft om te gaan op een locatie. Dit is de locatie waar het concept van [gebufferde capaciteiten](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) is handig. Gebufferde capaciteit in acht wordt genomen tijdens normale werking. Cluster Resource Manager kunnen knooppunten tot hun totale capaciteit (verbruikt de buffer) tijdens upgrades vullen indien nodig.

## <a name="next-steps"></a>Volgende stappen
* Vanaf het begin starten en [een inleiding tot de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
