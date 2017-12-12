---
title: Azure Service Fabric-toepassingsmodel | Microsoft Docs
description: Het model en beschrijven toepassingen en services in Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: ryanwi
ms.openlocfilehash: d5f6fbb9d9c0bc0d9762f8d6b4b4eb3b02d29adc
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="model-an-application-in-service-fabric"></a>Model van een toepassing in Service Fabric
Dit artikel bevat een overzicht van het model van Azure Service Fabric-toepassing en het definiëren van een toepassing en service via de manifest-bestanden.

## <a name="understand-the-application-model"></a>Inzicht in het toepassingsmodel
Een toepassing is een verzameling van de services die een bepaalde functie of functies uitvoeren. Een service kunt vervult een functie die volledig en zelfstandig starten en onafhankelijk van andere services worden uitgevoerd.  Een service bestaat uit code, configuratie en gegevens. Voor elke service code bestaat uit de uitvoerbare binaire bestanden, configuratie bestaat uit de service-instellingen die kunnen worden geladen tijdens runtime en gegevens bestaan uit willekeurige statische gegevens kunnen worden gebruikt door de service. Elk onderdeel in dit toepassingsmodel hiërarchische kan worden samengesteld en bijgewerkte onafhankelijk.

![Het Service Fabric-toepassing-model][appmodel-diagram]

Een toepassingstype een categorisatie van een toepassing is en bestaat uit een bundel van servicetypen. Een service is een categorisatie van een service. De categorisatie kan hebben verschillende instellingen en configuraties, maar de kernfunctionaliteit hetzelfde is gebleven. De exemplaren van een service zijn de andere service configuration variaties van hetzelfde servicetype.  

Klassen (of 'typen') van toepassingen en services via XML-bestanden (Toepassingsmanifesten en service manifesten) worden beschreven.  De manifesten toepassingen en services beschreven en worden de sjablonen op basis waarvan toepassingen uit het archief van de installatiekopie van het cluster kunnen worden gemaakt.  Manifesten worden behandeld in detail in [toepassing en service manifesten](service-fabric-application-and-service-manifests.md). De schemadefinitie voor het bestand ServiceManifest.xml en ApplicationManifest.xml is geïnstalleerd met de Service Fabric SDK en hulpprogramma's voor *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*. Het XML-schema wordt beschreven in [ServiceFabricServiceModel.xsd schemadocumentatie](service-fabric-service-model-schema.md).

De code voor exemplaren van een andere toepassing wordt uitgevoerd als afzonderlijke processen, zelfs wanneer deze wordt gehost door de dezelfde Service Fabric-knooppunt. Bovendien de levenscyclus van elk toepassingsexemplaar kan worden beheerd (bijvoorbeeld een bijgewerkt) onafhankelijk. Het volgende diagram toont hoe toepassingstypen servicetype die op zijn beurt bestaan uit code, configuratie en gegevenspakketten zijn samengesteld. Om te vereenvoudigen in het diagram, alleen de code/config/gegevens pakketten voor `ServiceType4` worden weergegeven, hoewel elk servicetype u enkele vindt zou of alle pakkettypen.

![Service Fabric-toepassing waardetypen en servicetypen][cluster-imagestore-apptypes]

Er kan een of meer exemplaren van een servicetype actief zijn in het cluster. Bijvoorbeeld, bereiken stateful service-exemplaren of replica's hoge betrouwbaarheid status repliceren tussen replica's zich op verschillende knooppunten in het cluster. Replicatie wordt in wezen biedt redundantie voor de service moet beschikbaar, zelfs als een knooppunt in een cluster is mislukt. Een [service gepartitioneerd](service-fabric-concepts-partitioning.md) verdere verdeelt de status (en toegangspatronen op de bijbehorende status) tussen knooppunten in het cluster.

Het volgende diagram toont de relatie tussen toepassingen en service-exemplaren, partities en replica's.

![Partities en replica's binnen een service][cluster-application-instances]

> [!TIP]
> U kunt de indeling van toepassingen weergeven in een cluster met behulp van het Service Fabric Explorer hulpprogramma beschikbaar op http://&lt;yourclusteraddress&gt;: 19080/Explorer. Zie voor meer informatie [uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
- Meer informatie over service [status](service-fabric-concepts-state.md), [partitioneren](service-fabric-concepts-partitioning.md), en [beschikbaarheid](service-fabric-availability-services.md).
- Meer informatie over hoe toepassingen en services worden gedefinieerd in [toepassing en service manifesten](service-fabric-application-and-service-manifests.md).
- [Toepassing hosten modellen](service-fabric-hosting-model.md) beschrijven de relatie tussen de replica's (of exemplaren) van een geïmplementeerde service en de ServiceHost proces.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png


