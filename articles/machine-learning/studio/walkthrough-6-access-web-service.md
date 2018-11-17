---
title: 'Stap 6: Toegang tot de Machine Learning-webservice | Microsoft Docs'
description: 'Stap 6 van het ontwikkelen van een overzicht van de voorspellende oplossing: toegang tot een active Azure Machine Learning-webservice.'
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.custom: (previous ms.author yahajiza)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: abb94165d08a9f4e45924d71ed11a088d662339e
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51822134"
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Kennismaken, stap 6: Toegang tot de Azure Machine Learning-webservice

Dit is de laatste stap van de procedure [predictive analytics-oplossing in Azure Machine Learning ontwikkelen](walkthrough-develop-predictive-solution.md)

1. [Een Machine Learning-werkruimte maken](walkthrough-1-create-ml-workspace.md)
2. [Bestaande gegevens uploaden](walkthrough-2-upload-data.md)
3. [Een nieuw experiment maken](walkthrough-3-create-new-experiment.md)
4. [De modellen trainen en evalueren](walkthrough-4-train-and-evaluate-models.md)
5. [De webservice implementeren](walkthrough-5-publish-web-service.md)
6. **De webservice openen**

- - -
In de vorige stap in dit scenario wordt een webservice die gebruikmaakt van ons tegoed risico voorspellingsmodel geïmplementeerd. Nu zich kunnen gebruikers voor die gegevens verzenden en ontvangen van resultaten. 

De webservice is een Azure-web-service die kan ontvangen en als resultaat de gegevens met behulp van REST-API's op twee manieren:  

* **Aanvraag/antwoord** : de gebruiker een of meer rijen met tegoed gegevens verzendt naar de service met behulp van een HTTP-protocol en de service reageert met een of meer sets met resultaten.
* **Batchuitvoering** : de gebruiker wordt opgeslagen in een of meer rijen met gegevens van de creditcard in een Azure blob- en verzendt u vervolgens de locatie van de blob naar de service. De service beoordeelt de rijen met gegevens in de blob, worden de resultaten opgeslagen in een andere blob en retourneert de URL van die container.  

De snelste en eenvoudigste manier voor toegang tot een klassieke webservice is via de [Web-App voor Azure ML Request Response Service](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) of [Azure ML Batch uitvoering Service Web-Appsjabloon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Deze web-app-sjablonen kunnen maken van een aangepaste web-app die u kent uw webservice invoergegevens en wat wordt geretourneerd. U hoeft alleen is toegang bieden tot uw web-service en de gegevens en doet de rest van de sjabloon.

Zie voor meer informatie over het gebruik van de web-appsjablonen [een Azure Machine Learning-webservice met een web-appsjabloon gebruiken](consume-web-service-with-web-app-template.md).

U kunt ook een aangepaste toepassing toegang tot de webservice met behulp van starter-code die u in R, ontwikkelen C#, en Python-programmeertalen.

U vindt meer informatie in [hoe u een Azure Machine Learning-webservice gebruiken](consume-web-services.md).

