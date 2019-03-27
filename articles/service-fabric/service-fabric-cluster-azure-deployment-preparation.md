---
title: Plan de implementatie van een Azure Service Fabric-cluster | Microsoft Docs
description: Meer informatie over het plannen en voorbereiden voor een productie-implementatie van Service Fabric-cluster in Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/20/2019
ms.author: ryanwi
ms.openlocfilehash: d29f9af4201eee41720fe60cc9b7a21abc7fc63d
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58449981"
---
# <a name="plan-and-prepare-for-a-cluster-deployment"></a>Plannen en voorbereiden voor de implementatie van een cluster

Plannen en voorbereiden voor de implementatie van een productie-cluster is erg belangrijk.  Er zijn veel factoren om te overwegen.  In dit artikel leidt u door de stappen van de voorbereiding van de implementatie van uw cluster.

## <a name="read-the-best-practices-information"></a>Lees de informatie voor aanbevolen procedures
Voor het beheren van Azure Service Fabric-toepassingen en -clusters is, zijn er bewerkingen dat we dat u uitvoeren raden voor het optimaliseren van de betrouwbaarheid van uw productieomgeving.  Lees voor meer informatie, [aanbevolen procedures voor Service Fabric-toepassing en cluster](service-fabric-best-practices-overview.md).

## <a name="select-the-os-for-the-cluster"></a>Selecteer het besturingssysteem voor het cluster
Service Fabric kunt u het maken van Service Fabric-clusters op alle virtuele machines of computers waarop Windows Server of Linux wordt uitgevoerd.  Voordat u uw cluster implementeert, moet u het besturingssysteem kiezen:  Windows- of Linux.  Elk knooppunt (VM) in het cluster het hetzelfde besturingssysteem wordt uitgevoerd, Windows en Linux-VM's kunnen niet worden gecombineerd in hetzelfde cluster.

## <a name="capacity-planning"></a>Capaciteitsplanning
Voor een productie-implementatie is plannen van capaciteit een belangrijke stap. Hier volgen enkele aandachtspunten voor dat proces.

* Het oorspronkelijke aantal knooppunttypen voor uw cluster 
* De eigenschappen van elk knooppunttype (grootte, het aantal exemplaren, primaire, internetgerichte, aantal VM's, enz.)
* De betrouwbaarheid en duurzaamheid van de clusterkenmerken

### <a name="select-the-initial-number-of-node-types"></a>Selecteer het eerste aantal knooppunttypen
Eerst moet u achterhalen wat het cluster die u maakt moet worden gebruikt voor er gebeurt. Welke soorten toepassingen die u van plan bent om te implementeren in dit cluster? Uw toepassing beschikt over meerdere services en moet een van deze openbare of internetservices zijn? Uw services (die gezamenlijk uw toepassing) beschikt over de infrastructuur voor verschillende behoeften, zoals meer RAM-geheugen of hogere CPU-cycli? Een Service Fabric-cluster kan bestaan uit meer dan één knooppunttype: een primaire knooppunttype en een of meer niet-primaire knooppunttypen. Elk knooppunttype is toegewezen aan een virtuele-machineschaalset. Elk knooppunttype kan dan onafhankelijk omhoog of omlaag worden geschaald, verschillende open poorten bevatten en diverse capaciteitsstatistieken hebben. [Eigenschappen van het knooppunt en plaatsingsbeperkingen] [ placementconstraints] kan worden ingesteld voor het beperken van specifieke services voor specifieke typen.  Lees voor meer informatie, [het nummer van uw cluster beginnen moet met knooppunttypen](service-fabric-cluster-capacity.md#the-number-of-node-types-your-cluster-needs-to-start-out-with).

### <a name="select-node-properties-for-each-node-type"></a>Selecteer het knooppunteigenschappen voor elk knooppunttype
Knooppunttypen worden de VM-SKU, het aantal en de eigenschappen van de virtuele machines in de bijbehorende schaalset definiëren.

De minimale grootte van virtuele machines voor elk knooppunttype wordt bepaald door de [duurzaamheidslaag] [ durability] u kiest voor het knooppunttype.

Het minimum aantal VM's voor het primaire knooppunttype wordt bepaald door de [betrouwbaarheidslaag] [ reliability] u kiest.

Zie de minimumvereisten voor [primaire knooppunttypen](service-fabric-cluster-capacity.md#primary-node-type---capacity-guidance), [stateful werkbelastingen op niet-primaire knooppunttypen](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateful-workloads), en [staatloze werkbelastingen op niet-primaire knooppunttypen](service-fabric-cluster-capacity.md#non-primary-node-type---capacity-guidance-for-stateless-workloads). 

Meer dan het minimum aantal knooppunten moet worden gebaseerd op het aantal replica's van de toepassing /-services die u wilt uitvoeren in dit knooppunttype.  [Capaciteitsplanning voor Service Fabric-toepassingen](service-fabric-capacity-planning.md) helpt u een schatting maken van de resources die u wilt uitvoeren van uw toepassingen. U kunt altijd de cluster omhoog of omlaag schalen later om aan te passen voor het wijzigen van de workload van toepassing. 

### <a name="select-the-durability-and-reliability-levels-for-the-cluster"></a>Selecteer het niveau van duurzaamheid en betrouwbaarheid voor het cluster
De duurzaamheidslaag wordt gebruikt om aan te geven aan het systeem de rechten van uw virtuele machines met de onderliggende Azure-infrastructuur. In het primaire knooppunttype kan deze bevoegdheid Service Fabric onderbreken van een virtuele machine op infrastructuur-aanvraag (zoals een VM opnieuw wordt opgestart, een virtuele machine terugzetten van een installatiekopie of een VM-migratie) die invloed hebben op de quorum-vereisten voor het systeem en de stateful services. In de knooppunttypen niet-primaire kan deze bevoegdheid Service Fabric onderbreken van een virtuele machine niveau infrastructuur aanvragen (zoals de VM opnieuw wordt opgestart, virtuele machine terugzetten van een installatiekopie en VM-migratie) die invloed hebben op de quorum-vereisten voor uw stateful services.  Voor de voordelen van de verschillende niveaus en aanbevelingen op welk niveau te gebruiken en wanneer, Zie [de duurzaamheid kenmerken van het cluster][durability].

De betrouwbaarheidslaag wordt gebruikt om het aantal replica's van de systeemservices die u wilt uitvoeren in dit cluster op het primaire knooppunttype instellen. De meer het aantal replica's, de betrouwbaarder zijn de systeemservices in uw cluster.  Voor de voordelen van de verschillende niveaus en aanbevelingen op welk niveau te gebruiken en wanneer, Zie [de betrouwbaarheidskenmerken van de van het cluster][reliability]. 

## <a name="enable-reverse-proxy-andor-dns"></a>Omgekeerde proxy en/of DNS inschakelen
Services met elkaar verbinden in een cluster in het algemeen kunnen rechtstreeks toegang tot de eindpunten van andere services, omdat de knooppunten in een cluster zich op hetzelfde lokale netwerk. Als u wilt maken het gemakkelijker verbinding maken tussen services, biedt Service Fabric aanvullende services: Een [DNS-service](service-fabric-dnsservice.md) en een [reverse proxy-service](service-fabric-reverseproxy.md).  Beide services kunnen worden ingeschakeld wanneer u een cluster implementeert.

Omdat veel services, services, met name in containers, kan de naam van een bestaande URL hebt, kunt deze oplossen met behulp van de standaard DNS-protocol (in plaats van het protocol Naming-Service) is kunnen handig zijn, met name in toepassingsscenario's 'lift and shift'. Dit is precies wat de DNS-service heeft. Hiermee kunt u het DNS-namen worden toegewezen aan een servicenaam en kan daarom oplossen eindpunt IP-adressen.

De omgekeerde proxy adressen services in het cluster die beschikbaar maken van HTTP-eindpunten (inclusief HTTPS). De omgekeerde proxy vereenvoudigt het aanroepen van andere services door te geven van een specifieke URI-indeling.  De omgekeerde proxy ook oplossen door verwerkt, verbinding en probeer de stappen die nodig zijn voor een service om te communiceren met een andere.

## <a name="prepare-for-disaster-recovery"></a>Voorbereiden op herstel na noodgeval
Een belangrijk onderdeel van het leveren van hoge beschikbaarheid is ervoor te zorgen dat services verschillende typen fouten meer storingen kunnen doorstaan. Dit is vooral belangrijk voor fouten die niet-geplande zijn en buiten het besturingselement. [Voorbereiden voor herstel na noodgevallen](service-fabric-disaster-recovery.md) beschrijft enkele algemene foutmodi die rampen worden kunnen als niet gemodelleerd en goed beheerd. Hierin worden ook oplossingen en acties moet uitvoeren als er een ramp toch opgetreden.

## <a name="production-readiness-checklist"></a>Controlelijst voor productiegereedheid
Uw toepassing en het cluster gereed is voor productieverkeer? Voordat u uw cluster implementeert naar productie, doorloopt u de [productie gereedheid controlelijst](service-fabric-production-readiness-checklist.md). Houd uw toepassing en cluster soepel door het uitvoeren van de items in deze controlelijst. Wij raden deze items om te worden ingeschakeld voordat u doorgaat naar de productie.

## <a name="next-steps"></a>Volgende stappen
* [Maken van een Service Fabric-cluster waarop Windows wordt uitgevoerd](service-fabric-best-practices-overview.md)
* [Maken van een Service Fabric-cluster waarop Linux wordt uitgevoerd](service-fabric-tutorial-create-vnet-and-linux-cluster.md)

[placementconstraints]: service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints
[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster