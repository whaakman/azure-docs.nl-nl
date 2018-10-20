---
title: Transcriptie richtlijnen voor de Service voor spraak-training
titleSuffix: Azure Cognitive Services
description: Informatie over het voorbereiden van de tekst die moet worden akoestische en taalmodellen en spraakstijlen aanpassen voor de Speech-Service.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: panosper
ms.openlocfilehash: 07b90942c8d231418888491fd47a9be3caff77ea
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49467732"
---
# <a name="transcription-guidelines-for-using-the-speech-service"></a>Transcriptie richtlijnen voor het gebruik van de Speech-Service

Om aan te passen **spraak naar tekst** of **tekst naar spraak**, moet u de tekst en spraak. Elke regel in de tekst die overeenkomt met een enkele utterance. De tekst moet de gesproken tekst zoveel mogelijk overeenkomen met. De tekst heet een *transcript*, en moet u deze maken in een specifieke indeling.

De Spraakservice normaliseert de invoer voor tekst consistent te houden. 

Dit artikel wordt beschreven voor beide typen normalizations. De richtlijnen verschillen voor verschillende talen.

## <a name="us-english-en-us"></a>Amerikaans-Engels (en-us)

Tekstgegevens moet één utterance per regel, worden geschreven als tekst zonder opmaak, met behulp van alleen de ASCII-tekenset.

Vermijd het gebruik van uitgebreide (Latijns-1) of Unicode-tekens. Deze tekens worden opgenomen per ongeluk bij het voorbereiden van de gegevens in een tekstverwerkingsprogramma of kan verzamelen van gegevens van webpagina's. De tekens vervangen door de juiste ASCII-vervangingen. Bijvoorbeeld:

| Tekens om te voorkomen | Vervanging |
|----- | ----- |
| "Hallo wereld" (openen en sluiten van dubbele aanhalingstekens) | 'Hallo wereld' (dubbele aanhalingstekens) |
| Johan dag (enkel aanhalingsteken) | Johan dag (apostrof) |
| het is goed: Nee, werd de fantastische! (em-streepje) | het is niet goed--, maar het was geweldige! (afbreekstreepjes) |

### <a name="text-normalization-rules-for-english"></a>Tekst normalisering regels voor Engels

De spraak-Service wordt uitgevoerd de volgende normalisatieregels:

* Kleine letters gebruikt voor alle tekst
* Alle interpunctie met uitzondering van word-interne apostroffen verwijderen
* Getallen uit te breiden naar gesproken formulier, met inbegrip van dollar bedragen

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| "Heilige betreft." Deze Batman. | Heilige betreft gezegd batman |
| 'Wat?' Deze Batman sidekick, Robin. | welke genoemde batman sidekick robin |
| Ga ophalen - em! | Ga get em |
| Ik ben double-jointed | Ik ben dubbele samengevoegd |
| 104 elm straat | een oh vier Elm straat |
| Stemmen op 102.7 | stemmen op een oh twee zeven |
| Pi is ongeveer 3,14 | pi is ongeveer drie punt een vier |
| Het kost $3,14 | kosten hiervoor drie 14 |

De volgende normalisering van toepassing op uw Transcripten tekst:

* Afkortingen moeten worden uitgeschreven in woorden.
* Niet-standaard numerieke tekenreeksen (zoals een bepaalde datum of accounting formulieren) moeten worden uitgeschreven in woorden.
* Woorden met niet-alfabetische tekens of gemengde alfanumerieke tekens moeten worden omgezet als uitgesproken.
* Afkortingen die worden uitgesproken als woorden ongewijzigd laat (bijvoorbeeld 'radar","laserprinter", 'RAM-geheugen' of 'NAVO').
* Schrijf aanmaningen afkortingen die worden uitgesproken als afzonderlijke, met letters die worden gescheiden door spaties (bijvoorbeeld 'IBM,""CPU,""FBI", 'Nog te bepalen' of 'NaN'). 

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| 14 NE 3rd Dr. | 14 noordoosten derde schijf |
| Dr. Bruce Banner | Doctor Bruce Banner |
| James Bond, 007 | James Bond, dubbelklik oh zeven |
| Ke$ ha | Kesha |
| Hoe lang is de 2 x 4 | Hoe lang is de twee door vier |
| De vergadering gaat van 1-3 uur | Deze zijn van één tot drie uur van de vergadering |
| Mijn bloed-type is O + | Mijn bloed O positief is |
| water is H20 | water is H 2 O |
| OU812 door Van Halen afspelen | O U 8 1 2 Halen Van afspelen |
| UTF-8 met stuklijst | U T F 8 met stuklijst |

## <a name="chinese-zh-cn"></a>Chinees (zh-cn)

Tekstgegevens die wordt geüpload naar de Custom Speech Service moet gebruiken UTF-8-codering met een bytevolgorde markering. Het bestand moet worden geschreven als één utterance per regel.

Vermijd het gebruik van de halve breedte leestekens. Deze tekens worden opgenomen per ongeluk bij het voorbereiden van de gegevens in een tekstverwerkingsprogramma of kan verzamelen van gegevens van webpagina's. Vervang ze door geschikte volledige breedte vervangingen. Bijvoorbeeld:

| Tekens om te voorkomen | Vervanging |
|----- | ----- |
| "你好" (openen en sluiten van dubbele aanhalingstekens) | "你好' (dubbele aanhalingstekens) |
| 需要什么帮助? (vraagteken) | 需要什么帮助? |

### <a name="text-normalization-rules-for-chinese"></a>Tekst normalisering regels voor Chinees

De spraak-Service wordt uitgevoerd de volgende normalisatieregels:

* Alle interpunctie verwijderen
* Getallen uit te breiden naar gesproken formulier
* Volledige breedte letters converteren naar halve breedte letters
* Hoofdletters gebruikt voor alle Nederlandse woorden

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

Voordat u uw tekst importeren, gelden de volgende normalisering:

* Afkortingen moeten worden uitgeschreven in woorden (zoals in gesproken formulier).
* Numerieke tekenreeksen in gesproken vorm uitschrijven.

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Andere talen

Gegevens geüpload naar de **spraak naar tekst** service UTF-8-codering met een markering bytevolgordemarkering moet gebruiken. Het bestand moet worden geschreven als één utterance per regel.

> [!NOTE]
> De volgende voorbeelden gebruiken Duits. Echter, de richtlijnen gelden voor alle talen die geen Amerikaans Engels of Chinees.

### <a name="text-normalization-rules-for-german"></a>Tekst normalisering regels voor Duitsland

De spraak-Service wordt uitgevoerd de volgende normalisatieregels:

* Kleine letters gebruikt voor alle tekst
* Verwijderen van alle interpunctie, met inbegrip van verschillende typen tussen aanhalingstekens ('test', 'test', 'test' en «testen» zijn OK)
* Verwijderen van rijen met speciale tekens uit de set ¢ ¤ ¥ ¦ artikel © ª ¬­® ° + ² µ × ÿ Ø¬¬
* Getallen uit te breiden naar word-formulier, met inbegrip van dollar of eurobedragen
* Umlauts accepteert alleen voor een, o, en u; anderen wordt vervangen door "do" of worden verwijderd

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| ¡EINE Frage! | een frage |
| WIR, haben | WIR haben |

Voordat u uw tekst importeren, gelden de volgende normalisering:

* Decimale punten moet ', 'en niet'. '.
* Scheidingstekens tijd tussen het aantal uren en minuten moet ': 'en niet'. ' (bijvoorbeeld 12:00 Uhr).
* Afkortingen zoals "ca". worden niet vervangen. U wordt aangeraden dat u het volledige formulier gebruiken.
* De vier belangrijkste rekenkundige operators (+, -, \*, en /), worden verwijderd. Het is raadzaam deze te vervangen door hun letterlijke vorm: "plus," "min," "onjuist" en "geteilt."
* Dezelfde regel van toepassing op vergelijkingsoperators (=, <, en >). Het is raadzaam deze te vervangen door "Tautologisch", "kleiner als," en 'grösser als'.
* Gebruik fracties, zoals 3/4, word-indeling (bijvoorbeeld ' drei viertel' in plaats van ¾).
* Het symbool € vervangen door de word-indeling "Euro."

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na de normalisering van gebruiker | Nadat system normalisering
|--------  | ----- | -------- |
| ES is 12.23 Uhr | ES is 12:23 Uhr | ES is zwölf uhr drei en zwanzig uhr |
| {12,45} | {12,45} | zwölf komma vier fünf ||
| 2 + 3-4 | 2 en 3 min 4 | u plus drei min vier|

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech Service ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Spraak herkennen in C#](quickstart-csharp-dotnet-windows.md)
