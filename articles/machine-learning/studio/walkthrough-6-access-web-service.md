---
title: 'Stap 6: Toegang tot de Machine Learning-webservice | Microsoft Docs'
description: 'Stap 6 van het ontwikkelen van een overzicht van de voorspellende oplossing: toegang tot een actieve Azure Machine Learning-webservice.'
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: d1a31bc95b249c3fa1caced138bbf3459eb6282c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Kennismaken, stap 6: Toegang tot de Azure Machine Learning-webservice

Dit is de laatste stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. **Toegang tot de webservice**

- - -
In de vorige stap in dit scenario wordt een webservice die gebruikmaakt van onze voorspelling van tegoed risicomodel geïmplementeerd. Nu zich kunnen gebruikers voor het gegevens naar verzenden en ontvangen van resultaten. 

De webservice is een Azure-web-service die u kunt ontvangen en retourneren van gegevens met behulp van REST-API's op twee manieren:  

* **Aanvragen/reacties** : de gebruiker een of meer rijen tegoed gegevens naar de service verzonden via een HTTP-protocol en de service reageert met een of meer sets van resultaten.
* **Batchuitvoering** : de gebruiker wordt opgeslagen in een of meer rijen tegoed gegevens in een Azure blob- en stuurt vervolgens de bloblocatie naar de service. De service alle rijen van de gegevens in de blob-invoerbron scores, slaat de resultaten in een andere blob en retourneert de URL van die container.  

De snelste en gemakkelijkste manier toegang krijgen tot een webservice klassieke is via de [Web-App voor Azure ML aanvragen en antwoorden Service](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) of [Azure ML Batch uitvoering Web App servicesjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Deze web-app-sjablonen kunnen maken van een aangepaste web-app die u kent uw webservice invoergegevens en wat wordt geretourneerd. U hoeft te doen is bieden toegang tot uw web-service en -gegevens en doet de rest van de sjabloon.

Zie voor meer informatie over het gebruik van de web-app sjablonen [een webservice Azure Machine Learning met een web-app-sjabloon gebruiken](consume-web-service-with-web-app-template.md).

U kunt ook een aangepaste toepassing voor toegang tot de webservice met starter code voorzien in R, C# en Python programmeertalen ontwikkelen.

U vindt meer informatie in [gebruiken van een Azure Machine Learning-webservice](consume-web-services.md).

