---
title: Toepassingsinstellingen
titleSuffix: Azure Cognitive Services
description: Toepassingsinstellingen voor apps van Language understanding begrijpen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 3682b9e0c38344be1522440290b46f8c10bd5607
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275912"
---
# <a name="application-settings"></a>Toepassingsinstellingen

Instellingen voor deze toepassing worden opgeslagen in de [geëxporteerd](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) app en [bijgewerkt](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) met de REST API's. Wijzigen van de instellingen van uw app-versie Hiermee stelt u de status van uw app-training op ongetrainde.

|Instelling|Standaardwaarde|Opmerkingen|
|--|--|--|
|NormalizePunctuation|Waar|Hiermee verwijdert u leestekens.|
|NormalizeDiacritics|Waar|Hiermee verwijdert u diakritische tekens.|

## <a name="diacritics-normalization"></a>Diakritische tekens normalisering 

Schakel utterance normalisering voor diakritische tekens naar uw LUIS-JSON-app-bestand in de `settings` parameter.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

De volgende uitingen laten zien hoe diakritische tekens normalisering van invloed is op uitingen:

|Met diakritische tekens ingesteld op false|Met diakritische tekens ingesteld op true|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Taalondersteuning voor diakritische tekens

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Portugees (Brazilië) `pt-br` diakritische tekens

|Diakritische tekens ingesteld op false|Diakritische tekens is ingesteld op true|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`| 
|||

#### <a name="dutch-nl-nl-diacritics"></a>Nederlands `nl-nl` diakritische tekens

|Diakritische tekens ingesteld op false|Diakritische tekens is ingesteld op true|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`| 
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Frans `fr-` diakritische tekens

Dit omvat zowel Franse en Canadese overenting.

|Diakritische tekens ingesteld op false|Diakritische tekens is ingesteld op true|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`| 
|`ê`|`e`| 
|`î`|`i`| 
|`ô`|`o`| 
|`û`|`u`| 
|`ç`|`c`| 
|`ë`|`e`| 
|`ï`|`i`| 
|`ü`|`u`| 
|`ÿ`|`y`| 

#### <a name="german-de-de-diacritics"></a>Duits `de-de` diakritische tekens

|Diakritische tekens ingesteld op false|Diakritische tekens is ingesteld op true|
|--|--|
|`ä`|`a`|
|`ö`|`o`| 
|`ü`|`u`| 

#### <a name="italian-it-it-diacritics"></a>Italiaans `it-it` diakritische tekens

|Diakritische tekens ingesteld op false|Diakritische tekens is ingesteld op true|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`| 
|`í`|`i`| 
|`î`|`i`| 
|`ò`|`o`| 
|`ó`|`o`| 
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>Spaans `es-` diakritische tekens

Dit omvat zowel Spaanse en Canadese Mexicaanse.

|Diakritische tekens ingesteld op false|Diakritische tekens is ingesteld op true|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`| 
|`ó`|`o`| 
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Leestekens normalisering

Schakel utterance normalisering voor leestekens naar uw LUIS-JSON-app-bestand in de `settings` parameter.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
] 
```

De volgende uitingen laten zien hoe diakritische tekens heeft gevolgen voor uitingen:

|Met diakritische tekens ingesteld op False|Met diakritische tekens ingesteld op True|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Interpunctie

De volgende leestekens is verwijderd met `NormalizePunctuation` is ingesteld op true.

|Leestekens|
|--|
|`-`| 
|`.`| 
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|
