---
title: Azure Dev Spaces | Microsoft Docs
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: tutorial
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 344947b7906d15e819e372e0affe4af3c34ba69b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34198755"
---
# <a name="azure-dev-spaces"></a>Azure Dev Spaces
Azure Dev Spaces stelt u in staat om snel te ontwikkelen met Kubernetes. Met Azure Dev Spaces krijgt u ook de beschikking over uitgebreide ontwikkelmogelijkheden, zoals het opsporen van fouten in Azure Kubernetes-containers. Daarnaast kunt iteratief containers ontwikkelen in de cloud met behulp van bekende tools zoals VS Code, Visual Studio of de opdrachtregel. Azure Dev Spaces is met name handig voor teamontwikkeling, waarbij isolatie van afzonderlijke codevertakkingen in hun eigen ruimten een essentieel onderdeel is van de ontwikkelingscyclus.

## <a name="how-azure-dev-spaces-simplifies-kubernetes-development"></a>Hoe Azure Dev Spaces de ontwikkeling van Kubernetes vereenvoudigt 

Deze aanpak biedt verschillende voordelen:

* U krijgt een ontwikkelomgeving zonder infrastructuur die representatief is voor productie, met volledige toegang tot resources in de cloud.
* U kunt rechtstreeks in Kubernetes fouten opsporen in containers van Node.js en .NET Core met VS Code of Visual Studio. Alle andere talen kunnen worden ontwikkeld met de opdrachtregelinterface.
* U kunt een instantie van Kubernetes delen binnen uw ontwikkelteam om kosten te besparen en de configuratie van lokale computers voor nieuwe teamleden tot een minimum te beperken.
* Ontwikkel uw code in een geïsoleerde omgeving en voer end-to-end tests uit met andere onderdelen zonder dat u afhankelijkheden hoeft te repliceren of na te bootsen.

[!INCLUDE[](includes/get-started.md)]

![](media/azure-dev-spaces/vscode-overview.png)