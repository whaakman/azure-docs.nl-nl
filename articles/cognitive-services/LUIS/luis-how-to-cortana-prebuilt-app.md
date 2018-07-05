---
title: Gebruik Cortana vooraf gemaakte app van LUIS | Microsoft Docs
description: Gebruik Cortana persoonlijke assistent, een vooraf gedefinieerde toepassing van Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/03/2018
ms.author: v-geberr
ms.openlocfilehash: 27900068d4420b3e70dacc6d1bdfdf0c52053ceb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37767306"
---
# <a name="cortana-prebuilt-app"></a>Vooraf gemaakte Cortana-App

> [!IMPORTANT]
> Het is raadzaam dat u de [vooraf gemaakte domeinen](./luis-how-to-use-prebuilt-domains.md), in plaats van de vooraf gemaakte Cortana-app. Bijvoorbeeld, in plaats van **builtin.intent.calendar.create_calendar_entry**, gebruikt u **Calendar.Add** uit de **agenda** vooraf gemaakte domein.
> De vooraf gemaakte domeinen bieden deze voordelen: 
> * Ze bieden pakketten van vooraf gedefinieerde en dat intenties en entiteiten die zijn ontworpen om te werken goed met elkaar. U kunt een vooraf gedefinieerde domein rechtstreeks in uw app integreren. Bijvoorbeeld, als u een tracering geschiktheid bouwt, kunt u toevoegen de **geschiktheid** domein en hebben een volledige set van intenties en entiteiten voor het bijhouden van geschiktheid activiteiten, met inbegrip van intents voor het bijhouden van gewicht en maaltijd planning, de resterende tijd of afstand, en opmerkingen bij de opslaan geschiktheid activiteit.
> * De vooraf gedefinieerde domein intents worden aangepast. Bijvoorbeeld, als u geven beoordelingen van hotels wilt, kunt u trainen en aanpassen van de **Places.GetReviews** intentie van de **plaatsen** domein voor het herkennen van aanvragen voor hotel beoordelingen.
> * De vooraf gemaakte domeinen worden uitgebreid. Bijvoorbeeld, als u wilt gebruiken de **plaatsen** vooraf gemaakte domein in een bot waarin wordt gezocht restaurants en moet een doel voor het ophalen van het type cuisine die u kunt bouwen en train een **Places.GetCuisine** intentie.

Naast de mogelijkheid u om uw eigen toepassingen te bouwen, biedt LUIS ook intenties en entiteiten uit de Microsoft Cortana persoonlijke assistent als een vooraf gemaakte app. Het gedrag van deze openbaar beschikbare LUIS-app kan niet worden gewijzigd. De intenties en entiteiten in deze toepassing kunnen niet worden bewerkt of geïntegreerd in andere LUIS-apps. Als u de clienttoepassing toegang hebben tot zowel deze vooraf gedefinieerde toepassing en uw eigen toepassing LUIS, klikt u vervolgens heeft de clienttoepassing om te verwijzen naar beide LUIS-apps.

De vooraf gedefinieerde persoonlijke assistent-app is beschikbaar in deze culturen (landinstellingen): Engels, Frans, Italiaans, Spaans en Chinees.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Haal het eindpunt voor de vooraf gemaakte Cortana-app

U kunt toegang tot de vooraf gemaakte Cortana-app met behulp van de volgende eindpunten: 

| Taal | Eindpunt|
|--------| ------------------|
| Nederlands| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Chinees| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Frans| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    Spaans| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    Italiaans| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


Het eindpunt van de URL's zijn ook toegankelijk in de [apps - ophalen persoonlijke assistent toepassingen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32) API.

## <a name="try-out-the-personal-assistant-app"></a>De app persoonlijke assistent uitproberen
Bijvoorbeeld, als de utterance die u wilt interpreteren is 'maken een afspraak voor teamvergadering', en vervolgens u die utterance aan de eindpunt-URL toevoegen kunt. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key=YOUR-SUBSCRIPTION-KEY&q=create%20an%20appointment%20for%20team%20meeting
```

Plak de URL in een webbrowser en vervang de eindpuntsleutel voor de `YOUR-SUBSCRIPTION-KEY` veld.

In de browser kunt u zien dat de vooraf gemaakte Cortana-app identificeert `builtin.intent.calendar.create_calendar_entry` als het doel en `builtin.calendar.title` als het entiteitstype, en voor de utterance `create an appointment for team meeting`.

```JSON
{
  "query": "create an appointment for team meeting",
  "topScoringIntent": {
    "intent": "builtin.intent.calendar.create_calendar_entry"
  },
  "entities": [
    {
      "entity": "team meeting",
      "type": "builtin.calendar.title"
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vooraf gemaakte Cortana-app verwijzen naar](./luis-reference-cortana-prebuilt.md)

