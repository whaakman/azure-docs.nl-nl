---
title: Transcriptie richtlijnen voor spraak-training
description: Informatie over het voorbereiden van tekst om aan te passen akoestische en taalmodellen en spraakstijlen voor de Speech-service.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/01/2018
ms.author: panosper
ms.openlocfilehash: a219db14d659348ef3ed4de1dffa640a948f1954
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283332"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Transcriptie richtlijnen voor het gebruik van spraak-service

Om aan te passen **spraak naar tekst** of **tekst naar spraak**, moet u de tekst en spraak. Elke regel in de tekst die overeenkomt met een enkele utterance. De tekst moet exact de gesproken tekst als mogelijk. De tekst heet een *transcript*, en moet u deze maken in een specifieke indeling.

De spraakservice normaliseert de invoer voor tekst consistent te houden. 

Dit artikel wordt beschreven voor beide typen normalizations. De richtlijnen verschillen voor verschillende talen.

## <a name="us-english-en-us"></a>Amerikaans-Engels (en-US)

Tekstgegevens moet één utterance per regel, worden geschreven als tekst zonder opmaak, met behulp van alleen de ASCII-tekenset.

Vermijd het gebruik van uitgebreide (Latijns-1) of Unicode-tekens. Deze tekens worden opgenomen per ongeluk tijdens het voorbereiden van de gegevens in een tekstverwerkingsprogramma of slijmen gegevens van webpagina's. Deze vervangen door de juiste ASCII-vervangingen. Bijvoorbeeld:

| Tekens om te voorkomen | Vervanging |
|----- | ----- |
| "Hallo wereld" (openen en sluiten van dubbele aanhalingstekens) | 'Hallo wereld' (dubbele aanhalingstekens) |
| Johan dag (enkel aanhalingsteken) | Johan dag (apostrof) |
| het is goed: Nee, werd de fantastische! (em-streepje) | het is niet goed--, maar het was geweldige! (afbreekstreepjes) |

### <a name="text-normalization-rules-for-english"></a>Tekst normalisering regels voor Engels

De spraak-service wordt uitgevoerd de volgende normalisatieregels.

*   Kleine hoofdlettergebruik alle tekst
*   Alle interpunctie met uitzondering van word-interne apostroffen verwijderen
*   Uitbreiding van de getallen in gesproken formulier, met inbegrip van dollar bedragen

Hier volgen enkele voorbeelden

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

De volgende normalisering van toepassing op uw Transcripten tekst.

*   Afkortingen moeten worden uitgeschreven in woorden
*   Niet-standaard numerieke tekenreeksen (zoals een bepaalde datum of accounting formulieren) moeten worden uitgeschreven in woorden
*   Woorden met niet-alfabetische tekens of gemengde alfanumerieke tekens moeten worden omgezet als uitgesproken
*   Afkortingen uitgesproken als woorden ongewijzigd laten. Bijvoorbeeld, radar laser, RAM, NAVO.
*   Schrijven afkortingen uitgesproken als afzonderlijke letters, met letters die worden gescheiden door spaties. Bijvoorbeeld, IBM, CPU, FBI, nog te bepalen, NaN. 

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

## <a name="chinese-zh-cn"></a>Chinees (zh-CN)

Tekstgegevens die zijn geüpload naar de Custom Speech Service moet gebruiken UTF-8-codering met de bytevolgorde markering. Het bestand moet worden geschreven als één utterance per regel.

Vermijd het gebruik van de halve breedte leestekens. Deze tekens worden opgenomen per ongeluk tijdens het voorbereiden van de gegevens in een tekstverwerkingsprogramma of slijmen gegevens van webpagina's. Vervang ze door geschikte volledige breedte vervangingen. Bijvoorbeeld:

| Tekens om te voorkomen | Vervanging |
|----- | ----- |
| "你好" (openen en sluiten dubbele aanhalingstekens) | "你好' (dubbele aanhalingstekens) |
| 需要什么帮助? (vraagteken) | 需要什么帮助? |

### <a name="text-normalization-rules-for-chinese"></a>Tekst normalisering regels voor Chinees

De spraak-service wordt uitgevoerd de volgende normalisatieregels.

*   Alle interpunctie verwijderen
*   Getallen uit te breiden naar gesproken formulier
*   Volledige breedte letters converteren naar halve breedte letters
*   Alle hoofdletters hoofdlettergebruik Nederlandse woorden

Hier volgen enkele voorbeelden.

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

De volgende normalisering van toepassing op uw tekst voordat u ze importeert.

*   Afkortingen moeten worden uitgeschreven in woorden (zoals in gesproken formulier)
*   Numerieke tekenreeksen in gesproken vorm uitschrijven.

Hier volgen enkele voorbeelden.

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Andere talen

Gegevens geüpload naar de **spraak naar tekst** service UTF-8-codering met de bytevolgorde markering moet gebruiken. Het bestand moet worden geschreven als één utterance per regel.

> [!NOTE]
> Deze voorbeelden gebruiken Duits. Deze richtlijnen gelden echter voor alle talen die geen Amerikaans Engels of Chinees.

### <a name="text-normalization-rules-for-german"></a>Tekst normalisering regels voor Duitsland

De spraak-service wordt uitgevoerd de volgende normalisatieregels.

*   Kleine hoofdlettergebruik alle tekst
*   Verwijderen van alle interpunctie, met inbegrip van verschillende typen aanhalingstekens ('test', 'test', 'test' of «testen» zijn ok)
*   Verwijderen van rijen met speciale tekens uit de set ¢ ¤ ¥ ¦ artikel © ª ¬­® ° + ² µ × ÿ Ø¬¬
*   Uitbreiding van de getallen in word-formulier, met inbegrip van dollar of euro bedragen
*   Umlauts worden geaccepteerd alleen voor een, o u; anderen wordt vervangen door "do" of worden verwijderd

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| ¡EINE Frage! | een frage |
| WIR, haben | WIR haben |

De volgende normalisering van toepassing op uw tekst voordat u ze importeert.

*   Decimale punt moet ', 'en niet'. '
*   Tijdscheidingsteken tussen de uren en minuten moet ': 'en niet'. ': 12:00 Uhr
*   Afkortingen zoals 'ca'. worden niet vervangen. Het is raadzaam dat het volledige formulier te gebruiken.
*   De vijf belangrijkste rekenkundige operators worden verwijderd: +, -, \*, /. Het is raadzaam deze te vervangen door hun letterlijke vorm: plus, min, mal, geteilt.
*   Hetzelfde geldt voor vergelijkingsoperators (=, <>,) - Tautologisch, kleiner als, grösser als
*   Gebruik fracties, zoals 3/4, word-indeling (zoals 'drei viertel' in plaats van ¾)
*   Het symbool € vervangen door de word-indeling "Euro"

Hier volgen enkele voorbeelden.

| Oorspronkelijke tekst | Na de normalisering van gebruiker | Nadat system normalisering
|--------  | ----- | -------- |
| ES is 12.23Uhr | ES is 12:23Uhr | ES is zwölf uhr drei en zwanzig uhr |
| {12,45} | {12,45} | zwölf komma vier fünf ||
| 2 + 3-4 | 2 en 3 min 4 | u plus drei min vier|

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Water in C#](quickstart-csharp-dotnet-windows.md)
