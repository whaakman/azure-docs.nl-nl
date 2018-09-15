---
title: 'Zelfstudie 5: Pattern.any entiteit voor vrije tekst'
titleSuffix: Azure Cognitive Services
description: De entiteit pattern.any gebruiken om gegevens te extraheren uit uitingen waar de uitingen goed opgemaakte is en waar het einde van de gegevens kan eenvoudig kan worden verward met de resterende woorden van de utterance.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: dce75710137f4d4160cb2f55f856066c7c93ac78
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45628978"
---
# <a name="tutorial-5-extract-free-form-data"></a>Zelfstudie: 5. Vrije-gegevens ophalen

In deze zelfstudie, gebruikt u de entiteit pattern.any om gegevens te extraheren uit uitingen waar de uitingen zich goed opgemaakte en waar het einde van de gegevens kan eenvoudig kan worden verward met de resterende woorden van de utterance. 

De entiteit pattern.any kunt u vrije om gegevens te vinden waar de tekst van de entiteit waardoor het lastig is om te bepalen van het einde van de entiteit op basis van de rest van de utterance. 

Deze Human Resources-app helpt medewerkers bedrijf formulieren zoeken. 

|Utterance|
|--|
|Waar is **HRF 123456**?|
|Auteur **HRF 123234**?|
|**HRF 456098** is gepubliceerd in het Frans?|

Elk formulier heeft echter zowel een opgemaakte naam, gebruikt in de voorgaande tabel, evenals een beschrijvende naam, zoals `Request relocation from employee new to the company 2018 version 5`. 

Uitingen met de naam van het aangepaste formulier er als volgt uitzien:

|Utterance|
|--|
|Waar is **herlocatie van werknemer bekend bent met de bedrijfsversie 2018 5 aanvragen**?|
|Auteur **'Verplaatsing van werknemer bekend bent met de bedrijfsversie 2018 5 aanvragen'**?|
|**Verplaatsing van werknemer bekend bent met de bedrijfsversie 2018 5 aanvragen** is gepubliceerd in het Frans?|

De verschillende lengten bevat woorden die verwarrend kunnen zijn LUIS over waar de entiteit eindigt. Met behulp van een entiteit Pattern.any in een patroon, kunt u het begin en einde van de naam van het formulier opgeven, zodat de naam van het formulier in LUIS correct worden geëxtraheerd.

|Voorbeeld van de sjabloon utterance|
|--|
|Waar is {FormName} [?]|
|{FormName} [?] auteur|
|{FormName} is gepubliceerd in het Frans [?]|

**In deze zelfstudie leert u hoe u:**

> [!div class="checklist"]
> * Gebruik bestaande zelfstudie-app
> * Voorbeeld utterances toevoegen aan bestaande entiteit
> * Pattern.any entiteit maken
> * Patroon maken
> * Trainen
> * Test nieuwe patroon

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Gebruik bestaande app
Ga door met de app hebt gemaakt in de laatste zelfstudie, met de naam **Personeelszaken**. 

Als u de app Personeelszaken uit de vorige zelfstudie hebt, gebruikt u de volgende stappen uit:

1.  Download en sla [app JSON-bestand](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-roles-HumanResources.json).

2. De JSON importeren in een nieuwe app.

3. Uit de **beheren** sectie, op de **versies** tabblad en noem het klonen van de versie `patt-any`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. Omdat de versienaam van de wordt gebruikt als onderdeel van de URL-route, kan niet de naam van de tekens die niet toegestaan in een URL zijn bevatten.

## <a name="add-example-utterances"></a>Voorbeeld utterances toevoegen 
De vooraf gedefinieerde keyPhrase entiteit verwijderen als het is moeilijk te maken en de entiteit FormName te labelen. 

1. Selecteer **bouwen** via de bovenste navigatiebalk, selecteert u vervolgens **Intents** in linkernavigatiebalk.

2. Selecteer **FindForm** in de lijst intents.

3. Een voorbeeld-utterances toevoegen:

    |Voorbeeld utterance|
    |--|
    |Waar wordt het formulier **wat te doen wanneer een fire in het Lab deelt** en die behoefte hebben aan deze ondertekenen nadat ik lezen?|
    |Waar is **verplaatsing van aanvragen van werknemer bekend bent met het bedrijf** op de server?|
    |Auteur "**gezondheid aanvragen op de belangrijkste campus**' en wat de meest recente versie is?|
    |Ik ben op zoek naar het formulier met de naam '**Office verplaatsen aanvraag met inbegrip van fysieke activa**'. |

    Zonder een entiteit Pattern.any, zou het lastig zijn voor LUIS om te begrijpen waar de titel van het formulier wordt beëindigd vanwege de veel variaties van de formuliernamen van het.

## <a name="create-a-patternany-entity"></a>Een entiteit Pattern.any maken
De entiteit Pattern.any extraheert entiteiten met verschillende lengten. Dit werkt alleen in een patroon omdat het patroon het begin en einde van de entiteit markeert. Als u dat het patroon zien wanneer deze een Pattern.any bevat, extraheert entiteiten onjuist, gebruikt u een [expliciete lijst](luis-concept-patterns.md#explicit-lists) om dit probleem te verhelpen. 

1. Selecteer **entiteiten** in het linkernavigatievenster.

2. Selecteer **nieuwe entiteit maken**, voer de naam `FormName`, en selecteer **Pattern.any** als het type. Selecteer **Done**. 

    U kunt de entiteit in het doel kan niet label, omdat een Pattern.any alleen geldig in een patroon is. 

    Als u wilt dat de opgehaalde gegevens om op te nemen van andere entiteiten, zoals nummer of datetimeV2, moet u een samengestelde entiteit waarin de Pattern.any, evenals het aantal en de datetimeV2 maken.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Een patroon dat gebruikmaakt van de Pattern.any toevoegen

1. Selecteer **patronen** in de linkernavigatiebalk.

2. Selecteer de **FindForm** intentie.

3. Voer de volgende sjabloon uitingen die gebruikmaken van de nieuwe entiteit:

    |Uitingen van sjabloon|
    |--|
    |Waar is de vorm ['] {FormName} ['] en moet die zich het nadat ik deze [?] lezen|
    |Waar is ['] {FormName} ['] op de server [?]|
    |Auteur ['] {FormName} ['] en wat is de meest recente versie [?]|
    |Ik ben op zoek naar het formulier met de naam ['] {FormName} ['] [.]|

    Als u wilt voor variaties in het formulier, zoals enkele aanhalingstekens in plaats van de dubbele aanhalingstekens of een punt in plaats van een vraagteken, maakt u een nieuw patroon voor elke variatie.

4. Als u de entiteit keyPhrase verwijderd, kunt u deze terug naar de app toevoegen. 

## <a name="train-the-luis-app"></a>LUIS-app trainen

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Het nieuwe patroon voor vrije gegevensextractie testen
1. Selecteer **testen** in de bovenste balk met om de test-deelvenster te openen. 

2. Voer de volgende utterance: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Selecteer **inspecteren** onder het resultaat om te zien van de testresultaten voor de entiteit en voor welk doel.

    De entiteit `FormName` eerst wordt gevonden, wordt het patroon wordt gevonden, bepalen het doel. Als u een testresultaat waar de entiteiten zijn niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon).

4. Sluit het deelvenster testen door te selecteren de **testen** knop in de bovenste navigatiebalk.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie voorbeeld uitingen toegevoegd aan een bestaande doel wordt gemaakt van een nieuwe Pattern.any voor naam van het formulier. De zelfstudie wordt vervolgens een patroon voor de bestaande doel gemaakt met de nieuwe voorbeeld-uitingen en de entiteit. Interactieve testen hebt u geleerd dat het patroon en de bedoeling zijn voorspeld omdat de entiteit is gevonden. 

> [!div class="nextstepaction"]
> [Informatie over het gebruik van functies met een patroon](luis-tutorial-pattern-roles.md)