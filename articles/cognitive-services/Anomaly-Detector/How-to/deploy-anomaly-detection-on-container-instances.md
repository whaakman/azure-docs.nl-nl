---
title: Azure Container Instances uitvoeren
titleSuffix: Azure Cognitive Services
description: De detectie van afwijkingen-container implementeren in Azure Container Instances en test in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: b5adc3ed9234050d3977e812202717a0ce83e842
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711690"
---
# <a name="deploy-an-anomaly-detector-container-to-azure-container-instances"></a>Een Anomaliedetectie Detector-container implementeren in Azure Container Instances

Informatie over het implementeren van de Cognitive Services [Anomaliedetectie Detector](../anomaly-detector-container-howto.md) container naar Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/). Deze procedure ziet u het maken van een bron voor de detectie van afwijkingen. Vervolgens we het binnenhalen van de bijbehorende containerinstallatiekopie bespreken. Ten slotte, selecteren we de mogelijkheid om uit te oefenen de indeling van de twee vanuit een browser. Met behulp van containers, kan de ontwikkelaar aandacht weg van beheer van infrastructuur in plaats daarvan gericht op de ontwikkeling van toepassingen verplaatsen.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

U moet eerst invullen en verzenden de [Anomaliedetectie Detector Container aanvraagformulier](https://aka.ms/adcontainer) toegang vragen tot de container.

[!INCLUDE [Request access](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

[!INCLUDE [Create an Anomaly Detector container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]