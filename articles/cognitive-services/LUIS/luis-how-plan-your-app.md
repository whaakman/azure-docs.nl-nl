---
title: Uw app plannen-LUIS
titleSuffix: Azure Cognitive Services
description: Overzicht van relevante app intenties en entiteiten en maak vervolgens uw toepassing plannen in Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dc648b30dc1236080be06044f510557ae0ce9476
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638311"
---
# <a name="plan-your-luis-app-with-subject-domain-intents-and-entities"></a>Uw LUIS-app plannen met het onderwerp domein, intenties en entiteiten

Als u uw app wilt plannen, identificeert u het domein van het onderwerp. Dit omvat mogelijk intenties en entiteiten die relevant zijn voor uw toepassing.  

## <a name="identify-your-domain"></a>Identificeren van uw domein

Een LUIS-app is gericht op een domein-specifieke onderwerp.  Bijvoorbeeld, wellicht u een reis-app die reserveren van tickets, vluchten, hotels en verhuur auto's uitvoert. Een andere app bieden mogelijk inhoud met betrekking tot te oefenen, geschiktheid inspanningen bijhouden en doelen instellen. Identificeren van het domein, kunt u zoeken naar woorden of zinnen die belangrijk voor uw domein zijn.

> [!TIP]
> LUIS biedt [vooraf gemaakte domeinen](luis-how-to-use-prebuilt-domains.md) voor veel algemene scenario's.
> Controleer als u een vooraf gedefinieerde domein als uitgangspunt voor uw app gebruiken kunt.

## <a name="identify-your-intents"></a>Uw intenties identificeren

Denk aan de [intents](luis-concept-intent.md) die belangrijk zijn voor de taak van uw toepassing. We gaan het voorbeeld van een reis-app, met functies voor een vlucht reserveren en controleert u het weer op de bestemming van de gebruiker. U kunt de intenties "BookFlight" en "GetWeather" voor deze acties kunt definiëren. In een meer complexe Apps met meer functies, hebt u meer intents en moet u deze zorgvuldig zodat deze niet worden ook specifieke definiëren. Bijvoorbeeld, "BookFlight" en "BookHotel" mogelijk afzonderlijke intents worden, maar 'BookInternationalFlight' en 'BookDomesticFlight' kan worden te veel.

> [!NOTE]
> Het is een best practice u alleen zo veel intents als nodig is voor de functies van uw app uitvoeren. Als u te veel intents definieert, wordt het moeilijker voor LUIS uitingen correct classificeren. Als u te weinig zij je misschien dus algemeen over overlappende.

## <a name="create-example-utterances-for-each-intent"></a>Voorbeeld-uitingen voor elk doel maken

Wanneer u de intenties hebt bepaald, maakt u voor elke intentie 15 tot 30 voor beeld uitingen. Als u met wilt beginnen, hoeft u niet minder dan dit aantal te hebben of te veel uitingen voor elke intentie te maken. Elke utterance moet verschillen van de vorige utterance. Goede verschillende in de uitingen bevat algemene woordentelling, word keuze term tegenwoordige en leestekens. 

Raadpleeg [uitingen](luis-concept-utterance.md) voor meer informatie.

## <a name="identify-your-entities"></a>Uw entiteiten identificeren

In de voorbeeld-uitingen identificeren de entiteiten die u wilt dat opgehaald. Als u een vlucht wilt boeken, hebt u informatie nodig zoals de doel-, datum-, vlieg tickets-, ticket categorie-en reis klasse. Maak entiteiten voor deze gegevens typen en markeer vervolgens de [entiteiten](luis-concept-entity-types.md) in het voor beeld uitingen omdat ze belang rijk zijn om een intentie te bereiken. 

Wanneer u welke entiteiten om te gebruiken in uw app bepaalt, houd er rekening mee dat er verschillende soorten entiteiten zijn voor het vastleggen van relaties tussen verschillende typen objecten. [Entiteiten in LUIS](luis-concept-entity-types.md) biedt meer informatie over de verschillende typen.

## <a name="next-steps"></a>Volgende stappen

Nadat uw app wordt getraind, gepubliceerd, en haalt de eindpunt-uitingen, wilt implementeren voorspelling verbeteringen met [actief leren](luis-how-to-review-endpoint-utterances.md), [lijsten woordgroep](luis-concept-feature.md), en [patronen](luis-concept-patterns.md). 


* Zie [Language Understanding Intelligent Services (LUIS)-app maken](luis-get-started-create-app.md) voor een snel overzicht van het maken van een LUIS-app.
