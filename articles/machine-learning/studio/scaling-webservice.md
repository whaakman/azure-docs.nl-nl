---
title: Schalen van een Machine Learning Studio-webservice - Azure | Microsoft Docs
description: Leer hoe u gelijktijdigheid van een Azure Machine Learning Studio-webservice verbeteren door het toevoegen van extra eindpunten.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18, previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.author: amlstudiodocs
editor: cgronlun
keywords: Azure machine learning-webservices, uitoefening, schaal, eindpunt, gelijktijdigheid
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.component: studio
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.openlocfilehash: 736c27f5c8ad6efb2b4e36f44998ce06d7b69248
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256346"
---
# <a name="scaling-an-azure-machine-learning-studio-web-service-by-adding-additional-endpoints"></a>Een Azure Machine Learning Studio-webservice schalen door extra eindpunten toe te voegen
> [!NOTE]
> Dit onderwerp wordt beschreven technieken die van toepassing op een **klassieke** Machine Learning-webservice. 
> 
> 

Standaard is elke gepubliceerde webservice is geconfigureerd voor ondersteuning van 20 gelijktijdige aanvragen, en mag wel 200 gelijktijdige aanvragen. Azure Machine Learning optimaliseert automatisch de instelling voor de beste prestaties voor uw webservice en de portal waarde wordt genegeerd. 

Als u wilt de API met een hogere belasting dan een waarde voor maximum aantal gelijktijdige aanroepen van 200 aanroepen ondersteunt, moet u meerdere eindpunten maken van de dezelfde Web-service. U kunt uw load willekeurig distribueren op alle.

De schaal van een webservice is een veelvoorkomende taak. Er zijn enkele redenen om te schalen naar meer dan 200 gelijktijdige aanvragen ondersteunen, hogere mate van beschikbaarheid via meerdere eindpunten of het bieden van afzonderlijke eindpunten voor de webservice. U kunt de schaal vergroten door het toevoegen van extra eindpunten voor de dezelfde webservice via de [Azure Machine Learning-webservice](https://services.azureml.net/) portal.

Zie voor meer informatie over het toevoegen van nieuwe eindpunten [eindpunten maken](create-endpoint.md).

Houd er rekening mee dat met behulp van een aantal hoge gelijktijdigheid schadelijk als u niet van de API met een overeenkomstige hoge frequentie aanroepen bent. U ziet er kunnen sporadisch time-outs en/of pieken in de latentie als u een relatief lage belasting voor een API die is geconfigureerd voor hoge belasting.

De synchrone API's worden meestal gebruikt in situaties waar een lage latentie is vereist. Hier latentie betekent de tijd die nodig is voor de API om een aanvraag te voltooien en niet wordt gehouden met eventuele vertragingen in het netwerk. Stel dat u hebt een API met een latentie 50 ms. Volledig gebruiken voor de beschikbare capaciteit met toevoerregelniveau hoog en het maximum aantal gelijktijdige aanroepen = 20, moet u deze API 20 * 1000 aanroepen / 50 = 400 keer per seconde. Een maximum aantal gelijktijdige aanroepen van 200 kunt dit verder uitgebreid, u voor het aanroepen van de API 4000 mislukte opvragingen per seconde, ervan uitgaande dat het een 50 ms latentie.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
