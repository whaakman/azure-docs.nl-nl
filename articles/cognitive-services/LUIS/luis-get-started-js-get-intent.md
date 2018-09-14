---
title: Tekst in natuurlijke taal analyseren in LUIS (Language Understanding) met JavaScript- Cognitive Services - Azure Cognitive Services | Microsoft Docs
description: In deze snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst. Gebruik JavaScript om de intentie van de gebruiker als tekst naar het HTTP-voorspellingseindpunt van de openbare app te verzenden. Bij het eindpunt wordt via LUIS het model van de openbare app toegepast om de betekenis van tekst in natuurlijke taal te analyseren. Hiermee wordt de algehele intentie bepaald en worden gegevens geëxtraheerd die relevant zijn voor het onderwerpdomein van de app.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 10210c3759611a77c4430a97896a19a6b97b9fa9
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "43769979"
---
# <a name="quickstart-analyze-text-using-javascript"></a>Snelstart: tekst analyseren met JavaScript

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Visual Studio Code](https://code.visualstudio.com/)
* Id van openbare app: df67dcdb-c37d-46af-88e1-8b97951ca1c2


## <a name="get-luis-key"></a>LUIS-sleutel ophalen

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Tekst analyseren met browser

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-javascript"></a>Tekst analyseren met JavaScript 

U kunt JavaScript gebruiken voor toegang tot dezelfde resultaten die u in het browservenster in de vorige stap hebt gezien. 

1. Kopieer de volgende code en sla deze op in een HTML-bestand:

   [!code-html[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/javascript/call-endpoint.html)]

2. Open het bestand in een browser. Voer de LUIS-eindpuntsleutel in het formulier in en selecteer **Verzenden**.

    ![HTML-voorbeeld weergegeven in de browser met LUIS-resultaten voor de app Huisautomatisering](./media/luis-get-started-js-get-intent/html-results.png)

    Het resultaat wordt weergegeven onder het formulier. 

## <a name="luis-keys"></a>LUIS-sleutels

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder het JavaScript-bestand.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-javascript-add-utterance.md)
