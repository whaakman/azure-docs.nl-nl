---
title: Informatie over het gebruik van Reliable Services geavanceerde | Microsoft Docs
description: Meer informatie over de geavanceerde informatie over het gebruik van de Service Fabric Reliable Services voor extra flexibiliteit in uw services.
services: Service-Fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: masnider
ms.assetid: f2942871-863d-47c3-b14a-7cdad9a742c7
ms.service: Service-Fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: a87924faaf5c6c43716b06b6d70ab5100c61f097
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Informatie over het gebruik van de Reliable Services programmeermodel geavanceerde
Azure Service Fabric vereenvoudigt schrijven en betrouwbare staatloze en stateful services beheren. Deze handleiding wordt gesproken over geavanceerde gebruik van Reliable Services meer controle en flexibiliteit via uw services krijgen. Voordat u deze handleiding leest, tijd om uzelf bekend met [het programmeermodel Reliable Services](service-fabric-reliable-services-introduction.md).

Stateful en staatloze services hebben twee primaire ingangspunten voor gebruikerscode:

* `RunAsync(C#) / runAsync(Java)`is een algemene toegangspunt voor uw servicecode.
* `CreateServiceReplicaListeners(C#)`en `CreateServiceInstanceListeners(C#) / createServiceInstanceListeners(Java)` is voor het openen van communicatielisteners voor clientaanvragen.

Voor de meeste services zijn deze twee ingangspunten voldoende. In zeldzame gevallen wanneer meer controle over de levenscyclus van een service vereist is, de levenscyclus van aanvullende gebeurtenissen zijn beschikbaar.

## <a name="stateless-service-instance-lifecycle"></a>Levenscyclus van staatloze service-exemplaar
Er is een staatloze service lifecycle zeer eenvoudig. Een stateless service kan alleen worden geopend, gesloten of afgebroken. `RunAsync`in een stateless service wordt uitgevoerd wanneer een service-exemplaar wordt geopend en wanneer een service-exemplaar is gesloten of afgebroken geannuleerd.

Hoewel `RunAsync` moet voldoende in bijna alle gevallen moet de geopend, sluit en gebeurtenissen in een stateless service afbreken zijn ook beschikbaar:

* `Task OnOpenAsync(IStatelessServicePartition, CancellationToken) - C# / CompletableFuture<String> onOpenAsync(CancellationToken) - Java`OnOpenAsync wordt aangeroepen wanneer het staatloze service-exemplaar wordt gebruikt. Initialisatie van uitgebreide servicetaken kunnen op dit moment worden gestart.
* `Task OnCloseAsync(CancellationToken) - C# / CompletableFuture onCloseAsync(CancellationToken) - Java`OnCloseAsync wordt aangeroepen wanneer het staatloze service-exemplaar gaat naar de juiste manier worden afgesloten. Dit kan gebeuren wanneer de code van de service wordt bijgewerkt, het service-exemplaar wordt verplaatst als gevolg van taakverdeling of een tijdelijke fout wordt gedetecteerd. OnCloseAsync kan worden gebruikt voor veilig sluit eventuele resources, eventuele achtergrondverwerking stoppen, hebt opgeslagen status van externe of bestaande verbindingen afgesloten.
* `void OnAbort() - C# / void onAbort() - Java`OnAbort wordt aangeroepen wanneer het staatloze service-exemplaar geforceerd wordt afgesloten. Dit wordt doorgaans als een permanente storing wordt gedetecteerd op het knooppunt, of wanneer het Service Fabric niet betrouwbaar beheren van de levenscyclus van het service-exemplaar als gevolg van interne fouten genoemd.

## <a name="stateful-service-replica-lifecycle"></a>Levenscyclus van de replica stateful service

> [!NOTE]
> Stateful betrouwbare services worden nog niet ondersteund in Java.
>
>

De levenscyclus van een stateful service-replica is veel complexer dan staatloze service-exemplaar. Bovendien afbreken gebeurtenissen openen en sluiten, ondergaat de replica van een stateful service rol tijdens de levensduur. Wanneer een replica stateful service-rol, verandert de `OnChangeRoleAsync` gebeurtenis wordt geactiveerd:

* `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`OnChangeRoleAsync wordt aangeroepen wanneer de replica stateful service rol, bijvoorbeeld primair of secundair wijzigen. Primaire replica's zijn opgegeven schrijven (toegestaan te maken en schrijven naar betrouwbare verzamelingen). Secundaire replica's zijn opgegeven leesstatus (kan alleen worden gelezen van bestaande betrouwbare verzamelingen). De meeste werk in een stateful service wordt uitgevoerd op de primaire replica. Secundaire replica's kunnen uitvoeren voor validatie van alleen-lezen, rapport genereren, gegevensanalyse of andere taken alleen-lezen.

In een stateful service alleen de primaire replica schrijftoegang heeft tot de status en is dus doorgaans wanneer de service echte werk wordt uitgevoerd. De `RunAsync` methode in een stateful service wordt alleen uitgevoerd wanneer de stateful service-replica primaire is. De `RunAsync` methode wanneer een primaire replica rol wordt gewijzigd van primaire, evenals tijdens de gebeurtenissen sluiten en afbreken is geannuleerd.

Met behulp van de `OnChangeRoleAsync` gebeurtenis kunt u taken, afhankelijk van de replicarol ook in reactie op een rol wijzigen.

Een stateful service biedt ook dezelfde vier lifecycle gebeurtenissen als een stateless service, met dezelfde semantiek en gebruiksvoorbeelden:

```csharp
* Task OnOpenAsync(IStatefulServicePartition, CancellationToken)
* Task OnCloseAsync(CancellationToken)
* void OnAbort()
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer geavanceerde onderwerpen die betrekking hebben op de Service Fabric, de volgende artikelen:

* [Configureren van stateful Reliable Services](service-fabric-reliable-services-configuration.md)
* [Service Fabric health Inleiding](service-fabric-health-introduction.md)
* [Systeemstatusrapporten gebruiken voor het oplossen van problemen](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Configureren van Services met Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-configure-services.md)
