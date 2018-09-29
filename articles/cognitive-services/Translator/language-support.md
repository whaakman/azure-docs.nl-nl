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
ms.openlocfilehash: 775e098eb2a067e3e0446bccc223c1c54e082347
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435479"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Ondersteuning voor taal en regio voor de Translator Text-API

De Translator Text-API ondersteunt de volgende talen voor tekst naar tekst converteren. Neurale machinevertalingen (NMT) is de nieuwe standaard voor AI aangestuurde automatische vertalingen van hoge kwaliteit en is beschikbaar als de standaard met V3 van de Translator Text-API wanneer een neurale systeem beschikbaar is. Neurale machinevertalingen is beschikbaar in V2 met behulp van de categorie 'generalnn'.

[Meer informatie over de werking van automatische vertaling](https://www.microsoft.com/translator/mt.aspx)

| Taal    | Type van de vertaling |Taalcode |
|:----------- |:-------:|:-------------:|
| Afrikaans      | Statistische |`af`          |
| Arabisch      | Neurale | `ar`          |
| Arabisch, Levantine    | Neurale | `apc`
| Bengalees      | Neurale |`bn`          |
| Bosnisch (Latijns)      | Statistische |`bs`          |
| Bulgaars     |  Neurale |`bg`          |
| Kantonees (Traditioneel)      | Statistische |`yue`          |
| Catalaans      | Statistische |`ca`          |
| Vereenvoudigd Chinees        |  Neurale |`zh-Hans`          |
| Traditioneel Chinees        |  Neurale |`zh-Hant`          |
| Kroatisch      | Neurale |`hr`          |
| Tsjechisch        |  Neurale |`cs`          |
| Deens        |  Neurale |`da`          |
| Nederlands        |  Neurale |`nl`          |
| Nederlands       |  Neurale |`en`          |
| Estisch      | Neurale |`et`          |
| Fiji      | Statistische |`fj`          |
| Filipijns      | Statistische |`fil`          |
| Fins      | Neurale |`fi`          |
| Frans        |  Neurale |`fr`          |
| Duits       |  Neurale |`de`          |
| Grieks      | Neurale |`el`          |
| Haïtiaans      | Statistische |`ht`          |
| Hebreeuws      | Neurale |`he`          |
| Hindi        |  Neurale |`hi`          |
| Hmong Daw      | Statistische |`mww`          |
| Hongaars      | Neurale |`hu`          |
| IJslands      |  Neurale |`is`           |
| Indonesisch      | Statistische |`id`          |
| Italiaans        |  Neurale |`it`          |
| Japans        |  Neurale |`ja`          |
| Swahili      | Statistische |`sw`          |
| Klingon      | Statistische |`tlh`          |
| Klingon (plqaD)      | Statistische |`tlh-Qaak`          |
| Koreaans        |  Neurale |`ko`          |
| Lets      | Neurale |`lv`          |
| Litouws      | Neurale |`lt`          |
| Malagassische      | Statistische |`mg`          |
| Maleis      | Statistische |`ms`          |
| Maltees      | Statistische |`mt`          |
| Noors        |  Neurale |`nb`          |
| Perzisch      | Statistische |`fa`          |
| Pools        |  Neurale |`pl`          |
| Portugees        |  Neurale |`pt`          |
| Queretaro Otomi      | Statistische |`otq`          |
| Roemeens        |  Neurale |`ro`          |
| Russisch        |  Neurale |`ru`          |
| Samoaanse      | Statistische |`sm`          |
| Servisch (Cyrillisch)      | Statistische |`sr-Cyrl`          |
| Servisch (Latijns)      | Statistische |`sr-Latn`          |
| Slowaaks     | Neurale |`sk`          |
| Sloveens      | Neurale |`sl`          |
| Spaans        |  Neurale |`es`          |
| Zweeds        |  Neurale |`sv`          |
| Tahitian      | Statistische |`ty`          |
| Tamil      | Statistische |`ta`          |
| Telugu   | Neurale   | `te` |
| Thais      | Neurale |`th`          |
| Tongan      | Statistische |`to`          |
| Turks       |  Neurale |`tr`          |
| Oekraïens      | Neurale |`uk`          |
| Urdu      | Statistische |`ur`          |
| Vietnamees      | Neurale |`vi`          |
| Welsh      | Neurale |`cy`          |
| Yucatec Maya      | Statistische |`yua`          |

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
| Malasian | ml | Malayalam | --> | Latijns |
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
