---
title: Intentie ophalen, Java
titleSuffix: Language Understanding - Azure Cognitive Services
description: In deze Java-snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst.
author: diberry
manager: nitinme
services: cognitive-services
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 02e03868f5a48088b78d5d9b0221387212f248cf
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958708"
---
# <a name="quickstart-get-intent-using-java"></a>Snelstart: Intentie ophalen met behulp van Java

In deze snelstart leest u hoe u utterances doorgeeft aan een LUIS-eindpunt en intenties en entiteiten terugkrijgt.

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Vereisten

* [JDK SE](https://aka.ms/azure-jdks) (Java Development Kit, Standard Edition)
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
