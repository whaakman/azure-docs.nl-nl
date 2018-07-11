---
title: Over het delen van Azure Dev spaties | Microsoft Docs
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
ms.openlocfilehash: 9345015a8732da02ea68711cd807556a543d8d01
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930141"
---
# <a name="share-azure-dev-spaces"></a>Azure Dev spaties delen

Met Azure Dev spaties, kunt u de adresruimte van uw dev met anderen in uw team delen. Elke ontwikkelaar kan werken in hun eigen ruimte zonder angst van anderen te analyseren. Ook kunt werken samen in één ruimte kunt u code end-to-end testen zonder te hoeven maken van mocks of afhankelijkheden simuleren. Zie de [meer informatie over teamontwikkeling](../team-development-nodejs.md) guide voor meer informatie.

## <a name="set-up-a-dev-space-for-multiple-developers"></a>Instellen van een dev-ruimte voor meerdere ontwikkelaars

1. Maak een spatie ontwikkelen in Azure. Kies [.NET Core- en VS Code](../get-started-netcore.md), [.NET Core en Visual Studio](../get-started-netcore-visualstudio.md), of [Node.js en VS Code](../get-started-nodejs.md). U moet eigenaar of bijdrager toegang hebben tot de geselecteerde Azure-abonnement.
1. Configureren van de Azure Dev-ruimte **resourcegroep** naar [medewerker toegang verlenen](/azure/active-directory/role-based-access-control-configure) voor elk teamlid. Met deze opdracht kunt u de resourcegroep een dev-ruimte controleren: `azds list`
1. Vragen teamleden **selecteert u de ruimte dev** om te ontwikkelen in het.
     * **Vanaf de opdrachtregel of in VS Code**: om te zien van bestaande Azure Dev spaties die u hebt toegang tot: `azds space list`. Om te selecteren van een dev-ruimte: `azds space select`.
     * **Visual Studio IDE**: Open een project in Visual Studio, selecteer **Azure Dev spaties** in de starten instellingen vervolgkeuzelijst. In het dialoogvenster dat wordt geopend, selecteert u een bestaand cluster.

    ![Visual Studio starten instellingen vervolgkeuzelijst](../media/get-started-netcore-visualstudio/LaunchSettings.png)

## <a name="next-steps"></a>Volgende stappen

Zie [meer informatie over teamontwikkeling](../team-development-nodejs.md) voor meer informatie.