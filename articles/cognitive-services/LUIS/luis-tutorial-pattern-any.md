---
title: Zelfstudie over het gebruiken van pattern.any entiteit voor het verbeteren van LUIS voorspellingen - Azure | Microsoft Docs
titleSuffix: Cognitive Services
description: In deze zelfstudie, gebruikt u de entiteit pattern.any voor het verbeteren van LUIS intentie en entiteit voorspellingen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: v-geberr;
ms.openlocfilehash: 95c88bf1e452e945948b47fdada5811db6a0e454
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174612"
---
# <a name="tutorial-improve-app-with-patternany-entity"></a>Zelfstudie: Een app met pattern.any entiteit verbeteren

In deze zelfstudie, gebruikt u de entiteit pattern.any intentie en entiteit voorspelling verhogen.  

> [!div class="checklist"]
* Meer informatie over wanneer en hoe u pattern.any
* Patroon die gebruikmaakt van pattern.any maken
* Voorspelling verbeteringen controleren

Voor dit artikel hebt u een gratis [LUIS](luis-reference-regions.md)-account nodig om uw LUIS-toepassing te creëren.

## <a name="before-you-begin"></a>Voordat u begint
Als u geen de Human Resources-app vanuit de [patroon rollen](luis-tutorial-pattern-roles.md) zelfstudie [importeren](luis-how-to-start-new-app.md#import-new-app) de JSON naar een nieuwe app in de [LUIS](luis-reference-regions.md#luis-website) website. De app voor het importeren is gevonden in de [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-roles-HumanResources.json) GitHub-opslagplaats.

Als u de oorspronkelijke Human Resources-app wilt gebruiken, kloont u de versie op de pagina [Settings](luis-how-to-manage-versions.md#clone-a-version) en wijzigt u de naam in `patt-any`. Klonen is een uitstekende manier om te experimenteren met verschillende functies van LUIS zonder dat de oorspronkelijke versie wordt gewijzigd. 

## <a name="the-purpose-of-patternany"></a>Het doel van pattern.any
De entiteit pattern.any kunt u vrije indeling om gegevens te vinden waar de tekst van de entiteit waardoor het lastig is om te bepalen van het einde van de entiteit op basis van de rest van de utterance. 

Deze Human Resources-app helpt medewerkers bedrijf formulieren zoeken. Formulieren zijn toegevoegd de [reguliere expressie zelfstudie](luis-quickstart-intents-regex-entity.md). De namen van de vorm van deze zelfstudie gebruikt een reguliere expressie om op te halen van de naam van een formulier die goed opgemaakte zoals de naam van het formulier is vet in de volgende utterance-tabel:

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

De verschillende lengten bevat zinnen die verwarrend kunnen zijn LUIS over waar de entiteit eindigt. Met behulp van een entiteit Pattern.any in een patroon, kunt u het begin en einde van de naam van het formulier opgeven, zodat de naam van het formulier in LUIS correct worden geëxtraheerd.

**Terwijl patronen u minder voorbeeld uitingen kunnen, als de entiteiten niet worden gedetecteerd, wordt het patroon komt niet overeen.**

## <a name="add-example-utterances-to-the-existing-intent-findform"></a>Voorbeeld utterances toevoegen met de bestaande intent FindForm 
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

5. De app trainen.


## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Het nieuwe patroon voor vrije gegevensextractie testen
1. Selecteer **testen** in de bovenste balk met om de test-deelvenster te openen. 

2. Voer de volgende utterance: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Selecteer **inspecteren** onder het resultaat om te zien van de testresultaten voor de entiteit en voor welk doel.

    De entiteit `FormName` eerst wordt gevonden, wordt het patroon wordt gevonden, bepalen het doel. Als u een testresultaat waar de entiteiten zijn niet gedetecteerd en daarom niet het patroon wordt gevonden hebt, moet u meer voorbeeld utterances toevoegen op het doel (niet het patroon).

4. Sluit het deelvenster testen door te selecteren de **testen** knop in de bovenste navigatiebalk.

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u de LUIS-app niet meer nodig hebt, kunt u deze verwijderen. Om dit te doen, selecteer het weglatingsteken (***...*** ) aan de rechterkant van de naam van de app in de lijst met Apps, selecteer **verwijderen**. Selecteer in het pop-upvenster **Delete app?** de optie **Ok**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over het gebruik van functies met een patroon](luis-tutorial-pattern-roles.md)