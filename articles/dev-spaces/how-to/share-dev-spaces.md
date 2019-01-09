---
title: Over het delen van Azure Dev Spaces | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 05/11/2018
ms.topic: article
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: 57ca0f7b7704b179a55ac75fea99d35e1024ee8e
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706412"
---
# <a name="share-azure-dev-spaces"></a>Azure Dev Spaces delen

Met Azure Dev Space kunt u uw ontwikkelruimte delen met anderen in uw team. Elke ontwikkelaar kan in zijn eigen ruimte werken zonder angst om die van anderen aan te tasten. Bovendien, door samen te werken in één ruimte kunt u code end-to-end testen zonder mocks te hoeven maken of afhankelijkheden te simuleren. Zie [meer informatie over teamontwikkeling](../team-development-nodejs.md) gids voor meer informatie.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Instellen van een ontwikkelruimte voor meerdere ontwikkelaars

1. Maak een Dev Space in Azure. Kies [.NET Core- en VS Code](../get-started-netcore.md), [.NET Core en Visual Studio](../get-started-netcore-visualstudio.md), of [Node.js en VS Code](../get-started-nodejs.md). U moet als eigenaar of bijdrager toegang hebben tot de geselecteerde Azure-abonnement.
1. Configureer de Azure Dev Space **resourcegroep** voor [medewerker toegang](/azure/active-directory/role-based-access-control-configure) voor elk teamlid. Met deze opdracht kunt u de resourcegroep van een ontwikkelruimte controleren: `azds list-up`
1. Vraag teamleden om de **ontwikkelruimte te selecteren** om erin te ontwikkelen.
     * **Vanaf de opdrachtregel of in VS Code**: om bestaande Azure Dev Spaces waar u toegang toe hebt te zien: `azds space list`. Om een ontwikkelruimte te selecteren: `azds space select`.
     * **Visual Studio IDE**: Open een project in Visual Studio, selecteer **Azure Dev Spaces** in de start-instellingen vervolgkeuzelijst. In het dialoogvenster dat wordt geopend selecteert u een bestaand cluster.

    ![Visual Studio start-instellingen vervolgkeuzelijst](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Volgende stappen

Zie [meer informatie over teamontwikkeling](../team-development-nodejs.md) voor meer informatie.
