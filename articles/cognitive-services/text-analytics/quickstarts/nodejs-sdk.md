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
ms.date: 06/11/2019
ms.author: shthowse
ms.openlocfilehash: 7e43d53c0916cf7fdc684c9e044e632015662c3b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081517"
---
# <a name="quickstart-using-nodejs-to-call-the-text-analytics-cognitive-service"></a>Quickstart: Node.js gebruiken om de Text Analytics Cognitive Service aan te roepen
<a name="HOLTop"></a>

Gebruik deze Quick Start om te beginnen met het analyseren van taal met de Text Analytics-SDK voor Node.js. Terwijl de [Tekstanalyse](//go.microsoft.com/fwlink/?LinkID=759711) REST-API is compatibel met de meeste moderne programmeertalen, de SDK biedt een eenvoudige manier om de service te integreren in uw toepassingen. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/textAnalytics.js).

Raadpleeg de [API-definities](//go.microsoft.com/fwlink/?LinkID=759346) voor technische documentatie voor de API's.

## <a name="prerequisites"></a>Vereisten

* [Node.js](https://nodejs.org/)
* De Text Analytics [SDK voor Node.js](https://www.npmjs.com/package/azure-cognitiveservices-textanalytics) kunt u de SDK installeren:

    `npm install azure-cognitiveservices-textanalytics`

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

U moet ook de [eindpunt- en toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) hebben die voor u is gegenereerd tijden de registratie.

## <a name="create-a-nodejs-application-and-install-the-sdk"></a>Een Node.js-toepassing maken en installeer de SDK

Na de installatie van Node.js, een knooppunt-project te maken. Maak een nieuwe map voor uw app en navigeer naar de map.

```mkdir myapp && cd myapp```

Voer ```npm init``` een node-toepassing maken met een package.json-bestand. Installeer de `ms-rest-azure` en `azure-cognitiveservices-textanalytics` NPM-pakketten:

```npm install azure-cognitiveservices-textanalytics ms-rest-azure```

Package.json-bestand van uw app wordt bijgewerkt met de afhankelijkheden.

## <a name="authenticate-your-credentials"></a>Verificatie van uw referenties

Maak een nieuw bestand `index.js` in het project de hoofd- en de geïnstalleerde bibliotheken importeren

```javascript
const CognitiveServicesCredentials = require("ms-rest-azure").CognitiveServicesCredentials;
const TextAnalyticsAPIClient = require("azure-cognitiveservices-textanalytics");
```

Maak een variabele voor de sleutel van uw Text Analytics-abonnement.

```javascript
let credentials = new CognitiveServicesCredentials(
    "enter-your-key-here"
);
```

> [!Tip]
> Voor de veilige distributie van geheimen in productiesystemen wordt u aangeraden [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Een Text Analytics-client maken

Maak een nieuwe `TextAnalyticsClient` object met `credentials` als parameter. Gebruik de juiste Azure-regio voor uw Text Analytics-abonnement.

```javascript
//Replace 'westus' with the correct region for your Text Analytics subscription
let client = new TextAnalyticsAPIClient(
    credentials,
    "https://westus.api.cognitive.microsoft.com/"
);
```

## <a name="sentiment-analysis"></a>Sentimentanalyse

Maak een lijst van objecten, met de documenten die u wilt analyseren. De nettolading voor de API bestaat uit een lijst met `documents`, waarin een `id`, `language`, en `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk `language` is de taal van het document en de `id` is een waarde. 

```javascript
const inputDocuments = {documents:[
    {language:"en", id:"1", text:"I had the best day of my life."},
    {language:"en", id:"2", text:"This was a waste of my time. The speaker put me to sleep."},
    {language:"es", id:"3", text:"No tengo dinero ni nada que dar..."},
    {language:"it", id:"4", text:"L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."}
]}
```

Bel `client.sentiment` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en gevoelsscore. Een score dichter bij 0 geeft aan dat een negatief gevoel, terwijl een score dichter bij 1 geeft aan een positieve stemming dat.

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

Voer uw code met `node index.js` in het consolevenster weergegeven.

### <a name="output"></a>Uitvoer

```console
[ { id: '1', score: 0.8723785877227783 },
  { id: '2', score: 0.1059873104095459 },
  { id: '3', score: 0.43635445833206177 },
  { id: '4', score: 1 } ]
```

## <a name="language-detection"></a>Taaldetectie

Maak een lijst van objecten die uw documenten. De nettolading voor de API bestaat uit een lijst met `documents`, waarin een `id` en `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk en de `id` is een waarde.

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

Bel `client.detectLanguage()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en de taal van de eerste geretourneerd.

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

Voer uw code met `node index.js` in het consolevenster weergegeven.

### <a name="output"></a>Uitvoer

```console
===== LANGUAGE EXTRACTION ======
ID: 1 Language English
ID: 2 Language Spanish
ID: 3 Language Chinese_Simplified
```

## <a name="entity-recognition"></a>Herkenning van entiteit

Maak een lijst van objecten, die uw documenten. De nettolading voor de API bestaat uit een lijst met `documents`, waarin een `id`, `language`, en `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk `language` is de taal van het document en de `id` is een waarde.

```javascript

    const inputDocuments = {documents:[
        {language:"en", id:"1", text:"Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800"},
        {language:"es", id:"2", text:"La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle."},
        ]}

}
```

Bel `client.entities()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID. Voor elke entiteit wordt gedetecteerd, de wikipedia-naam, het type en subtypen afdrukken (indien aanwezig) evenals de locaties in de oorspronkelijke tekst.

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

Voer uw code met `node index.js` in het consolevenster weergegeven.

### <a name="output"></a>Uitvoer

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

Maak een lijst van objecten, die uw documenten. De nettolading voor de API bestaat uit een lijst met `documents`, waarin een `id`, `language`, en `text` kenmerk. De `text` slaat de tekst die moet worden geanalyseerd, kenmerk `language` is de taal van het document en de `id` is een waarde.

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

Bel `client.keyPhrases()` en het resultaat. Vervolgens doorloopt u de resultaten en afdrukken van elk document-ID en eventuele gedetecteerde sleuteltermen.

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

Voer uw code met `node index.js` in het consolevenster weergegeven.

### <a name="output"></a>Uitvoer

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
