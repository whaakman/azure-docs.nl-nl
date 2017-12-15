---
title: Inleiding tot Azure Container Service voor Kubernetes | Microsoft Docs
description: Met Azure Container Service voor Kubernetes kunt u eenvoudig toepassingen op basis van containers implementeren en beheren in Azure.
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Kubernetes, Docker, containers, microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 9fba9fdda3503ec80fede845466858825e3677a5
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Inleiding tot Azure Container Service (AKS)

Azure Container Service (AKS) helpt bij het eenvoudig maken, configureren en beheren van een cluster virtuele machines die vooraf zijn geconfigureerd voor het uitvoeren van beperkte toepassingen. U kunt hierbij dan uitgaan van uw eigen vaardigheden of gebruikmaken van een grote en groeiende groep community experts om toepassingen op basis van containers te implementeren en beheren in Microsoft Azure.

Met AKS profiteert u van de hoogwaardige functies van Azure en behoudt u draagbaarheid van toepassingen via Kubernetes en het Docker-installatiekopieformaat.

## <a name="managed-kubernetes-in-azure"></a>Managed Kubernetes in Azure

AKS verkleind de complexiteit en de operationele overhead die gepaard gaan met het beheer van een Kubernetes-cluster door veel van deze taken naar Azure over te hevelen. Azure handelt als een gehoste Kubernetes-service cruciale taken voor u af zoals statuscontrole en onderhoud. Bovendien betaalt u alleen voor de agentknooppunten binnen uw clusters, niet voor de hoofdknooppunten. Als een beheerde Kubernetes-service heeft AKS het volgende te bieden:

> [!div class="checklist"]
> * Geautomatiseerd upgraden van Kubernetes-versies en patching
> * Eenvoudige schaalmogelijkheden voor clusters
> * Zelfherstellende gehoste controlelaag (hoofdknooppunten)
> * Kostenbesparingen: betaal alleen voor actieve agentknooppunten in de groep

Omdat Azure het beheer van de knooppunten in uw AKS-cluster afhandelt, hoeft u veel taken niet meer handmatig uit te voeren, zoals het upgraden van clusters. Omdat Azure deze cruciale onderhoudstaken voor u uitvoert, hebt u via AKS geen directe toegang (zoals met SSH) tot het cluster.

## <a name="using-azure-container-service-aks"></a>Azure Container Service (AKS) gebruiken
AKS is bedoeld om een hostomgeving voor containers te bieden door gebruik te maken van de open source tools en technologieën die tegenwoordig populair zijn onder onze klanten. Om dit te bereiken, maken we de standaard API-eindpunten van Kubernetes beschikbaar. Met behulp van deze standaard eindpunten kunt u gebruikmaken van alle software die geschikt is voor communicatie met een Kubernetes-cluster. U kunt bijvoorbeeld [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) of [draft](https://github.com/Azure/draft) gebruiken.

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Een Kubernetes-cluster maken met Azure Container Service (AKS)
Als u AKS wilt gebruiken, implementeert u eerst een AKS-cluster met behulp van [Azure CLI 2.0](./kubernetes-walkthrough.md) of via de portal (zoek in Marketplace op **Azure Container Service**). Als u een ervaren gebruiker bent die meer controle wil over de Azure Resource Manager-sjablonen, kunt u het open source [acs-engine](https://github.com/Azure/acs-engine)-project gebruiken om uw eigen aangepaste Kubernetes-cluster te bouwen en het te implementeren via de `az` CLI.

### <a name="using-kubernetes"></a>Kubernetes gebruiken
Kubernetes automatiseert het implementeren, schalen en beheren van toepassingen in containers. De applicatie bevat een uitgebreide set functies, zoals:
* Automatische binpacking
* Automatisch herstel
* Horizontaal schalen
* Servicedetectie en taakverdeling
* Geautomatiseerde implementaties en terugdraaiacties
* Geheimen- en configuratiebeheer
* Opslagindeling
* Batchuitvoering

## <a name="videos"></a>Video's

Azure Container Service (AKS) - Azure Friday, oktober 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Hulpprogramma's voor het ontwikkelen en implementeren van toepassingen in Kubernetes (Azure OpenDev, juni 2017):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

U vindt meer informatie over het implementeren en beheren van AKS in de bijbehorende Quick Start van AKS.

> [!div class="nextstepaction"]
> [AKS-zelfstudie](./kubernetes-walkthrough.md)