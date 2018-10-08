---
title: Java-snelstart - Intentie voorspellen - LUIS
titleSuffix: Azure Cognitive Services
description: In deze snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst. Gebruik Java om de intentie van de gebruiker als tekst naar het HTTP-voorspellingseindpunt van de openbare app te verzenden. Bij het eindpunt wordt via LUIS het model van de openbare app toegepast om de betekenis van tekst in natuurlijke taal te analyseren. Hiermee wordt de algehele intentie bepaald en worden gegevens geëxtraheerd die relevant zijn voor het onderwerpdomein van de app.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 03fd376e58062ae9b4593fcaf7904200157c4dc3
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031421"
---
# <a name="quickstart-get-intent-using-java"></a>Snelstart: intentie ophalen met Java

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Vereisten

* [JDK SE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Java Development Kit, Standard Edition)
* [Visual Studio Code](https://code.visualstudio.com/)
* Id van openbare app: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS-sleutel ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>De intentie via een browser ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>De intentie programmatisch ophalen 

U kunt Java gebruiken voor toegang tot de dezelfde resultaten die u in het browservenster in de vorige stap hebt gezien. 

1. Kopieer de volgende code om een klasse te maken in een bestand met de naam `LuisGetRequest.java`:

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. Vervang de waarde van de variabele `YOUR-KEY` door de LUIS-sleutel.

3. Compileer het Java-programma met `javac -cp ":lib/*" LuisGetRequest.java`. 

4. Voer de toepassing uit met `java -cp ":lib/*" LuisGetRequest.java`. De uitvoer bestaat uit de JSON die u eerder hebt gezien in het browservenster.

    ![In het consolevenster wordt het JSON-resultaat van LUIS weergegeven](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>LUIS-sleutels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder het Java-bestand. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Utterances toevoegen](luis-get-started-java-add-utterance.md)