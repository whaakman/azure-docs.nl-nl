---
title: Taalondersteuning - Translator Text-API
titleSuffix: Azure Cognitive Services
description: Een lijst van natuurlijke talen die worden ondersteund door de Translator Text-API.
services: cognitive-services
author: Jann-Skotdal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/21/2019
ms.author: v-jansko
ms.openlocfilehash: 3a99c0157ef3afe9823d73379775ddcf564faccc
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57193803"
---
# <a name="language-and-region-support-for-the-translator-text-api"></a>Ondersteuning voor taal en regio voor de Translator Text-API

De Translator Text-API ondersteunt de volgende talen voor tekst naar tekst converteren. Neurale machinevertalingen (NMT) is de nieuwe standaard voor AI aangestuurde automatische vertalingen van hoge kwaliteit en is beschikbaar als de standaard met V3 van de Translator Text-API wanneer een neurale systeem beschikbaar is.

[Meer informatie over de werking van automatische vertaling](https://www.microsoft.com/translator/mt.aspx)

**V2-Translator API**

> [!NOTE]
> V2 op 30 April 2018 is afgeschaft en zal worden buiten gebruik gesteld op 30 April 2019.

* Statistisch: Er is geen neurale system is beschikbaar voor deze taal.
* Neurale beschikbaar: Er is een neurale systeem beschikbaar. Gebruik de parameter `category=generalnn` toegang tot het neurale systeem.
* Neurale standaard: Neurale is de vertaling van het systeem. Gebruik de parameter `category=smt` voor toegang tot de statistische systeem voor gebruik met de Microsoft Translator-Hub.
* Neurale alleen: Alleen neurale vertalingen is beschikbaar.

**V3-API van Translator** de Translator-API V3 is standaard neurale en statistische systemen zijn alleen beschikbaar wanneer geen neurale systeem bestaat. Aangepaste Translator kan alleen worden gebruikt met neurale talen. [Weergeven van de talen die beschikbaar zijn in aangepaste Translator](#customization).

|Taal|  Taalcode|  V2 API| V3 API|
|:-----|:-----:|:-----|:-----|
|Afrikaans| `af`    |Alleen statistisch|  Neurale|
|Arabisch|    `ar`    |Neurale beschikbaar|  Neurale|
|Bengalees|    `bn`    |Neurale beschikbaar|  Neurale|
|Bosnisch (Latijns)|   `bs`    |Neurale beschikbaar|  Neurale|
|Bulgaars| `bg`    |Neurale beschikbaar|  Neurale|
|Kantonees (Traditioneel)|   `yue`   |Alleen statistisch|  Statistische|
|Catalaans|   `ca`    |Alleen statistisch|  Statistische|
|Vereenvoudigd Chinees|    `zh-Hans`   |Neurale standaard |Neurale|
|Traditioneel Chinees|   `zh-Hant`   |Neurale standaard |Neurale|
|Kroatisch|  `hr`    |Neurale beschikbaar|  Neurale|
|Tsjechisch| `cs`    |Neurale beschikbaar|  Neurale|
|Deens|    `da`    |Neurale beschikbaar   |Neurale|
|Nederlands| `nl`    |Neurale beschikbaar|  Neurale|
|Engels|   `en`    |Neurale beschikbaar|  Neurale|
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
|Malagassisch|  `mg`    |Alleen statistisch|  Statistische|
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
|Tongaans|    `to`    |Alleen statistisch|  Statistische|
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
| Arabisch | `ar` | Arabisch `Arab` | <--> | Latijns `Latn` |
|Bengalees  | `bn` | Bengali `Beng` | <--> | Latijns `Latn` |
| Chinees (vereenvoudigd) | `zh-Hans` | Vereenvoudigd Chinees `Hans`| <--> | Latijns `Latn` |
| Chinees (vereenvoudigd) | `zh-Hans` | Vereenvoudigd Chinees `Hans`| <--> | Traditioneel Chinees `Hant`|
| Chinees (traditioneel) | `zh-Hant` | Traditioneel Chinees `Hant`| <--> | Latijns `Latn` |
| Chinees (traditioneel) | `zh-Hant` | Traditioneel Chinees `Hant`| <--> | Vereenvoudigd Chinees `Hans` |
| Gujarati | `gu`  | Gujarati `Gujr` | --> | Latijns `Latn` |
| Hebreeuws | `he` | Hebreeuws `Hebr` | <--> | Latijns `Latn` |
| Hindi | `hi` | Devanagari `Deva` | <--> | Latijns `Latn` |
| Japans | `ja` | Japans `Jpan` | <--> | Latijns `Latn` |
| Kanarees | `kn` | Kannada `Knda` | --> | Latijns `Latn` |
| Malayalam | `ml` | Malayalam `Mlym` | --> | Latijns `Latn` |
| Mahratti | `mr` | Devanagari `Deva` | --> | Latijns `Latn` |
| Odia | `or` | Oriya `Orya` | <--> | Latijns `Latn` |
| Punjabi | `pa` | Gurmukhi `Guru`  | <--> | Latijns `Latn`  |
| Servisch (Cyrillisch) | `sr-Cyrl` | Cyrillisch `Cyrl`  | --> | Latijns `Latn` |
| Servisch (Latijns) | `sr-Latn` | Latijns `Latn` | --> | Cyrillisch `Cyrl`|
| Tamil | `ta` | Tamil `Taml` | --> | Latijns `Latn` |
| Telugu | `te` | Telugu `Telu` | --> | Latijns `Latn` |
| Thais | `th` | Thais `Thai` | <--> | Latijns `Latn` |

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

## <a name="detect"></a>Detect

De volgende talen worden ondersteund door de analyse-methode. Detecteren van mei identificeren van de talen die de Microsoft Translator-kan niet worden geconverteerd.

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
| Engels |
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

## <a name="access-the-translator-text-api-language-list-programmatically"></a>Via een programma toegang krijgen tot de lijst van de taal Translator Text-API

U kunt een lijst met ondersteunde talen voor de Translator Text-API-v3.0 met behulp van de methode talen ophalen. U kunt de lijst met door de functie, taal, evenals de naam van de taal weergeven in het Engels of een andere ondersteunde taal. Deze lijst wordt automatisch bijgewerkt door de Microsoft Translator-service wanneer nieuwe talen beschikbaar worden gesteld.

[Bekijk de naslagdocumentatie van talen bewerking](reference/v3-0-languages.md)

## <a name="customization"></a>Aanpassing

De volgende talen zijn beschikbaar voor aanpassing naar of van het gebruik van de Engelse [aangepaste Translator](http://aka.ms/CustomTranslator).

| Taal    | Taalcode |
|:----------- |:-------------:|
| Arabisch       | `ar`          |
| Bengalees      | `bn`          |
| Bosnisch (Latijns)      | `bs`          |
| Bulgaars      | `bg`          |
| Vereenvoudigd Chinees      | `zh-Hans`          |
| Kroatisch      | `hr`          |
| Tsjechisch      | `cs`          |
| Deens      | `da`          |
| Nederlands      | `nl`          |
| Nederlands    | `en`     |
| Estisch      | `et`          |
| Fins      | `fi`          |
| Frans      | `fr`          |
| Duits      | `de`          |
| Grieks      | `el`          |
| Hebreeuws      | `he`          |
| Hindi      | `hi`          |
| Hongaars      | `hu`          |
| Italiaans      | `it`          |
| Japans      | `ja`          |
| Koreaans      | `ko`          |
| Lets      | `lv`          |
| Litouws      | `lt`          |
| Noors      | `nb`          |
| Pools      | `pl`          |
| Portugees      | `pt`          |
| Roemeens      | `ro`          |
| Russisch      | `ru`          |
| Servisch (Latijns)      | `sr-Latn`          |
| Slowaaks     | `sk`          |
| Sloveens      | `sl`          |
| Spaans      | `es`          |
| Zweeds      | `sv`          |
| Thais      | `th`          |
| Turks      | `tr`          |
| Oekraïens      | `uk`          |
| Vietnamees      | `vi`          |

## <a name="access-the-list-on-the-microsoft-translator-website"></a>Toegang tot de lijst op de website van Microsoft Translator

Voor een kort overzicht van de talen ziet u de website van Microsoft Translator-alle talen die worden ondersteund door de Translator tekst en spraak-API's. Deze lijst bevat geen developer-specifieke informatie zoals taalcodes.

[Zie de lijst met talen](https://www.microsoft.com/translator/languages.aspx)
