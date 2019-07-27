---
title: Azure Container Instances-Speech-Service uitvoeren
titleSuffix: Azure Cognitive Services
description: Implementeer de speech service-container in een Azure-container exemplaar en test deze in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 41589a12f16f330188e2971e3069eb715d1a7163
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559712"
---
# <a name="deploy-the-speech-service-container-to-azure-container-instances"></a>De speech service-container implementeren op Azure Container Instances

Meer informatie over het implementeren van de Cognitive Services [Speech Service](speech-container-howto.md) -container in azure [container instances](https://docs.microsoft.com/azure/container-instances/). Deze procedure illustreert het maken van een Azure Speech Service-Resource. Vervolgens bespreken we het verzamelen van de bijbehorende container installatie kopie. Ten slotte markeren we de mogelijkheid om de indeling van de twee uit een browser uit te oefenen. Door gebruik te maken van containers kan de aandacht van de ontwikkel aars van de infra structuur afnemen in plaats van de ontwikkeling van toepassingen te richten.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Toegang aanvragen tot het container register

U moet eerst het [aanvraag formulier voor de Cognitive Services spraak containers](https://aka.ms/speechcontainerspreview/) volt ooien en verzenden om toegang tot de container aan te vragen. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Speech Service resource](includes/create-speech-resource.md)]

[!INCLUDE [Create an Speech Service container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
