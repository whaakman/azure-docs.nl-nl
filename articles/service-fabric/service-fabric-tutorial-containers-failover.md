---
title: Failover en een app van Azure Service Fabric-containers schalen | Microsoft Docs
description: Meer informatie over hoe failover in een toepassing Azure Service Fabric-containers wordt verwerkt.  Ook informatie over het schalen van de containers en -services in een cluster wordt uitgevoerd.
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: Docker, Containers, Microservices, Service Fabric, Azure
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 21dd9dfbc90c26236c43e2c334305ca97f63d361
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="demonstrate-fail-over-and-scaling-of-container-services-with-service-fabric"></a>Illustratie van meer dan en het schalen van containerservices met Service Fabric is mislukt

Deze zelfstudie maakt deel uit drie van een serie. In deze zelfstudie leert u hoe de failover in Service Fabric-containertoepassingen wordt verwerkt. Bovendien leert u hoe containers schalen. In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Meer informatie over failover van de container in een Service Fabric-cluster  
> * De web-front-containers schalen in een appliction

## <a name="prerequisites"></a>Vereisten
De toepassing van [deel 2](service-fabric-tutorial-package-containers.md) wordt uitgevoerd in een actieve Service Fabric-cluster.

## <a name="fail-over-a-container-in-a-cluster"></a>Failover uitvoeren voor een container in een cluster
Service Fabric wordt gecontroleerd of uw containerexemplaren automatisch verplaatst naar andere knooppunten in het cluster, moet er een storing optreedt. U kunt ook handmatig een knooppunt van containers en probleemloos verplaatsen naar andere knooppunten in het cluster. Er zijn meerdere manieren om services te schalen. In dit voorbeeld gebruiken we Service Fabric Explorer.

Doe het volgende om een failover uit te voeren voor de front-endcontainer:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klik op de **fabric: / TestContainer/azurevotefront** knooppunt in de structuurweergave en het partitie-knooppunt (vertegenwoordigd door een GUID zijn). Let op de naam van het knooppunt in de structuurweergave, waarin u de knooppunten die container momenteel wordt uitgevoerd op - voorbeeld`_nodetype_1`
3. Vouw de **knooppunten** knooppunt in de treeview. Klik op het weglatingsteken (drie punten) naast het knooppunt waarop de container wordt uitgevoerd.
1. Kies **Opnieuw starten** om dit knooppunt opnieuw te starten en bevestig deze actie. Door het opnieuw starten wordt voor de container een failover uitgevoerd naar een ander knooppunt in het cluster.

![noderestart][noderestart]

U ziet hoe de knooppunt naam te geven wanneer de front-containers wordt uitgevoerd, wordt nu gewijzigd naar een ander knooppunt in het cluster. Na enkele ogenblikken moet u het volgende kunnen de toepassing nu wordt uitgevoerd op een ander knooppunt en blader naar de toepassing opnieuw.

## <a name="scale-containers-and-services-in-a-cluster"></a>Scale-containers en -services in een cluster
Service Fabric-containers kunnen worden geschaald over een cluster te maken voor de belasting van de services. U kunt een container schalen door het wijzigen van het aantal exemplaren in het cluster wordt uitgevoerd.

Als u wilt de webfront-end schalen, gaat u de volgende stappen uit:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klik op het weglatingsteken (drie punten) naast de **fabric: / TestContainer/azurevotefront** knooppunt in de structuur weergeven en kies **Scale Service**.

![sfxscale][sfxscale]

U kunt nu kiezen voor het schalen van het aantal exemplaren van de webfront-end.

3. Wijzig het aantal in **2** en klik op **Service schalen**.
4. Klik op de **fabric: / TestContainer/azurevotefront** knooppunt in de structuur weergeven en het partitie-knooppunt (vertegenwoordigd door een GUID zijn).

![sfxscaledone][sfxscaledone]

U ziet nu dat de service twee exemplaren heeft. In de structuurweergave ziet u welke knooppunten de instanties worden uitgevoerd op.

Met deze eenvoudige beheertaak is het aantal beschikbare resources voor het verwerken van gebruikersbelasting voor onze front-endservice verdubbeld. Het is belangrijk te weten dat u niet meerdere exemplaren van een service nodig hebt om ervoor te zorgen dat deze op betrouwbare wijze wordt uitgevoerd. Als de service mislukt, wordt in Service Fabric een nieuw exemplaar van de service uitgevoerd in het cluster.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is de container failover en schalen van een toepassing gedemonstreerd. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Meer informatie over failover van de container in een Service Fabric-cluster  
> * De web-front-containers schalen in een appliction

In deze zelfstudie serie, hebt u geleerd hoe: 
> [!div class="checklist"]
> * Maken van installatiekopieën van de container
> * Installatiekopieën van de push-container in Azure Container register
> * Pakket Containers voor Service-infrastructuur met behulp van Yeoman
> * Bouwen en uitvoeren van een Service Fabric-toepassing met Containers
> * Hoe failover en schalen in Service Fabric worden verwerkt

[noderestart]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialnoderestart.png
[sfxscale]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscale.png
[sfxscaledone]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscaledone.png
