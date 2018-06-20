---
title: Plannen van uw toepassingen LUIS | Microsoft Docs
description: Overzicht van relevante app intents en entiteiten en maak vervolgens uw toepassing plannen in Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: DeniseMak
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 7aec5d5b90ac7145ce9f337ec74c590b4b88c6b1
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266359"
---
# <a name="plan-your-luis-app"></a>Uw app LUIS plannen

Het is belangrijk voor het plannen van uw app voordat u deze in LUIS maken. Bereid een overzicht of het schema van de mogelijke intents en de entiteiten die relevant voor het onderwerp domeinspecifieke van uw toepassing zijn.  

## <a name="identify-your-domain"></a>Uw domein identificeren
Een app LUIS wordt gecentreerd rond een onderwerp domeinspecifieke vereist.  Bijvoorbeeld wellicht hebt u een app reizen die reserveren van tickets, vlucht hotels en verhuur auto's uitvoert. Een andere app kan de inhoud die betrekking hebben op uitoefenen geschiktheid inspanningen en bij te houden doelen instellen bieden. 

> [!TIP]
> LUIS biedt [vooraf gedefinieerde domeinen](luis-how-to-use-prebuilt-domains.md) voor algemene scenario's.
> Controleer als u een vooraf gedefinieerde domein als een beginpunt voor uw app gebruiken kunt.

## <a name="identify-your-intents"></a>Uw intents identificeren
Denk aan de [intents](luis-concept-intent.md) die belangrijk zijn voor uw toepassing taak. Laten we het voorbeeld van een app reizen met functies een vlucht boek en controleer het weer op de bestemming van de gebruiker. U kunt de intents 'BookFlight' en 'GetWeather' voor deze acties definiëren. U hebt meer intents en moet u deze zorgvuldig dat ze niet te specifiek definiëren in een complexere app met meer functies. Bijvoorbeeld 'BookFlight' en 'BookHotel' moet mogelijk worden afzonderlijke intents, maar 'BookInternationalFlight' en 'BookDomesticFlight' mogelijk te veel.

> [!NOTE]
> Het is een best practice alleen gebruiken als veel intents als nodig is voor het uitvoeren van de functies van uw app. Als u te veel intents definieert, wordt het moeilijker voor LUIS utterances correct te classificeren. Als u te definiëren, enkele moeten kunnen worden dus algemeen garantie overlappen.


## <a name="identify-your-entities"></a>Identificeren van de entiteiten
Als u wilt een vlucht boek, kunt u gegevens zoals de bestemming, datum, luchtvaartmaatschappij ticket categorie nodig en reizen klasse. U kunt deze als toevoegen [entiteiten](luis-concept-entity-types.md) omdat ze belangrijk zijn voor het uitvoeren van de opzet. 

Houd er rekening mee dat er verschillende soorten entiteiten voor het vastleggen van de relaties tussen typen objecten tijdens het bepalen van welke entiteiten te gebruiken in uw app. [Entiteiten in LUIS](luis-concept-entity-types.md) biedt meer details over de verschillende typen.

### <a name="simple-entity"></a>enkele entiteit
Een enkele entiteit beschrijft een één concept.

![enkele entiteit](./media/luis-plan-your-app/simple-entity.png)

Zie [ophalen van gegevens](luis-concept-data-extraction.md#simple-entity-data) voor meer informatie over het uitpakken van de eenvoudige entiteit van het eindpunt JSON-query-antwoord. Probeer de eenvoudige entiteit [Quick Start](luis-quickstart-primary-and-secondary-data.md) voor meer informatie over het gebruik van een enkele entiteit.

### <a name="hierarchical-entity"></a>hiërarchische entiteit
Een hiërarchische entiteit is een speciaal type van een **eenvoudige** entiteit; voor het definiëren van een categorie en de bijbehorende leden in de vorm van bovenliggende / onderliggende relatie.

![hiërarchische entiteit](./media/luis-plan-your-app/hierarchical-entity.png)

Zie [ophalen van gegevens](luis-concept-data-extraction.md#hierarchical-entity-data) voor meer informatie over het uitpakken van de hiërarchische entiteit van het eindpunt JSON-query-antwoord. Probeer de hiërarchische entiteit [Quick Start](luis-quickstart-intent-and-hier-entity.md) voor meer informatie over het gebruik van een hiërarchische entiteit.

### <a name="composite-entity"></a>samengestelde entiteit
Een samengestelde entiteit bestaat uit andere partijen die onderdelen van een volledig vormen. 

![samengestelde entiteit](./media/luis-plan-your-app/composite-entity.png)

Zie [ophalen van gegevens](luis-concept-data-extraction.md#composite-entity-data) voor meer informatie over het uitpakken van de samengestelde entiteit van het eindpunt JSON-query-antwoord. Probeer de samengestelde entiteit [zelfstudie](luis-tutorial-composite-entity.md) voor meer informatie over het gebruik van een samengestelde entiteit.

### <a name="prebuilt-entity"></a>Vooraf gedefinieerde entiteit
LUIS biedt [vooraf gemaakte entiteiten](Pre-builtEntities.md) voor algemene typen als `Number`, die u kunt gebruiken voor het aantal tickets in een volgorde ticket.

![Aantal vooraf gedefinieerde entiteit](./media/luis-plan-your-app/number-entity.png)

Zie [ophalen van gegevens](luis-concept-data-extraction.md#prebuilt-entity-data) voor meer informatie over het uitpakken van de reguliere expressie entiteiten van het eindpunt JSON-query-antwoord. 

### <a name="list-entity"></a>lijst entiteit 
Een lijst entiteit is een expliciet opgegeven lijst met waarden. Elke waarde bestaat uit een of meer synoniemen. In een app reizen kunt u kiezen om een lijst entiteit te vertegenwoordigen luchthaven namen maken.

![lijst entiteit](./media/luis-plan-your-app/list-entity.png)

Zie [ophalen van gegevens](luis-concept-data-extraction.md#list-entity-data) voor meer informatie over het uitpakken van de lijst met entiteiten van het eindpunt JSON-query-antwoord. Probeer de [Quick Start](luis-quickstart-intent-and-list-entity.md) voor meer informatie over het gebruik van een entiteit van de lijst.

### <a name="regular-expression-entity"></a>Reguliere expressie entiteit
Een entiteit van de reguliere expressie kan LUIS gegevens te extraheren uit een utterance op basis van een regex-expressie.

![Reguliere expressie entiteit](./media/luis-plan-your-app/regex-entity.png)

Zie [ophalen van gegevens](luis-concept-data-extraction.md#regular-expression-entity-data) voor meer informatie over het uitpakken van de reguliere expressie entiteiten van het eindpunt JSON-query-antwoord. Probeer de [Quick Start](luis-quickstart-intents-regex-entity.md) voor meer informatie over het gebruik van een entiteit van de reguliere expressie.

## <a name="after-getting-endpoint-utterances"></a>Nadat de eindpunt utterances ophalen
Nadat u uw app opgehaald eindpunt utterances, wilt implementeren de verbeteringen van de voorspelling van [active learning](label-suggested-utterances.md), [een lijst met woorden](luis-concept-feature.md), en [patronen](luis-concept-patterns.md). 

### <a name="patternany-entity"></a>Pattern.any entiteit
Patterns.any is een tijdelijke aanduiding voor de variabele lengte uitsluitend gebruikt in een [patroon van](luis-concept-patterns.md) sjabloon utterance markeren waar de entiteit begint en eindigt. Sjabloon utterances voldoen aan [juiste syntaxis](luis-concept-patterns.md#pattern-syntax) om entiteiten en negeerbare tekst te identificeren.


## <a name="next-steps"></a>Volgende stappen
* Zie [maken van uw eerste app in de taal Understanding Intelligent Services (LUIS)] [ luis-get-started-create-app] voor een snel overzicht van hoe u een app LUIS maakt.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app