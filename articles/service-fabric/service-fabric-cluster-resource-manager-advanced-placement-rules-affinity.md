---
title: Service Fabric Cluster Resource Manager - affiniteit | Microsoft Docs
description: Overzicht van het configureren van affiniteit voor Service Fabric-Services
services: service-fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: ''
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 29377492b90f366227ca7bedf85890b7734ea25f
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663414"
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Configureren en gebruiken van serviceaffiniteit in Service Fabric
Affiniteit is een besturingselement die hoofdzakelijk is bedoeld om u te helpen bij de overgang van grotere monolithische toepassingen vereenvoudigen in de cloud en microservices wereld. Het wordt ook gebruikt als een optimalisatie voor het verbeteren van de prestaties van services, maar in dat geval kan neveneffecten.

Stel dat u een grotere app of een die zojuist is niet ontworpen met microservices Denk eraan dat u Service fabric (of een gedistribueerde omgeving) brengen. Dit type overgang is gebruikelijk. U start door het opheffen van de hele app in de omgeving, het verpakken en zorgen dat deze goed werkt. Vervolgens start u splitsen in verschillende kleinere services dat alles met elkaar communiceren.

Uiteindelijk merkt u misschien dat de toepassing enkele problemen ondervindt. De problemen kunnen meestal worden onderverdeeld in een van deze categorieën:

1. Sommige onderdeel X in de monolithische app afhankelijk van een niet-gedocumenteerde op onderdeel Y had, en u alleen die onderdelen in afzonderlijke services hebt ingeschakeld. Omdat deze services zijn nu wordt uitgevoerd op verschillende knooppunten in het cluster, zijn in feite verbroken.
2. Deze onderdelen met elkaar communiceren (lokale benoemde pipes | gedeeld geheugen | bestanden op schijf) en ze hoeven te worden kunnen nu wegschrijven naar een gedeelde lokale bron voor betere prestaties. Deze vaste afhankelijkheid wordt verwijderd later mogelijk.
3. Alles is prima, maar het blijkt dat deze twee onderdelen daadwerkelijk intensieve/prestaties gevoelige zijn. Wanneer ze deze in afzonderlijke services verplaatst algehele prestaties van toepassingen tanked of latentie verhoogd. Als gevolg hiervan de algemene toepassing niet voldoen aan de verwachtingen.

In dergelijke gevallen we niet wilt kwijtraken ons refactoring werk en gaat u terug naar de monoliet niet wilt. De laatste voorwaarde mogelijk ook wenselijk zijn als een gewone optimalisatie. Echter, totdat we de onderdelen om te werken op een natuurlijke manier als services te (of we kunnen de prestatieverwachtingen van een andere manier oplossen) gaan we zekere zin locatie nodig.

Hoe pakt u dit aan? U kan ook proberen affiniteit inschakelen.

## <a name="how-to-configure-affinity"></a>Affiniteit configureren
Als u affiniteit instelt, definieert u een affiniteitsrelatie tussen twee verschillende services. U kunt zien affiniteit als 'wijst' een service op een andere en de uitspraak "deze service kan alleen worden uitgevoerd waar of service wordt uitgevoerd." Soms verwijzen we naar affiniteit als een bovenliggende/onderliggende relatie (waar u wijst het onderliggende object op de bovenliggende). Affiniteit zorgt ervoor dat de replica's of exemplaren van een service worden geplaatst op de knooppunten dezelfde als die van een andere service.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Een onderliggende service alleen deel uitmaken van een relatie met één-relatie. Als u wilt het kind in één keer affiniteit worden gedefinieerd met twee bovenliggende services hebt u een aantal opties:
> - De relaties omgekeerde (hebt parentService1 en parentService2 verwijzen naar de huidige onderliggende service), of
> - Een van de ouders aanwijzen als een hub volgens de conventies en alle services die verwijzen naar die service hebben. 
>
> Het resulterende plaatsingsgedrag in het cluster moet gelijk zijn.
>

## <a name="different-affinity-options"></a>Relatie met verschillende opties
Affiniteit wordt weergegeven via een van verschillende schema's voor correlatie en heeft twee verschillende modi. De meest voorkomende modus van affiniteit is zogeheten NonAlignedAffinity. De replica's of exemplaren van de verschillende services in NonAlignedAffinity, worden geplaatst op de dezelfde knooppunten. De andere modus is AlignedAffinity. Uitgelijnde affiniteit is nuttig alleen met stateful services. Configureren van twee stateful services affiniteit zijn uitgelijnd, zorgt u ervoor dat de primaire van die services op de dezelfde knooppunten elkaar zijn geplaatst. Het is ook ervoor zorgt dat elk paar van de secundaire replica's voor de services die op de dezelfde knooppunten worden geplaatst. Het is ook mogelijk (hoewel minder algemeen) voor het configureren van NonAlignedAffinity voor stateful services. De andere replica's van de twee stateful services wordt uitgevoerd op de dezelfde knooppunten voor NonAlignedAffinity, maar hun primaire kunnen terechtkomen op verschillende knooppunten.

<center>

![Affiniteit modi en de gevolgen ervan][Image1]
</center>

### <a name="best-effort-desired-state"></a>Aanbevolen inspanning gewenste status
Een affiniteitsrelatie is best-effort. Biedt niet de dezelfde garanties van CO-locaties of betrouwbaarheid die uitgevoerd in dezelfde uitvoerbaar proces komt. De services in een affiniteitsrelatie zijn ingrijpend verschillende entiteiten die kunnen mislukken en onafhankelijk van elkaar worden verplaatst. Een affiniteitsrelatie kan ook afbreken, hoewel deze pauzes tijdelijk zijn. Beperkingen van de capaciteit kunnen bijvoorbeeld betekenen dat slechts een deel van de serviceobjecten in de affiniteitsrelatie op een bepaald knooppunt passen. Worden in dergelijke gevallen zelfs als er een affiniteitsrelatie aanwezig is, deze kan niet afgedwongen door de andere beperkingen. Als het is mogelijk om dit te doen, wordt later de schending automatisch gecorrigeerd.

### <a name="chains-vs-stars"></a>Ketens versus sterren
Vandaag nog met Cluster Resource Manager kan geen tot model ketens van affiniteit relaties. Dit betekent dat een service die een onderliggend item in een affiniteitsrelatie mag niet een bovenliggend item in een andere affiniteitsrelatie. Als u dit type relatie model wilt, hebt u effectief als het model als een ster, in plaats van een keten. Voor het verplaatsen van een keten van een ster, de laagste onderliggende zou worden bovenliggend element van het eerste onderliggende bovenliggende in plaats daarvan. Mogelijk moet u doen dit meerdere malen, afhankelijk van de rangschikking van uw services. Als er geen natuurlijke bovenliggende service, is het wellicht een die als een tijdelijke aanduiding fungeert te maken. Afhankelijk van uw vereisten, u kunt ook kijken [toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md).

<center>

![Ketens vs. Sterren in de Context van affiniteit relaties][Image2]
</center>

Iets anders te weten over de affiniteit relaties vandaag nog is dat ze standaard richting zijn. Dit betekent dat de regel voor de affiniteit alleen afgedwongen dat de onderliggende met de bovenliggende geplaatst. Niet kan worden gegarandeerd dat de bovenliggende met het onderliggende object bevindt. Dus als er een schending van de affiniteit en om op te lossen van de schending voor een of andere reden is het niet mogelijk de onderliggende om naar te verplaatsen van het bovenliggende knooppunt, klikt u vervolgens--, zelfs als het bovenliggende item verplaatst naar de kind-knooppunt zou hebt gecorrigeerd als de schending--wordt het bovenliggende object niet worden verplaatst naar th van e onderliggend knooppunt. Instellen van de configuratie [MoveParentToFixAffinityViolation](service-fabric-cluster-fabric-settings.md) aan waar de richting wilt verwijderen. Het is ook belangrijk te weten dat de affiniteitsrelatie kan niet de perfecte of direct afgedwongen worden omdat verschillende services met verschillende Lifecycle-beleid hebt en kunnen mislukken en onafhankelijk te verplaatsen. Stel dat de bovenliggende plotseling failover naar een ander knooppunt omdat deze is vastgelopen. Het Cluster Resource Manager en Failover Manager failover af te handelen de eerst sinds de services, consistent houden en de prioriteit is beschikbaar. Nadat de failover is voltooid, wordt de affiniteitsrelatie is verbroken, maar met Cluster Resource Manager als dat alles goed wordt beschouwd totdat deze meldingen die het onderliggende item is niet gevonden met de bovenliggende. Dit soort controles worden regelmatig uitgevoerd. Meer informatie over de manier waarop met Cluster Resource Manager beperkingen evalueert is beschikbaar in [in dit artikel](service-fabric-cluster-resource-manager-management-integration.md#constraint-types), en [deze](service-fabric-cluster-resource-manager-balancing.md) praat meer over het configureren van de frequentie waarop deze beperkingen zijn geëvalueerd.   


### <a name="partitioning-support"></a>Ondersteuning voor partitioneren
Het laatste merken over de affiniteit is die affiniteit relaties worden niet ondersteund wanneer het bovenliggende item is gepartitioneerd. Gepartitioneerde bovenliggende services kunnen uiteindelijk worden ondersteund, maar vandaag nog het is niet toegestaan.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Services voor een klein aantal machines beperken of samenvoegen van de belasting van services, gebruiken [toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png