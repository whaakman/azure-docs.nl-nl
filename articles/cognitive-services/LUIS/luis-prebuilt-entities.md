---
title: Vooraf gemaakte entiteiten in LUIS | Microsoft Docs
description: In dit artikel bevat een lijst met de vooraf gemaakte entiteiten die zijn opgenomen in Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: f7abf6d8a9f0fe18017fe5c54801ac0d3b6c379e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170780"
---
# <a name="prebuilt-entities"></a>Vooraf gemaakte entiteiten

LUIS bevat een set met vooraf gemaakte entiteiten voor het herkennen van algemene typen gegevens, zoals datums, tijden, getallen, metingen en valuta. Ondersteuning voor vooraf gedefinieerde entiteit is afhankelijk van de cultuur van uw LUIS-app. Zie voor een volledige lijst van de vooraf gemaakte entiteiten die LUIS worden ondersteund, met inbegrip van ondersteuning door cultuur, de [verwijzing naar de vooraf gedefinieerde entiteit](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> **Builtin.DateTime** is afgeschaft. Deze is vervangen door [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), waarmee u erkenning van datum en tijd bereiken, evenals verbeterde herkenning van niet-eenduidige datums en tijden.

## <a name="add-a-prebuilt-entity"></a>Een vooraf gedefinieerde entiteit toevoegen

1. Open uw app door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **entiteiten** in aan de linkerkant. 
2. Op de **entiteiten** pagina, klikt u op **vooraf gemaakte entiteiten beheren**.

    ![Entiteiten pagina: vooraf gemaakte entiteiten beheren](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. In **vooraf gemaakte entiteiten toevoegen** dialoogvenster vak, klikt u op de vooraf gedefinieerde entiteit die u wilt toevoegen (bijvoorbeeld 'datetimeV2'). Klik vervolgens op **Opslaan**.

    ![In het dialoogvenster van vooraf gedefinieerde entiteit toevoegen](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Een vooraf gedefinieerde numerieke entiteit gebruiken
Wanneer een vooraf gedefinieerde entiteit is opgenomen in uw toepassing, wordt de voorspellingen zijn opgenomen in uw gepubliceerde toepassing. Het gedrag van vooraf gemaakte entiteiten is vooraf getrainde en **kan geen** worden gewijzigd. Volg deze stappen om te zien hoe een vooraf gedefinieerde entiteit werkt:

1. Voeg een **getal** entiteit aan uw app, klikt u vervolgens [Train](luis-interactive-test.md) en [publiceren](luis-how-to-publish-app.md) de app.
2. Klik op de eindpunt-URL in de **App publiceren** pagina de LUIS-eindpunt in een webbrowser geopend. 
3. Een utterance toevoegen aan de URL die een numerieke expressie bevat. Bijvoorbeeld, u kunt typen `buy two plane ticktets`, en om te zien die LUIS identificeert `two` als een `builtin.number` entiteit, en identificeert `2` als de waarde in de `resolution` veld. De `resolution` veld helpt u bij het omzetten van getallen en datums in een canonieke vorm die is het eenvoudiger voor uw clienttoepassing te gebruiken. 

    ![utterance in browser met een aantal entiteit](./media/luis-use-prebuilt-entity/browser-query.png)

LUIS herkent op intelligente wijze nummers die zich niet in niet-standaard formulier. Probeer andere numerieke expressies in uw uitingen en Zie wat LUIS retourneert.

Het volgende voorbeeld ziet een JSON-antwoord van LUIS, die de resolutie van de waarde van 24 uur per dag, voor de utterance "tientallen" bevat.

```json
{
  "query": "order two dozen tickets for group travel",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.905443209
  },
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```
## <a name="use-a-prebuilt-datetimev2-entity"></a>Een vooraf gedefinieerde datetimeV2 entiteit gebruiken
De **datetimeV2** vooraf gedefinieerde entiteit herkent datums, tijden, datumbereiken en tijdsduren. Volg deze stappen om te zien hoe de `datetimeV2` vooraf gedefinieerde entiteit werkt:

1. Voeg een **datetimeV2** entiteit aan uw app, klikt u vervolgens [Train](luis-interactive-test.md) en [publiceren](luis-how-to-publish-app.md) de app.
2. Klik op de eindpunt-URL in de **App publiceren** pagina de LUIS-eindpunt in een webbrowser geopend. 
3. Een utterance toevoegen aan de URL die een bepaalde periode bevat. Bijvoorbeeld, u kunt typen `book a flight tomorrow`, en om te zien die LUIS identificeert `tomorrow` als een `builtin.datetimeV2.date` entiteit, en identificeert de datum van morgen als de waarde in de `resolution` veld. 

Het volgende voorbeeld ziet wat de JSON-antwoord van LUIS eruitziet als de datum van vandaag 31 oktober-2017.

```json
{
  "query": "book a flight tomorrow",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9063408
  },
  "entities": [
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 14,
      "endIndex": 21,
      "resolution": {
        "values": [
          {
            "timex": "2017-11-01",
            "type": "date",
            "value": "2017-11-01"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Verwijzing naar bestaande entiteit](./luis-reference-prebuilt-entities.md)
