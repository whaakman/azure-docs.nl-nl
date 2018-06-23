---
title: Vooraf gedefinieerde entiteiten in LUIS | Microsoft Docs
description: Dit artikel bevat een lijst met de vooraf gedefinieerde entiteiten die zijn opgenomen in Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: 4858de8b8517016ca385e9105ca8948b66aa683b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36322049"
---
# <a name="prebuilt-entities"></a>Vooraf gedefinieerde entiteiten

LUIS bevat een reeks vooraf gedefinieerde entiteiten voor het herkennen van algemene typen informatie, zoals datums, tijden, getallen, metingen en valuta. Vooraf gedefinieerde entiteit ondersteuning verschilt door de cultuur van uw app LUIS. Zie voor een volledige lijst van de vooraf gedefinieerde entiteiten die LUIS ondersteunt, inclusief ondersteuning door cultuur, de [vooraf gedefinieerde entiteitsverwijzing](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> **Builtin.DateTime** is afgeschaft. Het is vervangen door [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), waarmee u de opname van de datum en tijd bereiken, evenals verbeterde herkenning van niet-eenduidige datums en tijden.

## <a name="add-a-prebuilt-entity"></a>Een vooraf gedefinieerde entiteit toevoegen

1. Uw app openen door te klikken op de naam ervan op **mijn Apps** pagina en klik vervolgens op **entiteiten** aan de linkerkant. 
2. Op de **entiteiten** pagina, klikt u op **vooraf gemaakte entiteiten beheren**.

    ![Pagina entiteiten - vooraf gemaakte entiteiten beheren](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. In **vooraf gemaakte entiteiten toevoegen** dialoogvenster vak, klikt u op de vooraf gedefinieerde entiteit die u wilt toevoegen (bijvoorbeeld 'datetimeV2'). Klik vervolgens op **Opslaan**.

    ![Dialoogvenster vooraf gedefinieerde entiteit toevoegen](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Gebruik een vooraf gedefinieerde nummer entiteit
Wanneer een vooraf gedefinieerde entiteit is opgenomen in uw toepassing, worden de voorspellingen opgenomen in uw gepubliceerde toepassing. Het gedrag van vooraf gedefinieerde entiteiten is vooraf getraind en **kan niet** worden gewijzigd. Volg deze stappen om te zien hoe een vooraf gedefinieerde entiteit werkt:

1. Voeg een **getal** entiteit aan uw app vervolgens [Train](interactive-test.md) en [publiceren](PublishApp.md) de app.
2. Klik op de eindpunt-URL in de **App publiceren** pagina het eindpunt LUIS in een webbrowser geopend. 
3. Een utterance toevoegen aan de URL een numerieke expressie bevat. U kunt bijvoorbeeld `buy two plane ticktets`, en om te zien die LUIS identificeert `two` als een `builtin.number` entiteit, en identificeert `2` als de waarde in de `resolution` veld. De `resolution` -veld kunt u getallen en datums omzetten in een canonieke vorm die is het gemakkelijker om uw clienttoepassing te gebruiken. 

    ![utterance in een browser met een aantal entiteit](./media/luis-use-prebuilt-entity/browser-query.png)

LUIS herkent cijfers die zich niet in niet-standaard formulier op intelligente wijze. Andere numerieke expressies in uw utterances uitproberen en zien wat LUIS retourneert.

Het volgende voorbeeld ziet een JSON-reactie van LUIS, die de resolutie van de waarde van 24, voor de utterance "twintig" bevat.

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
## <a name="use-a-prebuilt-datetimev2-entity"></a>Gebruik van een vooraf gedefinieerde datetimeV2 entiteit
De **datetimeV2** vooraf gedefinieerde entiteit herkent datums, tijden, datumbereiken en tijdsduren. Volg deze stappen om te zien hoe de `datetimeV2` vooraf gedefinieerde entiteit werkt:

1. Voeg een **datetimeV2** entiteit aan uw app vervolgens [Train](interactive-test.md) en [publiceren](PublishApp.md) de app.
2. Klik op de eindpunt-URL in de **App publiceren** pagina het eindpunt LUIS in een webbrowser geopend. 
3. Een utterance toevoegen aan de URL een datumbereik bevat. U kunt bijvoorbeeld `book a flight tomorrow`, en om te zien die LUIS identificeert `tomorrow` als een `builtin.datetimeV2.date` entiteit, en identificeert de datum van morgen als de waarde in de `resolution` veld. 

Het volgende voorbeeld ziet wat het JSON-antwoord van LUIS als volgt uitzien als de datum van vandaag oktober 31 2017.

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
> [Vooraf gedefinieerde entiteitsverwijzing](./luis-reference-prebuilt-entities.md)
