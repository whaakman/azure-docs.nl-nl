---
title: Inhoud beheerder Azure - installatiekopie toezicht | Microsoft Docs
description: Controle van de installatiekopie op normaal ongeschikte installatiekopieën gebruiken
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: c7cbc343c6e9113642d0ac79f4a4d60a404e8171
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345639"
---
# <a name="image-moderation"></a>Beheer van afbeeldingen

Gebruik van de beheerder van de inhoud machine ondersteund door de installatiekopie van toezicht en [menselijke revisie hulpprogramma](Review-Tool-User-Guide/human-in-the-loop.md) op afbeeldingen voor de inhoud voor volwassenen en mooie normaal. Afbeeldingen voor tekstinhoud scannen en vlakken detecteren die tekst ophalen. U kunt installatiekopieën op basis van de aangepaste lijsten overeenkomen en verdere actie te ondernemen.

## <a name="evaluating-for-adult-and-racy-content"></a>Voor inhoud voor volwassenen en mooie evalueren

De **Evaluate** bewerking retourneert een vertrouwen score tussen 0 en 1. Ook wordt Boolean-waarde gelijk is aan waar of ONWAAR. Deze waarden te voorspellen of de installatiekopie van het potentiële inhoud voor volwassenen of mooie bevat. Wanneer u de API aanroept met de installatiekopie (bestand of URL), is het geretourneerde antwoord bevat de volgende informatie:

    "ImageModeration": {
      .............
      "adultClassificationScore": 0.019196987152099609,
      "isImageAdultClassified": false,
      "racyClassificationScore": 0.032390203326940536,
      "isImageRacyClassified": false,
      ............
      ],

> [!NOTE]

> - `isImageAdultClassified` Hiermee geeft u de mogelijke aanwezigheid van de installatiekopieën die kunnen worden beschouwd als seksueel expliciete of volwassenen in bepaalde situaties.
> - `isImageRacyClassified` Hiermee geeft u de mogelijke aanwezigheid van de installatiekopieën die kunnen worden beschouwd als seksuele suggestieve of volwassen in bepaalde situaties.
> - De scores moet tussen 0 en 1 liggen. Hoe hoger de score, hoe groter het model is voorspellen van de categorie mogelijk van toepassing. Dit voorbeeld is afhankelijk van een statistische model in plaats van handmatig gecodeerde resultaten. U wordt aangeraden testen door uw eigen inhoud om te bepalen hoe elke categorie wordt uitgelijnd aan uw vereisten.
> - De Booleaanse waarden zijn waar of ONWAAR afhankelijk van de interne score drempelwaarden. Klanten dient te beoordelen of wilt gebruiken deze waarde of op aangepaste drempelwaarden op basis van hun inhoud beleidsregels bepalen.
>

## <a name="detecting-text-with-optical-character-recognition-ocr"></a>Tekst met OCR (OCR) detecteren

De **OCR (OCR)** bewerking voorspelt de aanwezigheid van de tekst in een installatiekopie en pakt het voor de controle van de tekst, onder andere gebruiken om. U kunt de taal opgeven. Als u een taal niet opgeeft, wordt de detectie wordt standaard ingesteld op Engels.

Het antwoord bevat de volgende informatie:
- De oorspronkelijke tekst.
- De gedetecteerde tekstelementen met hun scores vertrouwen.

Voorbeeld extract:

    "TextDetection": {
      "status": {
        "code": 3000.0,
        "description": "OK",
        "exception": null
      },
      .........
      "language": "eng",
      "text": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
      "candidates": []
    },


## <a name="detecting-faces"></a>Vlakken detecteren

Vlakken detecteren helpt bij het detecteren van persoonsgegevens (PII) zoals vlakken in de installatiekopieën. U detecteren potentiële vlakken en het aantal mogelijke vlakken in elke installatiekopie.

Een reactie omvat deze informatie:

- Vlakken tellen
- Lijst met locaties vlakken gedetecteerd

Voorbeeld extract:


    "FaceDetection": {
       ......
      "result": true,
      "count": 2,
      "advancedInfo": [
      .....
      ],
      "faces": [
        {
          "bottom": 598,
          "left": 44,
          "right": 268,
          "top": 374
        },
        {
          "bottom": 620,
          "left": 308,
          "right": 532,
          "top": 396
        }
      ]
    }

## <a name="creating-and-managing-custom-lists"></a>Aangepaste lijsten maken en beheren

In veel onlinecommunity's, nadat gebruikers afbeeldingen of andere typen inhoud uploaden, kunnen aanstootgevende items ophalen gedeeld meerdere keren via de volgende dagen, weken en maanden. De kosten van herhaaldelijk scannen en dezelfde afbeelding of zelfs enigszins gewijzigde versies van de afbeelding vanaf meerdere locaties worden uitgefilterd zijn dure en gevoelig voor fouten.

In plaats van toezicht meerdere keren houden dezelfde installatiekopie, kunt u de aanstootgevende installatiekopieën toevoegen aan uw aangepaste lijst met geblokkeerde inhoud. Op die manier uw systeem inhoud toezicht vergelijkt binnenkomende afbeeldingen op basis van uw aangepaste lijsten en verdere verwerking stopt.

> [!NOTE]
> Er is een maximumlimiet van **5 installatiekopie lijsten** met elke lijst **niet meer dan 10.000 afbeeldingen**.
>

De beheerder inhoud biedt een complete [installatiekopie lijst Management API](try-image-list-api.md) met bewerkingen voor het beheren van lijsten met aangepaste installatiekopieën. Beginnen met de [afbeelding geeft een lijst van API Console](try-image-list-api.md) en de REST-API-codevoorbeelden gebruiken. Lees ook de [installatiekopie lijst .NET Quick Start](image-lists-quickstart-dotnet.md) als u bekend met Visual Studio en C# bent.

## <a name="matching-against-your-custom-lists"></a>Overeenkomende op basis van uw aangepaste lijsten

De bewerking overeen kunt fuzzy overeenkomst met binnenkomende afbeeldingen met een van de aangepaste lijsten gemaakt en beheerd via de bewerkingen na opvragen.

Als een overeenkomst wordt gevonden, wordt de bewerking retourneert de id en de labels van de controle van de overeenkomende afbeelding. Het antwoord bevat deze informatie:

- Score overeen (tussen 0 en 1)
- Overeenkomende installatiekopie
- Afbeelding labels (toegewezen tijdens de vorige controle)
- Labels van de afbeelding

Voorbeeld extract:

    {
    ..............,
    "IsMatch": true,
    "Matches": [
        {
            "Score": 1.0,
            "MatchId": 169490,
            "Source": "169642",
            "Tags": [],
            "Label": "Sports"
        }
    ],
    ....
    }

## <a name="human-review-tool"></a>Programma voor menselijke beoordeling

Voor meer nuanced gevallen gebruikt u de beheerder van de inhoud [hulpprogramma bekijken](Review-Tool-User-Guide/human-in-the-loop.md) en de bijbehorende API om op te halen van de resultaten van toezicht en de inhoud in de revisie voor uw menselijke moderators. Ze de labels machine is toegewezen en Bevestig de definitieve beslissingen.

![Installatiekopie van een menselijke moderators revisie](images/moderation-reviews-quickstart-dotnet.PNG)

## <a name="next-steps"></a>Volgende stappen

Uitprobeert de [installatiekopie toezicht API console](try-image-api.md) en de REST-API-codevoorbeelden gebruiken. Lees ook de [installatiekopie toezicht .NET Quick Start](image-moderation-quickstart-dotnet.md) als u bekend met Visual Studio en C# bent.
