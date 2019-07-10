---
title: Azure Container Instances uitvoeren
titleSuffix: Azure Cognitive Services
description: Het formulier herkenning-container implementeren in Azure Container Instances en test in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 3c424465678a9989940d92910c5d288fa2fb1cab
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711393"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Het formulier herkenning-container implementeren in Azure Container Instances

Informatie over het implementeren van de Cognitive Services [formulier herkenning](form-recognizer-container-howto.md) container naar Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/). Deze procedure ziet u het maken van een formulier herkenning van Azure-resource. Vervolgens we het binnenhalen van de bijbehorende containerinstallatiekopie bespreken. Ten slotte, selecteren we de mogelijkheid om uit te oefenen de indeling van de twee vanuit een browser. Met behulp van containers, kan de ontwikkelaar aandacht weg van beheer van infrastructuur in plaats daarvan gericht op de ontwikkeling van toepassingen verplaatsen.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Aanvraag voor toegang tot de container registry

U moet eerst invullen en verzenden de [Cognitive Services formulier herkenning Containers toegang krijgen tot aanvraagformulier](https://aka.ms/FormRecognizerRequestAccess) toegang vragen tot de container. In dat geval ook ondertekent u voor Computer Vision. U hoeft niet te registreren voor het aanvraagformulier voor de Computer Vision-afzonderlijk. 

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
