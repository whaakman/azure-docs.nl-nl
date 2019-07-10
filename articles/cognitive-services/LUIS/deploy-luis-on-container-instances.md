---
title: Azure Container Instances uitvoeren
titleSuffix: Azure Cognitive Services
description: De LUIS-container implementeren in Azure Container Instances en test in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 1d19d80bbc334a376f77eb285349fb1a87a91a54
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711569"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>De Language Understanding (LUIS)-container implementeren in Azure Container Instances

Informatie over het implementeren van de Cognitive Services [LUIS](luis-container-howto.md) container naar Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/). Deze procedure ziet u het maken van een bron voor de detectie van afwijkingen. Vervolgens we het binnenhalen van de bijbehorende containerinstallatiekopie bespreken. Ten slotte, selecteren we de mogelijkheid om uit te oefenen de indeling van de twee vanuit een browser. Met behulp van containers, kan de ontwikkelaar aandacht weg van beheer van infrastructuur in plaats daarvan gericht op de ontwikkeling van toepassingen verplaatsen.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Create LUIS Container Instance resource](../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
