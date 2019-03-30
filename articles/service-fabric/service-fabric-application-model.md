---
title: Azure Service Fabric-toepassingsmodel | Microsoft Docs
description: Het model en toepassingen en services in Service Fabric te beschrijven.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: atsenthi
ms.openlocfilehash: 750970233cbcb14d901dbb5fa94f649f6ff8ae6c
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666270"
---
# <a name="model-an-application-in-service-fabric"></a>Een toepassing modelleren in Service Fabric
Dit artikel bevat een overzicht van de Azure Service Fabric-toepassingsmodel en over het definiëren van een toepassing en service manifest-bestanden.

## <a name="understand-the-application-model"></a>Inzicht krijgen in het toepassingsmodel
Een toepassing is een verzameling van de services die een bepaalde functie of functies uitvoeren. Een service kunt vervult een functie volledig en zelfstandig en starten en onafhankelijk van andere services worden uitgevoerd.  Een service bestaat uit code, configuratie en gegevens. Voor elke service code bestaat uit de uitvoerbare binaire bestanden, configuratie bestaat uit service-instellingen die kunnen worden geladen tijdens de uitvoering en gegevens bestaat uit willekeurige statische gegevens om te worden verbruikt door de service. Elk onderdeel in dit toepassingsmodel hiërarchische kan worden samengesteld en bijgewerkte onafhankelijk van elkaar.

![De Service Fabric-toepassingsmodel][appmodel-diagram]

Een toepassingstype is een classificatie van een toepassing en bestaat uit een bundel van servicetypen. Een servicetype is een classificatie van een service. De categorisatie kan hebben verschillende instellingen en configuraties, maar de kernfunctionaliteit blijft hetzelfde. De exemplaren van een service worden de variaties van de configuratie van andere service van het type van de dezelfde service.  

Klassen (of 'typen') van toepassingen en services via XML-bestanden (Toepassingsmanifesten en servicemanifesten) worden beschreven.  De manifesten toepassingen en services beschreven en worden de sjablonen op basis waarvan toepassingen uit de installatiekopieopslag van het cluster kunnen worden gemaakt.  Manifesten worden behandeld in detail in [toepassings- en servicemanifesten](service-fabric-application-and-service-manifests.md). De schemadefinitie voor het bestand ServiceManifest.xml en ApplicationManifest.xml is geïnstalleerd met de Service Fabric SDK en hulpprogramma's voor *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*. Het XML-schema wordt gedocumenteerd in [ServiceFabricServiceModel.xsd schemadocumentatie](service-fabric-service-model-schema.md).

De code voor exemplaren van een andere toepassing wordt uitgevoerd als afzonderlijke processen, zelfs wanneer die wordt gehost door de dezelfde Service Fabric-knooppunt. Bovendien, de levenscyclus van elk toepassingsexemplaar kan worden beheerd (bijvoorbeeld een upgrade uitgevoerd) onafhankelijk van elkaar. Het volgende diagram toont hoe de soorten toepassingen worden samengesteld uit servicetypen, die op zijn beurt bestaan uit code, configuratie en gegevenspakketten. Ter vereenvoudiging van het diagram, alleen de code/config/gegevens pakketten voor `ServiceType4` worden weergegeven, hoewel u elk servicetype omvat enkele of alle pakkettypen.

![Service Fabric-toepassingstypen en servicetypen][cluster-imagestore-apptypes]

Er kan een of meer exemplaren van een servicetype actief zijn in het cluster. Stateful service-exemplaren of replica's, een bijvoorbeeld hoge betrouwbaarheid bereiken door de status te repliceren tussen de replica's zich op verschillende knooppunten in het cluster. In wezen bieden Replication redundantie voor de service moet beschikbaar zijn, zelfs als één knooppunt in een cluster is mislukt. Een [service gepartitioneerd](service-fabric-concepts-partitioning.md) verder verdeelt de status (en patronen voor databasetoegang naar deze staat) naar verschillende knooppunten in het cluster.

Het volgende diagram toont de relatie tussen toepassingen en service-exemplaren, partities en replica's.

![Partities en replica's in een service][cluster-application-instances]

> [!TIP]
> U kunt de indeling van toepassingen weergeven in een cluster met behulp van het hulpprogramma voor Service Fabric Explorer op http://&lt;yourclusteraddress&gt;: 19080/Explorer. Zie voor meer informatie, [uw cluster visualiseren met Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
- Meer informatie over service [status](service-fabric-concepts-state.md), [partitioneren](service-fabric-concepts-partitioning.md), en [beschikbaarheid](service-fabric-availability-services.md).
- Meer informatie over hoe toepassingen en services worden gedefinieerd in [toepassings- en servicemanifesten](service-fabric-application-and-service-manifests.md).
- [Toepassing die als host fungeert modellen](service-fabric-hosting-model.md) relatie tussen de replica's (of exemplaren) van een geïmplementeerde service en service-proces wordt beschreven.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png


