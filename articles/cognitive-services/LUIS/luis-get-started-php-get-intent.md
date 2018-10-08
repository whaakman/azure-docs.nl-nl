---
title: PHP-snelstart - Intentie voorspellen - LUIS
titleSuffix: Azure Cognitive Services
description: In deze snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst. Gebruik PHP om de intentie van de gebruiker als tekst naar het HTTP-voorspellingseindpunt van de openbare app te verzenden. Bij het eindpunt wordt via LUIS het model van de openbare app toegepast om de betekenis van tekst in natuurlijke taal te analyseren. Hiermee wordt de algehele intentie bepaald en worden gegevens geëxtraheerd die relevant zijn voor het onderwerpdomein van de app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 87c37de611056c5672ed0910afdd344325a0c6f7
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036892"
---
# <a name="quickstart-get-intent-using-php"></a>Snelstart: intentie ophalen met PHP

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Vereisten

* [PHP](http://php.net/)-programmeertaal
* [Visual Studio Code](https://code.visualstudio.com/)
* Id van openbare app: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS-sleutel ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>De intentie via een browser ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>De intentie programmatisch ophalen 

U kunt PHP gebruiken voor toegang tot de resultaten die u in het browservenster in de vorige stap hebt gezien. 

1. Kopieer de volgende code en sla deze op in een bestandsnaam `endpoint-call.php`:

   [!code-php[PHP code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/php/endpoint-call.php)]

2. Vervang `"YOUR-KEY"` door uw eindpuntsleutel.

3. Voer de PHP-toepassing uit met `php endpoint-call.php`. De uitvoer bestaat uit de JSON die u eerder hebt gezien in het browservenster.

## <a name="luis-keys"></a>LUIS-sleutels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder het PHP-bestand.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-php-add-utterance.md)