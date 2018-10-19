---
title: Controlelijst voor Azure Service Fabric productie gereedheid | Microsoft Docs
description: Bereid u voor uw Service Fabric-toepassing en cluster productie door de volgende aanbevolen procedures.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/10/2018
ms.author: subramar
ms.openlocfilehash: 7557e2b993a5059df8aea63c7394539acc28c110
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49403521"
---
# <a name="production-readiness-checklist"></a>Controlelijst voor productiegereedheid

Uw toepassing en het cluster gereed is voor productieverkeer? Uitvoeren en testen van uw toepassing en het cluster noodzakelijkerwijs niet dat deze klaar is voor gebruik in productie. Houd uw toepassing en cluster die probleemloos werken door te gaan via de volgende controlelijst. Wij raden deze items om te worden ingeschakeld. U kunt uiteraard alternatieve oplossingen gebruiken voor een bepaalde regelitem (bijvoorbeeld uw eigen frameworks diagnostische gegevens).


## <a name="pre-requisites-for-production"></a>Vereisten voor productie

1. Voor clusters met meer dan 20 kernen of 10 knooppunten, maakt u een toegewezen primaire knooppunttype van systeemservices. Voeg [plaatsingsbeperkingen](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) reserveren van het primaire knooppunttype van systeemservices. 
2. Gebruik een D2v2 of een hogere SKU voor het primaire knooppunttype. Het verdient aanbeveling een SKU kiezen met ten minste 50 GB harde schijfcapaciteit.
2. Productieclusters moeten [beveiligde](service-fabric-cluster-security.md). Zie voor een voorbeeld van het instellen van een beveiligd cluster [clustersjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG). Gebruik algemene namen voor certificaten en Vermijd het gebruik van zelf ondertekende certificaten.
4. Voeg [resource beperkingen met betrekking tot containers en services](service-fabric-resource-governance.md), zodat ze niet meer dan 75% van knooppunt resources verbruiken. 
5. Inzicht in en stel de [duurzaamheidsniveau](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Zilver of hoger duurzaamheidsniveau wordt aanbevolen voor knooppunttypen met stateful werkbelastingen. Het primaire knooppunttype moet een duurzaamheidsniveau ingesteld op Silver of hoger hebben.
6. Inzicht in en kies de [betrouwbaarheidsniveau](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) van het knooppunttype. Betrouwbaarheid van Silver- of hoger wordt aanbevolen.
7. Laden en de schaal test uw werklasten identificeren [capaciteitsvereisten](service-fabric-cluster-capacity.md) voor uw cluster. 
8. Uw services en toepassingen worden bewaakt en toepassingslogboeken worden gegenereerd en opgeslagen, met waarschuwingen. Zie bijvoorbeeld [logboekregistratie toevoegen aan uw Service Fabric-toepassing](service-fabric-how-to-diagnostics-log.md) en [bewaken van containers met Log Analytics](service-fabric-diagnostics-oms-containers.md).
9. Het cluster wordt bewaakt met waarschuwingen (bijvoorbeeld met [Log Analytics](service-fabric-diagnostics-event-analysis-oms.md)). 
10. De onderliggende infrastructuur van virtual machine scale set wordt gecontroleerd met waarschuwingen (bijvoorbeeld met [Log Analytics](service-fabric-diagnostics-oms-agent.md).
11. Het cluster heeft [primaire en secundaire certificaten](service-fabric-cluster-security-update-certs-azure.md) altijd (zodat u niet buitengesloten wordt uit).
12. Onderhouden afzonderlijke clusters voor ontwikkeling, fasering en productie. 
13. [Toepassingsupgrades](service-fabric-application-upgrade.md) en [upgrades van cluster](service-fabric-tutorial-upgrade-cluster.md) in ontwikkeling zijn getest en eerst staging-clusters. 
14. Automatische upgrades in productieclusters uitschakelen en inschakelen voor ontwikkelings- en staging-clusters (rollback indien nodig). 
15. Een Recovery Point Objective (RPO) voor uw service tot stand brengen en instellen van een [noodherstelprocedure](service-fabric-disaster-recovery.md) en testen.
16. Plannen voor [schalen](service-fabric-cluster-scaling.md) uw cluster handmatig of programmatisch.
17. Plannen voor [patches](service-fabric-patch-orchestration-application.md) uw clusterknooppunten. 
18. Stel een CI/CD-pijplijn zodat de meest recente wijzigingen voortdurend worden getest. Bijvoorbeeld, met behulp van [Azure DevOps](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) of [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
19. Test uw ontwikkeling- en staging-clusters onder belasting met de [Fault Analysis Service](service-fabric-testability-overview.md) en gecontroleerde veroorzaken [chaos](service-fabric-controlled-chaos.md). 
20. Plannen voor [schalen](service-fabric-concepts-scalability.md) uw toepassingen. 


Als u de Service Fabric Reliable Services of Reliable Actors-programmeermodel gebruikt, moeten de volgende items worden ingeschakeld:
21. Upgrade uitvoeren van toepassingen tijdens de lokale ontwikkeling om te controleren dat de servicecode van uw naleven van de annulering token in de `RunAsync` methode en aangepaste communicatielisteners te sluiten.
22. Vermijd [veel voorkomende valkuilen](service-fabric-work-with-reliable-collections.md) bij het gebruik van betrouwbare verzamelingen.
23. De prestaties van het geheugen .NET CLR tellers bij het uitvoeren van belastingtesten en controleren op hoge frequenties van garbagecollection of overmatig heap groei bewaken.
24. Offline back-up van onderhouden [Reliable Services en Reliable Actors](service-fabric-reliable-services-backup-restore.md) en testen van het herstelproces. 


## <a name="optional-best-practices"></a>Optionele procedures

De bovenstaande lijst zijn vereisten om te gaan naar de productie, moeten ook de volgende items worden beschouwd:
25. Koppelen aan de [Service Fabric-statusmodel](service-fabric-health-introduction.md) voor het uitbreiden van de ingebouwde evalueren en te rapporteren.
26. Implementeren van een aangepaste watchdog die wordt bewaakt door uw toepassing en de rapporten [laden](service-fabric-cluster-resource-manager-metrics.md) voor [bron-balancing](service-fabric-cluster-resource-manager-balancing.md). 


## <a name="next-steps"></a>Volgende stappen
* [Een Service Fabric Windows-cluster implementeren](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Een Linux Service Fabric-cluster implementeren](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Meer informatie over de [levenscyclus](service-fabric-application-lifecycle.md) van de Service Fabric-toepassing.
