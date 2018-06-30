---
title: Een LUIS App programmatisch met behulp van Node.js | Microsoft Docs
titleSuffix: Azure
description: Informatie over het bouwen van een app LUIS programmatisch van bestaande gegevens in CSV-indeling met behulp van de API LUIS ontwerpen.
services: cognitive-services
author: DeniseMak
manager: rstand
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/21/2018
ms.author: v-geberr
ms.openlocfilehash: e97dc184266bc9518ee5f909891bd97f7c71804b
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37113049"
---
# <a name="build-a-luis-app-programmatically-using-nodejs"></a>Een LUIS App programmatisch met behulp van Node.js

LUIS biedt een programmatische API die alles die werkt de [LUIS] [ LUIS] website komt. Dit bespaart tijd wanneer u bestaande gegevens en het normaal zou zijn sneller maken van een app LUIS programmatisch dan door de gegevens handmatig invoeren. 

## <a name="prerequisites"></a>Vereisten

* Meld u aan bij de [LUIS] [ LUIS] website en zoek uw [authoring sleutel](luis-concept-keys.md#authoring-key) in Accountinstellingen. U gebruikt deze sleutel om aan te roepen de ontwerp-API's.
* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
* Deze zelfstudie begint met een CSV voor een hypothetische bedrijf logboekbestanden van aanvragen van gebruikers. Download deze [hier](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/IoT.csv).
* Installeer de meest recente Node.js met NPM. Downloaden van [hier](https://nodejs.org/en/download/).
* **(Aanbevolen)**  Visual Studio Code voor IntelliSense en foutopsporing, downloaden via [hier](https://code.visualstudio.com/) gratis.

## <a name="map-preexisting-data-to-intents-and-entities"></a>Bestaande kaartgegevens intents en entiteiten
Zelfs als er een systeem dat niet is gemaakt met LUIS in gedachten, als deze tekstuele gegevens bevat die is toegewezen aan verschillende dingen gebruikers wilt doen, kunt u mogelijk met een toewijzing van de bestaande categorieën van gebruikersinvoer in intents in LUIS komen. Als u belangrijke woorden of zinnen in wat de gebruikers gezegd identificeren kunt, kunnen deze woorden toewijzen aan entiteiten.

Open het `IoT.csv`-bestand. Het bevat een logboek van gebruikersquery's naar een hypothetische thuis Automatiseringsservice, met inbegrip van hoe ze zijn gecategoriseerd, wat de gebruiker gezegd en sommige kolommen met nuttige informatie opgehaald buiten deze. 

![CSV-bestand](./media/luis-tutorial-node-import-utterances-csv/csv.png) 

U ziet dat de **RequestType** kolom kan worden intents, en de **aanvragen** kolom ziet u een voorbeeld utterance. De andere velden kunnen entiteiten zijn als ze in de utterance optreden. Omdat er intents en entiteiten voorbeeld utterances, hebt u de vereisten voor een eenvoudig voorbeeld-app.

## <a name="steps-to-generate-a-luis-app-from-non-luis-data"></a>Stappen voor het genereren van een app LUIS van niet-LUIS gegevens
Voor het genereren van een nieuwe LUIS-app uit het bronbestand, eerst u de gegevens van het CSV-bestand parseren en deze gegevens converteren naar een indeling die u kunt uploaden naar LUIS met behulp van de API ontwerpen. Van de geparseerde gegevens kunt u informatie verzamelen over wat intents en entiteiten er zijn. Vervolgens u API-aanroepen voor het maken van de app en voeg intents en entiteiten die zijn verzameld van de geparseerde gegevens. Nadat u de app LUIS hebt gemaakt, kunt u de voorbeeld-utterances toevoegen van de geparseerde gegevens. Hier ziet u deze overdracht in het laatste deel van de volgende code. Kopieer of [downloaden](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/index.js) deze code en opgeslagen in `index.js`.

   [!code-javascript[Node.js code for calling the steps to build a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/index.js)]


## <a name="parse-the-csv"></a>Parseren van de CSV

De kolom posten met de utterances CSV hebben om te worden geparseerd naar een JSON-indeling die LUIS kunt begrijpen. Deze JSON-indeling moet bevatten een `intentName` veld dat de doelstelling van de utterance identificeert. Ook moet het bevatten een `entityLabels` veld kan niet leeg zijn als er geen entiteiten in de utterance zijn. 

De vermelding voor 'De lichten inschakelen' is toegewezen aan deze JSON:

```json
        {
            "text": "Turn on the lights",
            "intentName": "TurnOn",
            "entityLabels": [
                {
                    "entityName": "Operation",
                    "startCharIndex": 5,
                    "endCharIndex": 6
                },
                {
                    "entityName": "Device",
                    "startCharIndex": 12,
                    "endCharIndex": 17
                }
            ]
        }
```

In dit voorbeeld wordt de `intentName` afkomstig is van de aanvraag van de gebruiker onder de **aanvraag** kolomkop in het CSV-bestand en de `entityName` afkomstig is van de andere kolommen met belangrijke informatie. Bijvoorbeeld, als er een vermelding voor **bewerking** of **apparaat**, en dat tekenreeks vindt ook plaats in de huidige aanvraag en deze kan worden aangemerkt als een entiteit. De volgende code toont dit proces te parseren. U kunt kopiëren of [downloaden](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_parse.js) deze en opslaan voor `_parse.js`.

   [!code-javascript[Node.js code for parsing a CSV file to extract intents, entities, and labeled utterances](~/samples-luis/examples/build-app-programmatically-csv/_parse.js)]



## <a name="create-the-luis-app"></a>De app LUIS maken
Toevoegen aan een app LUIS zodra de gegevens zijn geparseerd naar JSON. De volgende code maakt de LUIS-app. Kopieer of [downloaden](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_create.js) , en op te slaan in `_create.js`.

   [!code-javascript[Node.js code for creating a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/_create.js)]


## <a name="add-intents"></a>Intents toevoegen
Zodra u een app hebt, moet u intents aan. De volgende code maakt de LUIS-app. Kopieer of [downloaden](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_intents.js) , en op te slaan in `_intents.js`.

   [!code-javascript[Node.js code for creating a series of intents](~/samples-luis/examples/build-app-programmatically-csv/_intents.js)]


## <a name="add-entities"></a>Entiteiten toevoegen
De volgende code wordt de entiteiten in de app LUIS toegevoegd. Kopieer of [downloaden](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_entities.js) , en op te slaan in `_entities.js`.

   [!code-javascript[Node.js code for creating entities](~/samples-luis/examples/build-app-programmatically-csv/_entities.js)]
   


## <a name="add-utterances"></a>Utterances toevoegen
Zodra de entiteiten en intents zijn gedefinieerd in de app LUIS, kunt u de utterances toevoegen. De volgende code gebruikt de [Utterances_AddBatch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) API, zodat u kunt maximaal 100 utterances tegelijk toevoegen.  Kopieer of [downloaden](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/build-app-programmatically-csv/_upload.js) , en op te slaan in `_upload.js`.

   [!code-javascript[Node.js code for adding utterances](~/samples-luis/examples/build-app-programmatically-csv/_upload.js)]


## <a name="run-the-code"></a>De code uitvoeren


### <a name="install-nodejs-dependencies"></a>Installeer Node.js-afhankelijkheden
De Node.js-afhankelijkheden van NPM in de terminal/opdrachtregel installeren.

````
> npm install
````

### <a name="change-configuration-settings"></a>Configuratie-instellingen wijzigen
Om deze toepassing gebruiken, moet u de waarden in het bestand index.js wijzigen voor de eindpuntsleutel van uw eigen en geef de naam die u wilt dat de app te hebben. U kunt ook de cultuur van de app instellen of wijzigen van het versienummer.

Open het bestand index.js en deze waarden boven aan het bestand te wijzigen.


````JavaScript
// Change these values
const LUIS_programmaticKey = "YOUR_PROGRAMMATIC_KEY";
const LUIS_appName = "Sample App";
const LUIS_appCulture = "en-us"; 
const LUIS_versionId = "0.1";
````
### <a name="run-the-script"></a>Het script uitvoeren
Voer het script vanaf een terminal opdrachtregel met behulp van Node.js.

````
> node index.js
````
of
````
> npm start
````

### <a name="application-progress"></a>Voortgang van de toepassing
Terwijl de toepassing wordt uitgevoerd, wordt de opdrachtregel voortgang. De opdrachtregel-uitvoer bevat de indeling van de antwoorden van LUIS.

````
> node index.js
intents: ["TurnOn","TurnOff","Dim","Other"]
entities: ["Operation","Device","Room"]
parse done
JSON file should contain utterances. Next step is to create an app with the intents and entities it found.
Called createApp, created app with ID 314b306c-0033-4e09-92ab-94fe5ed158a2
Called addIntents for intent named TurnOn.
Called addIntents for intent named TurnOff.
Called addIntents for intent named Dim.
Called addIntents for intent named Other.
Results of all calls to addIntent = [{"response":"e7eaf224-8c61-44ed-a6b0-2ab4dc56f1d0"},{"response":"a8a17efd-f01c-488d-ad44-a31a818cf7d7"},{"response":"bc7c32fc-14a0-4b72-bad4-d345d807f965"},{"response":"727a8d73-cd3b-4096-bc8d-d7cfba12eb44"}]
called addEntity for entity named Operation.
called addEntity for entity named Device.
called addEntity for entity named Room.
Results of all calls to addEntity= [{"response":"6a7e914f-911d-4c6c-a5bc-377afdce4390"},{"response":"56c35237-593d-47f6-9d01-2912fa488760"},{"response":"f1dd440c-2ce3-4a20-a817-a57273f169f3"}]
retrying add examples...

Results of add utterances = [{"response":[{"value":{"UtteranceText":"turn on the lights","ExampleId":-67649},"hasError":false},{"value":{"UtteranceText":"turn the heat on","ExampleId":-69067},"hasError":false},{"value":{"UtteranceText":"switch on the kitchen fan","ExampleId":-3395901},"hasError":false},{"value":{"UtteranceText":"turn off bedroom lights","ExampleId":-85402},"hasError":false},{"value":{"UtteranceText":"turn off air conditioning","ExampleId":-8991572},"hasError":false},{"value":{"UtteranceText":"kill the lights","ExampleId":-70124},"hasError":false},{"value":{"UtteranceText":"dim the lights","ExampleId":-174358},"hasError":false},{"value":{"UtteranceText":"hi how are you","ExampleId":-143722},"hasError":false},{"value":{"UtteranceText":"answer the phone","ExampleId":-69939},"hasError":false},{"value":{"UtteranceText":"are you there","ExampleId":-149588},"hasError":false},{"value":{"UtteranceText":"help","ExampleId":-81949},"hasError":false},{"value":{"UtteranceText":"testing the circuit","ExampleId":-11548708},"hasError":false}]}]
upload done
````




## <a name="open-the-luis-app"></a>Open de app LUIS
Nadat het script is voltooid, kunt u zich aanmelden bij [LUIS] [ LUIS] en de LUIS-App die u hebt gemaakt onder **mijn Apps**. U moet kunnen zien van de utterances u toegevoegd onder de **inschakelen**, **uitschakelen**, en **geen** intents.

![De bedoeling inschakelen](./media/luis-tutorial-node-import-utterances-csv/imported-utterances-661.png)


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Testen en training van uw app in LUIS website](interactive-test.md)

## <a name="additional-resources"></a>Aanvullende resources

Deze voorbeeldtoepassing gebruikt de volgende LUIS APIs:
- [app maken](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)
- [intents toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0c)
- [entiteiten toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0e) 
- [utterances toevoegen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions

