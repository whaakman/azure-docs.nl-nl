---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Node.js | Microsoft Docs
description: Aan de slag verzenden van gebeurtenissen naar Event Hubs met behulp van Node.js.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 12/05/2018
ms.author: shvija
ms.openlocfilehash: e64aab3aed582a60140ee1357e79ee5ee4a4cdf4
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52965033"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Node.js

Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

Deze zelfstudie wordt beschreven hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Node.js.

> [!NOTE]
> U kunt deze snelstart als voorbeeld downloaden van de [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client), de tekenreeksen `EventHubConnectionString` en `EventHubName` vervangen door uw event hub-waarden en deze uitvoeren. U kunt ook de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Node.js versie 8.x en hoger. Download de nieuwste versie van de TNS van [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (aanbevolen) of een andere IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Volg de procedure in [dit artikel](event-hubs-create.md) om een naamruimte en een Event Hub te maken en ga daarna verder met de volgende stappen in deze zelfstudie.

De verbindingsreeks voor de event hub-naamruimte ophalen met de instructies in het artikel: [-verbindingsreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). U de verbindingsreeks later in deze zelfstudie.

## <a name="clone-the-sample-git-repository"></a>Kloon de voorbeeld-Git-opslagplaats
Kloon de voorbeeld-Git-opslagplaats van [Github](https://github.com/Azure/azure-event-hubs-node) op uw computer. 

## <a name="install-nodejs-package"></a>Node.js-pakket installeren
Node.js-pakket voor Azure Event Hubs op uw computer installeren. 

```nodejs
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>De Git-opslagplaats klonen
Download of kloon de [voorbeeld](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) vanuit Github. 

## <a name="send-events"></a>Gebeurtenissen verzenden
De SDK die u hebt gekloond bevat meerdere voorbeelden die laten zien u hoe u gebeurtenissen verzendt naar een event hub met behulp van node.js. In deze snelstartgids gebruikt u de **simpleSender.js** voorbeeld. U ziet dat gebeurtenissen worden ontvangen, opent u een andere terminal en gebeurtenissen ontvangen met de [ontvangen voorbeeld](event-hubs-node-get-started-receive.md).

1. Open het project in Visual Studio Code. 
2. Maak een bestand met de naam **.env** onder de **client** map. Kopieer en plak de omgevingsvariabelen voorbeeld van de **sample.env** in de hoofdmap.
3. Uw event hub-verbindingsreeks, de event hub-naam en de storage-eindpunt configureren. U kunt de verbindingsreeks kopiëren voor uw event hub uit **verbinding verbindingsreeks-primaire** key onder **RootManageSharedAccessKey** op de Event Hub-pagina in de Azure-portal. Zie voor gedetailleerde stappen [-verbindingsreeks ophalen](event-hubs-create.md#create-an-event-hubs-namespace).
4. Op uw Azure-CLI, gaat u naar de **client** mappad. Knooppunt-pakketten installeren en bouw het project door het uitvoeren van de volgende opdrachten:

    ```nodejs
    npm i
    npm run build
    ```
5. Beginnen met het verzenden van gebeurtenissen met de volgende opdracht: 

    ```nodejs
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>De voorbeeldcode controleren 
Hier volgt de voorbeeldcode voor het verzenden van gebeurtenissen naar een event hub met behulp van node.js. U kunt handmatig een bestand sampleSender.js maken en uitvoeren voor het verzenden van gebeurtenissen naar een event hub. 


```nodejs
const { EventHubClient, EventPosition } = require('@azure/event-hubs');

const client = EventHubClient.createFromConnectionString(process.env["EVENTHUB_CONNECTION_STRING"], process.env["EVENTHUB_NAME"]);

async function main() {
    // NOTE: For receiving events from Azure Stream Analytics, please send Events to an EventHub where the body is a JSON object/array.
    // const eventData = { body: { "message": "Hello World" } };
    const data = { body: "Hello World 1" };
    const delivery = await client.send(data);
    console.log("message sent successfully.");
}

main().catch((err) => {
    console.log(err);
});

```

Houd er rekening mee aan de omgevingsvariabelen worden ingesteld voordat het script is uitgevoerd. U kunt dit configureren op de opdrachtregel als in het volgende voorbeeld wordt weergegeven, of gebruik de [dotenv pakket](https://www.npmjs.com/package/dotenv#dotenv). 

```
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u berichten verzonden naar een event hub met behulp van Node.js. Zie voor meer informatie over gebeurtenissen ontvangen van een event hub met behulp van Node.js, [ontvangen van gebeurtenissen van event hub - Node.js](event-hubs-node-get-started-receive.md)

Bekijk andere Node.js-voorbeelden voor Event Hubs op [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
