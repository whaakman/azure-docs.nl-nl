---
title: Migreren naar V3 - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Informatie over het migreren van V2 naar V3 van de Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: v-jansko
ms.openlocfilehash: 89a97b583407b448f296b1a54f5aabbf825b8b65
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537742"
---
# <a name="translator-text-api-v2-to-v3-migration"></a>Translator Text-API V2 naar V3-migratie

> [!NOTE]
> V2 op 30 April 2018 is afgeschaft en zal worden buiten gebruik gesteld op 30 April 2019.

> De Microsoft Translator-Hub wordt buiten gebruik gesteld op 30 April 2019. [Migratie van belangrijke informatie en datums weergeven](https://www.microsoft.com/translator/business/hub/).  

Het team van Microsoft Translator is uitgebracht van versie 3 (V3) van de Translator Text-API. Deze versie bevat nieuwe functies, afgeschafte methoden en een nieuwe indeling voor het verzenden naar en ontvangen van gegevens uit de Microsoft Translator-Service. Dit document bevat informatie over het wijzigen van toepassingen kunnen gebruikmaken van V3. 

Het einde van dit document bevat nuttige koppelingen voor meer informatie.

## <a name="summary-of-features"></a>Overzicht van functies

* Geen tracering - In V3 niet traceren is van toepassing op alle prijscategorieën in Azure portal. Deze functie betekent dat er geen tekst verzonden naar de API V3, moeten worden opgeslagen door Microsoft.
* JSON - XML is vervangen door JSON. Alle gegevens naar de service verzonden en ontvangen van de service is in JSON-indeling.
* Meerdere talen voor doel in een enkele aanvraag - methode de vertalen accepteert meerdere 'to' talen voor vertalingen in één aanvraag. Een enkele aanvraag kan bijvoorbeeld worden 'Engels from' en 'to' Duits, Spaans en Japans of een andere groep van talen.
* Tweetalig woordenboek: een methode Tweetalig woordenboek is toegevoegd aan de API. Deze methode omvat 'lookup' en 'voorbeelden'.
* Transliteratie: een methode transliterate is toegevoegd aan de API. Deze methode wordt woorden en zinnen in één script (bijvoorbeeld converteren Arabisch) in een ander script (bijvoorbeeld Latijns).
* Talen, een nieuwe 'talen'-methode levert taalinformatie, in JSON-indeling, voor gebruik met de 'vertalen', 'woordenlijst' en 'transliteratie' methoden.
* Nieuwe vertalen - er zijn nieuwe mogelijkheden toegevoegd aan de methode 'vertalen' voor de ondersteuning van enkele van de functies die in de V2-API als afzonderlijke methoden zijn. Een voorbeeld is TranslateArray.
* Methode spreken - tekst-naar-spraak-functionaliteit wordt niet meer ondersteund in de Microsoft Translator-API. Tekst naar spraak-functionaliteit is beschikbaar in [Microsoft Speech Service](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech).

De volgende lijst met methoden V2 en V3 identificeert de API's waarmee u de functionaliteit die is geleverd met V2 en V3-methoden.

| V2-API-methode   | V3 API-compatibiliteit |
|:----------- |:-------------|
| `Translate`     | [Vertalen](reference/v3-0-translate.md)          |
| `TranslateArray`      | [Vertalen](reference/v3-0-translate.md)        |
| `GetLanguageNames`      | [Talen](reference/v3-0-languages.md)         |
| `GetLanguagesForTranslate`     | [Talen](reference/v3-0-languages.md)       |
| `GetLanguagesForSpeak`      | [Microsoft Speech-Service](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech)         |
| `Speak`     | [Microsoft Speech-Service](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech)          |
| `Detect`     | [Detecteren](reference/v3-0-detect.md)         |
| `DetectArray`     | [Detecteren](reference/v3-0-detect.md)         |
| `AddTranslation`     | Functie is niet meer ondersteund.       |
| `AddTranslationArray`    | Functie is niet meer ondersteund.          |
| `BreakSentences`      | [BreakSentence](reference/v3-0-break-sentence.md)       |
| `GetTranslations`      | Functie is niet meer ondersteund.         |
| `GetTranslationsArray`      | Functie is niet meer ondersteund.         |

## <a name="move-to-json-format"></a>Verplaatsen naar JSON-indeling

Microsoft Translator tekst vertalen V2 geaccepteerd en geretourneerd van gegevens in XML-indeling. Alle gegevens verzonden en ontvangen met behulp van de API wordt in V3 in JSON-indeling. XML wordt niet meer worden geaccepteerd of die worden geretourneerd in V3.

Deze wijziging heeft invloed op verschillende aspecten van een toepassing die is geschreven voor de V2 tekst Spraakomzettings-API. Als u een voorbeeld: De talen-API retourneert taalinformatie voor tekstvertaling vele methoden, en de twee woordenlijst. U kunt alle taalinformatie voor alle methoden in één aanroep aanvragen of ze afzonderlijk aanvraagt.

De methode talen vereist niet-authenticatie. door te klikken op de volgende koppeling ziet u de taalinformatie voor V3 in JSON:

[https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation, woordenlijst, vele](https://api.cognitive.microsofttranslator.com/languages?api-version=3.0&scope=translation,dictionary,transliteration)

## <a name="authentication-key"></a>Verificatiesleutel

De verificatiesleutel voor V2 worden geaccepteerd voor V3. U moet niet ophalen van een nieuw abonnement. U wordt mogelijk V2 en V3 in uw apps combineren tijdens de migratieperiode yearlong, waardoor het gemakkelijker voor u nieuwe versies terwijl u nog steeds van V2-XML naar V3-JSON migreert.

## <a name="pricing-model"></a>Prijsmodel

De prijs van Microsoft Translator V3 wordt berekend op dezelfde manier als die v2 is prijs; per teken, inclusief spaties. De nieuwe functies in V3 Breng enkele wijzigingen aan welke tekens worden meegeteld voor facturering.

| V3-methode   | Tekens meegeteld voor facturering |
|:----------- |:-------------|
| `Languages`     | Er is geen tekens verzonden, geen geteld, er zijn geen kosten in rekening gebracht.          |
| `Translate`     | Aantal is gebaseerd op het aantal tekens worden verzonden voor vertaling en het aantal talen de tekens worden vertaald. 50 tekens verzonden wordt, en 5 talen aangevraagd 50 x 5.           |
| `Transliterate`     | Aantal tekens dat is ingediend voor vele worden geteld.         |
| `Dictionary lookup & example`     | Aantal tekens dat is ingediend voor Dictionary lookup en voorbeelden worden geteld.         |
| `BreakSentence`     | Er zijn geen kosten in rekening gebracht.       |
| `Detect`     | Er zijn geen kosten in rekening gebracht.      |

## <a name="v3-end-points"></a>V3-eindpunten

Wereldwijd

* api.cognitive.microsofttranslator.com

## <a name="v3-api-text-translations-methods"></a>V3 API tekst vertalingen-methoden

[`Languages`](reference/v3-0-languages.md)

[`Translate`](reference/v3-0-translate.md)

[`Transliterate`](reference/v3-0-transliterate.md)

[`BreakSentence`](reference/v3-0-break-sentence.md)

[`Detect`](reference/v3-0-detect.md)

[`Dictionary/lookup`](reference/v3-0-dictionary-lookup.md)

[`Dictionary/example`](reference/v3-0-dictionary-examples.md)

## <a name="compatibility-and-customization"></a>Compatibiliteit en aanpassen

> [!NOTE] 

> De Microsoft Translator-Hub wordt buiten gebruik gesteld op 30 April 2019. [Migratie van belangrijke informatie en datums weergeven](https://www.microsoft.com/translator/business/hub/).   

Microsoft Translator V3 maakt standaard gebruik van neurale machinevertalingen. Deze kunnen daarom niet worden gebruikt met de Microsoft Translator-Hub. De Translator-Hub biedt alleen ondersteuning voor verouderde statistische machinevertalingen. Aanpassing voor neurale vertalingen is nu beschikbaar met de aangepaste Translator. [Meer informatie over het aanpassen van neurale machinevertalingen](custom-translator/overview.md)

Neurale vertalingen met de tekst van V3 API biedt geen ondersteuning voor het gebruik van standard categorieën (SMT, spraak, technische, generalnn).

| |Eindpunt|    GDPR-naleving voor Processor|  Translator Hub gebruiken| Gebruik aangepaste Translator (Preview)|
|:-----|:-----|:-----|:-----|:-----|
|Translator Text-API versie 2| api.microsofttranslator.com|    Nee  |Ja    |Nee|
|Translator Text-API versie 3| api.cognitive.microsofttranslator.com|  Ja|    Nee| Ja|

**Translator Text-API versie 3**
* Is algemeen beschikbaar en volledig wordt ondersteund.
* Is compatibel als een processor AVG en voldoet aan alle vereisten voor ISO 20001 en 20018 evenals SOC 3-certificering. 
* Kunt u de neural network vertalingssystemen die u hebt aangepast met aangepaste Translator (Preview), de nieuwe functie voor het aanpassen van de Translator NMT aanroepen. 
* Biedt geen toegang tot aangepaste vertaalsystemen gemaakt met behulp van de Microsoft Translator-Hub.

U gebruikt versie 3 van de Translator Text-API als u het eindpunt api.cognitive.microsofttranslator.com.

**Translator Text-API versie 2**
* Is afgeschaft. Het wordt buiten gebruik gesteld op 30 April 2019. 
* Voldoet niet aan alle 20001,20018 ISO en SOC 3-vereisten voor certificering. 
* Kunt u geen om aan te roepen van het neurale netwerk automatische vertalingssystemen die u met de functie voor het aanpassen van Translator hebt aangepast.
* Biedt toegang tot aangepaste vertaalsystemen gemaakt met behulp van de Microsoft Translator-Hub.
* U gebruikt versie 2 van de Translator Text-API als u het eindpunt api.microsofttranslator.com.

Er is geen versie van de API van Translator maakt een record van uw vertalingen. De vertalingen zijn nooit met niemand gedeeld. Meer informatie over de [Translator niet traceren](https://www.aka.ms/NoTrace) webpagina.

## <a name="links"></a>Koppelingen

* [Microsoft Privacy Policy](https://privacy.microsoft.com/privacystatement)
* [Juridische informatie voor Microsoft Azure](https://azure.microsoft.com/support/legal)
* [Voorwaarden voor Online Services](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [V3.0-documentatie weergeven](reference/v3-0-reference.md)
