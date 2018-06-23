---
title: Label entiteiten automatisch met de entiteit van een lijst met Nodejs | Microsoft Docs
description: Informatie over het toevoegen van een entiteit van de lijst om te helpen LUIS label variaties van een woord of woordgroep.
services: cognitive-services
author: v-geberr
titleSuffix: Azure
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/21/2018
ms.author: v-geberr
ms.openlocfilehash: e8558ecf4a64dbccef6e6367c1447bdcdb005126
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344539"
---
# <a name="use-a-list-entity-to-increase-entity-detection"></a>Een entiteit lijst gebruiken om te verhogen van de detectie van de entiteit 
Deze zelfstudie laat zien hoe u een [entiteit lijst](luis-concept-entity-types.md) entiteit detectie verhogen. Lijst entiteiten hoeft niet te worden gelabeld als ze een exacte overeenkomst met de voorwaarden zijn.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
* De entiteit van een lijst maken 
* Genormaliseerde waarden en synoniemen toevoegen
* Verbeterde entiteits-id valideren

## <a name="prerequisites"></a>Vereisten

> [!div class="checklist"]
> * Meest recente [Node.js](https://nodejs.org)
> * [HomeAutomation LUIS app](luis-get-started-create-app.md). Als u niet de app start Automation is gemaakt hebt, een nieuwe app maken en toevoegen van het vooraf gedefinieerde domein **HomeAutomation**. Trainen en publiceer de app. 
> * [AuthoringKey](luis-concept-keys.md#authoring-key), [EndpointKey](luis-concept-keys.md#endpoint-key) (als het uitvoeren van query's is vaak), app-ID, versie-ID en [regio](luis-reference-regions.md) voor de app LUIS.

> [!Tip]
> Als u nog geen een abonnement, kunt u registreren voor een [gratis account](https://azure.microsoft.com/free/).

Alle code in deze zelfstudie is beschikbaar op de [LUIS-Samples github-opslagplaats](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-list-entity). 

## <a name="use-homeautomation-app"></a>HomeAutomation app gebruiken
De app HomeAutomation geeft u de controle van apparaten zoals lichten, entertainmentsystemen en omgeving zoals verwarming en koeling bepaalt. Deze systemen diverse verschillende namen hebben die fabrikant namen, bijnaam acroniemen en taalgebruik kunnen bevatten. 

Een systeem met veel namen in verschillende culturen en demografische gegevens is thermostaat. Een thermostaat kunt beheren, zowel koeling en verwarming van een huis of een gebouw.

In het ideale geval de volgende utterances moeten worden omgezet naar de vooraf gedefinieerde entiteit **HomeAutomation.Device**:

|#|utterance|entiteit geïdentificeerd|score|
|--|--|--|--|
|1|de ac inschakelen|HomeAutomation.Device - 'ac'|0.8748562|
|2|inschakelen van de hitte|HomeAutomation.Device - 'hitte'|0.784990132|
|3|het kouder maken|||

De eerste twee utterances worden toegewezen aan verschillende apparaten. De derde utterance 'maken het kouder', niet is toegewezen aan een apparaat maar in plaats daarvan een resultaat-aanvragen. LUIS weet niet dat de term 'kouder' betekent dat thermostaat het opgegeven apparaat. In het ideale geval moet LUIS los alle deze utterances op hetzelfde apparaat. 

## <a name="use-a-list-entity"></a>Een entiteit lijst gebruiken
De entiteit HomeAutomation.Device is ideaal voor een klein aantal apparaten of met enkele variaties van de namen. Voor een kantoorgebouw of bereik, worden de namen van buiten het nut van de entiteit HomeAutomation.Device groeien. 

Een **entiteit lijst** is een goede keuze voor dit scenario omdat de set voorwaarden voor een apparaat in een gebouw of bereik een bekende set is, zelfs als deze een grote set. Met behulp van een entiteit lijst LUIS een mogelijke waarde in de verzameling van thermostaat ontvangen, en zet deze naar beneden alleen het één apparaat 'thermostaat'. 

Deze zelfstudie gaat maken van een Entiteitslijst met thermostaat. De alternatieve namen voor een thermostaat in deze zelfstudie zijn: 

|alternatieve namen voor thermostaat|
|--|
| AC |
| wisselstroomadapter|
| -c|
|verwarmingssysteem|
|hot|
|hotter|
|koude|
|kouder|

Als LUIS moet een nieuw alternatief vaak bepalen en vervolgens een [woordgroepenlijst](luis-concept-feature.md#how-to-use-phrase-lists) is een beter antwoord.

## <a name="create-a-list-entity"></a>De entiteit van een lijst maken
Maak een Node.js-bestand en kopieer de volgende code in de App. Wijzig de waarden authoringKey, appId, versionId en regio.

   [!code-javascript[Create DevicesList List Entity](~/samples-luis/documentation-samples/tutorial-list-entity/add-entity-list.js "Create DevicesList List Entity")]

Gebruik de volgende opdracht voor het installeren van de NPM-afhankelijkheden en voert u de code voor het maken van de entiteit lijst:

```Javascript
npm install && node add-entity-list.js
```

De uitvoer van de uitvoering is de ID van de entiteit lijst:

```Javascript
026e92b3-4834-484f-8608-6114a83b03a6
```
## <a name="train-the-model"></a>Het model trainen
Training LUIS in volgorde van de nieuwe lijst invloed kunnen zijn op de queryresultaten. Training is een tweedelige proces van training, klikt u vervolgens de status controleren als de training wordt uitgevoerd. Een app met veel modellen kan even duren om te trainen. De volgende code traint van de app vervolgens wordt gewacht totdat de training voltooid is. De code een strategie voor de wachttijd en opnieuw gebruikt om te voorkomen dat de 429 ' te veel aanvragen ' fout. 

Maak een Node.js-bestand en kopieer de volgende code in de App. Wijzig de waarden authoringKey, appId, versionId en regio.

   [!code-javascript[Train LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/train.js "Train LUIS")]

Gebruik de volgende opdracht in de code voor het trainen van de app uit te voeren:

```Javascript
node train.js
```

De uitvoer van de uitvoering is de status van elke herhaling van de training van de LUIS-modellen. De volgende uitvoering vereist slechts één selectievakje van training:

```Javascript
1 trained = true
[ { modelId: '2c549f95-867a-4189-9c35-44b95c78b70f',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '5530e900-571d-40ec-9c78-63e66b50c7d4',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '519faa39-ae1a-4d98-965c-abff6f743fe6',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9671a485-36a9-46d5-aacd-b16d05115415',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9ef7d891-54ab-48bf-8112-c34dcd75d5e2',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '8e16a660-8781-4abf-bf3d-f296ebe1bf2d',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } } ]

```
## <a name="publish-the-model"></a>Het model publiceren
Publiceren zodat de entiteit lijst beschikbaar via het eindpunt is.

Maak een Node.js-bestand en kopieer de volgende code in de App. Wijzig de waarden endpointKey, appId en regio. U kunt uw authoringKey gebruiken als u niet van plan bent om aan te roepen van dit bestand boven de quotumlimiet.

   [!code-javascript[Publish LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/publish.js "Publish LUIS")]

Gebruik de volgende opdracht in de code om op te vragen van de app uit te voeren:

```Javascript
node publish.js
```

De volgende uitvoer bevat de eindpunt-url voor query's. Echte JSON-resultaten, zou de echte appID bevatten. 

```JSON
{ 
  versionId: null,
  isStaging: false,
  endpointUrl: 'https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>',
  region: null,
  assignedEndpointKey: null,
  endpointRegion: 'westus',
  publishedDateTime: '2018-01-29T22:17:38Z' }
}
```

## <a name="query-the-app"></a>Query uitvoeren op de app. 
Query uitvoeren op de app van het eindpunt om te bewijzen dat de entiteit lijst helpt LUIS apparaattype bepalen.

Maak een Node.js-bestand en kopieer de volgende code in de App. Wijzig de waarden endpointKey, appId en regio. U kunt uw authoringKey gebruiken als u niet van plan bent om aan te roepen van dit bestand boven de quotumlimiet.

   [!code-javascript[Query LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/query.js "Query LUIS")]

Gebruik de volgende opdracht voert u de code en opvragen van de app:

```Javascript
node train.js
```

De uitvoer is de queryresultaten. Omdat de code toegevoegd de **uitgebreide** naam/waarde-paar met de queryreeks, wordt de uitvoer bevat alle intents en hun scores:

```JSON
{
  "query": "turn up the heat",
  "topScoringIntent": {
    "intent": "HomeAutomation.TurnOn",
    "score": 0.139018849
  },
  "intents": [
    {
      "intent": "HomeAutomation.TurnOn",
      "score": 0.139018849
    },
    {
      "intent": "None",
      "score": 0.120624863
    },
    {
      "intent": "HomeAutomation.TurnOff",
      "score": 0.06746891
    }
  ],
  "entities": [
    {
      "entity": "heat",
      "type": "HomeAutomation.Device",
      "startIndex": 12,
      "endIndex": 15,
      "score": 0.784990132
    },
    {
      "entity": "heat",
      "type": "DevicesList",
      "startIndex": 12,
      "endIndex": 15,
      "resolution": {
        "values": [
          "Thermostat"
        ]
      }
    }
  ]
}
```

Het apparaat van **thermostaat** wordt geïdentificeerd met een query resultaat gerichte van 'beurt van de hitte'. Omdat de oorspronkelijke HomeAutomation.Device entiteit nog steeds in de app is, kunt u ook de resultaten bekijken. 

Probeer de twee utterances om te zien dat ze ook als een thermostaat worden geretourneerd. 

|#|utterance|entiteit|type|waarde|
|--|--|--|--|--|
|1|de ac inschakelen| AC | DevicesList | Thermostaat|
|2|inschakelen van de hitte|hitte| DevicesList |Thermostaat|
|3|het kouder maken|kouder|DevicesList|Thermostaat|

## <a name="next-steps"></a>Volgende stappen

U kunt een andere lijst entiteit om uit te breiden apparaat locaties ruimten, grond verdieping of gebouwen maken. 
