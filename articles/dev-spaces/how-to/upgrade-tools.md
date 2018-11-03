---
title: Upgrade uitvoeren van hulpprogramma's voor Azure Dev spaties | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: iainfoulds
ms.author: iainfou
ms.date: 07/03/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure-Containerservice, containers
ms.openlocfilehash: 61e59abde2c3a06b8cf650306b67ac5142729a8c
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50976933"
---
# <a name="how-to-upgrade-azure-dev-spaces-tools"></a>Upgrade uitvoeren van hulpprogramma's voor Azure Dev spaties

Als er een nieuwe versie en u al Azure Dev opslagruimten gebruikt, moet u mogelijk upgraden van uw Azure Dev spaties clienthulpprogramma's.

## <a name="update-the-azure-cli"></a>Bijwerken van de Azure CLI

Als u de nieuwste Azure CLI hebt bijgewerkt, krijgt u ook de meest recente versie van de Dev spaties CLI-extensie.

U hoeft niet te verwijderen van de vorige versie, de juiste downloaden op net vinden [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).


## <a name="update-the-dev-spaces-cli-extension-and-command-line-tools"></a>Bijwerken van de Dev spaties CLI-extensie en de opdrachtregelprogramma 's

Voer de volgende opdracht uit:

```cmd
az aks use-dev-spaces -n <your-aks-cluster> -g <your-aks-cluster-resource-group> --update
```

## <a name="update-the-vs-code-extension"></a>Bijwerken van de VS Code-extensie

Na de installatie wordt de extensie automatisch bijgewerkt. U moet mogelijk opnieuw laden van de extensie voor het gebruik van de nieuwe functies. Open in Visual Studio Code, de **extensies** in het deelvenster kiest u de **Azure Dev spaties** extensies, en kies **opnieuw laden**.

## <a name="update-the-visual-studio-extension"></a>De Visual Studio-extensie bijwerken

Zoals met andere extensies en updates, Visual Studio wordt een melding wanneer er is een update beschikbaar voor de Visual Studio Tools voor Kubernetes, waaronder Azure Dev spaties. Zoek naar een vlagpictogram in de rechterbovenhoek van het scherm.

Voor het bijwerken van de hulpprogramma's in Visual Studio, kiest u de **Extra > extensies en Updates** menu item en aan de linkerkant, kies **Updates**. Zoek **Visual Studio-hulpprogramma's voor Kubernetes** en kies de **Update** knop.

## <a name="next-steps"></a>Volgende stappen

De nieuwe hulpprogramma's testen door een nieuw cluster maken. Probeer de snelstartgidsen en zelfstudies op [Azure Dev spaties](/azure/dev-spaces).

> [!WARNING]
> Azure Dev spaties in bestaande clusters wordt niet onmiddellijk de matrixeigenschap, zodat om er zeker van zijn u de meest recente versie gebruikt op al uw Azure-implementaties, een nieuw cluster maken na de upgrade van de hulpprogramma's.
