---
title: Word-informatie voor uitlijning met de API van Microsoft Translator tekst | Microsoft Docs
description: Word uitlijningsgegevens ontvangen van de API van Microsoft Translator tekst.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 9d8284db61235284ec0d5a1594c34687c89560e9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344816"
---
# <a name="how-to-receive-word-alignment-information"></a>Het woord uitlijning informatie te ontvangen

## <a name="receiving-word-alignment-information"></a>Word uitlijning informatie ontvangen
Uitlijning informatie ontvangen, gebruik de methode vertalen en de optionele includeAlignment-parameter opnemen.

## <a name="alignment-information-format"></a>Uitlijning informatie indeling
Uitlijning geretourneerd als een string-waarde van de volgende notatie voor elk woord in de bron. De informatie voor elk woord wordt gescheiden door een spatie, inclusief voor niet-een door spaties gescheiden talen (scripts) zoals Chinees:

[[SourceTextStartIndex]: [SourceTextEndIndex]: [TgtTextStartIndex]: [TgtTextEndIndex]] *

Voorbeeld van een tekenreeks uitlijning: ' 0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21 '.

Met andere woorden, de dubbele punt scheidt begin en einde index, het streepje scheidt de talen en ruimte scheidt de woorden. Een woord mogelijk zijn uitgelijnd met nul, één of meerdere woorden in de andere taal en de uitgelijnde woorden is mogelijk niet-aaneengesloten. Wanneer er geen uitlijning-informatie beschikbaar is, wordt de uitlijning element niet leeg zijn. De methode retourneert geen fout in dat geval.

## <a name="restrictions"></a>Beperkingen
Uitlijning wordt alleen geretourneerd voor een subset van de taal-paren die op dit moment:
* van de Engelse taal;
* vanuit een andere taal Engels, met uitzondering van vereenvoudigd Chinees, traditioneel Chinees en Lets naar het Engels
* van Japans Koreaans of van Koreaans naar het Japanse u ontvangt geen uitlijning informatie als de zin een voorgedefinieerde vertaling is. Voorbeeld van een voorgedefinieerde vertaling is 'Dit is een test', 'Ik hou van je' en andere zinnen met hoge frequentie.

## <a name="example"></a>Voorbeeld

JSON-voorbeeld

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
