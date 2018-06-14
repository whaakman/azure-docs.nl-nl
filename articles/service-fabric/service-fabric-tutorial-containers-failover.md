---
title: Failover uitvoeren voor een Azure Service Fabric-container-app en deze schalen | Microsoft Docs
description: In deze zelfstudie leert u hoe failovers worden verwerkt in een Azure Service Fabric-containertoepassing.  U leert ook hoe u de containers en services schaalt die in een cluster worden uitgevoerd.
services: service-fabric
documentationcenter: ''
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: Docker, containers, Microservices, Service Fabric, Azure
ms.assetid: ''
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 27d1918125b8c2f79f6506470ae43354e402f9af
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2018
ms.locfileid: "29555479"
---
# <a name="tutorial-demonstrate-fail-over-and-scaling-of-container-services-with-service-fabric"></a>Zelfstudie: failover uitvoeren voor containerservices met Service Fabric en deze schalen

Deze zelfstudie is deel drie van een serie. In deze zelfstudie leert u hoe failovers worden verwerkt in Service Fabric-containertoepassingen. Bovendien leert u hoe u containers kunt schalen. In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Meer informatie over het uitvoeren van een failover voor een container in een Service Fabric-cluster  
> * De webfront-endcontainers in een toepassing schalen

## <a name="prerequisites"></a>Vereisten
De toepassing uit [Deel 2](service-fabric-tutorial-package-containers.md) wordt uitgevoerd in een actief Service Fabric-cluster.

## <a name="fail-over-a-container-in-a-cluster"></a>Failover uitvoeren voor een container in een cluster
Service Fabric zorgt ervoor dat containerexemplaren automatisch worden verplaatst naar andere knooppunten in het cluster, mocht er een fout optreden. U kunt een knooppunt met containers ook handmatig leegmaken en de containers vervolgens probleemloos verplaatsen naar andere knooppunten in het cluster. Er zijn meerdere manieren om services te schalen. In dit voorbeeld gebruiken we Service Fabric Explorer.

Doe het volgende om een failover uit te voeren voor de front-endcontainer:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klik op het knooppunt **fabric:/TestContainer/azurevotefront** in de structuurweergave en vouw het partitieknooppunt uit (vertegenwoordigd door een GUID). Let op: de naam van het knooppunt in de structuurweergave laat zien op welke knooppunten deze container momenteel wordt uitgevoerd, bijvoorbeeld: `_nodetype_1`
3. Vouw het knooppunt **Knooppunten** in de structuurweergave uit. Klik op het weglatingsteken (drie punten) naast het knooppunt waarop de container wordt uitgevoerd.
1. Kies **Opnieuw starten** om dit knooppunt opnieuw te starten en bevestig deze actie. Door het opnieuw starten wordt voor de container een failover uitgevoerd naar een ander knooppunt in het cluster.

![noderestart][noderestart]

Let op: de naam van het knooppunt (die aangeeft waar de front-endcontainers worden uitgevoerd) wordt gewijzigd in een ander knooppunt in het cluster. Na enkele ogenblikken kunt u weer naar de toepassing gaan en ziet u dat deze nu wordt uitgevoerd op een ander knooppunt.

## <a name="scale-containers-and-services-in-a-cluster"></a>Containers en services in een cluster schalen
Service Fabric-containers kunnen worden geschaald in een cluster om plaats te bieden aan de belasting voor de services. U schaalt een container door het aantal exemplaren te wijzigen dat wordt uitgevoerd in het cluster.

Doe het volgende om de webfront-end te schalen:

1. Open Service Fabric Explorer in het cluster - bijvoorbeeld: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Klik op het weglatingsteken (drie punten) naast het knooppunt **fabric:/TestContainer/azurevotefront** in de structuurweergave en kies **Service schalen**.

![sfxscale][sfxscale]

U kunt er nu voor kiezen om het aantal exemplaren van de webfront-end te schalen.

3. Wijzig het aantal in **2** en klik op **Service schalen**.
4. Klik op het knooppunt **fabric:/TestContainer/azurevotefront** in de structuurweergave en vouw het partitieknooppunt uit (vertegenwoordigd door een GUID).

![sfxscaledone][sfxscaledone]

U ziet nu dat de service twee exemplaren heeft. In de structuurweergave ziet u op welke knooppunten de exemplaren worden uitgevoerd.

Met deze eenvoudige beheertaak is het aantal beschikbare resources voor het verwerken van gebruikersbelasting voor onze front-endservice verdubbeld. Het is belangrijk te weten dat u niet meerdere exemplaren van een service nodig hebt om ervoor te zorgen dat deze op betrouwbare wijze wordt uitgevoerd. Als de service mislukt, wordt in Service Fabric een nieuw exemplaar van de service uitgevoerd in het cluster.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gezien hoe u een failover voor een container kunt uitvoeren en een toepassing kunt schalen. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * Meer informatie over het uitvoeren van een failover voor een container in een Service Fabric-cluster  
> * De webfront-endcontainers in een toepassing schalen

In deze zelfstudie hebt u het volgende geleerd: 
> [!div class="checklist"]
> * Containerinstallatiekopieën maken
> * Containerinstallatiekopieën naar Azure Container Registry pushen
> * Containers voor Service Fabric verpakken met behulp van Yeoman
> * Service Fabric-toepassing met containers bouwen en uitvoeren
> * Hoe failovers en schalen worden verwerkt in Service Fabric

[noderestart]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialnoderestart.png
[sfxscale]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscale.png
[sfxscaledone]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscaledone.png
