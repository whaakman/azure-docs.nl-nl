---
title: Verzenden en ontvangen van gebeurtenissen met behulp van Node.js - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht voor het maken van een Node.js-toepassing die gebeurtenissen uit Azure Event Hubs verzendt.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: f03bfde8f7ea37989756ad47678369e94b831438
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677898"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Gebeurtenissen verzenden of ontvangen van gebeurtenissen uit Azure Event Hubs met behulp van Node.js

Azure Event Hubs is een Big Data-platform en gebeurtenis een gebeurtenisopneemservice die kan worden ontvangen en verwerken miljoenen gebeurtenissen per seconde streamen. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze zelfstudie wordt beschreven hoe u Node.js-toepassingen gebeurtenissen te verzenden of ontvangen van gebeurtenissen van een event hub maken.

> [!NOTE]
> U kunt deze snelstart als voorbeeld downloaden van de [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client), de tekenreeksen `EventHubConnectionString` en `EventHubName` vervangen door uw event hub-waarden en deze uitvoeren. U kunt ook de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- Node.js versie 8.x en hoger. Download de nieuwste versie van de TNS van [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (aanbevolen) of een andere IDE
- **Maak een Event Hubs-naamruimte en een event hub**. In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Als u wilt een naamruimte en een event hub maken, volgt u de procedure in [in dit artikel](event-hubs-create.md), gaat u verder met de volgende stappen in deze zelfstudie. Haal vervolgens de verbindingsreeks voor de event hub-naamruimte met de instructies in het artikel: [Verbindingstekenreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). U gebruikt de verbindingsreeks later in deze zelfstudie.
- Kloon de [GitHub-voorbeeldopslagplaats](https://github.com/Azure/azure-event-hubs-node) op uw computer. 


## <a name="send-events"></a>Gebeurtenissen verzenden
Deze sectie leest u over het maken van een Node.js-toepassing die gebeurtenissen naar een event hub verzendt. 

### <a name="install-nodejs-package"></a>Node.js-pakket installeren
Node.js-pakket voor Azure Event Hubs op uw computer installeren. 

```shell
npm install @azure/event-hubs
```

Als u dit nog niet hebt de Git-opslagplaats gekloond, zoals vermeld in de vereiste, downloadt u de [voorbeeld](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) vanuit GitHub. 

De SDK die u hebt gekloond bevat meerdere voorbeelden die laten zien u hoe u gebeurtenissen verzendt naar een event hub met behulp van node.js. In deze snelstartgids gebruikt u de **simpleSender.js** voorbeeld. U ziet dat gebeurtenissen worden ontvangen, opent u een andere terminal en gebeurtenissen ontvangen met de [ontvangen voorbeeld](event-hubs-node-get-started-receive.md).

1. Open het project in Visual Studio Code. 
2. Maak een bestand met de naam **.env** onder de **client** map. Kopieer en plak de omgevingsvariabelen voorbeeld van de **sample.env** in de hoofdmap.
3. Uw event hub-verbindingsreeks, de event hub-naam en de storage-eindpunt configureren. Voor instructies over het verkrijgen van een verbindingsreeks voor een event hub, [-verbindingsreeks ophalen](event-hubs-create.md#create-an-event-hubs-namespace).
4. Op uw Azure-CLI, gaat u naar de **client** mappad. Knooppunt-pakketten installeren en bouw het project door het uitvoeren van de volgende opdrachten:

    ```shell
    npm i
    npm run build
    ```
5. Beginnen met het verzenden van gebeurtenissen met de volgende opdracht: 

    ```shell
    node dist/examples/simpleSender.js
    ```


### <a name="review-the-sample-code"></a>De voorbeeldcode controleren 
Bekijk de voorbeeldcode in het bestand simpleSender.js gebeurtenissen naar een event hub verzendt.

```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const lib_1 = require("../lib");
const dotenv = require("dotenv");
dotenv.config();
const connectionString = "EVENTHUB_CONNECTION_STRING";
const entityPath = "EVENTHUB_NAME";
const str = process.env[connectionString] || "";
const path = process.env[entityPath] || "";

async function main() {
    const client = lib_1.EventHubClient.createFromConnectionString(str, path);
    const data = {
        body: "Hello World!!"
    };
    const delivery = await client.send(data);
    console.log(">>> Sent the message successfully: ", delivery.tag.toString());
    console.log(delivery);
    console.log("Calling rhea-promise sender close directly. This should result in sender getting reconnected.");
    await Object.values(client._context.senders)[0]._sender.close();
    // await client.close();
}

main().catch((err) => {
    console.log("error: ", err);
});

```

Houd er rekening mee aan de omgevingsvariabelen worden ingesteld voordat het script is uitgevoerd. U kunt ze configureren in de opdrachtregel als in het volgende voorbeeld wordt weergegeven, of gebruik de [dotenv pakket](https://www.npmjs.com/package/dotenv#dotenv). 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="receive-events"></a>Gebeurtenissen ontvangen
Deze zelfstudie laat zien hoe u gebeurtenissen ontvangen van een event hub met behulp van Azure [EventProcessorHost](event-hubs-event-processor-host.md) in een Node.js-toepassing. De EventProcessorHost (EPH) kunt u efficiënt gebeurtenissen ontvangen van een event hub door het maken van ontvangers voor alle partities in de consumentengroep van een event hub. Deze controlepunten metagegevens op de ontvangen berichten met regelmatige tussenpozen in een Azure Storage-Blob. Deze aanpak maakt het eenvoudig om door te gaan met het ontvangen van berichten van waar u gestopt op een later tijdstip bent.

De code voor deze snelstartgids is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor).

### <a name="clone-the-git-repository"></a>De Git-opslagplaats klonen
Download of kloon de [voorbeeld](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) vanuit GitHub. 

### <a name="install-the-eventprocessorhost"></a>Installeren van de EventProcessorHost
Installeer de EventProcessorHost voor Event Hubs-module. 

```shell
npm install @azure/event-processor-host
```

### <a name="receive-events-using-eventprocessorhost"></a>Gebeurtenissen ontvangen met EventProcessorHost
De SDK die u hebt gekloond bevat meerdere voorbeelden die laten zien u hoe gebeurtenissen ontvangen van een event hub met behulp van Node.js. In deze snelstartgids gebruikt u de **singleEPH.js** voorbeeld. U ziet dat gebeurtenissen worden ontvangen, opent u een andere terminal en verzenden van gebeurtenissen met behulp van de [steekproef te verzenden](event-hubs-node-get-started-send.md).

1. Open het project in Visual Studio Code. 
2. Maak een bestand met de naam **.env** onder de **processor** map. Kopieer en plak de omgevingsvariabelen voorbeeld van de **sample.env** in de hoofdmap.
3. Uw event hub-verbindingsreeks, de event hub-naam en de storage-eindpunt configureren. U kunt de verbindingsreeks kopiëren voor uw event hub uit **verbinding verbindingsreeks-primaire** key onder **RootManageSharedAccessKey** op de Event Hub-pagina in de Azure-portal. Zie voor gedetailleerde stappen [-verbindingsreeks ophalen](event-hubs-create.md#create-an-event-hubs-namespace).
4. Op uw Azure-CLI, gaat u naar de **processor** mappad. Knooppunt-pakketten installeren en bouw het project door het uitvoeren van de volgende opdrachten:

    ```shell
    npm i
    npm run build
    ```
5. Gebeurtenissen ontvangen met de event processor host met de volgende opdracht:

    ```shell
    node dist/examples/singleEph.js
    ```

### <a name="review-the-sample-code"></a>De voorbeeldcode controleren 
Hier volgt de voorbeeldcode voor het ontvangen van gebeurtenissen van een event hub met behulp van node.js. U kunt handmatig een sampleEph.js-bestand maken en uitvoeren om te ontvangen van gebeurtenissen naar een event hub. 

  ```javascript
  const { EventProcessorHost, delay } = require("@azure/event-processor-host");

  const path = process.env.EVENTHUB_NAME;
  const storageCS = process.env.STORAGE_CONNECTION_STRING;
  const ehCS = process.env.EVENTHUB_CONNECTION_STRING;
  const storageContainerName = "test-container";
  
  async function main() {
    // Create the Event Processor Host
    const eph = EventProcessorHost.createFromConnectionString(
      EventProcessorHost.createHostName("my-host"),
      storageCS,
      storageContainerName,
      ehCS,
      {
        eventHubPath: path,
        onEphError: (error) => {
          console.log("This handler will notify you of any internal errors that happen " +
          "during partition and lease management: %O", error);
        }
      }
    );
    let count = 0;
    // Message event handler
    const onMessage = async (context/*PartitionContext*/, data /*EventData*/) => {
      console.log(">>>>> Rx message from '%s': '%s'", context.partitionId, data.body);
      count++;
      // let us checkpoint every 100th message that is received across all the partitions.
      if (count % 100 === 0) {
        return await context.checkpoint();
      }
    };
    // Error event handler
    const onError = (error) => {
      console.log(">>>>> Received Error: %O", error);
    };
    // start the EPH
    await eph.start(onMessage, onError);
    // After some time let' say 2 minutes
    await delay(120000);
    // This will stop the EPH.
    await eph.stop();
  }
  
  main().catch((err) => {
    console.log(err);
  });
      
  ```

Houd er rekening mee aan de omgevingsvariabelen worden ingesteld voordat het script is uitgevoerd. U kunt dit configureren op de opdrachtregel als in het volgende voorbeeld wordt weergegeven, of gebruik de [dotenv pakket](https://www.npmjs.com/package/dotenv#dotenv). 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

U vindt meer voorbeelden [hier](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples).


## <a name="next-steps"></a>Volgende stappen
Lees de volgende artikelen:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Functies en de belangrijkste termen in de Azure Event Hubs](event-hubs-features.md)
- [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
- Bekijk andere Node.js-voorbeelden voor Event Hubs op [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
