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
ms.date: 10/24/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: fa46d16e2105c0354cc533c58fb9e0093c129ced
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Inleiding tot Azure Containerservice (AKS)

Azure Container Service (AKS) maakt het eenvoudig te maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor beperkte toepassingen worden uitgevoerd. U kunt hierbij dan uitgaan van uw eigen vaardigheden of gebruikmaken van een grote en groeiende groep community experts om toepassingen op basis van containers te implementeren en beheren in Microsoft Azure.

Met behulp van AKS, kunt u profiteren van de functies bedrijfsniveau van Azure, zonder dat zij de toepassing draagbaarheid via Kubernetes en de Docker-bestandsindeling.

## <a name="using-azure-container-service-aks"></a>Met behulp van Azure Containerservice (AKS)
Ons doel met AKS wordt een container hostomgeving met behulp van open-source hulpprogramma's en -technologieën die tegenwoordig populair onder onze klanten. Om dit te bereiken, maken we de standaard API-eindpunten van Kubernetes beschikbaar. Met behulp van deze standaard eindpunten kunt u gebruikmaken van alle software die geschikt is voor communicatie met een Kubernetes-cluster. U kunt bijvoorbeeld [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) of [draft](https://github.com/Azure/draft) gebruiken.

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Maken van een Kubernetes-cluster met behulp van Azure Container Service (AKS)
Om te beginnen met behulp van AKS, implementeert u een cluster AKS met de [Azure CLI](./kubernetes-walkthrough.md) of via de portal (zoek de Marketplace voor **Azure Container Service**). Als u een ervaren gebruiker bent die meer controle wil over de Azure Resource Manager-sjablonen, kunt u het open source [acs-engine](https://github.com/Azure/acs-engine)-project gebruiken om uw eigen aangepaste Kubernetes-cluster te bouwen en het te implementeren via de `az` CLI.

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

Ondersteuning voor Kubernetes in Azure Container Service (Azure Friday, januari 2017):

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

Hulpprogramma's voor het ontwikkelen en implementeren van toepassingen in Kubernetes (Azure OpenDev, juni 2017):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Meer informatie over het implementeren en beheren van AKS met de AKS Quick Start.

> [!div class="nextstepaction"]
> [AKS-zelfstudie](./kubernetes-walkthrough.md)