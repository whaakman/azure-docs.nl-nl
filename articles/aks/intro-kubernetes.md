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
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Inleiding tot Azure Containerservice (AKS)

Azure Container Service (AKS) maakt het eenvoudig te maken, configureren en beheren van een cluster van virtuele machines die vooraf zijn geconfigureerd voor beperkte toepassingen worden uitgevoerd. U kunt hierbij dan uitgaan van uw eigen vaardigheden of gebruikmaken van een grote en groeiende groep community experts om toepassingen op basis van containers te implementeren en beheren in Microsoft Azure.

Met behulp van AKS, kunt u profiteren van de functies bedrijfsniveau van Azure, zonder dat zij de toepassing draagbaarheid via Kubernetes en de Docker-bestandsindeling.

## <a name="managed-kubernetes-in-azure"></a>Beheerde Kubernetes in Azure

AKS beperkt de complexiteit en operationele overhead voor het beheer van een cluster Kubernetes door het offloaden van veel van deze functie aan Azure. Als een gehoste service voor Kubernetes, Azure ingangen kritieke taken zoals statuscontrole en onderhoud voor u. Bovendien betaalt u alleen voor de agent-knooppunten binnen clusters, niet voor de masters. Als een beheerde service voor het Kubernetes AKS biedt:

> [!div class="checklist"]
> * Geautomatiseerde Kubernetes versie-upgrades en patchen
> * Eenvoudig cluster schalen
> * Zelfherstellende gehoste besturingselement vlak (modellen)
> * Kostenbesparingen: betaal alleen voor het uitvoeren van agent knooppunten van de groep-

Met Azure afhandeling van het beheer van de knooppunten in het cluster AKS, moet u niet meer handmatig veel taken uitvoeren zoals cluster-upgrades. Omdat Azure deze kritieke onderhoudstaken voor u verwerkt, AKS biedt geen directe toegang (zoals met SSH) voor het cluster.

## <a name="using-azure-container-service-aks"></a>Met behulp van Azure Containerservice (AKS)
Het doel van AKS is naar een hostomgeving met behulp van open-source hulpprogramma's en -technologieën die tegenwoordig populaire klanten container bieden. Om dit te bereiken, maken we de standaard API-eindpunten van Kubernetes beschikbaar. Met behulp van deze standaard eindpunten kunt u gebruikmaken van alle software die geschikt is voor communicatie met een Kubernetes-cluster. U kunt bijvoorbeeld [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) of [draft](https://github.com/Azure/draft) gebruiken.

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

Azure Containerservice (AKS) - Azure vrijdag, oktober 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Hulpprogramma's voor het ontwikkelen en implementeren van toepassingen op Kubernetes - Azure OpenDev juni 2017:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Meer informatie over het implementeren en beheren van AKS met de AKS Quick Start.

> [!div class="nextstepaction"]
> [AKS-zelfstudie](./kubernetes-walkthrough.md)