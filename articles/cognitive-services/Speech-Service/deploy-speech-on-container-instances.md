---
title: Azure Container Instances uitvoeren
titleSuffix: Azure Cognitive Services
description: De spraak-Service-container implementeren in Azure Container Instances en test in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 062765be22135b12abb29ff6f7ce8a772c67adae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711514"
---
# <a name="deploy-the-speech-service-container-to-azure-container-instances"></a>De spraak-container implementeren in Azure Container Instances

Informatie over het implementeren van de Cognitive Services [Spraakservice](speech-container-howto.md) container naar Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/). Deze procedure ziet u het maken van een Azure-Service voor spraak-resource. Vervolgens we het binnenhalen van de bijbehorende containerinstallatiekopie bespreken. Ten slotte, selecteren we de mogelijkheid om uit te oefenen de indeling van de twee vanuit een browser. Met behulp van containers, kan de ontwikkelaar aandacht weg van beheer van infrastructuur in plaats daarvan gericht op de ontwikkeling van toepassingen verplaatsen.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Aanvraag voor toegang tot de container registry

U moet eerst invullen en verzenden de [Cognitive Services spraak Containers aanvraagformulier](https://aka.ms/speechcontainerspreview/) toegang vragen tot de container. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Speech Service resource](includes/create-speech-resource.md)]

[!INCLUDE [Create an Speech Service container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
