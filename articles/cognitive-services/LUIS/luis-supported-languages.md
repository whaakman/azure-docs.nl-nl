---
title: Ondersteuning voor lokalisatie LUIS apps in Azure | Microsoft Docs
description: Meer informatie over de talen die LUIS ondersteunt.
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2017
ms.author: cahann
ms.openlocfilehash: 1eabc01ee07f8791680738a156471e3efe2c44ff
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345969"
---
# <a name="culture-specific-understanding-in-luis-apps"></a>Cultuur-specifieke inzicht in LUIS apps

Een app LUIS is specifiek voor cultuur en kan niet worden gewijzigd als deze eenmaal is ingesteld. 

## <a name="multi-language-luis-apps"></a>Meerdere talen LUIS apps
Als u een clienttoepassing van meertalige LUIS zoals een chatbot nodig hebt, hebt u een aantal opties. Als LUIS alle talen ondersteunt, kunt u een app LUIS voor elke taal ontwikkelen. Elke app LUIS heeft een unieke app-ID en een eindpunt logboek. Als u bieden inzicht in voor een taal LUIS niet ondersteunt wilt, kunt u taal [Microsoft Translator API](../Translator/translator-info-overview.md) voor het vertalen van de utterance in een ondersteunde taal voor de utterance naar het eindpunt LUIS verzenden en ontvangen de resulterende scores.

## <a name="languages-supported"></a>Ondersteunde talen
LUIS begrijpt utterances in de volgende talen:


| Taal |Landinstelling  |  Vooraf gedefinieerde domein | Vooraf gedefinieerde entiteit | Woordgroep suggesties | **[Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) | 
|--|--|:--:|:--:|:--:|:--:|
| Amerikaans-Engels |`en-US` | ✔ | ✔  |✔|✔|
| Canadees Frans |`fr-CA` |-|   -   |-|✔|
| *[Chinees](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Nederlands |`nl-NL` |-|  -   |-|✔|
| Frans (Frankrijk) |`fr-FR` |-| ✔ |✔ |✔|
| Duits |`de-DE` |-| ✔ |✔ |✔|
| Italiaans |`it-IT` |-| ✔ |✔|✔|
| *[Japans](#japanese-support-notes) |`ja-JP` |-| ✔ |✔|Alleen sleutel woordgroep|
| Koreaans |`ko-KR` |-|   -   |-|Alleen sleutel woordgroep|
| Portugees (Brazilië) |`pt-BR` |-| ✔ |✔ |niet alle onderliggende culturen|
| Spaans (Spanje) |`es-ES` |-| ✔ |✔|✔|
| Spaans (Mexico)|`es-MX` |-|  -   |✔|✔|


Taalondersteuning varieert voor [vooraf gemaakte entiteiten](luis-reference-prebuilt-entities.md) en [vooraf gedefinieerde domeinen](luis-reference-prebuilt-domains.md). 

### <a name="chinese-support-notes"></a>* Opmerkingen bij de Chinees ondersteuning

 - In de `zh-cn` cultuur, LUIS verwacht de vereenvoudigd Chinees tekenset in plaats van de traditionele tekenset.
 - De namen van intents, entiteiten, functies en reguliere expressies mogelijk Chinese of Romeins tekens.
 - Zie de [vooraf gedefinieerde domeinen verwijzing ](luis-reference-prebuilt-domains.md) voor informatie over de vooraf gedefinieerde domeinen ondersteunde in de `zh-cn` cultuur.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>* Opmerkingen bij de Japanse ondersteuning

 - Omdat LUIS geen syntactische analyse biedt en wordt niet het verschil tussen Keigo en informele Japans, moet u gebruikmaken van de verschillende niveaus van formaliteit als voorbeelden training voor uw toepassingen. 
     - でございます is niet hetzelfde zijn als です. 
     - です is niet hetzelfde zijn als だ. 

### <a name="text-analytics-support-notes"></a>** Tekst analytics ondersteuning voor opmerkingen
Alleen Portugees wordt ondersteund voor overenting: `pt-PT` en `pt-BR`. Alle andere culturen worden ondersteund op het niveau van de primaire cultuur. Meer informatie over Tekstanalyse [ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). 

### <a name="speech-api-supported-languages"></a>Spraak ondersteunde API-talen
Zie spraak [ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) voor spraak dicteren modus talen.

### <a name="bing-spell-check-supported-languages"></a>Bing spellingcontrole ondersteunde talen
Zie spellingcontrole van Bing [ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) voor een lijst van ondersteunde talen en status.

## <a name="rare-or-foreign-words-in-an-application"></a>Zeldzame of afwijkende woorden in een toepassing
In de `en-us` cultuur, LUIS leert te onderscheiden van de meest Nederlandse woorden, met inbegrip van taalgebruik. In de `zh-cn` cultuur, LUIS leert te onderscheiden van de meeste Chinese tekens. Als u een zeldzame word in `en-us` of character in `zh-cn`, en u ziet dat LUIS kan niet worden onderscheiden van dit woord of teken lijkt, kunt u dat woord toevoegen of teken naar een [functie woordgroep lijst](luis-how-to-add-features.md). Bijvoorbeeld, moeten woorden buiten de cultuur van de toepassing--dat wil zeggen, vreemde woorden--worden toegevoegd aan een functie woordgroep-lijst. Deze woordgroepenlijst moet de status niet-verwisselbaar, om aan te geven dat de set van zeldzame woorden een klasse die LUIS moet meer vormt herkennen, maar ze geen synoniemen zijn of uitwisselbaar met elkaar.

### <a name="hybrid-languages"></a>Hybride-talen
Hybride talen combineren woorden uit twee culturen zoals Engels en Chinees. Deze talen worden niet ondersteund in LUIS omdat een app is gebaseerd op een enkele cultuur.

## <a name="tokenization"></a>Tokeniseren
Als u wilt uitvoeren in machine learning, verdeelt LUIS een utterance in [tokens](luis-glossary.md#token) op basis van cultuur. 

|Taal|  elke spatie of speciaal teken | niveau tekens|samengestelde woorden|[Tokens entiteit geretourneerd](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Chinees||✔||✔|
|Nederlands|||✔|✔|
|Engels (en-us)|✔ ||||
|Frans (fr-FR)|✔||||
|Frans (fr-CA)|✔||||
|Duits|||✔|✔|
|Italiaans|✔||||
|Japans||||✔|
|Koreaans||✔||✔|
|Portugees (Brazilië)|✔||||
|Spaans (es-ES)|✔||||
|Spaans (es-MX)|✔||||

 