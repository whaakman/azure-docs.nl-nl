---
title: Schrijffouten richtlijnen voor het trainen spraak | Microsoft Docs
description: Ontdek hoe tekst om aan te passen akoestisch en taal-modellen en stem lettertypen voorbereiden voor de service spraak.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 93ab7c81a773f692b2b970bb1901d82b7aceb5a2
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "35345979"
---
# <a name="transcription-guidelines-for-using-speech-service"></a>Schrijffouten richtlijnen voor het gebruik van spraak-service

Om aan te passen **spraak naar tekst** of **Text to Speech**, moet u tekst samen met spraak opgeven. Elke regel in de tekst die overeenkomt met een enkele utterance. De tekst, heet een *verslag*, en moet u deze maken in een specifieke indeling.

Sommige normalizations voor u uitvoert de spraak-service om uw tekst consistent maken. Andere normalisatie-taken moeten worden uitgevoerd voordat de tekst die wordt ingediend voor training. 

Beide typen normalizations beschreven. De richtlijnen verschillen voor verschillende talen.

## <a name="us-english-en-us"></a>Amerikaans-Engels (en-US)

Gegevens geüpload naar deze service worden geschreven als tekst zonder opmaak met behulp van alleen de ASCII-tekenset. Elke regel van het bestand moet de tekst voor een enkele utterance bevatten.

Het is belangrijk om te voorkomen dat het gebruik van uitgebreide (Latijns-1) of Unicode-tekens. Deze tekens worden opgenomen per ongeluk bij het voorbereiden van de gegevens in een tekstverwerkingsprogramma of slijmen gegevens van webpagina's. Deze vervangen door de juiste ASCII-vervangingen. Bijvoorbeeld:

| Tekens om te voorkomen | Vervanging |
|----- | ----- |
| 'Hallo wereld' (openen en sluiten van dubbele aanhalingstekens) | 'Hallo wereld' (dubbele aanhalingstekens) |
| Johan dag (enkel aanhalingsteken) | Johan dag (apostrof) |
| goede was: Nee, is het geweldig! (em koppelteken) | het is niet goed--, was geweldig! (afbreekstreepjes) |

### <a name="text-normalization-performed-by-the-service"></a>Tekst normalisatie uitgevoerd door de service.

De spraak-service voert de volgende tekst normalisering op transcripties van tekst.

*   Kleine hoofdlettergebruik alle tekst
*   Alle interpunctie met uitzondering van word interne apostrof verwijderen
*   Uitbreiding van de cijfers aan gesproken formulier, met inbegrip van dollar bedragen

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| Starbucks koffie | Starbucks koffie |
| 'Heilige betreft!' genoemde Batman. | Heilige betreft gezegd batman |
| 'Wat?' genoemde Batman sidekick, Robin. | welke genoemde batman sidekick robin |
| Ga get - em! | Ga get em |
| Ik ben double-jointed | Ik ben dubbele jointed |
| 104 Main straat | een geselecteerd vier belangrijkste straat |
| Afstemmen op 102.7 | afstemmen op een geselecteerd twee punt zeven |
| Pi is ongeveer 3,14 | pi is ongeveer drie punt een vier |
| Het kost $3,14 | het kost drie 14 |

### <a name="text-normalization-you-must-perform"></a>Tekst normalisatie die u moet uitvoeren

De volgende normalisering van toepassing op uw transcripties tekst.

*   Afkortingen moeten worden uitgeschreven in woorden in overeenstemming met gesproken formulier
*   Niet-standaard numerieke tekenreeksen (zoals bepaalde datum of accounting formulieren) moeten worden uitgeschreven in woorden
*   Woorden met niet-alfabetische tekens of gemengde alfanumerieke tekens bevatten moeten zoals uitgesproken als worden omgezet
*   Laat afkortingen uitgesproken als woorden intact gelaten. Bijvoorbeeld, radargrafiek, laser RAM-geheugen, NAVO en Dhr.
*   Afkortingen uitgesproken als afzonderlijke letters, waarbij letters worden gescheiden door spaties schrijven. Bijvoorbeeld IBM, CPU, FBI, nog te bepalen, NaN. 

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| 14 NE 3rd noodherstel. | veertien noordoosten derde station |
| Dr. Strangelove | Arts Strangelove |
| Jeroen obligatie 007 | James Bond Dubbelklik geselecteerd zeven |
| Ke$ ha | Kesha |
| Hoe lang is de 2 x 4 | Hoe lang is de twee door vier |
| De vergadering gaat van 1-3 uur | De vergadering gaat één tot drie uur |
| Mijn bloedgroep is O + | Mijn bloedgroep is positief O |
| water is H20 | water is H 2 O |
| OU812 door Van Halen afspelen | O U 8 1 2 Van Halen afspelen |
| UTF-8 met stuklijst | U T F 8 met stuklijst |

## <a name="chinese-zh-cn"></a>Chinees (zh-CN)

Gegevens geüpload naar de aangepaste spraak-Service moet gebruiken UTF-8-codering met bytevolgordemarkering markering. Elke regel van het bestand moet de tekst voor een enkele utterance bevatten.

Het is belangrijk om te voorkomen dat het gebruik van halve breedte leestekens. Deze tekens worden opgenomen per ongeluk bij het voorbereiden van de gegevens in een tekstverwerkingsprogramma of slijmen gegevens van webpagina's. Vervang ze door geschikte volledige breedte vervangingen. Bijvoorbeeld:

| Tekens om te voorkomen | Vervanging |
|----- | ----- |
| '你好' (openen en sluiten dubbele aanhalingstekens) | '你好' (dubbele aanhalingstekens) |
| 需要什么帮助? (vraagteken) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-service"></a>Tekst normalisatie uitgevoerd door de service.

De spraak-service voert de volgende tekst normalisering op transcripties van tekst.

*   Alle interpunctie verwijderen
*   Uitbreidbare cijfers gesproken formulier
*   Volledige breedte letters converteren naar een halve breedte letters
*   Gebruik van hoofdletters alle woorden voor Engels

Hier volgen enkele voorbeelden.

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-you-must-perform"></a>Tekst normalisatie die u moet uitvoeren

De volgende normalisering van toepassing op uw tekst voordat u deze importeert.

*   Afkortingen moeten worden uitgeschreven in woorden in overeenstemming met gesproken formulier
*   Deze service omvat niet alle numerieke hoeveelheden. Het is betrouwbaarder numerieke tekenreeksen in gesproken formulier schrijven.

Hier volgen enkele voorbeelden.

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="other-languages"></a>Andere talen

Gegevens geüpload naar de **spraak naar tekst** service UTF-8-codering met bytevolgordemarkering markering moet gebruiken. Elke regel van het bestand moet de tekst voor een enkele utterance bevatten.

> [!NOTE]
> Deze voorbeelden gebruiken Duits. Echter, deze richtlijnen gelden voor alle talen die niet Amerikaans Engels of Chinees zijn.

### <a name="text-normalization-performed-by-the-service"></a>Tekst normalisatie uitgevoerd door de service.

De spraak-service voert de volgende tekst normalisering op transcripties van tekst.

*   Kleine hoofdlettergebruik alle tekst
*   Verwijderen van alle interpunctie, met inbegrip van verschillende typen aanhalingstekens ('test', 'test', 'test' of «testen» zijn ok)
*   Een speciale tekens uit de set met rijen ^ ¢ £ ¤ ¥ ¦ artikel © ª ¬­® ° + ² µ × ÿ Ø¬¬
*   Uitbreiding van de getallen in word formulier, met inbegrip van of bedragen
*   Umlauts geaccepteerd alleen voor een, o u; anderen worden vervangen door 'e' of worden verwijderd

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| 'Hallo, Mama!' sagt die Tochter. | Hallo mama sagt die tochter |
| ¡EINE Frage! | een frage |
| WIR, haben | WIR haben |
| DAS macht $10 | DAS macht zehn bedragen |

### <a name="text-normalization-you-must-perform"></a>Tekst normalisatie die u moet uitvoeren

De volgende normalisering van toepassing op uw tekst voordat u deze importeert.

*   Drijvende komma moet ', 'en niet'. ': 2,3% en niet 2.3%
*   Tijdscheidingsteken tussen de uren en minuten moet ': 'en niet'. ': 12:00 Uhr
*   Afkortingen zoals 'ca.', 'bzw'. niet worden vervangen. Het is raadzaam om gebruik van de volledige vorm.
*   De belangrijkste vijf rekenkundige operatoren zijn verwijderd: +, -, \*, /. Het is raadzaam deze overal vervangen door hun letterlijke vorm: plus, min, mal, geteilt.
*   Dit geldt ook voor vergelijkingsoperators (=, <>,) - Tautologisch, kleiner als, grösser als
*   Gebruik breuken, zoals 3/4 in word-indeling (zoals 'drei viertel' in plaats van ¾)
*   Het symbool € vervangen door de word-indeling 'Euro'

Hier volgen enkele voorbeelden.

| Oorspronkelijke tekst | Na de normalisering van gebruiker | Nadat het systeem normalisatie
|--------  | ----- | -------- |
| ES ist 12.23Uhr | ES ist 12:23Uhr | ES ist zwölf uhr drei en zwanzig uhr |
| {12,45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als vier |
| 2 + 3 en 4 | 2 plus 3 min 4 | u plus drei min vier|
| DAS macht 12€ | DAS macht 12 euro 's | DAS macht zwölf euro 's |

## <a name="next-steps"></a>Volgende stappen

- [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
- [Herkent spraak in C#](quickstart-csharp-windows.md)
