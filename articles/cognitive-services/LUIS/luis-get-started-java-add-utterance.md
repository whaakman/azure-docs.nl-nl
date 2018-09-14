---
title: Snelstart Model wijzigen en LUIS-app trainen met behulp van Java - Azure Cognitive Services | Microsoft Docs
description: Voeg in deze Java-snelstart voorbeeldutterances toe aan een Home Automation-app en train de app. Voorbeeldutterances zijn teksten uit gesprekken met gebruikers die worden toegewezen aan een intentie. Door voorbeeldutterances op te geven voor intenties leert u LUIS welke soorten door de gebruiker geleverde tekst bij welke intentie horen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: 2554854507d127a7cf3ce016ed38310049b2c958
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43770075"
---
# <a name="quickstart-change-model-using-java"></a>Snelstart: Model wijzigen met behulp van Java 

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Vereisten

[!include[Quickstart prerequisites for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [JDK SE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Java Development Kit, Standard Edition)
* [De GSON JSON-bibliotheek van Google](https://github.com/google/gson).

[!include[Quickstart note about code repository](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>JSON-bestand met voorbeeldutterances

[!include[Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Snelstartcode maken

1. Voeg de Java-afhankelijkheden toe aan het bestand `AddUtterances.java`.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=23-26 "Java Dependencies")]

2. Maak de klasse `AddUtterances`. Deze klasse bevat alle codefragmenten die volgen.

    ```Java
    public class AddUtterances {
        // Insert code here
    }
    ```

3. Voeg de LUIS-constanten toe aan de klasse. Kopieer de volgende code en wijzig deze met uw creatiecode, toepassings-id en versie-id.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=33-44 "LUIS-based IDs")]

4. Voeg de in de LUIS-API aan te roepen methode toe aan de klasse AddUtterances. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=46-168 "HTTP request to LUIS")]

5. Voeg de methode voor het HTTP-antwoord van de LUIS-API toe aan de klasse AddUtterances.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=170-202 "HTTP response from LUIS")]

6. Voeg uitzonderingsafhandeling toe aan de klasse AddUtterances. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=205-243 "Exception Handling")]

7. Voeg de main-functie toe aan de klasse AddUtterances.

   [!code-java[Add main function](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=245-278 "Add main function")]

## <a name="build-code"></a>Code bouwen

AddUtterance compileren met de afhankelijkheden

```CMD
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

## <a name="run-code"></a>Code uitvoeren
Het aanroepen van `AddUtterance` zonder argumenten voegt de LUIS-utterances aan de app toe, zonder deze te trainen.

```CMD
> java -classpath .;gson-2.8.2.jar AddUtterances
```

Deze opdrachtregel toont de resultaten van het aanroepen van de API voor het toevoegen van utterances. 

[!include[Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Resources opschonen
Verwijder wanneer u klaar bent met de snelstart alle bestanden die in de snelstart zijn gemaakt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Programmatisch een LUIS-app bouwen](luis-tutorial-node-import-utterances-csv.md) 