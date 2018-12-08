---
title: Vooraf gedefinieerde intents
titleSuffix: Azure Cognitive Services
description: LUIS bevat een set vooraf gedefinieerde intents voor het snel toevoegen van algemene, gesprekken gebruikersscenario's.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 10/18/2018
ms.author: diberry
ms.openlocfilehash: 8fddbcf82bdbb052468b97754554da01bac7d82b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53103724"
---
# <a name="add-prebuilt-intents-for-common-intents"></a>Vooraf gedefinieerde intents voor algemene intents toevoegen 

LUIS bevat een set vooraf gedefinieerde intents van de vooraf gemaakte domeinen voor het snel toevoegen van algemene intents en uitingen. Dit is een snelle en eenvoudige manier om mogelijkheden toevoegen aan uw eigen client-app zonder te hoeven ontwerpen van de modellen voor deze mogelijkheden. 

## <a name="add-a-prebuilt-intent"></a>Een vooraf gedefinieerde doel toevoegen

1. Op de **mijn Apps** pagina, selecteert u uw app. Hiermee opent u uw app naar de **bouwen** sectie van de app. 

1. Op de **Intents** weergeeft, schakelt **vooraf gedefinieerde doel toevoegen** via de werkbalk boven de lijst intents. 

1. Selecteer de **Utilities.Cancel** intentie van het pop-updialoogvenster. 

    ![Vooraf gedefinieerde doel toevoegen](./media/luis-prebuilt-intents/prebuilt-intents-ddl.png)

1. Selecteer de **gedaan** knop.

## <a name="train-and-test"></a>Trainen en testen

1. Nadat de bedoeling is toegevoegd, de app trainen door te selecteren **trainen** rechts in de bovenste werkbalk. 

1. De nieuwe doel testen door te selecteren **Test** in de werkbalk rechts. 

1. Voer in het tekstvak uitingen voor annuleren:

    |Test utterance|Voorspellingsscore|
    |--|:--|
    |Ik wil mijn vertragingen van vluchten te annuleren.|0.67|
    |De aankoop annuleren.|0.52|
    |Annuleren van de vergadering.|0.56|

    ![Vooraf gedefinieerde doel testen](./media/luis-prebuilt-intents/test.png)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vooraf gemaakte entiteiten](./luis-prebuilt-entities.md)