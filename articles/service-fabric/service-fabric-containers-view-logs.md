---
title: Containers logboeken weergeven in Azure Service Fabric | Microsoft Docs
description: Beschrijft hoe om Logboeken van de container voor een actieve Service Fabric-containerservices met behulp van Service Fabric Explorer weer te geven.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/09/2018
ms.author: ryanwi
ms.openlocfilehash: 48ee54460454368deef44c8f84624e32856efafa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="view-logs-for-a-service-fabric-container-service"></a>Logboeken bekijken voor een Service Fabric-containerservice
Azure Service Fabric is een container orchestrator en ondersteunt zowel [Linux- en Windows-containers](service-fabric-containers-overview.md).  Dit artikel wordt beschreven hoe u logboeken van de container van een actieve containerservice weergeven, zodat u kunt onderzoeken en oplossen van problemen.

## <a name="access-container-logs"></a>Container toegangslogboeken
Container logboeken zijn toegankelijk via [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).  Open in een webbrowser, Service Fabric Explorer van het cluster management eindpunt door te navigeren naar [ http://mycluster.region.cloudapp.azure.com:19080/Explorer ](http://mycluster.region.cloudapp.azure.com:19080/Explorer).  

Container logboeken bevinden zich op het clusterknooppunt dat op de container service-exemplaar wordt uitgevoerd. Een voorbeeld: ophalen van de logboeken van de web-front-container van de [voorbeeldtoepassing Linux stemmen](service-fabric-quickstart-containers-linux.md). Vouw in de structuurweergave **Cluster**>**toepassingen**>**VotingType**>**fabric: / Voting / azurevotefront**.  Vervolgens vouwt u de partitie (d1aa737e-f22a-e347-be16-eec90be24bc1, in dit voorbeeld) en of de container wordt uitgevoerd op het clusterknooppunt *_lnxvm_0*.

In de structuurweergave, kunt u de codepakket-zoeken op de *_lnxvm_0* knooppunt door het uitbreiden van **knooppunten**>**_lnxvm_0**>**fabric: / stemmen**  > **azurevotfrontPkg**>**Code pakketten**>**code**.  Selecteer vervolgens de **Container logboeken** optie voor het weergeven van de logboeken van de container.

![Service Fabric-platform][Image1]


## <a name="next-steps"></a>Volgende stappen
- Doorloop de [maken van een zelfstudie over Linux-container](service-fabric-tutorial-create-container-images.md).
- Meer informatie over [Service Fabric en containers](service-fabric-containers-overview.md)

[Image1]: media/service-fabric-containers-view-logs/view-container-logs-sfx.png
