---
title: Geef de instellingen metrische gegevens en de plaatsing in Azure microservices | Microsoft Docs
description: Met een beschrijving van een Service Fabric-service door metrische gegevens, plaatsingsbeperkingen en andere plaatsingsbeleid te geven.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 0ae4e874d0fd0922295a4ec7ad719a0a1fb108c8
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Cluster resource manager-instellingen voor Service Fabric-services configureren
De Service Fabric Cluster Resource Manager kunt fijnmazig controle over de regels die bepalen van elke afzonderlijke service met de naam. Elke benoemde service kunt regels opgeven voor hoe moet worden toegewezen in het cluster. Elke benoemde service kunt ook de set met metrische gegevens die deze wil definiëren om te rapporteren, met inbegrip van hoe belangrijk dat ze zijn aan die service. Configureren van services uitvalt in drie verschillende taken:

1. Plaatsingsbeperkingen configureren
2. Metrische gegevens configureren
3. Configureren van geavanceerde plaatsingsbeleid en andere regels (minder algemeen)

## <a name="placement-constraints"></a>Plaatsingsbeperkingen
Plaatsingsbeperkingen worden gebruikt om te bepalen welke knooppunten in het cluster een service daadwerkelijk op kunnen uitvoeren. Doorgaans een bepaalde service-exemplaar of alle services van een bepaald type beperkte uit te voeren op een bepaald type knooppunt genoemd. Plaatsingsbeperkingen worden uitgebreid. U kunt een set eigenschappen per knooppunttype definiëren en vervolgens selecteren ze met beperkingen bij het maken van services. U kunt ook een service plaatsingsbeperkingen wijzigen terwijl deze wordt uitgevoerd. Hiermee kunt u om te reageren op wijzigingen in het cluster of de vereisten van de service. De eigenschappen van een bepaald knooppunt kunnen ook dynamisch worden bijgewerkt in het cluster. Meer informatie over plaatsingsbeperkingen en het configureren ervan vindt u in [in dit artikel](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

## <a name="metrics"></a>Metrische gegevens
Meetgegevens voor het aantal resources die een bepaalde benoemde service moet zijn. Configuratie van de service omvat hoeveel van de bron elke stateful replica of stateless exemplaar van die service standaard verbruikt. Metrische gegevens behoren ook een gewicht dat aangeeft hoe belangrijk balancing die metrische gegevens die service, in geval gebruik van systeembronnen nodig zijn.

## <a name="advanced-placement-rules"></a>Van geavanceerde plaatsingsregels
Er zijn andere typen regels die handig in een minder algemene scenario's zijn voor de plaatsing. Een aantal voorbeelden:
- Beperkingen die u bij geografisch verspreide clusters helpen
- Bepaalde toepassingsarchitecturen

Andere plaatsingsregels zijn via correlaties of beleidsregels geconfigureerd.

## <a name="next-steps"></a>Volgende stappen
- Metrische gegevens zijn hoe de Service Fabric-Cluster Resource Manager beheert gebruiks- en capaciteit in het cluster. Bekijk voor meer informatie over metrische gegevens en het configureren ervan [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
- Affiniteit is één modus die u voor uw services configureren kunt. Wordt meestal niet, maar als u deze nodig hebt u meer over deze [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Er zijn veel verschillende plaatsingsregels die in de service voor het afhandelen van aanvullende scenario's kunnen worden geconfigureerd. U vindt informatie over deze beleidsregels voor verschillende plaatsing [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- Vanaf het begin starten en [Maak kennis met de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
- Als u wilt weten over hoe de Cluster Resource Manager beheert en een compromis tussen de werklast van het cluster, Raadpleeg het artikel op [load balancing](service-fabric-cluster-resource-manager-balancing.md)
- De Cluster Resource Manager bevat veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
