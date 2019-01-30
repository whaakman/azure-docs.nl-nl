---
title: Alle vooraf gemaakte entiteiten
titleSuffix: Azure Cognitive Services
description: In dit artikel bevat een lijst met de vooraf gemaakte entiteiten die zijn opgenomen in Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 4d5ce9775e7844fcc82aa993f5b01c7cc7ae4779
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213732"
---
# <a name="entities-per-culture-in-your-luis-model"></a>Entiteiten per cultuur in uw LUIS-model

Language Understanding (LUIS) biedt vooraf gemaakte entiteiten. Wanneer een vooraf gedefinieerde entiteit is opgenomen in uw toepassing, bevat LUIS de bijbehorende voorspelling van de entiteit in het antwoord van het eindpunt. Alle uitingen van voorbeeld zijn ook met de entiteit gelabeld. Het gedrag van vooraf gemaakte entiteiten **kan geen** worden gewijzigd. Tenzij anders vermeld, is vooraf gemaakte entiteiten zijn beschikbaar in alle LUIS toepassing landinstellingen (culturen). De volgende tabel ziet u de vooraf gemaakte entiteiten die worden ondersteund voor elke cultuur.

|Culture|Overenting|
|--|--|
|Chinees|[zh-CN](#chinese-entity-support)|
|Nederlands|[NL-NL](#dutch-entity-support)|
|Nederlands|[en-US (Amerikaans)](#english-american-entity-support)|
|Frans|[fr-CA (Canada)](#french-canadian-entity-support), [fr-FR (Frankrijk)](#french-france-entity-support), |
|Duits|[nl-nl](#german-entity-support)|
|Italiaans|[IT-IT](#italian-entity-support)|
|Japans|[ja-JP](#japanese-entity-support)|
|Koreaans|[ko-KR](#korean-entity-support)|
|Portugees|[pt-BR (Brazilië)](#portuguese-brazil-entity-support)|
|Spaans|[es-ES (Spanje)](#spanish-spain-entity-support), [es-MX (Mexico)](#spanish-mexico-entity-support)|

## <a name="chinese-entity-support"></a>Ondersteuning voor Chinese entiteit

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```zh-CN``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    -   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="dutch-entity-support"></a>Nederlands entiteit ondersteuning

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```nl-NL``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    -   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="english-american-entity-support"></a>Ondersteuning voor Engels (Amerikaans) entiteit

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```en-US``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-france-entity-support"></a>Ondersteuning van de entiteit Frans (Frankrijk)

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```fr-FR``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-canadian-entity-support"></a>Frans (Canadees) entiteit ondersteuning

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```fr-CA``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="german-entity-support"></a>Ondersteuning voor Duitse entiteit

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```de-DE``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="italian-entity-support"></a>Italiaans entiteit ondersteuning

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```it-IT``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="japanese-entity-support"></a>Ondersteuning voor Japans entiteit

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```ja-JP``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="korean-entity-support"></a>Koreaans entiteit ondersteuning

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```ko-KR``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    -   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    -   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    -   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    -   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    -   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    -   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="portuguese-brazil-entity-support"></a>Ondersteuning voor Portugees (Brazilië)-entiteit

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```pt-BR``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-spain-entity-support"></a>Spaans (Spanje) entiteit ondersteuning

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```es-ES``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    ✔   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    ✔   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    ✔   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-mexico-entity-support"></a>Ondersteuning van de entiteit Spaans (Mexico)

De volgende entiteiten worden ondersteund:

|Vooraf gemaakte entiteiten|```es-MX``` |
------|:------:|
[Leeftijd](luis-reference-prebuilt-age.md):<br>jaar<br>maand<br>week<br>dag   |    -   |
[Valuta](luis-reference-prebuilt-currency.md):<br>dollar<br>decimale eenheid (ex: afronding)  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md):<br>date<br>DateRange<br>time<br>timerange   |    -   | 
[Dimensie](luis-reference-prebuilt-dimension.md):<br>volume<br>gebied<br>gewicht<br>informatie (ex: bits/byte)<br>de lengte (ex: meter)<br>snelheid (ex: mijl per uur)  |    -   | 
[E-mail](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[KeyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[Number](luis-reference-prebuilt-number.md)   |    ✔   |  
[Volgnummer](luis-reference-prebuilt-ordinal.md)   |    -   |  
[Percentage](luis-reference-prebuilt-percentage.md)   |    -   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[Telefoonnummer](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[Temperatuur](luis-reference-prebuilt-temperature.md):<br>Fahrenheit<br>Kelvin<br>rankine<br>delisle<br>Celsius   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

Zie Opmerkingen op [afgeschaft vooraf gemaakte entiteiten](luis-reference-prebuilt-deprecated.md)

KeyPhrase is niet beschikbaar in alle overenting Portugees (Brazilië) - ```pt-BR```.

## <a name="contribute-to-prebuilt-entity-cultures"></a>Bijdragen aan de vooraf gedefinieerde entiteit culturen
De vooraf gemaakte entiteiten zijn ontwikkeld in de tekst van de kenmerken die zijn open source-project. [Bijdragen](https://github.com/Microsoft/Recognizers-Text) aan het project. Dit project bevat voorbeelden van valuta per cultuur. 

GeographyV2 en PersonName zijn niet opgenomen in het project kenmerken tekst. Open voor problemen met deze vooraf gemaakte entiteiten, een [ondersteuningsaanvraag](../../azure-supportability/how-to-create-azure-support-request.md). 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [getal](luis-reference-prebuilt-number.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), en [valuta](luis-reference-prebuilt-currency.md) entiteiten. 
