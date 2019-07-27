---
title: Azure Container Instances uitvoeren-Text Analytics
titleSuffix: Azure Cognitive Services
description: Implementeer de tekst analyse-containers met de sentiment-analyse afbeelding, naar de Azure-container instantie en test deze in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 9ef529c9d505e5b305602c80a8dbef906f52269c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552532"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances"></a>Een Sentimentanalyse-container implementeren op Azure Container Instances

Meer informatie over het implementeren van de Cognitive Services [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) -container met de sentimentanalyse installatie kopie naar Azure [container instances](https://docs.microsoft.com/azure/container-instances/). Met deze procedure wordt het maken van een Text Analytics resource, het maken van een gekoppelde Sentimentanalyse installatie kopie en de mogelijkheid om deze indeling van de twee vanuit een browser uit te kunnen oefenen, exemplifies. Door gebruik te maken van containers kan de aandacht van de ontwikkel aars van de infra structuur afnemen in plaats van de ontwikkeling van toepassingen te richten.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement gebruiken. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Volgende stappen 

* Meer [Cognitive Services containers](../../cognitive-services-container-support.md) gebruiken
* De [Text Analytics verbonden service](../vs-text-connected-service.md) gebruiken
