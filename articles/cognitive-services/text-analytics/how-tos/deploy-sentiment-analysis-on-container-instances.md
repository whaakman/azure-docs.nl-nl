---
title: Azure Container Instances uitvoeren
titleSuffix: Azure Cognitive Services
description: De text analytics-containers met de installatiekopie van het sentiment-analyse implementeren voor de Azure Container Instances en test in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 9f174d54fcc74eed613eb69412bc0e515f15897b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711723"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances"></a>Een Sentimentanalyse-container implementeren in Azure Container Instances

Informatie over het implementeren van de Cognitive Services [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) container met de installatiekopie van het Sentimentanalyse naar Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/). Deze procedure verklaart het maken van een Text Analytics-resource, het maken van een bijbehorende Sentimentanalyse-installatiekopie en de mogelijkheid om uit te oefenen deze indeling van de twee vanuit een browser. Met behulp van containers, kan de ontwikkelaar aandacht weg van beheer van infrastructuur in plaats daarvan gericht op de ontwikkeling van toepassingen verplaatsen.

## <a name="prerequisites"></a>Vereisten

* Gebruik een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](../../containers/includes/create-container-instances-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Volgende stappen 

* Meer [Cognitive Services-Containers](../../cognitive-services-container-support.md)
* Gebruik de [Tekstanalyse verbonden Service](../vs-text-connected-service.md)
