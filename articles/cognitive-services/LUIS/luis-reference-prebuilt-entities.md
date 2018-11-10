---
title: Vooraf gemaakte entiteiten - LUIS
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat een lijst met de vooraf gemaakte entiteiten die zijn opgenomen in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: bc23f2e5d8304400802c74093d4d78a1fcd8cf22
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51219479"
---
# <a name="entities-per-culture"></a>Entiteiten per cultuur

Language Understanding (LUIS) biedt vooraf gemaakte entiteiten. Wanneer een vooraf gedefinieerde entiteit is opgenomen in uw toepassing, bevat LUIS de bijbehorende voorspelling van de entiteit in het antwoord van het eindpunt. Alle uitingen van voorbeeld zijn ook met de entiteit gelabeld. Het gedrag van vooraf gemaakte entiteiten **kan geen** worden gewijzigd. Tenzij anders vermeld, is vooraf gemaakte entiteiten zijn beschikbaar in alle LUIS toepassing landinstellingen (culturen). De volgende tabel ziet u de vooraf gemaakte entiteiten die worden ondersteund voor elke cultuur.

Vooraf gemaakte entiteiten   |   Engels (Verenigde Staten)<br>```En-us```   |   Frans (Frankrijk)<br>```fr-FR```   |   Italiaans (Italië)<br>```it-IT```   |   Spaans (Spanje)<br>```es-ES```   |   Chinees<br>```zh-CN```   |   Duits<br>```de-DE```   |   Portugees (Brazilië)<br>```pt-BR```   |   Japans (Japan)<br>```ja-JP```   |   Koreaans (Korea)<br>```ko-kr```   | Frans (Canada)<br>```fr-CA```   |   Spaans (Mexico)<br>```es-MX```   |   Nederlands (Nederland)<br>```nl-NL```   |
------|:------:|------|------|------|------|------|------|------|------|------|------|------|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>tijd<br>timerange   |    ✔   |   ✔   |   -   |   ✔   |    ✔   |   -   |   ✔   |   -   |   -   |   -   |   -   |   -   |
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   |   ✔   |   ✔   |   ✔   |   -   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |   ✔   |
[Aantal](luis-reference-prebuilt-number.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   |    -   |    -   |    -   |    ✔   |    -   |    -   |    -   |   -   |   -   |   -   |   -   |
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |    ✔   |   -   |   -   |   -   |   ✔   |
[URL](luis-reference-prebuilt-url.md)   |    ✔   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |   -   |

Zie Opmerkingen op [afgeschaft vooraf gemaakte entiteiten](luis-reference-prebuilt-deprecated.md)

KeyPhrase is niet beschikbaar in alle overenting Portugees (Brazilië) - ```pt-BR```.

## <a name="contribute-to-prebuilt-entity-cultures"></a>Bijdragen aan de vooraf gedefinieerde entiteit culturen
De vooraf gemaakte entiteiten zijn ontwikkeld in de tekst van de kenmerken die zijn open source-project. [Bijdragen](https://github.com/Microsoft/Recognizers-Text) aan het project. Dit project bevat voorbeelden van valuta per cultuur. 

GeographyV2 en PersonName zijn niet opgenomen in het project kenmerken tekst. Open voor problemen met deze vooraf gemaakte entiteiten, een [ondersteuningsaanvraag](../../azure-supportability/how-to-create-azure-support-request.md). 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [getal](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), en [valuta](luis-reference-prebuilt-currency.md) entiteiten. 
