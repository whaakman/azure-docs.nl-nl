---
title: Transcriptie richtlijnen in de Custom Speech Service op Azure | Microsoft Docs
description: Informatie over het voorbereiden van uw gegevens voor Custom Speech Service in Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 6c64b89c2eb933e9bcc24bdc3097e7a0b2aab368
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342463"
---
# <a name="transcription-guidelines"></a>Transcriptierichtlijnen

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Om te zorgen dat het beste gebruik van uw gegevens voor akoestische en modelaanpassing, moeten de volgende transcriptie richtlijnen worden gevolgd. Deze richtlijnen zijn specifieke taal.

## <a name="text-normalization"></a>Tekst normaliseren

Voor optimale gebruik in de modelaanpassing akoestische of taal, moeten de gegevens worden genormaliseerd, wat betekent dat omgezet in een standaard, ondubbelzinnige formulier kan worden gelezen door het systeem. Deze sectie beschrijft de normalisering van de tekst die door de Custom Speech Service wordt uitgevoerd wanneer gegevens worden geïmporteerd en de normalisering van de tekst die de gebruiker voorafgaand aan het importeren van gegevens uitvoeren moet.

## <a name="inverse-text-normalization"></a>Inverse tekst normalisering

Het proces van het 'raw' niet-opgemaakte tekst converteren naar opgemaakte tekst, dat wil zeggen met gebruik van hoofdletters en leestekens, wordt normalisering inverse tekst (toevoegen) genoemd. Toevoegen wordt uitgevoerd op de resultaten geretourneerd door de Microsoft Cognitive Services Speech-API. Een aangepast eindpunt geïmplementeerd met behulp van de Custom Speech Service maakt gebruik van de dezelfde toevoegen als de Microsoft Cognitive Services Speech-API. Echter biedt deze service momenteel geen ondersteuning aangepaste toevoegen, zodat de nieuwe voorwaarden die zijn geïntroduceerd door een aangepast taalmodel niet ingedeeld in de resultaten.

## <a name="transcription-guidelines-for-en-us"></a>Transcriptie richtlijnen voor het en-US

Tekstgegevens die zijn geüpload naar deze service moet worden geschreven als tekst zonder opmaak met behulp van alleen de ASCII afdrukbare tekenset. Elke regel van het bestand moet de tekst voor een enkele utterance alleen bevatten.

Het is belangrijk om te voorkomen dat het gebruik van Unicode-tekens. Dit kan per ongeluk gebeuren als het voorbereiden van de gegevens in een word verwerking programma of slijmen gegevens van webpagina's. Deze vervangen door de juiste ASCII-vervangingen. Bijvoorbeeld:

| Unicode om te voorkomen | ASCII-vervanging |
|----- | ----- |
| "Hallo wereld" (openen en sluiten van dubbele aanhalingstekens) | 'Hallo wereld' (dubbele aanhalingstekens) |
| Johan dag (enkel aanhalingsteken) | Johan dag (apostrof) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Tekst normalisering uitgevoerd door de Custom Speech Service

Deze service wordt de normalisering van de volgende tekst op gegevens die zijn geïmporteerd als een gegevensset taal of transcripties voor een akoestisch gegevensset uitvoeren. Dit omvat

*   Kleine hoofdlettergebruik alle tekst
*   Alle interpunctie met uitzondering van word-interne apostroffen verwijderen
*   Uitbreiding van de getallen in gesproken formulier, met inbegrip van dollar bedragen

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| Ergens koffie | ergens koffie |
| "Heilige betreft." Deze Batman. | Heilige betreft gezegd batman |
| 'Wat?' Deze Batman sidekick, Robin. | welke genoemde batman sidekick robin |
| Ga ophalen - em! | Ga get em |
| Ik ben double-jointed | Ik ben dubbele jointed |
| 104 Main straat | een oh vier belangrijkste straat |
| Stemmen op 102.7 | af te stemmen op een oh twee punt zeven |
| Pi is ongeveer 3,14 | pi is ongeveer drie punt een vier |
| Het kost $3,14 | kosten hiervoor drie 14 |

### <a name="text-normalization-required-by-users"></a>Tekst normalisering vereist door gebruikers

Als u wilt zorgen dat het beste gebruik van uw gegevens, moeten de volgende normalisatieregels worden toegepast op uw gegevens voordat u ze importeert.

*   Afkortingen moeten worden uitgeschreven in woorden in overeenstemming met gesproken formulier
*   Niet-standaard numerieke tekenreeksen moeten worden uitgeschreven in woorden
*   Woorden met niet-alfabetische tekens of gemengde alfanumerieke tekens moeten worden omgezet als uitgesproken
*   Algemene acroniemen kunnen worden behouden als één entiteit zonder punten of spaties tussen de letters, maar alle andere acroniemen moeten worden uitgeschreven in afzonderlijke letters, met elke letter gescheiden door een spatie

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| 14 NE 3rd Dr. | 14 noordoosten derde schijf |
| Dr. Strangelove | Doctor Strangelove |
| James Bond 007 | James obligaties dubbele oh zeven |
| Ke$ ha | Kesha |
| Hoe lang is de 2 x 4 | Hoe lang is de twee door vier |
| De vergadering gaat van 1-3 uur | Deze zijn van één tot drie uur van de vergadering |
| Mijn bloed-type is O + | Mijn bloed O positief is |
| water is H20 | water is H 2 O |
| OU812 door Van Halen afspelen | O U 8 1 2 Halen Van afspelen |

## <a name="transcription-guidelines-for-zh-cn"></a>Transcriptie richtlijnen voor zh-CN

Tekstgegevens die zijn geüpload naar de Custom Speech Service moet gebruiken **UTF-8-codering (inclusief stuklijst)**. Elke regel van het bestand moet de tekst voor een enkele utterance alleen bevatten.

Het is belangrijk om te voorkomen dat het gebruik van de halve breedte leestekens. Dit kan per ongeluk gebeuren als het voorbereiden van de gegevens in een word verwerking programma of slijmen gegevens van webpagina's. Deze vervangen door de juiste vervangingen voor volledige breedte. Bijvoorbeeld:

| Unicode om te voorkomen | ASCII-vervanging |
|----- | ----- |
| "你好" (openen en sluiten dubbele aanhalingstekens) | "你好' (dubbele aanhalingstekens) |
| 需要什么帮助? (vraagteken) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Tekst normalisering uitgevoerd door de Custom Speech Service

Deze speech-service wordt de normalisering van de volgende tekst op gegevens die zijn geïmporteerd als een gegevensset taal of transcripties voor een akoestisch gegevensset uitvoeren. Dit omvat

*   Alle interpunctie verwijderen
*   Uitbreiding van de getallen om een gesproken
*   Volledige breedte letters geconverteerd naar halve breedte letters.
*   Alle hoofdletters hoofdlettergebruik Nederlandse woorden

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

### <a name="text-normalization-required-by-users"></a>Tekst normalisering vereist door gebruikers

Om te zorgen dat het beste gebruik van uw gegevens, moeten de volgende normalisatieregels worden toegepast op uw gegevens _voorafgaande_ te importeren.

*   Afkortingen moeten worden uitgeschreven in woorden in overeenstemming met gesproken formulier
*   Deze service biedt geen betrekking heeft op alle numerieke hoeveelheden. Het is betrouwbaarder numerieke tekenreeksen in gesproken vorm schrijven

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>Transcriptie richtlijnen voor nl-nl

Gebruik gegevens geüpload naar de Custom Speech Service alleen **UTF-8-codering (inclusief stuklijst)**. Elke regel van het bestand moet de tekst voor een enkele utterance alleen bevatten.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Tekst normalisering uitgevoerd door de Custom Speech Service

Deze service wordt de normalisering van de volgende tekst op gegevens die zijn geïmporteerd als een gegevensset taal of transcripties voor een akoestisch gegevensset uitvoeren. Dit omvat

*   Kleine hoofdlettergebruik alle tekst
*   Verwijderen van alle leestekens zoals Engels of Duits aanhalingstekens ('test', 'test', 'test' of «testen» zijn ok)
*   Verwijderen van rijen met elk ander speciaal teken, waaronder: ^ ¢ £ ¤ ¥ ¦ artikel © ª ¬­® ° + ² µ × ÿ Ø¬¬
*   Uitbreiding van de getallen in word-formulier, met inbegrip van dollar of euro bedragen
*   We accepteren alleen umlauts voor een, o u; anderen wordt vervangen door "do" of verwijderd

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normaliseren |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| "Hallo, Mama!" sagt die Tochter. | Hallo mama sagt die tochter |
| ¡EINE Frage! | een frage |
| WIR, haben | WIR haben |
| DAS-macht $10 | DAS macht zehn dollar |


### <a name="text-normalization-required-by-users"></a>Tekst normalisering vereist door gebruikers

Als u wilt zorgen dat het beste gebruik van uw gegevens, moeten de volgende normalisatieregels worden toegepast op uw gegevens voordat u ze importeert.

*   Decimale punt moet zijn, en niet. bijvoorbeeld, 2,3% en niet 2.3%
*   Tijdscheidingsteken tussen de uren en minuten moet zijn: en niet., bijvoorbeeld 12:00 Uhr
*   Afkortingen zoals 'ca.', 'bzw'. niet vervangen. We raden aan het volledige formulier gebruiken om de juiste uitspraak.
*   De vijf belangrijkste rekenkundige operators worden verwijderd: +, -, \*, /.
 We raden aan om deze te vervangen door hun letterlijke formulier plus, min, mal, geteilt.
*   Hetzelfde geldt voor de comparators (=, <>,) - Tautologisch, kleiner als, grösser als
*   Fracties zoals 3/4 in woordvorm drei viertel in plaats van ¾
*   Het symbool € vervangen door de word-indeling "Euro"


Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na de normalisering van gebruiker | Nadat system normalisering
|--------  | ----- | -------- |
| ES is 12.23Uhr | ES is 12:23Uhr | ES is zwölf uhr drei en zwanzig uhr |
| {12,45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als vier |
| 2 + 3-4 | 2 en 3 min 4 | u plus drei min vier|
| DAS-macht 12€ | DAS-macht 12 euro 's | DAS macht zwölf euro 's |



## <a name="next-steps"></a>Volgende stappen
* [Het gebruik van een aangepaste spraak-naar-tekst-eindpunt](cognitive-services-custom-speech-create-endpoint.md)
* De nauwkeurigheid vergroten met uw [aangepast akoestisch model](cognitive-services-custom-speech-create-acoustic-model.md)
* De nauwkeurigheid vergroten met een [aangepast taalmodel](cognitive-services-custom-speech-create-language-model.md)
