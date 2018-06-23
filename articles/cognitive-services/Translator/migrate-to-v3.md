---
title: Microsoft Translator tekst API - migreren naar V3 | Microsoft Docs
description: Informatie over het migreren van V2 naar V3 van de API van de tekst conversieprogramma.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: translator
ms.topic: article
ms.date: 03/27/2018
ms.author: v-jansko
ms.openlocfilehash: 16fec351af5b5b3875657ee244c18f305311d965
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345608"
---
# <a name="microsoft-translator-text-api-v2-to-v3-migration"></a>Microsoft Translator tekst API V2 voor V3-migratie

Het team van Microsoft Translator heeft uitgegeven versie 3 (V3) van de API van de vertaling van tekst. Deze versie bevat nieuwe functies, afgeschafte methoden en een nieuwe indeling voor het verzenden naar en ontvangen van gegevens van de Microsoft Translator-Service. Dit document bevat informatie over het wijzigen van toepassingen V3 gebruiken. V2 op 30 April 2018 wordt afgeschaft en op 30 April 2019 worden afgebroken.

Het einde van dit document bevat nuttige koppelingen voor meer informatie.

## <a name="summary-of-features"></a>Overzicht van functies

* Er is geen Trace - V3 Nee-Trace geldt voor alle prijscategorieën in de Azure portal. Dit betekent dat er geen tekst verzonden naar de V3-API worden opgeslagen door Microsoft.
* JSON - XML is vervangen door JSON. Alle gegevens naar de service verzonden en ontvangen van de service is in JSON-indeling.
* Meerdere talen voor doel in een afzonderlijke aanvraag - methode de vertalen accepteert meerdere 'to' talen voor vertaling in één aanvraag. Een enkele aanvraag kan bijvoorbeeld 'Engels from' en 'tot' Duits, Spaans en Japans of een andere groep van talen.
* Tweetalige woordenlijst - een methode tweetalige woordenlijst is toegevoegd aan de API. Deze methode omvat 'lookup' en 'voorbeelden'.
* Transliterate - een methode transliterate is toegevoegd aan de API. Deze methode wordt geconverteerd woorden en zinnen in een script (bijvoorbeeld Arabisch) in een ander script (bijvoorbeeld Latijns).
* Talen: een nieuwe 'talen'-methode biedt taalinformatie, in JSON-indeling voor gebruik met de 'vertalen', 'woordenlijst' en 'transliterate' methoden.
* Nieuwe te vertalen - nieuwe mogelijkheden toegevoegd aan de methode 'vertalen' voor de ondersteuning van enkele van de functies die in de V2-API als afzonderlijke methoden zijn. Een voorbeeld is TranslateArray.
* De methode Speak - Text to speech-functionaliteit wordt niet meer ondersteund in de Microsoft Translator-API. Text to speech-functionaliteit is beschikbaar in de Microsoft Azure cognitieve Bing Speech-API-services.

De volgende lijst met methoden V2 en V3 identificeert de API's waarmee de functionaliteit die is meegeleverd met V2 en V3-methoden.

| V2-API-methode   | V3-compatibiliteit API |
|:----------- |:-------------|
| Vertalen     | Vertalen          |
| TranslateArray      | Vertalen          |
| GetLanguageNames      | Talen          |
| GetLanguagesForTranslate     | Talen        |
| GetLanguagesForSpeak      | Cognitieve Services Speech-API         |
| Uitspreken     | Cognitieve Services Speech-API          |
| Detecteren     | Detecteren         |
| DetectArray     | Detecteren         |
| AddTranslation     | Microsoft Translator HUB API         |
| AddTranslationArray    | Microsoft Translator HUB API          |
| BreakSentences      | BreakSentence         |
| GetTranslations      | Functie wordt niet langer ondersteund.         |
| GetTranslationsArray      | Functie wordt niet langer ondersteund.         |

## <a name="move-to-json-format"></a>Verplaatsen naar JSON-indeling

Microsoft Translator tekst vertaling V2 geaccepteerd en gegevens in XML-indeling geretourneerd. Alle gegevens worden verzonden en ontvangen via de API wordt in V3-JSON-indeling. XML wordt niet meer worden geaccepteerd of geretourneerd in V3. 

Deze wijziging heeft betrekking op verschillende aspecten van een toepassing die is geschreven voor de API V2 tekst vertaling. Als een voorbeeld: talen om de API retourneert taalgegevens voor de tekstvertaling van, transliteration en de methoden twee woordenlijst. U kunt alle taalinformatie voor alle methoden in één aanroep aanvragen of deze afzonderlijk aanvragen.

De methode talen vereist geen verificatie. u kunt de taalinformatie voor V3 in JSON zien door te klikken op de volgende koppeling:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, dictionary, transliteration](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Verificatiesleutel

De verificatiesleutel die u voor V2 gebruikt worden geaccepteerd voor V3. U moet niet ophalen van een nieuw abonnement. U wordt mogelijk V2 en V3 mengen in uw apps tijdens de migratieperiode yearlong, waardoor het gemakkelijker voor u nieuwe versies terwijl u nog steeds van V2-XML naar V3-JSON migreert.

## <a name="pricing-model"></a>Prijsmodel

Microsoft Translator V3 wordt berekend op dezelfde manier die v2 is prijs; per teken, inclusief spaties. De nieuwe functies in V3 aanbrengen sommige wijzigingen in welke tekens worden geteld voor facturering.

| V3-methode   | Tekens die worden geteld voor facturering |
|:----------- |:-------------|
| Talen     | Er worden geen tekens verzonden, geen geteld, zonder kosten.          |
| Vertalen     | Aantal is gebaseerd op hoeveel tekens zijn ingediend voor vertaling en hoeveel talen de tekens worden vertaald. 50 tekens worden verzonden en 5 talen aangevraagd 50 x 5 zijn.           |
| Transliterate     | Aantal tekens dat voor de transliteration worden meegeteld.         |
| Woordenboek & voorbeeld     | Aantal tekens dat voor de woordenlijst opzoeken en voorbeelden worden meegeteld.         |
| BreakSentence     | Zonder kosten.       |
| Detecteren     | Zonder kosten.      |

## <a name="v3-end-points"></a>V3-eindpunten

Globaal

* API.cognitive.microsofttranslator.com


## <a name="v3-api-text-translations-methods"></a>V3-API tekst vertalingen methoden

[Talen](reference/v3-0-languages.md)

[Vertalen](reference/v3-0-translate.md)

[Transliterate](reference/v3-0-transliterate.md)

[BreakSentence](reference/v3-0-break-sentence.md)

[Detecteren](reference/v3-0-detect.md)

[Woordenlijst/lookup](reference/v3-0-dictionary-lookup.md)

[Woordenlijst/voorbeeld](reference/v3-0-dictionary-examples.md)

## <a name="customization"></a>Aanpassen

Microsoft Translator V3 neural machinevertaling standaard gebruikt. Deze kan niet als zodanig worden gebruikt bij de Microsoft Translator-Hub die uitsluitend verouderde statistische vertalen ondersteunt. Aanpassing voor neural vertaling is nu beschikbaar via de aangepaste-vertaler. [Meer informatie over het aanpassen van neural vertalen](customization.md)

Neural vertaling met de tekst V3 API biedt geen ondersteuning voor het gebruik van standaard categorieën (smt, spraak, tekst, generalnn).


## <a name="links"></a>Koppelingen

* [Microsoft-privacybeleid](https://privacy.microsoft.com/privacystatement)
* [Juridische informatie van Microsoft Azure](https://azure.microsoft.com/support/legal)
* [Online Services-voorwaarden](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Weergave V3.0 documentatie](reference/v3-0-reference.md)
