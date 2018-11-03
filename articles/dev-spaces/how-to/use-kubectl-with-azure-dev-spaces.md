---
title: Kubectl gebruiken met Azure Dev spaties | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 05/11/2018
ms.topic: article
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: 6af96556d816e8773c3419b4545198148f6eca3a
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978329"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Kubectl gebruiken met een Azure Dev-ruimte

U kunt toegang tot het Kubernetes-cluster in een Azure Dev-ruimte, en gebruik bestaande Kubernetes-hulpprogramma's zoals `kubectl`.

Met `az aks use-dev-spaces` opdracht automatisch wordt toegevoegd een `kubectl` configuratiecontext voor u, zodat u kubectl al moet zijn verbonden met uw Azure Dev spaties Kubernetes-cluster. Voorbeelden:
- Controleer of de huidige context: `kubectl config current-context`
- Lijst met alle beschikbare contexten: `kubectl config get-contexts`. 
- De context wijzigen: `kubectl config use-context <context-name>`
- Het Kubernetes dashboard bekijken: uitvoeren `kubectl proxy`, open uw browser naar het adres dat met deze opdracht verzendt (append `/ui` naar de URL om te navigeren naar het Kubernetes-dashboard).
- Lijst met de actieve services in de standaard Azure Dev spaties ruimte met de naam *standaard*: `kubectl get services --namespace=default`

