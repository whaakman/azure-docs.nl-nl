---
title: Vooraf gemaakte modellen
titleSuffix: Language Understanding - Azure Cognitive Services
description: Vooraf gemaakte modellen bieden domeinen, intents uitingen en entiteiten. U kunt uw app te starten met een vooraf gedefinieerde domein of later een relevante domein toevoegen aan uw app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: 75644fd94e6e00e92547cd9e1d42b26c6a55807f
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910625"
---
# <a name="prebuilt-domain-intent-and-entity-models"></a>Vooraf gemaakte modellen voor domein, kunt u lezen wat en entiteit

Vooraf gemaakte modellen bieden domeinen, intents uitingen en entiteiten. U kunt uw app te starten met een vooraf gedefinieerde domein of later een relevante domein toevoegen aan uw app. 

## <a name="types-of-prebuilt-models"></a>Typen van vooraf gemaakte modellen

Er zijn 3 soorten vooraf gemaakte modellen die LUIS biedt. Elk model kan worden toegevoegd aan uw app op elk gewenst moment. 

|Modeltype|Bevat|
|--|--|
|Domain|Intents, uitingen, entiteiten|
|Intents|Intents, uitingen|
|Entiteiten|Alleen entiteiten| 

## <a name="prebuilt-domains"></a>Vooraf gemaakte domeinen

Language Understanding (LUIS) biedt *vooraf gemaakte domeinen*, die zijn vooraf gedefinieerde sets [intents](luis-how-to-add-intents.md) en [entiteiten](luis-concept-entity-types.md) die samen worden gebruikt voor domeinen of algemene categorieën clienttoepassingen. 

De vooraf gemaakte domeinen zijn getraind en gereed is om toe te voegen aan uw LUIS-app. De intenties en entiteiten in een vooraf gedefinieerde domein kunnen volledig worden aangepast wanneer u ze hebt toegevoegd aan uw app. 

Als u vanaf het aanpassen van een volledig domein van de vooraf gedefinieerde start, verwijdert u de intenties en entiteiten die uw app niet hoeven te gebruiken. U kunt ook enkele intents of entiteiten toevoegen aan de set waarmee het bestaande domein al. Als u bijvoorbeeld de **gebeurtenissen** vooraf gemaakte domein voor een app van de gebeurtenis sport, kunt u om toe te voegen entiteiten voor sportteams. Wanneer u begint met [bieden uitingen](luis-how-to-add-example-utterances.md) naar LUIS, omvatten voorwaarden die specifiek voor uw app zijn. LUIS leert herkennen en aangepast, zodat het bestaande domein intenties en entiteiten aan de behoeften van uw app. 

> [!TIP]
> De intenties en entiteiten in een vooraf gedefinieerde domein werken het beste samen. Is het beter om intenties en entiteiten op basis van hetzelfde domein indien mogelijk te combineren.
> De hulpprogramma's vooraf gemaakte domein heeft intents die u voor gebruik in elk domein aanpassen kunt. Bijvoorbeeld, u kunt toevoegen `Utilities.Repeat` aan uw app en train herkennen elke gebruiker acties mogelijk wilt herhalen in uw toepassing. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Het gedrag van de intentie van een vooraf gedefinieerde domein wijzigen

Mogelijk vindt u een vooraf gedefinieerde domein bevat een intent die vergelijkbaar is met een doel dat u in uw LUIS-app wilt hebben, maar u wilt zich anders gedragen. Bijvoorbeeld, de **plaatsen** vooraf gemaakte domein biedt een `MakeReservation` doel voor het maken van een reservering restaurant, maar u wilt dat uw app maken met dit doel. In dat geval kunt u het gedrag van dit doel wijzigen door uitingen leveren aan LUIS over het maken van reserveringen hotel en labelen deze met behulp van de `MakeReservation` intentie, dus klik LUIS kan opnieuw worden getraind om te herkennen de `MakeReservation` intentie in een aanvraag bij een hotel boek .

U vindt een volledige lijst van de vooraf gemaakte domeinen in de [vooraf gemaakte domeinen verwijzing](./luis-reference-prebuilt-domains.md).

## <a name="prebuilt-intents"></a>Vooraf gedefinieerde intents

LUIS biedt vooraf gedefinieerde intents en hun uitingen. Intents kunnen worden toegevoegd zonder het hele domein toe te voegen. Toevoegen van een doel is het proces van het toevoegen van een doel en de uitingen. Zowel de naam van de intentie en de lijst met utterance kan worden aangepast.  

## <a name="prebuilt-entities"></a>Vooraf gemaakte entiteiten

LUIS bevat een set met vooraf gemaakte entiteiten voor het herkennen van algemene typen gegevens, zoals datums, tijden, getallen, metingen en valuta. Ondersteuning voor vooraf gedefinieerde entiteit is afhankelijk van de cultuur van uw LUIS-app. Zie voor een volledige lijst van de vooraf gemaakte entiteiten die LUIS worden ondersteund, met inbegrip van ondersteuning door cultuur, de [verwijzing naar de vooraf gedefinieerde entiteit](./luis-reference-prebuilt-entities.md).

Wanneer een vooraf gedefinieerde entiteit is opgenomen in uw toepassing, wordt de voorspellingen zijn opgenomen in uw gepubliceerde toepassing. Het gedrag van vooraf gemaakte entiteiten is vooraf getrainde en **kan geen** worden gewijzigd. Volg deze stappen om te zien hoe een vooraf gedefinieerde entiteit werkt:

> [!NOTE]
> **Builtin.DateTime** is afgeschaft. Deze is vervangen door [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), waarmee u erkenning van datum en tijd bereiken, evenals verbeterde herkenning van niet-eenduidige datums en tijden.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [vooraf gemaakte entiteiten toevoegen](luis-prebuilt-entities.md) aan uw app.