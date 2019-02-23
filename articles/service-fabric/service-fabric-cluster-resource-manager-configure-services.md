---
title: Instellingen voor metrische gegevens en plaatsing opgeven in Azure Service Fabric | Microsoft Docs
description: Leer hoe u een Service Fabric-service door op te geven de metrische gegevens, plaatsingsbeperkingen en andere plaatsingsbeleid te beschrijven.
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: ''
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 55d75bb0bae4bf3feb370b7fdf4d80dc43bdc0ca
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56736886"
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>Cluster resource manager-instellingen voor Service Fabric-services configureren
De Service Fabric Cluster Resource Manager kunt heel nauwkeurig bepalen de regels op basis van elke afzonderlijke gebruiker met de naam service. Elke benoemde service kan regels opgeven voor hoe moet worden toegewezen in het cluster. Elke benoemde service kan ook definiëren voor de set van metrische gegevens die zij wil rapport, met inbegrip van hoe belangrijk dat ze zijn die service. Configureren van services problematisch zijn, in drie verschillende taken:

1. Plaatsingsbeperkingen configureren
2. Configureren van metrische gegevens
3. Configureren van geavanceerde plaatsingsbeleid en andere regels (minder algemeen)

## <a name="placement-constraints"></a>Plaatsingsbeperkingen
Plaatsingsbeperkingen worden gebruikt om te bepalen welke knooppunten in het cluster een service daadwerkelijk op kunnen uitvoeren. Normaal gesproken een bepaalde service-exemplaar of alle services van een bepaald type beperkte om uit te voeren op een bepaald type knooppunt genoemd. Plaatsingsbeperkingen worden uitgebreid. U kunt een set eigenschappen per knooppunttype definiëren en vervolgens selecteren voor deze met beperkingen bij het maken van services. U kunt ook een service-plaatsingsbeperkingen wijzigen terwijl deze wordt uitgevoerd. Hiermee kunt u om te reageren op wijzigingen in het cluster of de vereisten van de service. De eigenschappen van een bepaald knooppunt kunnen ook dynamisch worden bijgewerkt in het cluster. Meer informatie over plaatsingsbeperkingen en hoe u ze configureert vindt u [in dit artikel](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

## <a name="metrics"></a>Metrische gegevens
Metrische gegevens zijn de set van resources die een bepaalde benoemde service nodig heeft. Configuratie van de service omvat welk deel van die resource elke stateful doelreplica of stateless van die service standaard verbruikt. Metrische gegevens bevatten ook een gewicht die aangeeft hoe belangrijk is taakverdeling die metrische gegevens die service, in geval compromissen nodig zijn.

## <a name="advanced-placement-rules"></a>Geavanceerde plaatsingsregels
Er zijn andere typen plaatsingsregels die handig zijn in minder algemene scenario's. Een aantal voorbeelden:
- Beperkingen die u bij geografisch verspreide clusters helpen
- Bepaalde architecturen voor toepassingen

Andere plaatsingsregels zijn via correlaties of beleidsregels geconfigureerd.

## <a name="next-steps"></a>Volgende stappen
- Metrische gegevens zijn hoe gebruiks- en capaciteit in het cluster worden beheerd door de Service Fabric-Cluster Resource Manager. Bekijk voor meer informatie over metrische gegevens en hoe u ze configureert, [in dit artikel](service-fabric-cluster-resource-manager-metrics.md)
- Affiniteit is één modus die u voor uw services configureren kunt. Het is niet gebruikelijk, maar als u deze nodig hebt vindt u informatie over het [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Er zijn veel verschillende plaatsingsregels die kunnen worden geconfigureerd op uw service voor het afhandelen van aanvullende scenario's. U vindt informatie over deze verschillende plaatsingsbeleid [hier](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- Vanaf het begin starten en [een inleiding tot de Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)
- Als u wilt weten over hoe met Cluster Resource Manager beheert en verdeelt de taken in het cluster, Zie het artikel op [taakverdeling](service-fabric-cluster-resource-manager-balancing.md)
- Cluster Resource Manager beschikt over veel opties voor het beschrijven van het cluster. Meer informatie over deze, Bekijk dit artikel op [met een beschrijving van een Service Fabric-cluster](service-fabric-cluster-resource-manager-cluster-description.md)
