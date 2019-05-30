---
title: Inleiding tot Azure Dev-opslagruimten
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
ms.author: zarhoads
ms.date: 05/07/2019
ms.topic: overview
description: Inleiding tot Azure Dev-opslagruimten
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, kubectl, k8s
manager: jeconnoc
ms.openlocfilehash: 3e887409518f5cc97238a0168213a7918e318c04
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66394011"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces

Azure Dev opslagruimten is een snelle, herhaaldelijke Kubernetes-ontwikkelervaring voor teams in Azure Kubernetes Service (AKS)-clusters. U kunt samenwerken met uw team in een gedeelde AKS-cluster. Azure Dev opslagruimten kunt u de onderdelen van uw toepassing testen in AKS zonder repliceren of nabootsing van afhankelijkheden. U kunt iteratief uitvoeren en fouten opsporen in containers in AKS met minimale ontwikkeling machine setup.

![](media/azure-dev-spaces/collaborate-graphic.gif)


## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Hoe Azure Dev Spaces de ontwikkeling van Kubernetes vereenvoudigt

Azure Dev spaties helpt teams zich richten op de ontwikkeling en snelle iteratie van hun toepassing microservice doordat teams om rechtstreeks met hun volledige microservicearchitectuur of de toepassing die wordt uitgevoerd in AKS te werken. Azure Dev opslagruimten biedt ook een manier om bij te werken onafhankelijk van elkaar delen van uw architectuur met microservices in een geïsoleerde omgeving zonder gevolgen voor de rest van het AKS-cluster of andere ontwikkelaars. Azure Dev opslagruimten is bedoeld voor ontwikkeling en testen op lager niveau ontwikkeling en testomgevingen en is niet bedoeld om uit te voeren op productieclusters AKS.

Omdat teams kunnen met de gehele toepassing werken en rechtstreeks in AKS, Azure Dev spaties samenwerken:

* Minimaliseert de installatie van de lokale computer
* Afname insteltijd voor nieuwe ontwikkelaars van het team
* Verhoogt de snelheid via sneller iteratie van een team
* Vermindert het aantal redundante omgevingen voor ontwikkelen en integreren van omdat teamleden kunnen een cluster delen
* Hiermee verwijdert u de noodzaak om te repliceren of maken van afhankelijkheden gesimuleerde
* Verbetert de samenwerking tussen teams voor ontwikkeling, evenals de teams die zij, zoals DevOps-teams werken

Azure Dev opslagruimten biedt hulpprogramma's voor het genereren van Docker en Kubernetes activa voor uw projecten. Deze hulpprogramma's kunt u eenvoudig nieuwe en bestaande toepassingen toevoegen aan zowel een dev-ruimte en andere clusters AKS.

Zie voor meer informatie over de werking van Azure Dev spaties [hoe Azure Dev spaties werkt en is geconfigureerd][how-dev-spaces-works].

## <a name="supported-regions-and-configurations"></a>Ondersteunde regio's en configuraties

Azure Dev Spaces wordt alleen ondersteund door AKS-clusters in de regio's **US - oost**, **US - oost 2**, **US - centraal**, **US - west 2**, **EU - noord**, **EU - west**, **UK - zuid**, **Azië - zuidoost**, **Australië - oost**, **Canada - centraal** en **Canada - oost**. Azure Dev Spaces ondersteunt het gebruik van [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) of [Visual Studio Code](https://code.visualstudio.com/download) met een [Azure Dev Spaces-extensie](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) die is geïnstalleerd in Linux, MacOS of Windows 8 of hoger, om uw toepassingen te ontwikkelen en in AKS uit te voeren. Het biedt ook ondersteuning met behulp van [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) geïnstalleerd op Windows 8 of hoger. Voor Visual Studio 2019 moet u de Azure-ontwikkelworkload. Voor Visual Studio 2017, moet u de werkbelasting van de ontwikkeling van Web- en [Visual Studio-hulpprogramma's voor Kubernetes](https://aka.ms/get-vsk8stools).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over snelle, herhaaldelijke ontwikkeling voor teams met Azure Dev spaties met de Snelstartgids voor team-ontwikkeling.

> [!div class="nextstepaction"]
> [Team Development quickstart](quickstart-team-development.md)


[how-dev-spaces-works]: how-dev-spaces-works.md
[team-development-quickstart]: quickstart-team-development.md
