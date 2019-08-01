---
title: 'Quickstart: Node.js gebruiken om de Text Analytics-API aan te roepen'
titleSuffix: Azure Cognitive Services
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Text Analytics-API.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: shthowse
ms.openlocfilehash: 9b8a713d58d5753e04de050e0bc961b5e8388123
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697487"
---
# <a name="quickstart-using-nodejs-to-call-the-text-analytics-cognitive-service"></a>Quickstart: Node.js gebruiken om de Text Analytics Cognitive Service aan te roepen
<a name="HOLTop"></a>

Gebruik deze Quick Start om de taal te analyseren met de Text Analytics SDK voor node. js. Hoewel de [Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711) rest API compatibel is met de meeste programmeer talen, biedt de SDK een gemakkelijke manier om de service te integreren in uw toepassingen. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js).

Raadpleeg de [API-definities](//go.microsoft.com/fwlink/?LinkID=759346) voor technische documentatie voor de API's.

## <a name="prerequisites"></a>Vereisten

* [Node.js](https://nodejs.org/)
* De Text Analytics [SDK voor node. js](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) kunt u de SDK installeren met:

    `npm install azure-cognitiveservices-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

U moet ook de [eindpunt- en toegangssleutel](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) hebben die voor u is gegenereerd tijden de registratie.

## <a name="create-a-nodejs-application-and-install-the-sdk"></a>Een node. js-toepassing maken en de SDK installeren

Maak na de installatie van node. js een knooppunt project. Maak een nieuwe map voor uw app en navigeer naar de map.

```mkdir myapp && cd myapp```

Voer ```npm init``` uit om een knooppunt toepassing te maken met een package. JSON-bestand. Installeer de `ms-rest-azure` en `azure-cognitiveservices-textanalytics` NPM-pakketten:

```npm install azure-cognitiveservices-textanalytics ms-rest-azure```

Het bestand Package. json van uw app wordt bijgewerkt met de afhankelijkheden.

## <a name="authenticate-your-credentials"></a>Uw referenties verifiëren

Maak een nieuw bestand `index.js` in de hoofdmap van het project en importeer de geïnstalleerde bibliotheken

```javascript
const CognitiveServicesCredentials = require("ms-rest-azure").CognitiveServicesCredentials;
const TextAnalyticsAPIClient = require("azure-cognitiveservices-textanalytics");
```

Maak een variabele voor uw Text Analytics-abonnements sleutel.

```javascript
let credentials = new CognitiveServicesCredentials(
    "enter-your-key-here"
);
```

> [!Tip]
> Voor een veilige implementatie van geheimen in productie systemen raden wij u aan [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net)te gebruiken.
>

## <a name="create-a-text-analytics-client"></a>Een Text Analytics-client maken

Maak een nieuw `TextAnalyticsClient` object met `credentials` als para meter. Gebruik de juiste Azure-regio voor uw Text Analytics-abonnement.

```javascript
//Replace 'westus' with the correct region for your Text Analytics subscription
let client = new TextAnalyticsAPIClient(
    credentials,
    "https://westus.api.cognitive.microsoft.com/"
);
```

## <a name="sentiment-analysis"></a>Sentimentanalyse

Maak een lijst met objecten die de documenten bevatten die u wilt analyseren. De payload van de API bestaat uit een lijst met `documents`die een `id`, `language`, en `text` kenmerk bevat. Het `text` kenmerk slaat de tekst op die moet worden geanalyseerd `language` , is de taal van het document en de `id` kan een wille keurige waarde zijn. 

```javascript
const inputDocuments = {documents:[
    {language:"en", id:"1", text:"I had the best day of my life."},
    {language:"en", id:"2", text:"This was a waste of my time. The speaker put me to sleep."},
    {language:"es", id:"3", text:"No tengo dinero ni nada que dar..."},
    {language:"it", id:"4", text:"L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."}
]}
```

Roep `client.sentiment` het resultaat en ontvang het. Herhaal vervolgens de resultaten en druk de document-ID en sentiment Score af. Een score dichter bij 0 geeft aan dat er een negatieve sentiment is, terwijl een score dichter bij 1 wijst op een positieve sentiment.

```javascript
const operation = client.sentiment({multiLanguageBatchInput: inputDocuments})
operation
.then(result => {
    console.log(result.documents);
})
.catch(err => {
    throw err;
});
```

Voer uw code `node index.js` uit in het console venster.

### <a name="output"></a>Output

```console
[ { id: '1', score: 0.8723785877227783 },
  { id: '2', score: 0.1059873104095459 },
  { id: '3', score: 0.43635445833206177 },
  { id: '4', score: 1 } ]
```

## <a name="language-detection"></a>Taaldetectie

Maak een lijst met objecten die uw documenten bevatten. De payload van de API bestaat uit een lijst met `documents`, die een `id` and `text` -kenmerk bevat. Het `text` kenmerk slaat de tekst die moet worden geanalyseerd en de `id` kan een wille keurige waarde zijn.

```javascript
// The documents to be submitted for language detection. The ID can be any value.
const inputDocuments = {
    documents: [
        { id: "1", text: "This is a document written in English." },
        { id: "2", text: "Este es un document escrito en Español." },
        { id: "3", text: "这是一个用中文写的文件" }
    ]
    };
```

Roep `client.detectLanguage()` het resultaat en ontvang het. Herhaal vervolgens de resultaten en druk de ID van elk document af en de eerste geretourneerde taal.

```javascript
const operation = client.detectLanguage({
    languageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`ID: ${document.id}`);
        document.detectedLanguages.forEach(language =>
        console.log(`\tLanguage: ${language.name}`)
        );
    });
    })
    .catch(err => {
    throw err;
    });
```

Voer uw code `node index.js` uit in het console venster.

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
ID: 1 Language English
ID: 2 Language Spanish
ID: 3 Language Chinese_Simplified
```

## <a name="entity-recognition"></a>Entiteit herkenning

Maak een lijst met objecten die uw documenten bevatten. De payload van de API bestaat uit een lijst met `documents`die een `id`, `language`, en `text` kenmerk bevat. Het `text` kenmerk slaat de tekst op die moet worden geanalyseerd `language` , is de taal van het document en de `id` kan een wille keurige waarde zijn.

```javascript

    const inputDocuments = {documents:[
        {language:"en", id:"1", text:"Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800"},
        {language:"es", id:"2", text:"La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."},
        ]}

}
```

Roep `client.entities()` het resultaat en ontvang het. Herhaal vervolgens de resultaten en druk de document-ID af. Voor elke gedetecteerde entiteit drukt u de naam van de Wikipedia, het type en subtype (indien aanwezig) af, evenals de locaties in de oorspronkelijke tekst.

```javascript
const operation = client.entities({
    multiLanguageBatchInput: inputDocuments
});
operation
    .then(result => {
    result.documents.forEach(document => {
        console.log(`Document ID: ${document.id}`)
        document.entities.forEach(e =>{
        console.log(`\tName: ${e.name} Type: ${e.type} Sub Type: ${e.type}`)
        e.matches.forEach(match => (
            console.log(`\t\tOffset: ${match.offset} Length: ${match.length} Score: ${match.entityTypeScore}`)
        ))
        })
    });
    })
    .catch(err => {
    throw err;
    });
```

Voer uw code `node index.js` uit in het console venster.

### <a name="output"></a>Output

```console
Document ID: 1
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 0 Length: 9 Score: 1
    Name: Bill Gates Type: Person Sub Type: Person
            Offset: 25 Length: 10 Score: 0.999786376953125
    Name: Paul Allen Type: Person Sub Type: Person
            Offset: 40 Length: 10 Score: 0.9988105297088623
    Name: April 4 Type: Other Sub Type: Other
            Offset: 54 Length: 7 Score: 0.8
    Name: April 4, 1975 Type: DateTime Sub Type: DateTime
            Offset: 54 Length: 13 Score: 0.8
    Name: BASIC Type: Other Sub Type: Other
            Offset: 89 Length: 5 Score: 0.8
    Name: Altair 8800 Type: Other Sub Type: Other
            Offset: 116 Length: 11 Score: 0.8
Document ID: 2
    Name: Microsoft Type: Organization Sub Type: Organization
            Offset: 21 Length: 9 Score: 0.999755859375
    Name: Redmond (Washington) Type: Location Sub Type: Location
            Offset: 60 Length: 7 Score: 0.9911284446716309
    Name: 21 kilómetros Type: Quantity Sub Type: Quantity
            Offset: 71 Length: 13 Score: 0.8
    Name: Seattle Type: Location Sub Type: Location
            Offset: 88 Length: 7 Score: 0.9998779296875
```

## <a name="key-phrase-extraction"></a>Sleuteltermextractie

Maak een lijst met objecten die uw documenten bevatten. De payload van de API bestaat uit een lijst met `documents`die een `id`, `language`, en `text` kenmerk bevat. Het `text` kenmerk slaat de tekst op die moet worden geanalyseerd `language` , is de taal van het document en de `id` kan een wille keurige waarde zijn.

```javascript
    let inputLanguage = {
    documents: [
        {language:"ja", id:"1", text:"猫は幸せ"},
        {language:"de", id:"2", text:"Fahrt nach Stuttgart und dann zum Hotel zu Fu."},
        {language:"en", id:"3", text:"My cat might need to see a veterinarian."},
        {language:"es", id:"4", text:"A mi me encanta el fútbol!"}
    ]
    };
```

Roep `client.keyPhrases()` het resultaat en ontvang het. Herhaal vervolgens de resultaten en druk elke document-ID en alle gedetecteerde sleutel zinnen af.

```javascript
    let operation = client.keyPhrases({
    multiLanguageBatchInput: inputLanguage
    });
    operation
    .then(result => {
        console.log(result.documents);
    })
    .catch(err => {
        throw err;
    });
```

Voer uw code `node index.js` uit in het console venster.

### <a name="output"></a>Output

```console
[ 
    { id: '1', keyPhrases: [ '幸せ' ] },
    { id: '2', keyPhrases: [ 'Stuttgart', 'Hotel', 'Fahrt', 'Fu' ] },
    { id: '3', keyPhrases: [ 'cat', 'veterinarian' ] },
    { id: '4', keyPhrases: [ 'fútbol' ] } 
]
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Zie ook

 [Overzicht van Text Analytics](../overview.md) [Veelgestelde vragen](../text-analytics-resource-faq.md)
