---
title: Taalondersteuning - Translator Text-API
titleSuffix: Azure Cognitive Services
description: Een lijst van natuurlijke talen die worden ondersteund door de Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 09/25/2018
ms.author: v-jansko
ms.openlocfilehash: 0b1187083c14fc7c536f6a32f3a41957f53f299b
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679712"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Ondersteuning voor taal en regio voor de Translator Text-API

De Translator Text-API ondersteunt de volgende talen voor tekst naar tekst converteren. Neurale machinevertalingen (NMT) is de nieuwe standaard voor AI aangestuurde automatische vertalingen van hoge kwaliteit en is beschikbaar als de standaard met V3 van de Translator Text-API wanneer een neurale systeem beschikbaar is. 

[Meer informatie over de werking van automatische vertaling](https://www.microsoft.com/translator/mt.aspx)

**V2-Translator API**

> [!NOTE]
> V2 op 30 April 2018 is afgeschaft en zal worden buiten gebruik gesteld op 30 April 2019.

* Alleen statistisch: Er is geen neurale systeem is beschikbaar voor deze taal.
* Neurale beschikbaar: een neurale systeem beschikbaar is. Gebruik de parameter `category=generalnn` toegang tot het neurale systeem.
* Neurale standaard: Neurale is de vertaling van het systeem. Gebruik de parameter `category=smt` voor toegang tot de statistische systeem voor gebruik met de Microsoft Translator-Hub.
* Alleen Neurale: slechts neurale vertalingen beschikbaar is.

**V3-API van Translator** de Translator-API V3 is standaard neurale en statistische systemen zijn alleen beschikbaar wanneer geen neurale systeem bestaat. Aangepaste Translator kan alleen worden gebruikt met neurale talen. 

|Taal|  Taalcode|  V2-API| V3-API|
|:-----|:-----:|:-----|:-----|
|Afrikaans| `af`    |Alleen statistisch|  Neurale|
|Arabisch|    `ar`    |Neurale beschikbaar|  Neurale|
|Arabisch, Levantine| `apc`   |Neurale beschikbaar|  Neurale|
|Bengalees|    `bn`    |Neurale beschikbaar|  Neurale|
|Bosnisch (Latijns)|   `bs`    |Alleen statistisch|  Statistische|
|Bulgaars| `bg`    |Neurale beschikbaar|  Neurale|
|Kantonees (Traditioneel)|   `yue`   |Alleen statistisch|  Statistische|
|Catalaans|   `ca`    |Alleen statistisch|  Statistische|
|Vereenvoudigd Chinees|    `zh-Hans`   |Neurale standaard |Neurale|
|Traditioneel Chinees|   `zh-Hant`   |Neurale standaard |Neurale|
|Kroatisch|  `hr`    |Neurale beschikbaar|  Neurale|
|Tsjechisch| `cs`    |Neurale beschikbaar|  Neurale|
|Deens|    `da`    |Neurale beschikbaar   |Neurale|
|Nederlands| `nl`    |Neurale beschikbaar|  Neurale|
|Nederlands|   `en`    |Neurale beschikbaar|  Neurale|
|Estisch|  `et`    |Neurale beschikbaar|  Neurale|
|Fiji|    `fj`    |Alleen statistisch|  Statistische|
|Filipijns|  `fil`   |Alleen statistisch|  Statistische|
|Fins|   `fi`    |Neurale beschikbaar|  Neurale|
|Frans|    `fr`    |Neurale beschikbaar|  Neurale|
|Duits|    `de`    |Neurale beschikbaar|  Neurale|
|Grieks| `el`    |Neurale beschikbaar|  Neurale|
|Haïtiaans|    `ht`    |Alleen statistisch   |Statistische|
|Hebreeuws |`he`   |Neurale beschikbaar   |Neurale|
|Hindi| `hi`    |Neurale standaard|    Neurale|
|Hmong Daw| `mww`   |Alleen statistisch|  Statistische|
|Hongaars| `hu`    |Neurale beschikbaar|  Neurale|
|IJslands| `is`    |Alleen Neurale|   Neurale|
|Indonesisch|    `id`    |Alleen statistisch|  Statistische|
|Italiaans|   `it`    |Neurale beschikbaar|  Neurale|
|Japans|  `ja`    |Neurale beschikbaar|  Neurale|
|Swahili| `sw`    |Alleen statistisch|  Statistische|
|Klingon|   `tlh`   |Alleen statistisch|  Statistische|
|Klingon (plqaD)|   `tlh-Qaak`  |Alleen statistisch|  Statistische|
|Koreaans |`ko`   |Neurale beschikbaar|  Neurale|
|Lets|   `lv`    |Neurale beschikbaar|  Neurale|
|Litouws|    `lt`    |Neurale beschikbaar|  Neurale|
|Malagassische|  `mg`    |Alleen statistisch|  Statistische|
|Maleis| `ms`    |Alleen statistisch   |Statistische|
|Maltees|   `mt`    |Alleen statistisch|  Statistische|
|Noors| `nb`    |Neurale beschikbaar|  Neurale|
|Perzisch|   `fa`    |Alleen statistisch|  Statistische|
|Pools|    `pl`    |Neurale beschikbaar|  Neurale|
|Portugees|    `pt`    |Neurale beschikbaar|  Neurale|
|Queretaro Otomi|   `otq`   |Alleen statistisch|  Statistische|
|Roemeens|  `ro`    |Neurale beschikbaar|  Neurale|
|Russisch|   `ru`    |Neurale beschikbaar|  Neurale|
|Samoaanse|    `sm`    |Alleen statistisch|  Statistische|
|Servisch (Cyrillisch)|    `sr-Cyrl`   |Alleen statistisch|  Statistische|
|Servisch (Latijns)|   `sr-Latn`   |Alleen statistisch   |Statistische|
|Slowaaks|    `sk`    |Neurale beschikbaar|  Neurale|
|Sloveens| `sl`    |Neurale beschikbaar|  Neurale|
|Spaans|   `es`    |Neurale beschikbaar|  Neurale|
|Zweeds|   `sv`    |Neurale beschikbaar   |Neurale|
|Tahitian|  `ty`    |Alleen statistisch|  Statistische|
|Tamil| `ta`    |Alleen statistisch|  Statistische|
|Telugu|    `te`    |Alleen Neurale|   Neurale|
|Thais|  `th`    |Neurale beschikbaar|  Neurale|
|Tongan|    `to`    |Alleen statistisch|  Statistische|
|Turks|   `tr`    |Neurale beschikbaar   |Neurale|
|Oekraïens| `uk`    |Neurale beschikbaar|  Neurale|
|Urdu|  `ur`    |Alleen statistisch|  Statistische|
|Vietnamees|    `vi`    |Neurale beschikbaar|  Neurale|
|Welsh| `cy`    |Neurale beschikbaar|  Neurale|
|Yucatec Maya|  `yua`   |Alleen statistisch|  Statistische|

## <a name="transliteration"></a>Transliteratie

De methode Transliterate ondersteunt de volgende talen. In de 'naar/van", '<>--' geeft aan dat de taal, kan worden getranscribeerd van of naar een van de scripts die worden vermeld. De '-->' geeft aan dat de taal kan alleen worden, getranscribeerd van één script naar de andere.

| Taal    | Taalcode | Script | Aan/uit | Script|
|:----------- |:-------------:|:-------------:|:-------------:|:-------------:|
| Arabisch | ar | Arabisch | <--> | Latijns |
|Bengalees  | bn | Bengaals | <--> | Latijns |
| Chinees (vereenvoudigd) | zh-Hans | Vereenvoudigd Chinees | <--> | Latijns |
| Chinees (vereenvoudigd) | zh-Hans | Vereenvoudigd Chinees | <--> | Traditioneel Chinees |
| Chinees (traditioneel) | zh-Hant | Traditioneel Chinees | <--> | Latijns |
| Chinees (traditioneel) | zh-Hant | Traditioneel Chinees | <--> | Vereenvoudigd Chinees |
| Gujarati | Gu  | Gujarati | --> | Latijns |
| Hebreeuws | hij | Hebreeuws | <--> | Latijns |
| Hindi | Hallo | Devanagari | <--> | Latijns |
| Japans | ja | Japans | <--> | Latijns |
| Kanarees | kN | Kanarees | --> | Latijns |
| Malayalam | ml | Malayalam | --> | Latijns |
| Mahratti | MR | Devanagari | --> | Latijns |
| Odia | of | Odia | <--> | Latijns |
| Punjabi | Pa | Gurmukhi | <--> | Latijns  |
| Servisch (Cyrillisch) | SR-Cyrl | Cyrillisch  | --> | Latijns |
| Servisch (Latijns) | SR-Latn | Latijns | --> | Cyrillisch |
| Tamil | TA | Tamil | --> | Latijns |
| Telugu | Ken | Telugu | --> | Latijns |
| Thais | e | Thais | <--> | Latijns |

## <a name="dictionary"></a>Woordenlijst

De woordenlijst ondersteunt de volgende talen naar of van Engels met behulp van de opzoek- en voorbeeldmethoden.

| Taal    | Taalcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalees      | `bn`          |
| Bosnisch (Latijns)      | `bs`          |
| Bulgaars      | `bg`          |
| Catalaans      | `ca`          |
| Vereenvoudigd Chinees      | `zh-Hans`          |
| Kroatisch      | `hr`          |
| Tsjechisch      | `cs`          |
| Deens      | `da`          |
| Nederlands      | `nl`          |
| Estisch      | `et`          |
| Fins      | `fi`          |
| Frans      | `fr`          |
| Duits      | `de`          |
| Grieks      | `el`          |
| Haïtiaans      | `ht`          |
| Hebreeuws      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Hongaars      | `hu`          |
| IJslands    | `is`  |
| Indonesisch      | `id`          |
| Italiaans      | `it`          |
| Japans      | `ja`          |
| Swahili      | `sw`          |
| Klingon      | `tlh`          |
| Koreaans      | `ko`          |
| Lets      | `lv`          |
| Litouws      | `lt`          |
| Maleis      | `ms`          |
| Maltees      | `mt`          |
| Noors      | `nb`          |
| Perzisch      | `fa`          |
| Pools      | `pl`          |
| Portugees      | `pt`          |
| Roemeens      | `ro`          |
| Russisch      | `ru`          |
| Servisch (Latijns)      | `sr-Latn`          |
| Slowaaks     | `sk`          |
| Sloveens      | `sl`          |
| Spaans      | `es`          |
| Zweeds      | `sv`          |
| Tamil      | `ta`          |
| Thais      | `th`          |
| Turks      | `tr`          |
| Oekraïens      | `uk`          |
| Urdu      | `ur`          |
| Vietnamees      | `vi`          |
| Welsh      | `cy`          |

## <a name="languages-detected-by-the-detect-method"></a>Talen die zijn gedetecteerd door de analyse-methode

De volgende talen kunnen worden gedetecteerd door de analyse-methode. Detecteren van mei detecteren van talen die Microsoft Translator kan niet worden geconverteerd.

| Taal    |
|:----------- |
| Afrikaans |
| Albanees |
| Arabisch |
| Baskisch |
| Belarussisch |
| Bulgaars |
| Catalaans |
| Chinees |
| Chinees (vereenvoudigd) |
| Chinees (traditioneel) |
| Kroatisch |
| Tsjechisch |
| Deens |
| Nederlands |
| Nederlands |
| Esperanto |
| Estisch |
| Fins |
| Frans |
| Galicisch |
| Duits |
| Grieks |
| Haïtiaans |
| Hebreeuws |
| Hindi |
| Hongaars |
| IJslands |
| Indonesisch |
| Iers |
| Italiaans |
| Japans |
| Koreaans |
| Koerdisch (Arabisch) |
| Koerdisch (Latijns) |
| Latijns |
| Lets |
| Litouws |
| Macedonisch |
| Maleis |
| Maltees |
| Noors |
| Noors (Nynorsk) |
| Pashto |
| Perzisch |
| Pools |
| Portugees |
| Roemeens |
| Russisch |
| Servisch (Cyrillisch) |
| Servisch (Latijns) |
| Slowaaks |
| Sloveens |
| Somali |
| Spaans |
| Swahili |
| Zweeds |
| Tagalog |
| Telugu |
| Thais |
| Turks |
| Oekraïens |
| Urdu |
| Oezbeeks (Cyrillisch) |
| Oezbeeks (Latijns) |
| Vietnamees |
| Welsh |
| Jiddisch |

## <a name="access-the-list-programmatically"></a>Via een programma toegang krijgen tot de lijst

U kunt toegang tot de lijst met ondersteunde talen programmatisch met behulp van de werking van de talen van de V3.0 Text-API. U kunt de lijst met door de functie, taal, evenals de naam van de taal weergeven in het Engels of een andere ondersteunde taal. Deze lijst wordt automatisch bijgewerkt door de service Microsoft Translator zodra nieuwe talen beschikbaar.

[Bekijk de naslagdocumentatie van talen bewerking](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Toegang tot de lijst op de website van Microsoft Translator

Voor een kort overzicht van de talen ziet u de website van Microsoft Translator-alle talen die worden ondersteund door de Translator tekst en spraak-API's. Deze lijst bevat geen developer-specifieke informatie zoals taalcodes.

[Zie de lijst met talen](https://www.microsoft.com/translator/languages.aspx)
