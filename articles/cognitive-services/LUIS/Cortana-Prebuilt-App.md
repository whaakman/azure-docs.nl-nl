---
title: Gebruik Cortana vooraf gedefinieerde app van LUIS | Microsoft Docs
description: Cortana persoonlijke assistent, een vooraf gedefinieerde toepassing van Language Understanding Intelligent Services (LUIS) gebruiken.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: c7249cb8d8cff29f419412025c69e3b2b76b49d1
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110654"
---
# <a name="cortana-prebuilt-app"></a>Vooraf gedefinieerde Cortana-App

> [!IMPORTANT]
> Het is raadzaam dat u de [vooraf gedefinieerde domeinen](./luis-how-to-use-prebuilt-domains.md), in plaats van de vooraf gedefinieerde Cortana-app. Bijvoorbeeld, in plaats van **builtin.intent.calendar.create_calendar_entry**, gebruik **Calendar.Add** van de **kalender** vooraf gemaakte domein.
> De vooraf gedefinieerde domeinen bieden deze voordelen: 
> * Ze bieden pakketten van vooraf gedefinieerde en pretrained intents en entiteiten die zijn ontworpen voor gebruik met elkaar. U kunt een vooraf gedefinieerde domein rechtstreeks in uw app integreren. Bijvoorbeeld, als u een tracering geschiktheid maakt, kunt u toevoegen de **geschiktheid** domein en een volledige reeks intents en entiteiten voor het bijhouden van geschiktheid activiteiten, met inbegrip van intents voor het bijhouden van gewicht en maaltijd planning resterende tijd hebben of afstand en notities van de activiteit opgeslagen geschiktheid.
> * De vooraf gedefinieerde domein die kunnen worden aangepast. Bijvoorbeeld, als u geven beoordelingen van hotels wilt, u kunt trainen en aanpassen de **Places.GetReviews** opzet van de **plaatsen** domein voor het herkennen van aanvragen voor hotel revisies.
> * De vooraf gedefinieerde domeinen worden uitgebreid. Bijvoorbeeld, als u wilt gebruiken de **plaatsen** vooraf gemaakte domein in een bot waarin wordt gezocht naar restaurant en is het nodig de opzet voor het ophalen van het type cuisine die u kunt bouwen en trainen een **Places.GetCuisine** bedoeld.

Naast de mogelijkheid die u om uw eigen toepassingen te bouwen, biedt LUIS ook intents en entiteiten van Microsoft Cortana personal assistant als een vooraf gedefinieerde app. Het gedrag van deze openbaar LUIS app kan niet worden gewijzigd. De intents en entiteiten in deze toepassing kunnen niet worden bewerkt of geïntegreerd in andere LUIS-apps. Als u de clienttoepassing toegang hebben tot zowel deze vooraf gemaakte toepassing en uw eigen toepassing LUIS dat wilt, heeft uw clienttoepassing verwijzen naar beide LUIS-apps.

De vooraf gedefinieerde persoonlijke assistent-app is beschikbaar in deze culturen (landinstellingen): Engels, Frans, Italiaans, Spaans en Chinees.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Ophalen van het eindpunt voor de vooraf gedefinieerde Cortana-app

U kunt de vooraf gedefinieerde Cortana-app met behulp van de volgende eindpunten openen: 

| Taal | Eindpunt|
|--------| ------------------|
| Nederlands| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Chinees| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Frans| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    Spaans| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    Italiaans| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


> [!NOTE]
> Het eindpunt URL's zijn ook toegankelijk in de [apps - persoonlijke assistent toepassingen ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32) API.

## <a name="try-out-the-personal-assistant-app"></a>De app persoonlijke assistent uitproberen
U kunt uw eindpunt sleutel argument en query-tekenreeks voor het aanroepen van het eindpunt toevoegen aan het eindpunt. 

Bijvoorbeeld, als de utterance die u wilt interpreteren is 'maken een afspraak voor teamvergadering', en vervolgens u die utterance aan de eindpunt-URL toevoegen kunt. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key={YOUR-SUBSCRIPTION-KEY}&q=create an appointment for team meeting
```

Plak de URL in een webbrowser en vervang de eindpunt-toegangssleutel voor de `{YOUR-SUBSCRIPTION-KEY}` veld.

In de browser kunt u zien dat de vooraf gedefinieerde app Cortana identificeert `builtin.intent.calendar.create_calendar_entry` als het doel en `builtin.calendar.title` als het entiteitstype, en voor de utterance `create an appointment for team meeting`.

![Vooraf gedefinieerde Cortana-app gebruiken](./media/luis-how-to-prebuilt-cortana/luis-prebuilt-cortana-browser.png)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Vooraf gedefinieerde app Cortana verwijst naar](./luis-reference-cortana-prebuilt.md)

