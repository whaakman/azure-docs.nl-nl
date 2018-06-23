---
title: Schrijffouten richtlijnen in aangepaste spraak-Service op Azure | Microsoft Docs
description: Informatie over het voorbereiden van uw gegevens voor aangepaste spraak Service in cognitieve Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 2785a35ac7583ac3d9503cb721d10078d86aa365
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344557"
---
# <a name="transcription-guidelines"></a>Schrijffouten richtlijnen
Om te controleren of het beste gebruik van uw tekstgegevens voor aanpassing van het model acoustic- en taalinstellingen, moeten de volgende richtlijnen voor schrijffouten worden gevolgd. Deze richtlijnen zijn specifieke taal zijn gebonden.

## <a name="text-normalization"></a>Tekst normalisatie

Voor optimale gebruik in het model acoustic of taal aanpassen, moet de gegevens worden genormaliseerd, wat betekent dat omgezet in een standaard, niet-ambigue formulier kan worden gelezen door het systeem. Deze sectie beschrijft de tekst normalisering door de aangepaste spraak-Service wordt uitgevoerd wanneer gegevens worden geïmporteerd en de normalisering van tekst die de gebruiker voordat het importeren van gegevens uitvoeren moet.

## <a name="inverse-text-normalization"></a>Tekst omkeren normalisatie

Het proces van het converteren van 'onbewerkte' platte tekst terug naar opgemaakte tekst, dat wil zeggen met gebruik van hoofdletters en leestekens, wordt de inverse tekst normalisatie (ITN) genoemd. ITN wordt uitgevoerd op de resultaten geretourneerd door de Microsoft cognitieve Services Speech-API. Een aangepaste eindpunt geïmplementeerd met behulp van de aangepaste spraak-Service gebruikt de dezelfde ITN als de Microsoft cognitieve Services Speech-API. Maar ondersteunt deze service momenteel geen aangepaste ITN zodat de nieuwe voorwaarden geïntroduceerd door een aangepaste taalmodel niet in de herkenningsresultaten die zijn geformatteerd.

## <a name="transcription-guidelines-for-en-us"></a>Schrijffouten richtlijnen voor het en-US

Gegevens geüpload naar deze service worden geschreven in tekst zonder opmaak met alleen de ASCII afdrukbare tekenset. Elke regel van het bestand moet de tekst voor een enkele utterance alleen bevatten.

Het is belangrijk om te voorkomen dat het gebruik van Unicode-tekens. Dit kan per ongeluk gebeuren als het voorbereiden van de gegevens in een woord verwerken programma of slijmen gegevens van webpagina's. Deze vervangen door de juiste ASCII-vervangingen. Bijvoorbeeld:

| Unicode om te voorkomen | ASCII-vervanging |
|----- | ----- |
| 'Hallo wereld' (openen en sluiten van dubbele aanhalingstekens) | 'Hallo wereld' (dubbele aanhalingstekens) |
| Johan dag (enkel aanhalingsteken) | Johan dag (apostrof) |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Tekst normalisatie uitgevoerd door de aangepaste spraak-Service.

Deze service wordt de volgende tekst normalisering uitvoeren op gegevens die zijn geïmporteerd als een gegevensset taal of transcriptie voor een akoestisch gegevensset. Dit omvat

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

### <a name="text-normalization-required-by-users"></a>Tekst normalisatie-gebruikers

Om te controleren of het beste gebruik van uw gegevens, moeten de volgende normalisatieregels worden toegepast op uw gegevens voordat u deze importeert.

*   Afkortingen moeten worden uitgeschreven in woorden in overeenstemming met gesproken formulier
*   Niet-standaard numerieke tekenreeksen moeten worden uitgeschreven in woorden
*   Woorden met niet-alfabetische tekens of gemengde alfanumerieke tekens bevatten moeten zoals uitgesproken als worden omgezet
*   Algemene acroniemen kunnen blijven als één entiteit zonder punten of spaties tussen de letters, maar alle andere acroniemen moeten worden uitgeschreven in afzonderlijke letters met elke letter gescheiden door een spatie

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| 14 NE 3rd noodherstel. | veertien noordoosten derde station |
| Dr. Strangelove | Arts Strangelove |
| Jeroen obligatie 007 | Jeroen obligatie dubbele geselecteerd zeven |
| Ke$ ha | Kesha |
| Hoe lang is de 2 x 4 | Hoe lang is de twee door vier |
| De vergadering gaat van 1-3 uur | De vergadering gaat één tot drie uur |
| Mijn bloedgroep is O + | Mijn bloedgroep is positief O |
| water is H20 | water is H 2 O |
| OU812 door Van Halen afspelen | O U 8 1 2 Van Halen afspelen |

## <a name="transcription-guidelines-for-zh-cn"></a>Schrijffouten richtlijnen voor het zh-CN

Gegevens geüpload naar de aangepaste spraak-Service moet gebruiken **UTF-8-codering (inclusief BOM)**. Elke regel van het bestand moet de tekst voor een enkele utterance alleen bevatten.

Het is belangrijk om te voorkomen dat het gebruik van halve breedte leestekens. Dit kan per ongeluk gebeuren als het voorbereiden van de gegevens in een woord verwerken programma of slijmen gegevens van webpagina's. Deze vervangen door de juiste volledige breedte vervangingen. Bijvoorbeeld:

| Unicode om te voorkomen | ASCII-vervanging |
|----- | ----- |
| '你好' (openen en sluiten dubbele aanhalingstekens) | '你好' (dubbele aanhalingstekens) |
| 需要什么帮助? (vraagteken) | 需要什么帮助? |

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Tekst normalisatie uitgevoerd door de aangepaste spraak-Service.

Deze service spraak wordt de volgende tekst normalisering uitvoeren op gegevens die zijn geïmporteerd als een gegevensset taal of transcriptie voor een akoestisch gegevensset. Dit omvat

*   Alle interpunctie verwijderen
*   Uitbreiding van de getallen gesproken formulier
*   Volledige breedte letters niet converteren naar halve breedte letters.
*   Gebruik van hoofdletters alle woorden voor Engels

Hier volgen enkele voorbeelden:

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| 3.1415 | 三 点 一 四 一 五 |
| ￥3.5 | 三 元 五 角 |
| w f y z | W F Y Z |
| 1992年8月8日 | 一 九 九 二 年 八 月 八 日 |
| 你吃饭了吗? | 你 吃饭 了 吗 |
| 下午5:00的航班 | 下午 五点 的 航班 |
| 我今年21岁 | 我 今年 二十 一 岁 |

### <a name="text-normalization-required-by-users"></a>Tekst normalisatie-gebruikers

Om te controleren of het beste gebruik van uw gegevens, moeten de volgende normalisatieregels worden toegepast op uw gegevens _voorafgaande_ om te importeren.

*   Afkortingen moeten worden uitgeschreven in woorden in overeenstemming met gesproken formulier
*   Deze service dekt niet alle numerieke hoeveelheden. Het is betrouwbaarder numerieke tekenreeksen in gesproken vorm schrijven

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| 我今年21 | 我今年二十一 |
| 3号楼504 | 三号 楼 五 零 四 |

## <a name="transcription-guidelines-for-de-de"></a>Schrijffouten richtlijnen voor het nl-nl

Gebruik tekstgegevens geüpload naar de aangepaste spraak Service alleen **UTF-8-codering (inclusief BOM)**. Elke regel van het bestand moet de tekst voor een enkele utterance alleen bevatten.

### <a name="text-normalization-performed-by-the-custom-speech-service"></a>Tekst normalisatie uitgevoerd door de aangepaste spraak-Service.

Deze service wordt de volgende tekst normalisering uitvoeren op gegevens die zijn geïmporteerd als een gegevensset taal of transcriptie voor een akoestisch gegevensset. Dit omvat

*   Kleine hoofdlettergebruik alle tekst
*   Verwijderen van alle interpunctie inclusief Engels of Duits aanhalingstekens ('test', 'test', 'test' of «testen» zijn ok)
*   Elke rij met een speciaal teken inclusief negeren: ^ ¢ £ ¤ ¥ ¦ artikel © ª ¬­® ° + ² µ × ÿ Ø¬¬
*   Uitbreiding van de getallen in word formulier, met inbegrip van of bedragen
*   We accepteren alleen umlauts voor een, o u; anderen worden vervangen door 'e' of verwijderd

Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na normalisatie |
|----- | ----- |
| Frankfurter Ring | frankfurter ring |
| 'Hallo, Mama!' sagt die Tochter. | Hallo mama sagt die tochter |
| ¡EINE Frage! | een frage |
| WIR, haben | WIR haben |
| DAS macht $10 | DAS macht zehn bedragen |


### <a name="text-normalization-required-by-users"></a>Tekst normalisatie-gebruikers

Om te controleren of het beste gebruik van uw gegevens, moeten de volgende normalisatieregels worden toegepast op uw gegevens voordat u deze importeert.

*   Drijvende komma moet zijn, en niet. bijvoorbeeld 2,3% en niet 2.3%
*   Tijdscheidingsteken tussen de uren en minuten moet: en not., bijvoorbeeld, 12:00 Uhr
*   Afkortingen zoals 'ca.', 'bzw'. niet worden vervangen. We raden je aan het gebruik van de volledige vorm om de juiste uitspraak.
*   De belangrijkste vijf rekenkundige operatoren zijn verwijderd: +, -, \*, /.
 Het is raadzaam om deze te vervangen door hun letterlijke formulier plus, min, mal, geteilt.
*   Dit geldt ook voor de comparators (=, <>,) - Tautologisch, kleiner als, grösser als
*   Fractie zoals 3/4 in word-formulier drei viertel in plaats van ¾ gebruiken
*   Het symbool € vervangen door de word-indeling 'Euro'


Hier volgen enkele voorbeelden

| Oorspronkelijke tekst | Na de normalisering van gebruiker | Nadat het systeem normalisatie
|--------  | ----- | -------- |
| ES ist 12.23Uhr | ES ist 12:23Uhr | ES ist zwölf uhr drei en zwanzig uhr |
| {12,45} | {12,45} | zwölf komma vier fünf |
| 3 < 5 | 3 kleiner als 5 | drei kleiner als vier |
| 2 + 3 en 4 | 2 plus 3 min 4 | u plus drei min vier|
| DAS macht 12€ | DAS macht 12 euro 's | DAS macht zwölf euro 's |



## <a name="next-steps"></a>Volgende stappen
* [Het gebruik van een aangepaste spraak naar tekst-eindpunt](cognitive-services-custom-speech-create-endpoint.md)
* De nauwkeurigheid vergroten met uw [aangepaste akoestisch model](cognitive-services-custom-speech-create-acoustic-model.md)
* De nauwkeurigheid vergroten met een [aangepaste taalmodel](cognitive-services-custom-speech-create-language-model.md)
