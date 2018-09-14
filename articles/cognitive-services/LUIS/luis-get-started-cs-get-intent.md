---
title: Tekst in natuurlijke taal analyseren in Language Understanding (LUIS) met C# - Azure Cognitive Services | Microsoft Docs
description: In deze snelstart gebruikt u een beschikbare openbare LUIS-app om de intentie van een gebruiker te bepalen aan de hand van beschrijvende tekst. Gebruik C# om de intentie van de gebruiker als tekst naar het HTTP-voorspellingseindpunt van de openbare app te verzenden. Bij het eindpunt past LUIS het model van de openbare app toe om de betekenis van tekst in natuurlijke taal te analyseren. Hiermee wordt de algehele intentie bepaald en worden gegevens geëxtraheerd die relevant zijn voor het onderwerpdomein van de app.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: b6ddd48fc6bfa5c099e42f3717a2113f871b4f9a
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163257"
---
# <a name="quickstart-analyze-text-using-c"></a>Snelstart: Tekst analyseren met C#

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Vereisten

* [Visual Studio Community 2017-editie](https://visualstudio.microsoft.com/vs/community/)
* C#-programmeertaal (meegeleverd met VS Community 2017)
* Id van openbare app: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>LUIS-sleutel ophalen

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Tekst analyseren met browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-c"></a>Tekst analyseren met behulp van C# 

Gebruik C# om een query uit te voeren voor de GET [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) van het voorspellingseindpunt om dezelfde resultaten te behalen als die u kreeg in het browservenster in het vorige gedeelte. 

1. Maak in Visual Studio een nieuwe consoletoepassing. 

    ![Toegang tot menu voor LUIS-gebruikersinstellingen](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. Selecteer de optie **Referentie toevoegen** in het Visual Studio-project in de Solutions Explorer en selecteer vervolgens **System.Web** op het tabblad Assembly's.

    ![Toegang tot menu voor LUIS-gebruikersinstellingen](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Overschrijf Program.cs met de volgende code:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Vervang de waarde van `YOUR_KEY` door uw LUIS-sleutel.

5. Bouw en voer de consoletoepassing uit. De uitvoer bestaat uit de JSON die u eerder hebt gezien in het browservenster.

    ![In het consolevenster wordt het JSON-resultaat van LUIS weergegeven](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>LUIS-sleutels

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met deze snelstart, sluit u het Visual Studio-project en verwijdert u de projectmap uit het bestandssysteem. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Utterances toevoegen en trainen met C#](luis-get-started-cs-add-utterance.md)