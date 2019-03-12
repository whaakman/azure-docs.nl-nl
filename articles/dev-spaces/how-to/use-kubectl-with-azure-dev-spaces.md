---
title: Kubectl gebruiken met Azure Dev spaties
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: conceptual
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s '
ms.openlocfilehash: 0dfe88966deeb4dcf0196aa1f1584a06794b36a7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57773262"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Gebruik kubectl met een Azure Dev Space

U kunt toegang krijgen tot het Kubernetes-cluster in een Azure Dev Space, en gebruik maken van bestaande Kubernetes-hulpprogramma's zoals `kubectl`.

Met de opdracht `az aks use-dev-spaces` wordt automatisch een `kubectl` configuratiecontext voor u toegevoegd, zodat kubectl al zal zijn verbonden met uw Azure Dev Spaces Kubernetes-cluster. Voorbeelden:
- Controleer de huidige context: `kubectl config current-context`
- Toon een lijst met alle beschikbare contexten: `kubectl config get-contexts`. 
- Wijzig de context: `kubectl config use-context <context-name>`
- Bekijk het Kubernetes dashboard: voer uit `kubectl proxy`, open uw browser naar het adres dat deze opdracht weergeeft (voeg `/ui` toe aan de URL om naar het Kubernetes-dashboard te navigeren).
- Toon een lijst met de actieve services in de standaard Azure Dev Spaces ruimte met de naam *default*: `kubectl get services --namespace=default`

