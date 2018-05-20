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
description: Snelle Kubernetes ontwikkeling met containers en microservices op Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 38a433a14ab977fb56a8331a057d27241f1d9783
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Kubectl met een Azure-Dev-ruimte gebruiken

U hebt toegang tot het cluster Kubernetes binnen een Azure-Dev-adresruimte en gebruik bestaande Kubernetes hulpprogramma's, zoals `kubectl`.

Met `azds resource create` of `azds resource select` worden automatisch toegevoegd een `kubectl` configuratiecontext voor u, zodat kubectl moeten al zijn verbonden met uw Azure Dev spaties Kubernetes-cluster. Voorbeelden:
- Controleer de huidige context. `kubectl config current-context`
- Lijst van alle beschikbare contexten: `kubectl config get-contexts`. 
- Context wijzigen: `kubectl config use-context <context-name>`
- Het dashboard Kubernetes weergeven: uitvoeren `kubectl proxy`, open uw browser het adres dat u deze opdracht verzendt (append `/ui` naar de URL om te navigeren naar het dashboard Kubernetes).
- Lijst met de actieve services in de standaard Azure Dev spaties ruimte met de naam *standaard*: `kubectl get services --namespace=default`

