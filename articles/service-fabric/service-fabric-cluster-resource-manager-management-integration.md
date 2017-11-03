---
title: Service Fabric Cluster Resource Manager - Management-integratie | Microsoft Docs
description: Een overzicht van de integratiepunten tussen de Cluster Resource Manager en Service Fabric-beheer.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 956cd0b8-b6e3-4436-a224-8766320e8cd7
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 70c0cc37a1d362c937ab86bd630c5ab051e63870
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="cluster-resource-manager-integration-with-service-fabric-cluster-management"></a>Cluster resource manager-integratie met Service Fabric-Clusterbeheer
De Service Fabric Cluster Resource Manager biedt geen upgrades station in Service Fabric, maar het is betrokken. De eerste manier die de Cluster Resource Manager met management is door de gewenste status van het cluster en de services in het bijhouden. De Cluster Resource Manager verzendt statusrapporten wanneer deze niet kan het cluster in de gewenste configuratie geplaatst. Bijvoorbeeld, als er onvoldoende capaciteit verzendt de Cluster Resource Manager health-waarschuwingen en fouten die wijzen op het probleem. Een andere stukje integratie heeft te maken met de werking van upgrades. De Cluster Resource Manager verandert het gedrag enigszins tijdens upgrades.  

## <a name="health-integration"></a>Health-integratie
De Cluster Resource Manager houdt voortdurend de regels die u hebt gedefinieerd voor de plaatsing van uw services. Het houdt ook de resterende capaciteit voor elke metriek op de knooppunten in het cluster en in het cluster als geheel. Als deze kan niet voldoen aan deze regels of als er onvoldoende capaciteit, status, waarschuwingen en fouten verzonden. Als bijvoorbeeld een knooppunt ligt boven de capaciteit en de Cluster Resource Manager zal proberen de situatie oplossen door het verplaatsen van services. Als deze niet de situatie oplossen zendt het een waarschuwing die aangeeft welk knooppunt is overbelast en voor welke metrische gegevens.

Een ander voorbeeld van de Resource Manager-waarschuwingen is schendingen van plaatsingsbeperkingen. Bijvoorbeeld, als u een beperking voor de plaatsing hebt gedefinieerd (zoals `“NodeColor == Blue”`) en de Resource Manager detecteert een schending van deze beperking, het verzendt een waarschuwing. Dit geldt voor aangepaste beperkingen en de standaardbeperkingen (zoals de beperkingen Foutdomein en het upgraden van domein).

Hier volgt een voorbeeld van een dergelijke statusrapport. In dit geval is het statusrapport voor een van de partities van de service. De health-bericht geeft aan dat de replica's van de betreffende partitie tijdelijk worden verpakt in te weinig domeinen upgraden.

```posh
PS C:\Users\User > Get-WindowsFabricPartitionHealth -PartitionId '00000000-0000-0000-0000-000000000001'


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

Dit is wat dit bericht health zorgt ervoor dat ons is:

1. De replica's zichzelf zijn in orde: elk AggregatedHealthState heeft: Ok
2. De beperking van het upgraden van domein distributie is momenteel wordt geschonden. Dit betekent dat een bepaald domein voor het upgraden meer replica's van deze partitie dan nodig is.
3. Welk knooppunt bevat de replica waardoor de schending. In dit geval is het knooppunt met de naam 'Node.8'
4. Hiermee wordt aangegeven of een upgrade die momenteel gebeurt er voor deze partitie ('momenteel upgraden--false")
5. Het distributiebeleid voor deze service: 'Distributie beleid--verpakking'. Hiermee wordt bepaald door de `RequireDomainDistribution` [plaatsing beleid](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). 'Verpakking' geeft aan dat in dit geval DomainDistribution _niet_ vereist, zodat de juiste plaatsing beleid is niet opgegeven voor deze service. 
6. Wanneer het rapport is er gebeurd - 8-10-2015 7:13:02 PM

Informatie over deze bevoegdheden waarschuwingen die worden geactiveerd in productie te laten u weten iets is een fout opgetreden en wordt ook gebruikt om te detecteren en onjuiste upgrades is gestopt. In dit geval willen we zien als we kunt achterhalen waarom de Resource Manager moest de replica's in het domein Upgrade pack. Meestal verpakking is tijdelijke omdat de knooppunten in de andere Upgrade domeinen, bijvoorbeeld zijn.

Stel dat de Cluster Resource Manager wordt geprobeerd bepaalde services plaatsen, maar er oplossingen die werken niet. Wanneer services kunnen niet worden geplaatst, is het meestal voor een van de volgende redenen:

1. Sommige tijdelijke toestand heeft gemaakt, het onmogelijk om deze service-exemplaar of de replica correct plaatsen
2. Vereisten voor de plaatsing van de service zijn ongeldig.

In dergelijke gevallen kunnen statusrapporten van de Cluster Resource Manager u bepalen waarom de service kan niet worden geplaatst. We noemen dit proces de beperking eliminatie volgorde. Tijdens het wordt het systeem begeleid bij de geconfigureerde beperkingen die invloed hebben op de service en registreert wat het elimineren. Deze manier wanneer de services niet kunnen worden geplaatst, kunt u zien welke knooppunten zijn geëlimineerd en waarom.

## <a name="constraint-types"></a>Beperkingstypen
We hebben over elk van de andere beperkingen in deze health-rapporten. Hier ziet u health berichten die zijn gerelateerd aan deze beperkingen als replica's kunnen niet worden geplaatst.

* **ReplicaExclusionStatic** en **ReplicaExclusionDynamic**: deze beperkingen geeft aan dat er een oplossing is afgewezen omdat twee serviceobjecten van dezelfde partitie zou hebben op hetzelfde knooppunt worden geplaatst. Dit is niet toegestaan omdat vervolgens fout van dat knooppunt overmatig gevolgen zou hebben voor deze partitie. ReplicaExclusionStatic en ReplicaExclusionDynamic zijn bijna dezelfde regel en de verschillen niet echt uitmaken. Als u een beperking eliminatie reeks die de ReplicaExclusionStatic of ReplicaExclusionDynamic beperking bevat ziet, wordt de Cluster Resource Manager denkt dat er niet voldoende knooppunten. Hiervoor moet de resterende oplossingen voor het gebruik van deze ongeldige plaatsingen die zijn niet toegestaan. De andere beperkingen in de reeks wordt meestal Vertel ons waarom knooppunten in de eerste plaats worden wordt geëlimineerd.
* **PlacementConstraint**: als u dit bericht ziet, betekent dit dat we sommige knooppunten geëlimineerd omdat ze niet overeenkomen met de plaatsingsbeperkingen voor de service. We traceren uit de momenteel geconfigureerde plaatsingsbeperkingen als onderdeel van dit bericht. Dit is normaal als er een plaatsing-beperking gedefinieerd. Echter, als de beperking van de plaatsing van onjuist wordt veroorzaakt door te veel knooppunten om te worden geëlimineerd dit is hoe merkt u.
* **NodeCapacity**: deze beperking betekent dat de Cluster Resource Manager kan geen, de replica's op de aangegeven knooppunten plaatsen omdat die ze via capaciteit plaatst.
* **Affiniteit**: deze beperking geeft aan dat we de replica kan niet plaats op de betrokken knooppunten omdat hierdoor een schending van de affiniteit-beperking. Meer informatie over de affiniteit is in [in dit artikel](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
* **FaultDomain** en **UpgradeDomain**: deze beperking elimineert knooppunten if plaatsen van de replica op de knooppunten aangegeven dat verpakken in een bepaalde fout of een upgradedomein. Enkele voorbeelden voor het bespreken van deze beperking worden weergegeven in het onderwerp op [probleem- en upgrade domain-beperkingen en het resulterende gedrag](service-fabric-cluster-resource-manager-cluster-description.md)
* **PreferredLocation**: er mag niet deze beperking verwijderen van knooppunten van de oplossing omdat deze wordt uitgevoerd als een optimalisatie standaard normaal. De voorkeur locatiebeperking is ook aanwezig zijn tijdens upgrades. Tijdens de upgrade wordt gebruikt voor het verplaatsen van services terug naar waar ze zijn wanneer de upgrade wordt gestart.

## <a name="blocklisting-nodes"></a>Blocklisting knooppunten
Een andere health-bericht is de Cluster Resource Manager-rapporten wanneer knooppunten zijn blocklisted. U kunt blocklisting beschouwen als een tijdelijke beperking die automatisch voor u is toegepast. Knooppunten krijgen blocklisted wanneer ze herhaalde fouten optreden tijdens het starten van exemplaren van dat servicetype. Knooppunten zijn blocklisted op basis van de per-service-type. Een knooppunt is mogelijk blocklisted voor één service-type maar niet nog een. 

Ziet u blocklisting kick in vaak tijdens de ontwikkeling: sommige bug zorgt ervoor dat uw ServiceHost vastloopt bij het opstarten. Service Fabric probeert te maken van de ServiceHost een paar keer en de fout blijft optreden. Na enkele pogingen het knooppunt blocklisted opgehaald en de Cluster Resource Manager zal proberen de service elders te maken. Als deze fout op meerdere knooppunten voordoet blijft, is het mogelijk dat alle knooppunten in de cluster-end van geldige geblokkeerd. Blocklisting kan ook zo veel knooppunten dat niet voldoende is de service om te voldoen aan de gewenste schaal kunt starten verwijderen. Doorgaans ziet u extra fouten of waarschuwingen van de Cluster Resource Manager die aangeeft dat de service onder de gewenste replica of aantal exemplaren, evenals de gezondheid van berichten die aangeven wat de fout is dat leidt tot het blocklisting in de eerste plaats.

Blocklisting is geen permanente voorwaarde. Het knooppunt is verwijderd uit de blocklist na een paar minuten en Service Fabric kunnen de services op dat knooppunt opnieuw activeren. Services kunnen blijven mislukken, dan is het knooppunt opnieuw blocklisted voor dat servicetype. 

### <a name="constraint-priorities"></a>Beperking prioriteiten

> [!WARNING]
> Beperking prioriteiten wijzigen wordt niet aangeraden en mogelijk belangrijke nadelige gevolgen hebben voor uw cluster. De onderstaande informatie is opgegeven voor de verwijzing van de prioriteiten van de beperking standaard en hun gedrag. 
>

Met alle van deze beperkingen u mogelijk hebt is denkt 'Hey – ik denk dat dat veroorzaakt domein beperkingen het belangrijkste in mijn systeem is zijn. Om ervoor te zorgen dat de beperking van fouttolerantie domein niet wordt geschonden, ben ik bereid andere beperkingen schenden."

Beperkingen kunnen worden geconfigureerd met verschillende prioriteitsniveaus. Dit zijn:

   - 'harde' (0)
   - 'soft' (1)
   - 'optimalisatie' (2)
   - "off" (-1). 
   
De meeste van de beperkingen zijn standaard geconfigureerd als harde beperkingen.

Als u de prioriteit van beperkingen is ongewoon. Er zijn keren waar beperking prioriteiten nodig zijn voor het wijzigen, meestal voor een andere fout of probleem dat invloed had op de omgeving te omzeilen. In het algemeen de flexibiliteit van de infrastructuur van de prioriteit beperking uitstekend heeft gewerkt, maar vaak is niet nodig. De meeste gevallen alles bevindt zich op hun Standaardprioriteiten. 

Het niveau van de prioriteit niet inhouden dat een bepaalde beperking _wordt_ worden geschonden, evenmin dat zal altijd worden voldaan. Beperking prioriteiten definiëren de volgorde waarin u beperkingen gelden. Prioriteiten definiëren de voor-en nadelen wanneer is het onmogelijk om te voldoen aan alle beperkingen. De beperkingen kunnen doorgaans worden voldaan tenzij er iets anders aan de hand in de omgeving. Enkele voorbeelden van scenario's die tot schendingen van plaatsingsbeperkingen leiden zijn strijdige beperkingen of grote aantallen gelijktijdige fouten.

U kunt de prioriteiten beperking wijzigen in geavanceerde situaties. Stel dat u wilt ervoor te zorgen dat affiniteit altijd zouden worden geschonden, wanneer dat nodig is voor het oplossen van problemen met knooppunt-capaciteit. Om dit te bereiken, kan u de prioriteit van de beperking affiniteit 'soft' (1) instellen en laat de capaciteit beperking is ingesteld op 'harde' (0).

De standaardwaarden van de prioriteit voor de andere beperkingen zijn opgegeven in de volgende configuratie:

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

gehoste clusters via ClusterConfig.json voor zelfstandige implementaties of Template.json voor Azure:

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

## <a name="fault-domain-and-upgrade-domain-constraints"></a>Beperkingen voor fouttolerantie domein- en domein
De Cluster Resource Manager wil services verdeeld tussen domeinen met fouten en upgrade te behouden. Deze modellen dit als een beperking in de Cluster Resource Manager-engine. Voor meer informatie over hoe ze worden gebruikt en hun specifieke gedrag, Raadpleeg het artikel op [clusterconfiguratie](service-fabric-cluster-resource-manager-cluster-description.md#fault-and-upgrade-domain-constraints-and-resulting-behavior).

De Cluster Resource Manager moet mogelijk een aantal replica's in een upgradedomein pack om door te gaan met upgrades, fouten of andere schendingen van plaatsingsbeperkingen. Normaal gesproken verpakken in domeinen met fouten of upgrade gebeurt alleen wanneer er meerdere fouten of andere verloop in het systeem zo wordt voorkomen dat correcte plaatsing. Als u wenst te verpakken zelfs tijdens deze situaties voorkomen, kunt u gebruikmaken van de `RequireDomainDistribution` [plaatsing beleid](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md#requiring-replica-distribution-and-disallowing-packing). Houd er rekening mee dat dit mogelijk servicebeschikbaarheid en betrouwbaarheid als een neveneffect van invloed zijn op, dus zorgvuldig rekening te houden.

De omgeving correct is geconfigureerd, alle beperkingen zijn volledig in acht genomen, zelfs tijdens upgrades. Het belangrijkste is dat dat de Cluster Resource Manager is Let op de beperkingen. Als er een schending gedetecteerd wordt onmiddellijk gerapporteerd en probeert om het probleem te verhelpen.

## <a name="the-preferred-location-constraint"></a>De voorkeur locatiebeperking
De beperking PreferredLocation is enigszins verschillen, als er twee verschillende manieren worden gebruikt. Er is een gebruik van deze beperking tijdens toepassingsupgrades. De Cluster Resource Manager beheert automatisch deze beperking tijdens upgrades. Wordt gebruikt om ervoor te zorgen dat wanneer updates voltooid zijn of replica's naar de oorspronkelijke locatie retourneren. Het gebruik van de PreferredLocation-beperking is voor de [ `PreferredPrimaryDomain` plaatsing beleid](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md). Deze optimalisaties zijn en daarom de PreferredLocation-beperking is de enige beperking standaard ingesteld op 'Optimalisatie'.

## <a name="upgrades"></a>Upgrades
De Cluster Resource Manager helpt ook tijdens de toepassing en cluster, upgrades, waarin deze twee taken bevat:

* Zorg ervoor dat de regels van het cluster niet worden gecompromitteerd
* Probeer te helpen bij de upgrade probleemloos

### <a name="keep-enforcing-the-rules"></a>Houd de regels afdwingen
Het belangrijkste wat u moet rekening houden met is dat de regels – de strikte beperkingen zoals plaatsingsbeperkingen en capaciteit - nog steeds worden afgedwongen tijdens upgrades. Plaatsingsbeperkingen Zorg ervoor dat de werkbelasting van uw alleen uitgevoerd waar ze zijn toegestaan, zelfs tijdens upgrades. Wanneer services maximaal worden beperkt, kunnen upgrades langer duren. Wanneer de service of het knooppunt dat wordt uitgevoerd wordt verbroken voor een update worden enkele opties voor waar gaan.

### <a name="smart-replacements"></a>Smart vervangingen
Wanneer een upgrade wordt gestart, maakt de Resource Manager een momentopname van de huidige indeling van het cluster. Omdat elk domein Upgrade is voltooid, wordt geprobeerd om te retourneren van de services die in dat domein upgraden naar hun oorspronkelijke positie zijn. Op deze manier zijn er maximaal twee overgangen voor een service tijdens de upgrade. Er is een verplaatsing uit van de betrokken knooppunt en een teruggaan in. Het cluster of de service wordt teruggezonden naar de manier waarop het was vóór de upgrade zorgt er ook voor dat de upgrade heeft geen invloed op de indeling van het cluster. 

### <a name="reduced-churn"></a>Verminderde verloop
Een andere wat er tijdens upgrades gebeurt is die de Cluster Resource Manager schakelt netwerktaakverdeling. Zo wordt voorkomen dat netwerktaakverdeling voorkomt u dat onnodige reacties naar de upgrade zelf, zoals het verplaatsen van services in knooppunten die u voor de upgrade verwijderd. Als de desbetreffende upgrade de upgrade van een Cluster is, klikt u vervolgens het volledige cluster is niet in balans tijdens de upgrade. Beperking controles blijft actief, alleen verkeer op basis van de proactieve verdeling van metrische gegevens is uitgeschakeld.

### <a name="buffered-capacity--upgrade"></a>Buffer capaciteit & Upgrade
Doorgaans wilt u de upgrade is voltooid, zelfs als het cluster wordt begrensd of bijna vol. Het beheren van de capaciteit van het cluster is zelfs belangrijker tijdens upgrades dan normaal. Afhankelijk van het aantal upgradedomeinen moet tussen 5 en 20 procent van de capaciteit worden gemigreerd als de upgrade wordt de via het cluster. Die werken heeft om te gaan op een locatie. Dit is wanneer het begrip [gebufferd capaciteiten](service-fabric-cluster-resource-manager-cluster-description.md#buffered-capacity) is nuttig. De capaciteit van de buffer is tijdens de normale werking in acht genomen. De Cluster Resource Manager mogelijk knooppunten tot de totale capaciteit (de buffer verbruikt) tijdens upgrades vult, indien nodig.

## <a name="next-steps"></a>Volgende stappen
* Vanaf het begin starten en [Maak kennis met de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
