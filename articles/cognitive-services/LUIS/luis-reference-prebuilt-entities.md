---
title: Vooraf gedefinieerde LUIS entiteiten referentie - Azure | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat een lijst met de vooraf gedefinieerde entiteiten die zijn opgenomen in het Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 7ce50e4c0be605e1700a2c18533cb087384f524c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316881"
---
# <a name="entities-per-culture"></a>Entiteiten per cultuur

Language Understanding (LUIS) biedt vooraf gedefinieerde entiteiten. Wanneer een vooraf gedefinieerde entiteit is opgenomen in uw toepassing, omvat LUIS de overeenkomstige entiteit voorspelling in het antwoord van het eindpunt. Alle voorbeeld utterances zijn ook voorzien van de entiteit. Het gedrag van vooraf gedefinieerde entiteiten **kan niet** worden gewijzigd. Tenzij anders vermeld, is vooraf gemaakte entiteiten zijn beschikbaar in alle LUIS toepassing landinstellingen (culturen). De volgende tabel toont de vooraf gedefinieerde entiteiten die worden ondersteund voor elke cultuur.

Vooraf gedefinieerde entiteit   |   Engels (Verenigde Staten)<br>```En-us```   |   Frans (Frankrijk)<br>```fr-FR```   |   Italiaans (Italië)<br>```it-IT```   |   Spaans (Spanje)<br>```es-ES```   |   Chinees<br>```zh-CN```   |   Duits<br>```de-DE```   |   Portugees (Brazilië)<br>```pt-BR```   |   Japans (Japan)<br>```ja-JP```   |   Koreaans (Korea)<br>```ko-kr```   | Frans (Canada)<br>```fr-CA```   |   Spaans (Mexico)<br>```es-MX```   |   Nederlands (Nederland)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>breukdeel unit (ex: cent)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>datum<br>DateRange<br>tijd<br>timerange   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Aantal](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Rangtelwoord](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>c   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   -   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Zie de opmerkingen op [afgeschaft vooraf gemaakte entiteiten](luis-reference-prebuilt-deprecated.md)

## <a name="examples-of-prebuilt-entities-in-en-us-culture"></a>Voorbeelden van vooraf gedefinieerde entiteiten in de norm en-us cultuur
De volgende tabel bevat vooraf gedefinieerde entiteiten met voorbeeldgegevens en de retourwaarden.

Vooraf gedefinieerde entiteit   |   Voorbeeld utterance   |   JSON
------|------|------|
 ```builtin.age```   |   ```100 year old```   |```{ "type": "builtin.age", "entity": "100 year old" }```|  
 ```builtin.age```   |   ```19 years old```   |```{ "type": "builtin.age", "entity": "19 years old" }```|
 ```builtin.currency```     |   ```1000.00 US dollars```   |```{ "type": "builtin.currency", "entity": "1000.00 us dollars" }```
 ```builtin.currency```     |   ```$ 67.5 B```   |```{ "type": "builtin.currency", "entity": "$ 67.5" }```|
 ```builtin.datetimeV2``` | Zie [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) | Zie [builtin.datetimeV2](luis-reference-prebuilt-datetimev2.md) |
 ```builtin.dimension```     |   ```2 miles```   |```{ "type": "builtin.dimension", "entity": "2 miles" }```|
 ```builtin.dimension```     |  ```650 square kilometers```   |```{ "type": "builtin.dimension", "entity": "650 square kilometers" }```|
 ```builtin.email```     |  ```patti.owens@microsoft.com```   |```{ "type": "builtin.email", "entity": "patti.owens@microsoft.com" }```|
 ```builtin.number```     |  ```ten```   |``` { "type": "builtin.number", "entity": "ten" } ```|
 ```builtin.number```     |   ```3.1415```   |```  { "type": "builtin.number", "entity": "3 . 1415" }``` |
 ```builtin.ordinal```     |   ```first```   |```{ "type": "builtin.ordinal", "entity": "first" }``` |
 ```builtin.ordinal```     |   ```10th```   | ```{ "type": "builtin.ordinal", "entity": "10th" }``` |  
 ```builtin.percentage```   |   ```The stock price increase by 7 $ this year```   |```{ "type": "builtin.percentage", "entity": "7 %" }```|
 ```builtin.phonenumber```   |   ```my mobile is 00 44 161 1234567```   |```{ "type": "builtin.phonenumber", "entity": "00 44 161 1234567" }```|
 ```builtin.temperature```     |   ```10 degrees celsius```   | ```{ "type": "builtin.temperature", "entity": "10 degrees celcius" }```|   
 ```builtin.temperature```     |   ```78 F```   |```{ "type": "builtin.temperature", "entity": "78 f" }```|
 ```builtin.url```     |   ```http://www.luis.ai is a great cognitive service```   |```{ "type": "builtin.url", "entity": "http://www.luis.ai" }```|


## <a name="contribute-to-prebuilt-entity-cultures"></a>Bijdragen aan vooraf gedefinieerde entiteit culturen
De vooraf gedefinieerde entiteiten worden ontwikkeld in de kenmerken tekst open source-project. Neem [bijdragen](https://github.com/Microsoft/Recognizers-Text) aan het project. Dit project bevat voorbeelden van valuta per cultuur. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [getal](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), en [valuta](luis-reference-prebuilt-currency.md) entiteiten. 