---
title: Human etiket transcripties richtlijnen - spraakservices
titlesuffix: Azure Cognitive Services
description: Als u ter verbetering van spraakherkenning nauwkeurigheid, met name problemen die worden veroorzaakt wanneer de woorden zijn verwijderd of verkeerd vervangen, moet u de human etiket transcripties, samen met uw audiogegevens gebruiken. Wat zijn human etiket transcripties? Dat kan eenvoudig, maar per woord, verbatim transcripties van een geluidsbestand.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 1fca2a21758a060dbfdc4acb2123a59fcae585fd
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606549"
---
# <a name="how-to-create-human-labeled-transcriptions"></a>Over het maken van menselijke etiket transcripties

Als u ter verbetering van spraakherkenning nauwkeurigheid, met name problemen die worden veroorzaakt wanneer de woorden zijn verwijderd of verkeerd vervangen, moet u de human etiket transcripties, samen met uw audiogegevens gebruiken. Wat zijn human etiket transcripties? Dat kan eenvoudig, maar per woord, verbatim transcripties van een geluidsbestand.

Een grote reeks transcriptie gegevens is vereist om te verbeteren, het is raadzaam bieden tussen 10 en 1000 uur aan transcriptie gegevens. Op deze pagina wordt gecontroleerd om te helpen u bij het maken van hoge kwaliteit transcripties richtlijnen. Deze handleiding wordt opgedeeld met landinstellingen, met de secties voor Amerikaans Engels, Mandarijn China en Duitsland.

## <a name="us-english-en-us"></a>Amerikaans-Engels (en-US)

Human etiket transcripties voor Engels audio moeten worden opgegeven als tekst zonder opmaak, met alleen ASCII-tekens. Vermijd het gebruik van Latijns-1 of Unicode-tekens. Deze tekens worden vaak per ongeluk toegevoegd bij het kopiëren van tekst uit een tekstverwerkingsprogramma of slijmen gegevens van webpagina's. Als deze tekens gebruikt worden, zorg ervoor dat u deze bijwerken met de juiste ASCII-vervanging.

Enkele voorbeelden:

| Tekens om te voorkomen | Vervanging | Opmerkingen |
|---------------------|--------------|-------|
| "Hallo wereld" | "Hallo wereld" | De aanhalingstekens openen en sluiten zijn met de juiste ASCII-tekens vervangen. |
| Johan dag | Johan dag | De apostrof heeft is vervangen door het juiste ASCII-teken. |
| het is goed: Nee, werd de fantastische! | het is niet goed--, maar het was geweldige! | De em-streepje is vervangen door twee afbreekstreepjes bevatten. |

### <a name="text-normalization-for-us-english"></a>Tekst normalisering voor Amerikaans Engels

Tekst normalisatie is de omzetting van woorden in een consistente indeling die wordt gebruikt bij het trainen van een model. Sommige regels normalisering wordt toegepast op tekst automatisch, maar wordt u aangeraden deze richtlijnen bij het voorbereiden van de gegevens van uw human etiket transcriptie:

* Uitschrijven afkortingen in woorden.
* Schrijven van niet-standaard numerieke tekenreeksen in woorden (zoals accounting-voorwaarden).
* Niet-alfabetische tekens of gemengde alfanumerieke tekens moeten worden omgezet als uitgesproken.
* Afkortingen die worden uitgesproken als woorden mag niet worden bewerkt (zoals "radar", "laserprinter", 'RAM-geheugen' of 'NAVO').
* Schrijven van afkortingen die worden uitgesproken als afzonderlijke letters met elke letter gescheiden door een spatie.

Hier volgen enkele voorbeelden van normalisering die moeten worden uitgevoerd op de transcriptie:

| Oorspronkelijke tekst | Tekst na normaliseren |
|---------------|--------------------------|
| Dr. Bruce Banner | Doctor Bruce Banner |
| James Bond, 007 | James Bond, dubbelklik oh zeven |
| Ke$ha | Kesha |
| Hoe lang is de 2 x 4 | Hoe lang is de twee door vier |
| De vergadering gaat van 1-3 uur | Deze zijn van één tot drie uur van de vergadering |
| Mijn bloed-type is O + | Mijn bloed O positief is |
| water is H20 | water is H 2 O |
| OU812 door Van Halen afspelen | O U 8 1 2 Halen Van afspelen |
| UTF-8 met stuklijst | U T F 8 met stuklijst |

De volgende normalisering regels worden automatisch toegepast op transcripties:

* Gebruik kleine letters.
* Verwijder alle interpunctie behalve apostroffen binnen woorden.
* Getallen uitbreiden naar gesproken woorden/formulier, zoals bedragen.

Hier volgen enkele voorbeelden van normalisering automatisch uitgevoerd op de transcriptie:

| Oorspronkelijke tekst | Tekst na normaliseren |
|---------------|--------------------------|
| "Heilige betreft." Deze Batman. | Heilige betreft gezegd batman |
| 'Wat?' Deze Batman sidekick, Robin. | welke genoemde batman sidekick robin |
| Ga ophalen - em! | Ga get em |
| Ik ben double-jointed | Ik ben dubbele samengevoegd |
| 104 Elm Street | een oh vier Elm straat |
| Stemmen op 102.7 | stemmen op een oh twee zeven |
| Pi is ongeveer 3,14 | pi is ongeveer drie punt een vier |
Het kost $3,14| kosten hiervoor drie 14 |

## <a name="mandarin-chinese-zh-cn"></a>Mandarijn Chinees (zh-CN)

Human etiket transcripties voor Chinese audio Mandarijn moet UTF-8 wordt gecodeerd met een bytevolgorde markering. Vermijd het gebruik van de halve breedte leestekens. Deze tekens worden opgenomen per ongeluk bij het voorbereiden van de gegevens in een tekstverwerkingsprogramma of kan verzamelen van gegevens van webpagina's. Als deze tekens gebruikt worden, zorg ervoor dat u deze bijwerken met de juiste vervanging voor volledige breedte.

Enkele voorbeelden:

| Tekens om te voorkomen | Vervanging | Opmerkingen |
|---------------------|--------------|-------|
| "你好" | "你好" | De aanhalingstekens openen en sluiten zijn vervangen met de juiste tekens. |
| 需要什么帮助? | 需要什么帮助？ | Het vraagteken heeft is vervangen door de juiste tekens. |

### <a name="text-normalization-for-mandarin-chinese"></a>Tekst normalisering voor Mandarijn Chinees

Tekst normalisatie is de omzetting van woorden in een consistente indeling die wordt gebruikt bij het trainen van een model. Sommige regels normalisering wordt toegepast op tekst automatisch, maar wordt u aangeraden deze richtlijnen bij het voorbereiden van de gegevens van uw human etiket transcriptie:

* Uitschrijven afkortingen in woorden.
* Numerieke tekenreeksen in gesproken vorm uitschrijven.

Hier volgen enkele voorbeelden van normalisering die moeten worden uitgevoerd op de transcriptie:

| Oorspronkelijke tekst | Tekst na normaliseren |
|---------------|--------------------------|
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

De volgende normalisering regels worden automatisch toegepast op transcripties:

* Alle interpunctie verwijderen
* Getallen uitbreiden naar gesproken formulier
* Volledige breedte letters converteren naar halve breedte letters
* Hoofdletters gebruikt voor alle Nederlandse woorden

Hier volgen enkele voorbeelden van normalisering automatisch uitgevoerd op de transcriptie:

| Oorspronkelijke tekst | Tekst na normaliseren |
|---------------|--------------------------|
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z |W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

## <a name="german-de-de-and-other-languages"></a>Duits (nl-nl) en andere talen

Human etiket transcripties voor Duitse audio (en andere niet-Engelse of Chinese talen Mandarijn) moet UTF-8 wordt gecodeerd met een bytevolgorde markering. Een human etiket transcript moet worden opgegeven voor elk geluidsbestand.

### <a name="text-normalization-for-german"></a>Tekst normalisering voor Duitsland

Tekst normalisatie is de omzetting van woorden in een consistente indeling die wordt gebruikt bij het trainen van een model. Sommige regels normalisering wordt toegepast op tekst automatisch, maar wordt u aangeraden deze richtlijnen bij het voorbereiden van de gegevens van uw human etiket transcriptie:

*   Decimale punten als schrijven ', 'en niet'. '.
*   Tijd scheidingstekens als schrijven ': 'en niet'. ' (bijvoorbeeld: 12:00 Uhr).
*   Afkortingen zoals "ca". worden niet vervangen. U wordt aangeraden dat u de volledige gesproken vorm.
*   De vier belangrijkste rekenkundige operators (+, -, \*, en /), worden verwijderd. Het is raadzaam deze te vervangen door de schriftelijke vorm: "plus," "min," "onjuist" en "geteilt."
*   Vergelijkingsoperators zijn verwijderd (=, <, en >). Het is raadzaam deze te vervangen door "Tautologisch", "kleiner als," en 'grösser als'.
*   Schrijven van delen, zoals 3/4, schriftelijke (bijvoorbeeld: "drei viertel' in plaats van 3/4).
*   Het symbool "€" vervangen door de schriftelijke vorm "Euro."

Hier volgen enkele voorbeelden van normalisering die moeten worden uitgevoerd op de transcriptie:

| Oorspronkelijke tekst | Tekst achter normalisering van gebruiker | Tekst achter system normalisering |
|---------------|-------------------------------|---------------------------------|
| ES is 12.23 Uhr | ES is 12:23 Uhr | ES is zwölf uhr drei en zwanzig uhr |
| {12.45} | {12,45} | zwölf komma vier fünf |
| 2 + 3 - 4 | 2 en 3 min 4 | u plus drei min vier |

De volgende normalisering regels worden automatisch toegepast op transcripties:

* Kleine letters worden gebruikt voor alle tekst.
* Verwijder alle interpunctie, met inbegrip van verschillende typen tussen aanhalingstekens ('test', 'test', 'test' en «testen» zijn OK).
* Verwijderen van rijen met geen speciale tekens uit deze set: ¢ ¤ ¥ ¦ artikel © ª ¬­® ° + ² µ × ÿ Ø¬¬.
* Getallen uitbreiden naar gesproken vormen, waaronder dollar of eurobedragen.
* Umlauts accepteert alleen voor een, o, en u. Anderen wordt vervangen door "do" of worden verwijderd.

Hier volgen enkele voorbeelden van normalisering automatisch uitgevoerd op de transcriptie:

| Oorspronkelijke tekst | Tekst na normaliseren |
|---------------|--------------------------|
| Frankfurter Ring | frankfurter ring |
| ¡EINE Frage! | een frage |
| WIR, haben | WIR haben |

## <a name="next-steps"></a>Volgende stappen

* [Voorbereiden en testen van uw gegevens](how-to-custom-speech-test-data.md)
* [Controleer uw gegevens](how-to-custom-speech-inspect-data.md)
* [Evalueren van uw gegevens](how-to-custom-speech-evaluate-data.md)
* [Uw model te trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)
