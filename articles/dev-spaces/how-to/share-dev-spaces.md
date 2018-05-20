---
title: Het delen van Azure Dev spaties | Microsoft Docs
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
ms.openlocfilehash: 140a487a0a605e0f03122868af0d2d12bcdba35b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="share-azure-dev-spaces"></a>Azure Dev spaties delen

Met Azure Dev spaties, kunt u uw space dev met anderen in uw team delen. Elke ontwikkelaar kunt werken in hun eigen ruimte geen last van anderen op te splitsen. Bovendien kunt werken samen in één spatie kunt u code end-to-end testen zonder te hoeven maken mocks of afhankelijkheden simuleren. Zie de [meer informatie over het ontwikkelen van een team](../get-started-nodejs.md#learn-about-team-development) guide voor meer informatie.

Instellen van een omgeving voor meerdere ontwikkelaars:
1. Maak een spatie Dev in Azure. Kies [.NET Core en Code van de VS](../get-started-netcore.md), [.NET Core en Visual Studio](../get-started-netcore-visualstudio.md), of [Node.js en Code van de VS](../get-started-nodejs.md). U moet de eigenaar of bijdrager toegang hebben tot de geselecteerde Azure-abonnement.
1. Configureren van de Azure Dev-ruimte **resourcegroep** naar [Inzender toegang verlenen](/azure/active-directory/role-based-access-control-configure) voor elk teamlid. Met deze opdracht kunt u de resourcegroep een omgeving controleren: `azds resource list`
1. Vraag teamleden **Selecteer de omgeving** om deze ontwikkelen.
     * **Vanaf de opdrachtregel of Code van de VS**: bestaande Azure Dev Spacess zien die u hebt toegang tot: `azds resource list`. Selecteer een omgeving: `azds resource select`.
     * **Visual Studio IDE**: Open een project in Visual Studio, selecteer **Azure Dev spaties** van de lancering instellingen vervolgkeuzelijst. Selecteer een bestaande ontwikkelomgeving in het dialoogvenster dat wordt geopend.

![Visual Studio starten instellingen vervolgkeuzelijst](../media/get-started-netcore-visualstudio/LaunchSettings.png)