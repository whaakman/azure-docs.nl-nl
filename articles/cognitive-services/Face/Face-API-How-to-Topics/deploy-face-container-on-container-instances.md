---
title: Azure Container Instances uitvoeren
titleSuffix: Azure Cognitive Services
description: De Face-container implementeren in Azure Container Instances en test in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: e67846b6b304b5425f7e8334eb3a4499a029d5ab
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711591"
---
# <a name="deploy-the-face-container-to-azure-container-instances"></a>De Face-container implementeren in Azure Container Instances

Informatie over het implementeren van de Cognitive Services [Face](../face-how-to-install-containers.md) container naar Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/). Deze procedure ziet u het maken van een Azure-Face-resource. Vervolgens we het binnenhalen van de bijbehorende containerinstallatiekopie bespreken. Ten slotte, selecteren we de mogelijkheid om uit te oefenen de indeling van de twee vanuit een browser. Met behulp van containers, kan de ontwikkelaar aandacht weg van beheer van infrastructuur in plaats daarvan gericht op de ontwikkeling van toepassingen verplaatsen.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

[!INCLUDE [Request access to private container registry](../../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Create a Cognitive Services Face resource](../includes/create-face-resource.md)]

[!INCLUDE [Create an Face container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]