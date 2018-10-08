---
title: JavaScript-snelstart - Intentie voorspellen - LUIS
titleSuffix: Azure Cognitive Services
description: In deze snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst. Gebruik JavaScript om de intentie van de gebruiker als tekst naar het HTTP-voorspellingseindpunt van de openbare app te verzenden. Bij het eindpunt wordt via LUIS het model van de openbare app toegepast om de betekenis van tekst in natuurlijke taal te analyseren. Hiermee wordt de algehele intentie bepaald en worden gegevens geëxtraheerd die relevant zijn voor het onderwerpdomein van de app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: cbbf0d2388f83f4e38f0a63b6bc181d4a24e0aa0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039477"
---
# <a name="quickstart-get-intent-using-javascript"></a>Snelstart: intentie ophalen met JavaScript

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="prerequisites"></a>Vereisten

* [Visual Studio Code](https://code.visualstudio.com/)
* Id van openbare app: df67dcdb-c37d-46af-88e1-8b97951ca1c2


## <a name="get-luis-key"></a>LUIS-sleutel ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>De intentie via een browser ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>De intentie programmatisch ophalen

U kunt JavaScript gebruiken voor toegang tot dezelfde resultaten die u in het browservenster in de vorige stap hebt gezien. 

1. Kopieer de volgende code en sla deze op in een HTML-bestand:

   [!code-html[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/javascript/call-endpoint.html)]

2. Open het bestand in een browser. Voer de LUIS-eindpuntsleutel in het formulier in en selecteer **Verzenden**.

    ![HTML-voorbeeld weergegeven in de browser met LUIS-resultaten voor de app Huisautomatisering](./media/luis-get-started-js-get-intent/html-results.png)

    Het resultaat wordt weergegeven onder het formulier. 

## <a name="luis-keys"></a>LUIS-sleutels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder het JavaScript-bestand.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-javascript-add-utterance.md)
