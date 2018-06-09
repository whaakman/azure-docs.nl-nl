---
title: Kubectl gebruiken met Azure Dev spaties | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 3dc0dd4b571f716bcabb67c4cbef1ea6d762eb94
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248655"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Kubectl met een Azure-Dev-ruimte gebruiken

U hebt toegang tot het cluster Kubernetes binnen een Azure-Dev-adresruimte en gebruik bestaande Kubernetes hulpprogramma's, zoals `kubectl`.

Met `az aks use-dev-spaces` opdracht automatisch wordt toegevoegd een `kubectl` configuratiecontext voor u, zodat kubectl moeten al zijn verbonden met uw Azure Dev spaties Kubernetes-cluster. Voorbeelden:
- Controleer de huidige context. `kubectl config current-context`
- Lijst van alle beschikbare contexten: `kubectl config get-contexts`. 
- Context wijzigen: `kubectl config use-context <context-name>`
- Het dashboard Kubernetes weergeven: uitvoeren `kubectl proxy`, open uw browser het adres dat u deze opdracht verzendt (append `/ui` naar de URL om te navigeren naar het dashboard Kubernetes).
- Lijst met de actieve services in de standaard Azure Dev spaties ruimte met de naam *standaard*: `kubectl get services --namespace=default`

