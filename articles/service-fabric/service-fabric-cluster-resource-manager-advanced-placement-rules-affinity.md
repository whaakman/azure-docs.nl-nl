---
title: Service Fabric Cluster Resource Manager - affiniteit | Microsoft Docs
description: Overzicht van de affiniteit voor Service Fabric-Services configureren
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 678073e1-d08d-46c4-a811-826e70aba6c4
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 8122f1644da25a9a2ab05291dafc33c77a91147f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Configureren en gebruiken van serviceaffiniteit in Service Fabric
Affiniteit is een besturingselement die hoofdzakelijk wordt geleverd bij de overgang van grotere monolithische toepassingen in de cloud en microservices wereld vereenvoudigen. Dit wordt ook gebruikt als een optimalisatie voor het verbeteren van de prestaties van services, maar in dat geval neveneffecten hebben.

Stel dat u u brengt een grotere app of een die zojuist is niet ontworpen voor microservices rekening naar Service Fabric (of een gedistribueerde omgeving). Dit type overgang is gebruikelijk. U start door het opheffen van de hele app in de omgeving, het verpakken en soepel ervoor dat wordt uitgevoerd. U start splitsen in verschillende kleinere services dat alle met elkaar communiceren.

Uiteindelijk merkt u dat de toepassing enkele problemen ondervindt. De problemen vallen gewoonlijk in een van deze categorieën:

1. Sommige onderdeel X in de app monolithische had een niet-gedocumenteerde afhankelijkheid voor Y-onderdeel en u deze onderdelen alleen omgezet in afzonderlijke services. Aangezien deze services zijn nu wordt uitgevoerd op verschillende knooppunten in het cluster, zijn ze zijn verbroken.
2. Deze onderdelen communiceren (lokale named-pipes | gedeeld geheugen | bestanden op schijf) en ze hoeven te worden kunnen nu schrijven naar een gedeelde lokale bron van de prestaties. Deze harde afhankelijkheid opgehaald later verwijderd, mogelijk.
3. Alles werkt prima, maar het blijkt dat deze twee componenten daadwerkelijk chatty/prestatiegebeurtenissen gevoelige zijn. Wanneer ze deze naar de afzonderlijke services verplaatst algemene toepassingsprestaties tanked of latentie verhoogd. Daardoor voldoet de algehele toepassing niet aan de verwachtingen.

In dergelijke gevallen we niet wilt kwijtraken ons refactoring werk en niet wilt terugkeren naar de monoliet. De laatste voorwaarde wenselijk zelfs als een gewone optimalisatie. Echter, totdat we de onderdelen natuurlijk werken als services kunnen ontwerpen (of de prestatieverwachtingen een andere manier kunnen worden opgelost) gaan we moeten sommige beeld krijgt van plaats.

Hoe pakt u dit aan? U kan ook proberen affiniteit inschakelen.

## <a name="how-to-configure-affinity"></a>Affiniteit configureren
Als u affiniteit instelt, moet u een affiniteitsrelatie tussen twee verschillende services definiëren. U kunt zien affiniteit als 'wijst' een service op een andere en spreken "deze service kan alleen uitgevoerd wanneer dat service wordt uitgevoerd." Soms is affiniteit als een bovenliggende/onderliggende relatie (waar u wijst u de onderliggende op de bovenliggende). Affiniteit zorgt ervoor dat de replica's of exemplaren van een service zijn geplaatst op de knooppunten dezelfde als die van een andere service.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Een onderliggende service alleen deel uitmaken van een relatie met één-relatie. Als u wilt dat het kind tegelijk affiniteit worden gedefinieerd met twee bovenliggende services hebt u een aantal opties:
> - De relaties reverse (hebben parentService1 en wijst u de huidige onderliggende service parentService2), of
> - Een van de ouders aanwijzen als een hub met conventie en alle services, wijst u op of de service hebben. 
>
> Het resulterende plaatsingsgedrag in het cluster moet hetzelfde zijn.
>

## <a name="different-affinity-options"></a>Affiniteit van verschillende opties
Affiniteit wordt voorgesteld via een van de verschillende schema's voor correlatie en heeft twee verschillende modi. De meest voorkomende modus van de affiniteit is zogeheten nonalignedaffinity zijn. In nonalignedaffinity zijn, worden de replica's of exemplaren van de andere services op de dezelfde knooppunten geplaatst. De andere modus is AlignedAffinity. Uitgelijnde affiniteit is nuttig alleen met stateful services. Configureren van twee stateful services om de affiniteit hebben uitgelijnd zorgt ervoor dat de primaire van die services op de knooppunten elkaar zijn geplaatst. Het is ook ervoor zorgt dat elk paar secundaire replica's voor deze services op de dezelfde knooppunten worden geplaatst. Het is ook mogelijk (hoewel minder algemeen) nonalignedaffinity zijn voor stateful services configureren. De andere replica's van de twee stateful services wordt uitgevoerd op de dezelfde knooppunten voor nonalignedaffinity zijn, maar hun primaire kunnen terechtkomen op verschillende knooppunten.

<center>
![Affiniteit tussen modi en de gevolgen ervan][Image1]
</center>

### <a name="best-effort-desired-state"></a>Beste toestand van de moeite gewenst
Een affiniteitsrelatie is zo goed mogelijke poging. Biedt geen dezelfde waarborgen van onderbrengen of betrouwbaarheid waarop in dezelfde uitvoerbaar proces komt. De services in een affiniteitsrelatie zijn fundamenteel andere entiteiten die mislukken en onafhankelijk van elkaar worden verplaatst. Een affiniteitsrelatie kan ook afbreken, hoewel deze einden tijdelijk zijn. Beperkingen van de capaciteit kunnen bijvoorbeeld betekenen dat slechts een deel van de serviceobjecten in de affiniteitsrelatie op een bepaald knooppunt past. In dergelijke gevallen zelfs als er een affiniteitsrelatie aanwezig is, is worden deze niet afgedwongen als gevolg van de andere beperkingen. Als het is mogelijk om dit te doen, wordt de schending automatisch later gecorrigeerd.

### <a name="chains-vs-stars"></a>Ketens versus sterren
De Cluster Resource Manager is vandaag de dag niet kunnen model-ketens van affiniteit relaties. Dit betekent dat een service die een onderliggend element in een affiniteitsrelatie is niet een bovenliggend item in een andere affiniteitsrelatie. Als u dit type relatie modelleren wilt, hebt u effectief als het model als een ster in plaats van een keten. Voor het verplaatsen van een keten een ster, de laagste onderliggende zou bovenliggend element zijn van het eerste onderliggende bovenliggende in plaats daarvan. Afhankelijk van de organisatie van uw services, moet u wellicht dat meerdere keren doen. Als er geen service natuurlijke bovenliggende, is het wellicht een die als een tijdelijke aanduiding fungeert te maken. Afhankelijk van uw vereisten, u kunt ook om te zoeken naar [toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md).

<center>
![Vs ketens. De sterren in de Context van affiniteit van relaties][Image2]
</center>

Iets anders te weten over affiniteit relaties vandaag is dat ze gericht zijn. Dit betekent dat de regel affiniteit alleen wordt afgedwongen dat het onderliggende element met de bovenliggende geplaatst. Niet kan worden gegarandeerd dat de bovenliggende met de onderliggende bevindt. Het is ook belangrijk te weten dat de affiniteitsrelatie kan niet perfecte of direct afgedwongen worden omdat verschillende services met verschillende levenscycli hebt en kunnen mislukken en afzonderlijk verplaatsen. Stel dat de bovenliggende plotseling wordt overgenomen door een ander knooppunt omdat deze is vastgelopen. De Cluster Resource Manager en Failover Manager verwerken van de failover eerst sinds houden de services, consistente, en de prioriteit is beschikbaar. Zodra de failover is voltooid, wordt de affiniteitsrelatie is verbroken, maar de Cluster Resource Manager denkt dat alles is voldoende totdat deze merkt dat het kind niet opgeslagen met de bovenliggende is. Dit soort controles worden regelmatig uitgevoerd. Meer informatie over hoe de beperkingen worden geëvalueerd in de Cluster Resource Manager is beschikbaar in [in dit artikel](service-fabric-cluster-resource-manager-management-integration.md#constraint-types), en [deze](service-fabric-cluster-resource-manager-balancing.md) wordt gesproken meer over het configureren van de waarop deze beperkingen zijn uitgebracht geëvalueerd.   


### <a name="partitioning-support"></a>Ondersteuning voor partitioneren
Het laatste dat opvalt over de affiniteit is die affiniteit relaties worden niet ondersteund wanneer het bovenliggende item is gepartitioneerd. Gepartitioneerde bovenliggende services mogelijk uiteindelijk wordt ondersteund, maar tegenwoordig het is niet toegestaan.

## <a name="next-steps"></a>Volgende stappen
- Voor meer informatie over het configureren van services, [meer informatie over het configureren van Services](service-fabric-cluster-resource-manager-configure-services.md)
- Voor het beperken van services voor een klein aantal machines of samenvoegen van het laden van services, gebruik [toepassingsgroepen](service-fabric-cluster-resource-manager-application-groups.md)

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png