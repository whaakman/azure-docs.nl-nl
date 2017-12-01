---
title: Het verhogen van gelijktijdigheid van een Azure Machine Learning-webservice | Microsoft Docs
description: Informatie over het verbeteren van gelijktijdigheid van een Azure Machine Learning-webservice door het toevoegen van extra eindpunten.
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: Azure machine learning-webservices, uitoefening, schaal, eindpunt, gelijktijdigheid van taken
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: neerajkh
ms.openlocfilehash: 2b4fcef51b2704f07f5d1d08a4bd16970864b0fd
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Een Azure Machine Learning-webservice schalen door extra eindpunten toe te voegen
> [!NOTE]
> Dit onderwerp wordt beschreven technieken die van toepassing op een **klassieke** Machine Learning-webservice. 
> 
> 

Standaard elke gepubliceerde Web-service is geconfigureerd voor ondersteuning van 20 gelijktijdige aanvragen en mag maximaal 200 gelijktijdige aanvragen. Azure Machine Learning optimaliseert automatisch de instelling voor de beste prestaties voor uw webservice en de portal waarde wordt genegeerd. 

Als u wilt de API met een hogere belasting dan een waarde van het maximumaantal gelijktijdige aanroepen van 200 aanroepen ondersteunt, moet u meerdere eindpunten maken op dezelfde webservice. U kunt uw load willekeurig distribueren voor alle hiervan.

De schaal van een webservice is een algemene taak. Enkele redenen om te schalen zijn voor ondersteuning van meer dan 200 gelijktijdige aanvragen, beschikbaarheid dankzij meerdere eindpunten te verhogen of afzonderlijke eindpunten voor de webservice leveren. U kunt de schaal verhogen door toe te voegen extra eindpunten voor dezelfde webservice via de [Azure Machine Learning-webservice](https://services.azureml.net/) portal.

Zie voor meer informatie over het toevoegen van nieuwe eindpunten [eindpunten maken](create-endpoint.md).

Houd er rekening mee dat met een aantal hoge gelijktijdigheid schadelijk als u de API met een navenant hoge frequentie niet bellen. U ziet sporadisch time-outs en/of pieken in de latentie als u een relatief lage belasting voor een API die is geconfigureerd voor hoge belasting.

De synchrone API's worden meestal gebruikt in situaties waarin een lage latentie gewenst is. Hier latentie betekent de tijd die nodig is voor de API om een aanvraag te voltooien en account komt niet voor alle vertragingen in het netwerk. Stel dat u hebt een API met een latentie van 50-ms. Volledig gebruiken voor de beschikbare capaciteit met vertraging niveau Hoog en het maximumaantal gelijktijdige aanroepen = 20, moet u deze API 20 * 1000 aan te roepen / 50 = 400 keren per seconde. Een maximumaantal gelijktijdige aanroepen van 200 kunt dit verder uitbreiden, u de API 4000 keren per seconde, ervan uitgaande dat een 50 ms latentie aanroepen.

<!--Image references-->
[1]: ./media/scaling-webservice/machlearn-1.png
[2]: ./media/scaling-webservice/machlearn-2.png
