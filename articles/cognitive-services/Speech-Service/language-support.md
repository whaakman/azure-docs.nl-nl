---
title: Taalondersteuning - Speech Service-API
titleSuffix: Azure Cognitive Services
description: De Azure-Services voor spraak-ondersteuning voor vele talen voor spraak-naar-tekst en spraak conversie, samen met spraakomzetting. Dit artikel bevat een uitgebreide lijst van taalondersteuning door de service.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 688194c8d5ec0c2594d8c7178d8cae3925b8e482
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54413364"
---
# <a name="language-and-region-support-for-speech-service-api"></a>Ondersteuning voor spraak-API voor Service taal en regio

Andere talen worden ondersteund voor verschillende functies van de spraakservices. De volgende tabellen geven een overzicht van ondersteuning voor talen.

## <a name="speech-to-text"></a>Spraak-naar-tekst

De Microsoft-spraakherkenning API ondersteunt de volgende talen. Verschillende niveaus van aanpassingen zijn beschikbaar voor elke taal.

  Code | Taal | [Akoestische aanpassing](how-to-customize-acoustic-models.md) | [Aanpassing van taal](how-to-customize-language-model.md) | [Uitspraak van aanpassing](how-to-customize-pronunciation.md)
 ------|----------|---------------------|---------------------|-------------------------
 ar-bijvoorbeeld | Arabisch (Egypte), moderne standard | Nee | Ja | Nee
 CA-ES | Catalaans | Nee | Nee | Nee
 da-DK | Deens (Denemarken) | Nee | Nee | Nee
 de-DE | Duits (Duitsland) | Ja | Ja | Nee
 en-AU | Engels (Australië) | Nee | Ja | Ja
 NL-CA | Engels (Canada) | Nee | Ja | Ja
 en-GB | Engels (Verenigd Koninkrijk) | Nee | Ja | Ja
 NL-IN | English (India) | Ja | Ja | Ja
 NL-NZ | Engels (Nieuw-Zeeland) | Nee | Ja | Ja  
 en-US | Engels (Verenigde Staten) | Ja | Ja | Ja
 es-ES | Spaans (Spanje) | Ja | Ja | Nee
 es-MX | Spaans (Mexico) | Nee | Ja | Nee
 fi-FI | Fins (Finland) | Nee | Nee | Nee
 fr-CA | Frans (Canada) | Nee | Ja | Nee
 fr-FR | Frans (Frankrijk) | Ja | Ja | Nee
 hi-IN | Hindi (India) | Nee | Ja | Nee
 IT-IT | Italiaans (Italië) | Ja | Ja | Nee
 ja-JP | Japans (Japan) | Nee | Ja | Nee
 ko-KR | Koreaans (Korea) | Nee | Ja | Nee
 nb-NO | Noors (Bokmål) (Noorwegen) | Nee | Nee | Nee
 NL-NL | Nederlands (Nederland) | Nee | Ja | Nee
 pl-PL | Pools (Polen) | Nee | Nee | Nee
 pt-BR | Portugees (Brazilië) | Ja | Ja | Nee
 pt-PT | Portugees (Portugal) | Nee | Ja | Nee
 ru-RU | Russisch (Rusland) | Ja | Ja | Nee
 SV-SE | Zweeds (Zweden) | Nee | Nee | Nee
 zh-CN | Chinees (Mandarijn, vereenvoudigd) | Ja | Ja | Nee
 zh-HK | Chinees (Mandarijn, traditionele) | Nee | Ja | Nee
 zh-TW | Chinees (Taiwan Mandarijn) | Nee | Ja | Nee
 e-e | Thai (Thailand) | Nee | Nee | Nee


## <a name="text-to-speech"></a>Tekst naar spraak

De Text to Speech REST-API biedt ondersteuning voor deze stemmen, die ondersteuning biedt voor een specifieke taal en een regionale taal, geïdentificeerd door de landinstelling.

> [!IMPORTANT]
> Prijzen zijn afhankelijk van de standaard, aangepaste en neurale stemmen. Ga naar de [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/) pagina voor meer informatie.

### <a name="neural-voices-preview"></a>Neurale stemmen (Preview)

Neutrale tekst naar spraak is een nieuw type spraaksynthese mogelijk gemaakt door deep neural networks. Bij het gebruik van een neural stem is kunstmatige spraak bijna te onderscheiden van de menselijke opnamen.

Neurale stemmen kunnen worden gebruikt voor interactie met chatbots en virtuele assistenten natuurlijker maken en aantrekkelijke digitale teksten, zoals e-books converteren naar audiobooks en in de auto-navigatiesystemen te verbeteren. Met de menselijke natuurlijke prosody en duidelijke afbakening van woorden verkorten neural stemmen aanzienlijk luisterende moeheid wanneer gebruikers met AI-systemen communiceren.

Zie voor een volledige lijst van neurale stemmen en regionale beschikbaarheid, [regio's](regions.md#neural-voices).

| Landinstelling | Taal | Geslacht | De toewijzing van service|
|--------|----------|---------|--------------------|
| en-US | English (US) | Man | "Microsoft Server Speech Text to Speech Voice (en-US, GuyNeural)" |
| en-US | English (US) | Vrouw | "Microsoft Server Speech Text to Speech Voice (en-US, JessaNeural)" |

### <a name="standard-voices"></a>Standard stemmen

Meer dan 75 standard stemmen zijn beschikbaar in meer dan 45 talen en landinstellingen, waarmee u kunt tekst converteren naar kunstmatige spraak. Zie voor meer informatie over regionale beschikbaarheid [regio's](regions.md#standard-voices).

Landinstelling | Taal | Geslacht | De toewijzing van service
-------|----------|---------|--------------------
ar-bijvoorbeeld\* | Arabisch (Egypte) | Vrouw | "Microsoft Server spraak tekst en spraak spraak (ar-bijvoorbeeld Hoda)"
ar-SA | Arabisch (Saoedi-Arabië) | Man | "Microsoft Server spraak tekst en spraak, spraak (ar-SA, Naayf)"
bg-BG | Bulgaars | Man | "Microsoft Server tekst naar spraak stem (bg-BG, Ivan)"
CA-ES | Catalaans | Vrouw | "Microsoft Server tekst naar spraak stem (ca-ES, HerenaRUS)"
cs-CZ | Tsjechisch | Man | "Microsoft Server spraak tekst en spraak spraak (cs-CZ, Jakub)"
da-DK | Deens | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (da-DK, HelleRUS)"
de-AT | Duits (Oostenrijk) | Man | "Microsoft Server spraak tekst en spraak, spraak (de-AT, Michael)"
de CH | Duits (Zwitserland) | Man | "Microsoft Server spraak tekst en spraak, spraak (de-h, Karsten)"
de-DE | Duits (Duitsland) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, Hedda)"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, HeddaRUS)"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (nl-nl, Stefan, Apollo)"
el GR | Grieks | Man | "Microsoft Server spraak tekst en spraak, spraak (el-GR, Stefanos)"
en-AU | Engels (Australië) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-AU, Catherine)"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-AU, HayleyRUS)"
NL-CA | Engels (Canada) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-CA, Linda)"
| | | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-CA, HeatherRUS)"
en-GB | English (UK) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-GB, Susan, Apollo)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-GB, HazelRUS)"
| | |Man | "Microsoft Server spraak tekst en spraak, spraak (en-GB, George, Apollo)"
NL-Internet Explorer | Engels (Ierland) |Man | "Microsoft Server spraak tekst en spraak, spraak (en Internet Explorer, Jan)"
NL-IN | English (India) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-IN, Heera, Apollo)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-IN, PriyaRUS)"
| | |Man | "Microsoft Server spraak tekst en spraak, spraak (en-IN, Ravi, Apollo)"
en-US | English (US) |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, ZiraRUS)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, JessaRUS)"
| | |Man | "Microsoft Server spraak tekst en spraak, spraak (en-US, BenjaminRUS)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (en-US, Jessa24kRUS)"
| | |Man | "Microsoft Server spraak tekst en spraak, spraak (en-US, Guy24kRUS)"
es-ES | Spaans (Spanje) |Vrouw | "Microsoft Server tekst naar spraak stem (es-ES, Laura, Apollo)"
| | |Vrouw | "Microsoft Server tekst naar spraak stem (es-ES, HelenaRUS)"
| | |Man | "Microsoft Server tekst naar spraak stem (es-ES, Pablo, Apollo)"
es-MX | Spaans (Mexico) | Vrouw | "Microsoft Server-stem tekst naar spraak (es-MX, HildaRUS)"
| | | Man | "Microsoft Server-stem tekst naar spraak (es-MX, Raul, Apollo)"
fi-FI | Fins | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (fi-FI, HeidiRUS)"
fr-CA | Frans (Canada) |Vrouw | "Microsoft Server tekst naar spraak stem (fr-CA, Caroline)"
| | | Vrouw | "Microsoft Server tekst naar spraak stem (fr-CA, HarmonieRUS)"
FR-h | Frans (Zwitserland)|Man | "Microsoft Server tekst naar spraak stem (fr-h, Guillaume)"
fr-FR | Frans (Frankrijk)|Vrouw | "Microsoft Server tekst naar spraak stem (fr-FR, Julia, Apollo)"
| | |Vrouw | "Microsoft Server tekst naar spraak stem (fr-FR, HortenseRUS)"
| | |Man | "Microsoft Server tekst naar spraak stem (fr-FR, Paul, Apollo)"
hij IL| Hebreeuws (Israël) | Man| "Microsoft Server spraak tekst en spraak, spraak (hij-IL, Asaf)"
hi-IN | Hindi (India) | Vrouw | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Kalpana, Apollo)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Kalpana)"
| | | Man | "Microsoft Server spraak tekst en spraak, spraak (High-IN, Hemant)"
hr-HR | Kroatisch | Man | "Microsoft Server tekst naar spraak stem (hr-HR, Matej)"
hu-HU | Hongaars | Man | "Microsoft Server spraak tekst en spraak, spraak (hu-HU, Szabolcs)"
id-ID | Indonesisch| Man | "Microsoft Server spraak tekst en spraak, spraak (id-ID, Andika)"
IT-IT | Italiaans |Man | "Microsoft Server spraak tekst en spraak, spraak (it-IT, Cosimo, Apollo)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (it-IT, LuciaRUS)"
ja-JP | Japans |Vrouw | "Microsoft Server tekst naar spraak stem (ja-JP, Ayumi, Apollo)"
| | |Man | "Microsoft Server tekst naar spraak stem (ja-JP, Ichiro, Apollo)"
| | |Vrouw | "Microsoft Server tekst naar spraak stem (ja-JP, HarukaRUS)"
ko-KR | Koreaans |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (ko-KR, HeamiRUS)"
ms-MY | Maleis | Man | "Microsoft Server-stem tekst naar spraak (ms-mijn Rizwan)"
nb-NO | Noors | Vrouw | "Microsoft Server-stem tekst naar spraak (nb-NO HuldaRUS)"
NL-NL | Nederlands | Vrouw | "Microsoft Server tekst naar spraak stem (nl-NL, HannaRUS)"
pl-PL | Pools | Vrouw | "Microsoft Server tekst naar spraak stem (pl-PL, PaulinaRUS)"
pt-BR | Portugees (Brazilië) | Vrouw | "Microsoft Server tekst naar spraak stem (pt-BR, HeloisaRUS)"
| | |Man | "Microsoft Server tekst naar spraak stem (pt-BR, Daniel, Apollo)"
pt-PT | Portugees (Portugal) | Vrouw | "Microsoft Server tekst naar spraak stem (pt-PT, HeliaRUS)"
ro-RO | Roemeens | Man | "Microsoft Server spraak tekst en spraak, spraak (ro-RO, Andrei)"
ru-RU |Russisch| Vrouw | "Microsoft Server tekst naar spraak stem (ru-RU, Irina, Apollo)"
| | |Man | "Microsoft Server tekst naar spraak stem (ru-RU, Pavel, Apollo)"
| | |Vrouw | "Microsoft Server tekst naar spraak stem (ru-RU, EkaterinaRUS)"
sk-SK | Slowaaks|Man | "Microsoft Server tekst naar spraak stem (sk-SK, Filip)"
sl-SI | Sloveens|Man | "Microsoft Server tekst naar spraak stem (sl-SI, Lado)"
SV-SE | Zweeds|Vrouw | "Microsoft Server spraak tekst en spraak, spraak (sv-SE, HedvigRUS)"
TA-IN | Tamil (India) |Man | "Microsoft Server spraak tekst en spraak, spraak (ta-IN, Valluvar)"
te-IN | Telugu (India) |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (te-IN, Chitra)"
e-e | Thais|Man | "Microsoft Server tekst naar spraak stem (th-TH, Pattara)"
tr-TR |Turks| Vrouw | "Microsoft Server tekst naar spraak stem (tr-TR, SedaRUS)"
vi-VN | Vietnamees|Man | "Microsoft Server-stem tekst naar spraak (vi-VN, een)"
zh-CN | Chinees (vasteland)|Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, HuihuiRUS)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, Yaoyao, Apollo)"
| | |Man | "Microsoft Server spraak tekst en spraak, spraak (zh-CN, Kangkang, Apollo)"
zh-HK | Chinees (Hongkong SAR)|Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-HK Tracy, Apollo)"
| | |Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-HK TracyRUS)"
| || Man | "Microsoft Server spraak tekst en spraak, spraak (zh-HK Danny, Apollo)"
zh-TW | Chinees (Taiwan)|Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, Yating, Apollo)"
| || Vrouw | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, HanHanRUS)"
| || Man | "Microsoft Server spraak tekst en spraak, spraak (zh-TW, Zhiwei, Apollo)"

\* *ar bijvoorbeeld ondersteunt moderne Standard Arabisch (MSA).*

### <a name="customization"></a>Aanpassing

Stem aanpassing is beschikbaar voor ons Engels (en-US), vasteland Chinees (zh-CN), Frans (fr-FR), Duits (nl-nl) en Italiaans (it-IT).

> [!NOTE]
> Frans, Duits en Italiaans trainingen voor de toon begint met een gegevensset van 2000 + uitingen. Chinees-Engels tweetalige modellen worden ook ondersteund met een initiële gegevensset van meer dan 2000 uitingen.

## <a name="speech-translation"></a>Spraakomzetting

De **Spraakomzetting** API biedt ondersteuning voor verschillende talen voor spraak-naar-spraak- en spraak naar tekst converteren. De source-taal moet altijd zijn uit de tabel van de taal spraak-naar-tekst. De beschikbare talen voor doel, is afhankelijk van of het doel van de vertaling spraak of tekst is. U mag vertalen binnenkomende spraak in meer dan [60 talen](https://www.microsoft.com/translator/business/languages/). Een subset van deze talen zijn beschikbaar voor [spraaksynthese](language-support.md#text-languages).

### <a name="text-languages"></a>Tekst-talen

| Taal van de tekst    | Taalcode |
|:----------- |:-------------:|
| Afrikaans      | `af`          |
| Arabisch       | `ar`          |
| Bengalees      | `bn`          |
| Bosnisch (Latijns)      | `bs`          |
| Bulgaars      | `bg`          |
| Kantonees (Traditioneel)      | `yue`          |
| Catalaans      | `ca`          |
| Vereenvoudigd Chinees      | `zh-Hans`          |
| Traditioneel Chinees      | `zh-Hant`          |
| Kroatisch      | `hr`          |
| Tsjechisch      | `cs`          |
| Deens      | `da`          |
| Nederlands      | `nl`          |
| Nederlands      | `en`          |
| Estisch      | `et`          |
| Fiji      | `fj`          |
| Filipijns      | `fil`          |
| Fins      | `fi`          |
| Frans      | `fr`          |
| Duits      | `de`          |
| Grieks      | `el`          |
| Haïtiaans      | `ht`          |
| Hebreeuws      | `he`          |
| Hindi      | `hi`          |
| Hmong Daw      | `mww`          |
| Hongaars      | `hu`          |
| Indonesisch      | `id`          |
| Italiaans      | `it`          |
| Japans      | `ja`          |
| Swahili      | `sw`          |
| Klingon      | `tlh`          |
| Klingon (plqaD)      | `tlh-Qaak`          |
| Koreaans      | `ko`          |
| Lets      | `lv`          |
| Litouws      | `lt`          |
| Malagassische      | `mg`          |
| Maleis      | `ms`          |
| Maltees      | `mt`          |
| Noors      | `nb`          |
| Perzisch      | `fa`          |
| Pools      | `pl`          |
| Portugees      | `pt`          |
| Queretaro Otomi      | `otq`          |
| Roemeens      | `ro`          |
| Russisch      | `ru`          |
| Samoaanse      | `sm`          |
| Servisch (Cyrillisch)      | `sr-Cyrl`          |
| Servisch (Latijns)      | `sr-Latn`          |
| Slowaaks     | `sk`          |
| Sloveens      | `sl`          |
| Spaans      | `es`          |
| Zweeds      | `sv`          |
| Tahitian      | `ty`          |
| Tamil      | `ta`          |
| Thais      | `th`          |
| Tongan      | `to`          |
| Turks      | `tr`          |
| Oekraïens      | `uk`          |
| Urdu      | `ur`          |
| Vietnamees      | `vi`          |
| Welsh      | `cy`          |
| Yucatec Maya      | `yua`          |


## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie voor het herkennen van gesproken tekst in C#](quickstart-csharp-dotnet-windows.md)
