---
title: 'Zelfstudie: utterances toevoegen aan een LUIS-app met behulp van Node.js | Microsoft Docs'
description: In deze zelfstudie leert u hoe u een LUIS-app aanroept met behulp van Node.js.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: a1e028395ce4b1679a367ce110c2ba120128c501
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264223"
---
# <a name="tutorial-add-utterances-app-using-nodejs"></a>Zelfstudie: utterances aan een app toevoegen met behulp van Node.js 
In deze zelfstudie schrijft u een programma om een utterance aan een intentie toe te voegen met behulp van de creatie-API's in Node.js.

<!-- green checkmark -->
> [!div class="checklist"]
> * Een Visual Studio-consoleproject maken 
> * Methode toevoegen voor het aanroepen van de LUIS-API om een utterance toe te voegen en een app te trainen
> * JSON-bestand toevoegen met voorbeeldutterances voor BookFlight-intentie
> * Console uitvoeren en trainingsstatus weergeven voor utterances

Raadpleeg voor meer informatie de technische documentatie voor de API’s voor het [toevoegen van voorbeeldutterances aan intenties](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [trainen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) en [trainingsstatus](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Voor dit artikel hebt u een gratis [LUIS][LUIS]-account nodig om de LUIS-toepassing te maken.

## <a name="prerequisites"></a>Vereisten

* Meest recente [**Node.js**](https://nodejs.org/en/download/) met NPM.
* NPM-afhankelijkheden voor dit artikel: [**request**](https://www.npmjs.com/package/request), [**request-promise**](https://www.npmjs.com/package/request-promise), [**fs-extra**](https://www.npmjs.com/package/fs-extra).  
* **[Aanbevolen]** [Visual Studio Code](https://code.visualstudio.com/) voor IntelliSense en foutopsporing.
* Uw LUIS-**[creatiesleutel](luis-concept-keys.md#authoring-key)**. U vindt deze sleutel onder Accountinstellingen op de website van [LUIS](luis-reference-regions.md).
* Uw bestaande LUIS-[**toepassings-id**](./luis-get-started-create-app.md). De toepassings-id wordt weergegeven op het toepassingsdashboard. De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `add-utterances.js` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten. 
* De **versie-id** in de toepassing die de utterances ontvangt. De standaard-id is '0.1'
* Maak een nieuw bestand met de naam `add-utterances.js`-project in VSCode.

> [!NOTE] 
> Het volledige `add-utterances.js`-bestand is beschikbaar in de Github-opslagplaats [**LUIS Samples**](https://github.com/Microsoft/LUIS-Samples/tree/master/examples/add-utterances/nodejs).


## <a name="write-the-nodejs-code"></a>De Node.js-code schrijven

Voeg de NPM-afhankelijkheden toe aan het bestand.

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=16-19 "NPM Dependencies")]

Voeg de LUIS-constanten toe aan het bestand. Kopieer de volgende code en wijzig deze met uw creatiecode, toepassings-id en versie-id.

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=22-29 "LUIS key and IDs")]

Voeg de naam en locatie toe van het uploadbestand dat uw utterances bevat. 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=31-33 "Add upload file")]

Voeg de variabelen toe die de opdrachtregelwaarden bevatten.

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=35-49 "Add upload file")]


Voeg de functie `sendUtteranceToApi` toe om HTTP-aanroepen te verzenden en ontvangen. 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=135-151 "Send the HTTP request")]

Voeg het JSON-configuratie-object toe dat wordt gebruikt door de functie `addUtterance` .

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=52-59 "Add configuration information for adding utterance")]

Voeg de functie `addUtterance` toe voor het beheren van de API-aanvraag en -antwoord dat wordt gebruikt door `SendUtteranceToApp`.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=62-92 "Add configuration information for adding utterance")]

Voeg het JSON-configuratie-object toe dat wordt gebruikt door de functie `train` .

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=94-101 "Add configuration information for training LUIS")]

Voeg de functie `train` toe om het trainingsproces te starten. 

   [!code-javascript[Train the application](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=103-133 "Train the application")]

Voeg de code toe waarmee wordt gekozen welke actie moet worden ondernomen (utterance toevoegen of trainen) op basis van de opdrachtregelvariabelen.

   [!code-javascript[Train the application](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=153-184 "Train the application")]

## <a name="specify-utterances-to-add"></a>Toe te voegen utterances opgeven
Maak en bewerk het bestand `utterances.json` om de **matrix met utterances** op te geven die u aan de LUIS-app wilt toevoegen. De intentie en entiteiten **moeten** al aanwezig zijn in de LUIS-app.

> [!NOTE]
> De LUIS-toepassing met de intenties en entiteiten die in het `utterances.json`-bestand worden gebruikt, moet bestaan ​​voordat de code in `add-utterances.js` wordt uitgevoerd. De intenties en entiteiten worden niet gemaakt met de code in dit artikel. Deze code voegt alleen de utterances toe voor bestaande intenties en entiteiten.

Het veld `text` bevat de tekst van de utterance. Het veld `intentName` moet overeenkomen met de naam van een intentie in de LUIS-app. Het veld `entityLabels` is vereist. Als u geen entiteiten wilt labelen, geeft u een lege lijst op, zoals in het volgende voorbeeld.

Als de lijst met entityLabels niet leeg is, moeten `startCharIndex` en `endCharIndex` de entiteit markeren waarnaar wordt verwezen in het veld `entityName`. Beide indices worden vanaf 0 geteld, wat betekent dat 6 in het bovenste voorbeeld verwijst naar de 'S' van Seattle en niet naar de spatie vóór de hoofdletter S.

```json
[
    {
        "text": "go to Seattle",
        "intentName": "BookFlight",
        "entityLabels": [
            {
                "entityName": "Location::LocationTo",
                "startCharIndex": 6,
                "endCharIndex": 12
            }
        ]
    },
    {
        "text": "book a flight",
        "intentName": "BookFlight",
        "entityLabels": []
    }
]
```

## <a name="add-an-utterance-from-the-command-line"></a>Toevoegen van een utterance vanaf de opdrachtregel

Voer de toepassing uit vanaf een opdrachtregel met Node.js.

Het aanroepen van add-utterance zonder argumenten voegt een utterance aan de app toe, zonder deze te trainen.
````
> node add-utterances.js
````

In dit voorbeeld wordt een bestand gemaakt met de `results.json` die de resultaten bevat van het aanroepen van de API voor het toevoegen van utterances. Het veld `response` heeft deze notatie voor utterances die zijn toegevoegd. `hasError` is onwaar, wat aangeeft dat de utterance is toegevoegd.  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

## <a name="add-an-utterance-and-train-from-the-command-line"></a>Een utterance toevoegen en trainen vanaf de opdrachtregel
Roep add-utterance aan met het argument `-train` om een aanvraag om te trainen te verzenden, en vraag vervolgens om een ​​trainingsstatus. De status wordt onmiddellijk nadat de training begint in de wachtrij gezet. Statusdetails worden naar een bestand geschreven.

````
> node add-utterances.js -train
````

> [!NOTE]
> Dubbele utterances worden niet opnieuw toegevoegd, maar veroorzaken geen fout. De `response` bevat de id van de oorspronkelijke utterance.

Wanneer u het voorbeeld aanroept met het argument `-train`, wordt er een `training-results.json`-bestand gemaakt dat aangeeft dat de aanvraag voor het trainen van de LUIS-app met succes in de wachtrij is geplaatst. 

Het volgende toont het resultaat van een succesvolle trainingsaanvraag:
```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Nadat de trainingsaanvraag in de wachtrij is geplaatst, kan het even duren voordat de training is voltooid.

## <a name="get-training-status-from-the-command-line"></a>Trainingsstatus ophalen vanaf de opdrachtregel
Roep het voorbeeld aan met het argument `-status` om de trainingsstatus te controleren en statusdetails naar een bestand te schrijven.

````
> node add-utterances.js -status
````

## <a name="clean-up-resources"></a>Resources opschonen
Wanneer u klaar bent met de zelfstudie, verwijdert Visual Studio en de console-toepassing als u deze niet meer nodig hebt. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"] 
> [Programmatisch een LUIS-app bouwen](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
