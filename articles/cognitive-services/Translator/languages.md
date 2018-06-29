---
title: Ondersteunde talen in de Microsoft Translator-API | Microsoft Docs
description: Talen die worden ondersteund door de API van Microsoft Translator tekst weergeven.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: v-jansko
ms.openlocfilehash: 5542c1c0eee5e4c7c85c566908dc8999b118f06e
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37032450"
---
# <a name="supported-languages-in-the-microsoft-translator-text-api"></a>Ondersteunde talen in de API van Microsoft Translator tekst

De API van Microsoft Translator tekst ondersteunt de volgende talen voor de vertaling van tekst naar tekst. Neural machinevertaling (NMT) is de nieuwe standaard voor vertalingen van hoge kwaliteit machine AI van stroom voorzien en is beschikbaar als de standaard met V3 van de API van de tekst conversieprogramma wanneer een neural systeem beschikbaar is. Neural machinevertaling is beschikbaar in V2 met behulp van de categorie 'generalnn'.

[Meer informatie over de werking van automatische vertalingen](https://www.microsoft.com/translator/mt.aspx)

| Taal    | De vertaling van Type |Taalcode |
|:----------- |:-------:|:-------------:|
| Afrikaans      | Statistische |`af`          |
| Arabisch      | Neural | `ar`          |
| Arabisch, Levantine    | Neural | `apc`
| Bengalees      | Neural |`bn`          |
| Bosnisch (Latijns)      | Statistische |`bs`          |
| Bulgaars     |  Neural |`bg`          |
| Kantonees (Traditioneel)      | Statistische |`yue`          |
| Catalaans      | Statistische |`ca`          |
| Vereenvoudigd Chinees        |  Neural |`zh-Hans`          | 
| Traditioneel Chinees        |  Neural |`zh-Hant`          |
| Kroatisch      | Neural |`hr`          |
| Tsjechisch        |  Neural |`cs`          |
| Deens        |  Neural |`da`          |
| Nederlands        |  Neural |`nl`          |
| Nederlands       |  Neural |`en`          |
| Estisch      | Neural |`et`          |
| Fiji      | Statistische |`fj`          |
| Filipijns      | Statistische |`fil`          |
| Fins      | Neural |`fi`          |
| Frans        |  Neural |`fr`          |
| Duits       |  Neural |`de`          |
| Grieks      | Neural |`el`          |
| Haïtiaans      | Statistische |`ht`          |
| Hebreeuws      | Neural |`he`          |
| Hindi        |  Neural |`hi`          |
| Hmong Daw      | Statistische |`mww`          |
| Hongaars      | Neural |`hu`          |
| IJslands      |  Neural |`is`           |
| Indonesisch      | Statistische |`id`          |
| Italiaans        |  Neural |`it`          |
| Japans        |  Neural |`ja`          |
| Swahili      | Statistische |`sw`          |
| Klingon      | Statistische |`tlh`          |
| Klingon (plqaD)      | Statistische |`tlh-Qaak`          |
| Koreaans        |  Neural |`ko`          |
| Lets      | Neural |`lv`          |
| Litouws      | Neural |`lt`          |
| Malagassische      | Statistische |`mg`          |
| Maleis      | Statistische |`ms`          |
| Maltees      | Statistische |`mt`          |
| Noors        |  Neural |`nb`          |
| Perzisch      | Statistische |`fa`          |
| Pools        |  Neural |`pl`          |
| Portugees        |  Neural |`pt`          |
| Queretaro Otomi      | Statistische |`otq`          |
| Roemeens        |  Neural |`ro`          |
| Russisch        |  Neural |`ru`          |
| Samoaanse      | Statistische |`sm`          |
| Servisch (Cyrillisch)      | Statistische |`sr-Cyrl`          |
| Servisch (Latijns)      | Statistische |`sr-Latn`          |
| Slowaaks     | Neural |`sk`          |
| Sloveens      | Neural |`sl`          |
| Spaans        |  Neural |`es`          |
| Zweeds        |  Neural |`sv`          |
| Tahitian      | Statistische |`ty`          |
| Tamil      | Statistische |`ta`          |
| Thais      | Neural |`th`          |
| Tongan      | Statistische |`to`          |
| Turks       |  Neural |`tr`          |
| Oekraïens      | Neural |`uk`          |
| Urdu      | Statistische |`ur`          |
| Vietnamees      | Neural |`vi`          |
| Welsh      | Neural |`cy`          |
| Yucatec Maya      | Statistische |`yua`          |

## <a name="transliteration"></a>Transliteratie

De methode Transliterate ondersteunt de volgende talen. In de '/ naar', '<>--' geeft aan dat de taal, kan worden getranscribeerd van of naar een van de scripts die worden vermeld. De '-->' geeft aan dat de taal kan alleen worden, getranscribeerd van een script naar het andere.

| Taal    | Taalcode | Script | / Naar | Script|
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
| Tamil | DBC | Tamil | --> | Latijns |
| Telugu | eren | Telugu | --> | Latijns |
| Thais | e | Thais | <--> | Latijns |

## <a name="dictionary"></a>Woordenlijst

Het woordenboek ondersteunt de volgende talen naar of van Engels met de methoden opzoeken en voorbeelden.

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

## <a name="languages-detected-by-the-detect-method"></a>Talen die zijn gedetecteerd door de methode detecteren

De volgende talen kunnen worden gedetecteerd door de methode detecteren. Kan detecteren detecteren talen die Microsoft Translator kan niet worden geconverteerd.

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
| Chinese_Simplified |
| Chinese_Traditional |
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
| Haitian_Creole |
| Hebreeuws |
| Hindi |
| Hongaars |
| IJslands |
| Indonesisch |
| Iers |
| Italiaans |
| Japans |
| Koreaans |
| Kurdish_Arabic |
| Kurdish_Latin |
| Latijns |
| Lets |
| Litouws |
| Macedonisch |
| Maleis |
| Maltees |
| Noors |
| Norwegian_Nynorsk |
| Pashto |
| Perzisch |
| Pools |
| Portugees |
| Roemeens |
| Russisch |
| Serbian_Cyrillic |
| Serbian_Latin |
| Slowaaks |
| Sloveens |
| Somali |
| Spaans |
| Swahili |
| Zweeds |
| Tagalog |
| Thais |
| Turks |
| Oekraïens |
| Urdu |
| Uzbek_Cyrillic |
| Uzbek_Latin |
| Vietnamees |
| Welsh |
| Jiddisch |

## <a name="access-the-list-programmatically"></a>Programmatisch toegang krijgen tot de lijst

U kunt toegang tot de lijst met ondersteunde talen programmatisch met behulp van de werking van de talen van de tekst V3.0 API. U kunt de lijst door de functie, taalcode, evenals de naam van de taal weergeven in het Engels of elke ondersteunde taal. Deze lijst wordt automatisch bijgewerkt door de service Microsoft Translator zodra nieuwe talen beschikbaar.

[Bekijk de naslagdocumentatie bewerking talen](reference/v3-0-languages.md)

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Toegang tot de lijst op de website van Microsoft Translator

Voor een kort overzicht van de talen ziet u de website Microsoft Translator alle talen die worden ondersteund door de vertaler tekst en Speech-API's. Deze lijst bevat geen developer-specifieke informatie zoals taalcodes.

[Zie de lijst met talen](https://www.microsoft.com/translator/languages.aspx)
