---
title: Uw toepassingen met Language Understanding (LUIS) plannen
titleSuffix: Azure Cognitive Services
description: Overzicht van relevante app intenties en entiteiten en maak vervolgens uw toepassing plannen in Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 971d5ed47c5778315e2b7c75997c945e8489b1ce
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44052238"
---
# <a name="plan-your-luis-app"></a>Uw LUIS-app plannen

Het is belangrijk voor het plannen van uw app. Identificeer uw domein, met inbegrip van mogelijke intenties en entiteiten die relevant voor uw toepassing zijn.  

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
Nadat u hebt vastgesteld dat de intenties, maakt u 10 of 15 voorbeeld uitingen voor elk doel. Te beginnen met minder dan dit aantal of niet veel uitingen voor elk doel maken. Elke utterance moet verschillen van de vorige utterance. Goede verschillende in de uitingen bevat algemene woordentelling, word keuze term tegenwoordige en leestekens. 

## <a name="identify-your-entities"></a>Uw entiteiten identificeren
In de voorbeeld-uitingen identificeren de entiteiten die u wilt dat opgehaald. Als u wilt een vlucht reserveren, kunt u enkele gegevens, zoals de bestemming, datum, luchtvaartmaatschappij, ticket categorie en klasse reizen. U maakt entiteiten voor deze gegevenstypen en markeert vervolgens de [entiteiten](luis-concept-entity-types.md) in de voorbeeld-uitingen omdat ze belangrijk zijn voor het uitvoeren van een doel. 

Wanneer u welke entiteiten om te gebruiken in uw app bepaalt, houd er rekening mee dat er verschillende soorten entiteiten zijn voor het vastleggen van relaties tussen verschillende typen objecten. [Entiteiten in LUIS](luis-concept-entity-types.md) biedt meer informatie over de verschillende typen.

### <a name="simple-entity"></a>Eenvoudige entiteit
Een enkele entiteit beschrijft een enkele concept.

![eenvoudige entiteit](./media/luis-plan-your-app/simple-entity.png)

Zie [gegevensextractie](luis-concept-data-extraction.md#simple-entity-data) voor meer informatie over het uitpakken van de eenvoudige entiteit van het eindpunt JSON-query-antwoord. Probeer dit [snelstartgids](luis-quickstart-primary-and-secondary-data.md) voor meer informatie over het gebruik van een enkele entiteit.

### <a name="hierarchical-entity"></a>Hiërarchische entiteit
Een hiërarchische entiteit is een speciaal type een **eenvoudige** entiteit, een categorie en de bijbehorende leden in de vorm van bovenliggende / onderliggende relatie te definiëren. De relatie wordt bepaald door de context van binnen de utterance. Onderliggende items van een hiërarchische entiteit zijn ook enkele entiteiten.

![hiërarchische entiteit](./media/luis-plan-your-app/hierarchical-entity.png)

Zie [gegevensextractie](luis-concept-data-extraction.md#hierarchical-entity-data) voor meer informatie over het uitpakken van de hiërarchische entiteit van het eindpunt JSON-query-antwoord. Probeer dit [snelstartgids](luis-quickstart-intent-and-hier-entity.md) voor meer informatie over het gebruik van een hiërarchische entiteit.

### <a name="composite-entity"></a>Samengestelde entiteit
Een samengestelde entiteit is opgebouwd uit andere entiteiten die onderdelen van een geheel vormen. Een samengestelde entiteit bevat diverse Entiteitstypen.

![Samengestelde entiteit](./media/luis-plan-your-app/composite-entity.png)

Zie [gegevensextractie](luis-concept-data-extraction.md#composite-entity-data) voor meer informatie over het uitpakken van de samengestelde entiteit van het eindpunt JSON-query-antwoord. Probeer dit [zelfstudie](luis-tutorial-composite-entity.md) voor meer informatie over het gebruik van een samengestelde entiteit.

### <a name="prebuilt-entity"></a>Vooraf gemaakte entiteiten
LUIS biedt [vooraf gemaakte entiteiten](luis-prebuilt-entities.md) voor veelvoorkomende gegevenstypen, zoals aantal, gegevens, e-mailadres en een URL. U kunt het aantal vooraf gedefinieerde entiteit gebruiken voor het aantal tickets in de volgorde van een ticket.

![Aantal vooraf gedefinieerde entiteit](./media/luis-plan-your-app/number-entity.png)

Zie [gegevensextractie](luis-concept-data-extraction.md#prebuilt-entity-data) voor meer informatie over het uitpakken van de vooraf gedefinieerde entiteit van het eindpunt JSON-query-antwoord. 

### <a name="list-entity"></a>Lijstentiteit 
Een lijst met entiteit is een expliciet opgegeven lijst met waarden. Elke waarde bestaat uit een of meer synoniemen. In een reis-app kunt u een lijst met entiteit om weer te geven luchthaven namen maken.

![lijst met entiteiten](./media/luis-plan-your-app/list-entity.png)

Zie [gegevensextractie](luis-concept-data-extraction.md#list-entity-data) voor meer informatie over de lijst met entiteiten extraheren uit het eindpunt van de JSON-query-antwoord. Probeer dit [snelstartgids](luis-quickstart-intent-and-list-entity.md) voor meer informatie over het gebruik van een entiteit lijst.

### <a name="regular-expression-entity"></a>Reguliere expressie entiteit
Een reguliere expressie entiteit kunt LUIS goed opgemaakte gegevens ophalen uit een utterance op basis van een reguliere expressie.

![Reguliere expressie entiteit](./media/luis-plan-your-app/regex-entity.png)

Zie [gegevensextractie](luis-concept-data-extraction.md#regular-expression-entity-data) voor meer informatie over de reguliere expressie entiteiten extraheren uit het eindpunt van de JSON-query-antwoord. Probeer de [snelstartgids](luis-quickstart-intents-regex-entity.md) voor meer informatie over het gebruik van een entiteit reguliere expressie.

## <a name="next-steps"></a>Volgende stappen
Nadat uw app wordt getraind, gepubliceerd, en haalt de eindpunt-uitingen, wilt implementeren voorspelling verbeteringen met [actief leren](luis-how-to-review-endoint-utt.md), [lijsten woordgroep](luis-concept-feature.md), en [patronen](luis-concept-patterns.md). 


* Zie [Language Understanding Intelligent Services (LUIS)-app maken](luis-get-started-create-app.md) voor een snel overzicht van het maken van een LUIS-app.
