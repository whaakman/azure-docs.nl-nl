---
title: Beheer van tekst - Content Moderator
description: Gebruik beheer van tekst voor mogelijke tekst, persoonlijke gegevens ongewenste, en aangepaste van voorwaarden lijsten.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 5a1007f2408b48c96f5eeaf585b94c8caa7ceb45
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757765"
---
# <a name="learn-text-moderation-concepts"></a>Informatie over concepten voor beheer van tekst

Gebruik de Content Moderator geautomatiseerd teksttoezicht en [menselijke beoordeling](Review-Tool-User-Guide/human-in-the-loop.md) mogelijkheden voor het beheren van tekstinhoud.

U blokkeren, goedkeuren of Controleer de inhoud die op basis van uw beleid en drempelwaarden. Gebruiken om te verbeteren van menselijk toezicht van omgevingen waar partners, werknemers en consumenten tekstinhoud genereren. Het gaat hierbij om chatruimten, discussieborden, chatbots, e-commerce-catalogi en documenten. 

Antwoord van de service bevat de volgende informatie:

- Grof taalgebruik: op basis van een term die overeenkomt met de ingebouwde lijst van grove voorwaarden in verschillende talen
- Classificatie: geautomatiseerd classificatie in drie categorieën
- Persoonlijke gegevens
- Tekst automatisch worden gecorrigeerd
- Oorspronkelijke tekst
- Taal

## <a name="profanity"></a>Grof taalgebruik

Als de API alle ongepaste termen in een van detecteert de [ondersteunde talen](Text-Moderation-API-Languages.md), deze voorwaarden zijn opgenomen in het antwoord. Het antwoord bevat ook de locatie (`Index`) in de oorspronkelijke tekst. De `ListId` in het volgende voorbeeld JSON naar de voorwaarden zijn beschikbaar verwijst [aangepaste terminologielijsten](try-terms-list-api.md) indien beschikbaar.

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> Voor de **taal** parameter toewijzen `eng` of laat het veld leeg om te zien van de computer-ondersteund **classificatie** antwoord (preview-functie). **Deze functie ondersteunt alleen Engels**.
>
> Voor **grof taalgebruik voorwaarden** detectie, gebruik de [ISO 639-3-code](http://www-01.sil.org/iso639-3/codes.asp) van de ondersteunde talen die worden vermeld in dit artikel of laat het veld leeg.

## <a name="classification"></a>Classificatie

Content Moderator de geautomatiseerd **classificatie tekstfunctie** ondersteunt **alleen Engels**, en helpt detecteren van potentieel ongewenste inhoud. De gemarkeerde inhoud kan worden beoordeeld als ongepast, afhankelijk van de context. Het overbrengen van de kans op elke categorie en kan aanraden om een menselijke beoordeling. De functie maakt gebruik van een getraind model voor het identificeren van mogelijke beledigende, negatieve of discriminerende taal. Dit omvat slang, verkorte woorden en aanstootgevende en opzettelijk verkeerd gespelde woorden voor controle. 

Het volgende uittreksel in het JSON-extract toont een voorbeeld van uitvoer:

    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
            },
        "Category2": {
            "Score": 0.12747249007225037
            },
        "Category3": {
            "Score": 0.98799997568130493
        }
    }

### <a name="explanation"></a>Uitleg

- `Category1` verwijst naar de mogelijke aanwezigheid van de taal die kan worden beschouwd als seksueel expliciet of volwassenen in bepaalde situaties.
- `Category2` verwijst naar de mogelijke aanwezigheid van de taal die kan worden beschouwd als seksueel suggestieve of volwassen in bepaalde situaties.
- `Category3` verwijst naar de mogelijke aanwezigheid van de taal die mogelijk aanstootgevend in bepaalde situaties.
- `Score` ligt tussen 0 en 1. Hoe hoger de score, hoe hoger het model is voorspellen van de categorie mogelijk van toepassing zijn. Deze functie is afhankelijk van een statistische model in plaats van handmatig gecodeerde resultaten. Het is raadzaam om eerst te testen met uw eigen inhoud om te bepalen hoe elke categorie overeenstemt met uw vereisten.
- `ReviewRecommended` waar of ONWAAR afhankelijk van de interne score drempelwaarden is. Klanten moeten beoordelen of u wilt deze waarde wordt gebruikt of moet u besluiten welke aangepaste drempelwaarden op basis van hun inhoud beleid.

## <a name="personal-data"></a>Persoonlijke gegevens

De functie PII detecteert de mogelijke aanwezigheid van deze informatie:

- E-mailadres
- Verzendadres VS
- IP-adres
- Amerikaans telefoonnummer
- Verenigd Koninkrijk-telefoonnummer
- Social Security Number (SSN)

Het volgende voorbeeld ziet u een voorbeeldantwoord weergegeven:

    "PII": {
        "Email": [{
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 32
            }],
        "IPA": [{
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 72
            }],
        "Phone": [{
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 56
            }, {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 212
            }, {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 208
            }, {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 228
            }, {
            "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 245
            }],
        "Address": [{
            "Text": "1 Microsoft Way, Redmond, WA 98052",
            "Index": 89
            }],
        "SSN": [{
            "Text": "999999999",
            "Index": 56
            }, {
            "Text": "999-99-9999",
            "Index": 267
            }]
        }

## <a name="auto-correction"></a>Automatische correctie

Stel dat de ingevoerde tekst is (de 'lzay' en 'f0x' zijn opzettelijke):

    The qu!ck brown f0x jumps over the lzay dog.

Als u om automatische correctie vraagt, bevat het antwoord de gecorrigeerde versie van de tekst:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Het maken en beheren van uw aangepaste lijsten van voorwaarden

Terwijl de standaardinstelling, globale lijst met voorwaarden ideaal voor de meeste gevallen werkt, kunt u op het scherm met de termen die specifiek voor uw bedrijfsbehoeften zijn. Bijvoorbeeld, kunt u uit een concurrerende merken van berichten door gebruikers te filteren.

> [!NOTE]
> Er is een maximumlimiet van **5 terminologielijsten** waarbij elke lijst **niet meer dan 10.000 termen mag bevatten**.
>

Het volgende voorbeeld ziet u de bijbehorende lijst-ID:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

De Content Moderator biedt een [Term lijst API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) met bewerkingen voor het beheren van de termijn van aangepaste lijsten. Beginnen met de [Term geeft een lijst van API-Console](try-terms-list-api.md) en gebruiken van de REST-API-codevoorbeelden. Lees ook de [Term geeft een lijst van .NET-snelstartgids](term-lists-quickstart-dotnet.md) als u bekend met Visual Studio en C# bent.

## <a name="next-steps"></a>Volgende stappen

U kunt uitproberen de [tekst afbeeldingstoezicht-API-console](try-text-api.md) en gebruiken van de REST-API-codevoorbeelden. Lees ook de [tekst toezicht .NET snelstartgids](text-moderation-quickstart-dotnet.md) als u bekend met Visual Studio en C# bent.
