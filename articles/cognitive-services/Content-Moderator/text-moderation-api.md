---
title: Azure Content beheerder - tekst toezicht | Microsoft Docs
description: Controle van de tekst gebruiken voor mogelijk ongewenste tekst, PII-, en aangepaste van termen lijsten.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/30/2018
ms.author: sajagtap
ms.openlocfilehash: 5783a7a06d75a409969abad011de3bbd31dec292
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344855"
---
# <a name="text-moderation"></a>Beheer van tekst

Gebruik van de beheerder van de inhoud tekst machine ondersteunde toezicht en [human in de lus](Review-Tool-User-Guide/human-in-the-loop.md) mogelijkheden voor het gemiddelde tekstinhoud.

Bedrijven gebruiken de tekst toezicht service blokkeren, goedkeuren of Controleer de inhoud op basis van hun beleid en de drempelwaarden. De tekst toezicht-service kan worden gebruikt om te vergroten menselijke toezicht van omgevingen waarvoor partners, werknemers en consumenten tekstinhoud genereren. Het gaat hierbij om chatruimten, discussieforums chatbots, e-commerce catalogussen, documenten en meer. 

De API voor taalgebruik de binnenkomende tekst (maximaal 1024 tekens) wordt gescand, classificeert voor mogelijke ongewenste tekst (preview) autocorrects tekst, en detecteert mogelijke persoonsgegevens (PII). Deze overeenkomt met ook op basis van de aangepaste lijsten van termen. De functie AutoCorrectie kunt catch opzettelijk verkeerd gespelde woorden. Nadat de inhoud wordt verwerkt, wordt in de service een gedetailleerde antwoord geretourneerd. U gebruikt het antwoord te maken van een menselijke bekijken in het hulpprogramma voor beoordeling of dit medium meenemen omlaag, enzovoort.

Het service-antwoord bevat de volgende informatie:

- Taalgebruik: op basis van een term die overeenkomt met de lijst met ingebouwde schennende termen in meerdere talen
- Classificatie: machine-ondersteunde indeling in drie categorieën
- Persoonsgegevens (PII)
- Tekst automatisch opgelost
- Oorspronkelijke tekst
- Taal

## <a name="profanity"></a>Taalgebruik

Als de API eventuele schennende voorwaarden in een van detecteert de [ondersteunde talen](Text-Moderation-API-Languages.md), deze voorwaarden zijn opgenomen in het antwoord. Het antwoord bevat ook de locatie (`Index`) in de oorspronkelijke tekst. De `ListId` in het volgende voorbeeld JSON naar voorwaarden die zijn gevonden verwijst in [aangepaste term lijsten](try-terms-list-api.md) indien beschikbaar.

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> Voor de **taal** parameter toewijzen `eng` of laat het veld leeg voor een overzicht van de machine-ondersteunde **classificatie** antwoord (preview-functie). **Deze functie ondersteunt alleen Engels**.
>
> Voor **taalgebruik voorwaarden** detectie, gebruik de [ISO 639-3-code](http://www-01.sil.org/iso639-3/codes.asp) van de ondersteunde talen die worden vermeld in dit artikel of laat het veld leeg.

## <a name="classification"></a>Classificatie

Inhoud beheerder de machine-ondersteunde **tekst classificatie functie** ondersteunt **alleen Engels**, en het helpt bij het detecteren van mogelijk ongewenste inhoud. De gemarkeerde inhoud kan als ongepast markeren, afhankelijk van de context beschouwd. Naast het overbrengen van de kans op elke categorie, kan deze een menselijke beoordeling van de inhoud aanbevelen. De functie gebruikt een getraind model mogelijk misbruik, negatieve of discriminerende taal vast te stellen. Dit omvat taalgebruik, afgekorte woorden, aanstootgevende en opzettelijk verkeerd gespelde woorden worden beoordeeld. 

Het volgende extract in het JSON-extract toont een voorbeeld van uitvoer:

> [!NOTE]
> De functie 'Classificatie' machine ondersteund is een Preview-versie.

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

- `Category1` Hiermee geeft u de mogelijke aanwezigheid van de taal die kan worden beschouwd als seksueel expliciete of volwassenen in bepaalde situaties.
- `Category2` Hiermee geeft u de mogelijke aanwezigheid van de taal die kan worden beschouwd als seksuele suggestieve of volwassen in bepaalde situaties.
- `Category3` Hiermee geeft u de mogelijke aanwezigheid van de taal die mogelijk aanstootgevend in bepaalde situaties.
- `Score` ligt tussen 0 en 1. Hoe hoger de score, hoe groter het model is voorspellen van de categorie mogelijk van toepassing. Dit voorbeeld is afhankelijk van een statistische model in plaats van handmatig gecodeerde resultaten. U wordt aangeraden testen door uw eigen inhoud om te bepalen hoe elke categorie wordt uitgelijnd aan uw vereisten.
- `ReviewRecommended` waar of ONWAAR afhankelijk van de interne score drempelwaarden is. Klanten dient te beoordelen of wilt gebruiken deze waarde of op aangepaste drempelwaarden op basis van hun inhoud beleidsregels bepalen.

## <a name="personally-identifiable-information-pii"></a>Persoonsgegevens (PII)

De functie PII detecteert de mogelijke aanwezigheid van deze informatie:

- E-mailadres
- VS-verzendadres
- IP-adres
- Telefoonnummer van de VS
- VK telefoonnummer
- Sociaal-fiscaal nummer (Sofinummer)

Het volgende voorbeeld ziet u een voorbeeldantwoord:

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

## <a name="auto-correction"></a>Automatische correcties

Stel dat de ingevoerde tekst is (de 'lzay' en 'f0x' zijn opzettelijk):

    The qu!ck brown f0x jumps over the lzay dog.

Als u automatische correcties aanvraagt, wordt in het antwoord de gecorrigeerde versie van de tekst bevat:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Maken en beheren van uw aangepaste lijsten met voorwaarden

De standaard globale lijst met termen zeer geschikt voor de meeste gevallen, maar mogelijk wilt scherm met de termen die specifiek voor uw bedrijfsbehoeften zijn. U wilt bijvoorbeeld uitfilteren concurrerende merk namen van berichten door gebruikers. De drempelwaarde van toegestane tekstinhoud afwijken van de lijst met.

> [!NOTE]
> Er is een maximumlimiet van **geeft een lijst van 5 term** met elke lijst **niet meer dan 10.000 voorwaarden**.
>

Het volgende voorbeeld ziet u de overeenkomende-ID:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

De beheerder inhoud biedt een [Term lijst API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) met bewerkingen voor het beheer van aangepaste term bevat. Beginnen met de [Term bevat API Console](try-terms-list-api.md) en de REST-API-codevoorbeelden gebruiken. Lees ook de [Term bevat .NET Quick Start](term-lists-quickstart-dotnet.md) als u bekend met Visual Studio en C# bent.

## <a name="next-steps"></a>Volgende stappen

Uitprobeert de [tekst toezicht API console](try-text-api.md) en de REST-API-codevoorbeelden gebruiken. Lees ook de [tekst toezicht .NET Quick Start](text-moderation-quickstart-dotnet.md) als u bekend met Visual Studio en C# bent.
