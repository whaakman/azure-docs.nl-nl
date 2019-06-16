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
ms.openlocfilehash: e67be59e0ed78b2080986acb73a33fc87599c9d3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65539332"
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
- **Maak een Event Hubs-naamruimte en een event hub**. In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Als u wilt een naamruimte en een event hub maken, volgt u de procedure in [in dit artikel](event-hubs-create.md), gaat u verder met de volgende stappen in deze zelfstudie. Haal vervolgens de verbindingsreeks voor de event hub-naamruimte met de instructies in het artikel: [Verbindingstekenreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). U de verbindingsreeks later in deze zelfstudie.


### <a name="install-npm-package"></a>Npm-pakket installeren
Voor het installeren van de [npm-pakket voor Event Hubs](https://www.npmjs.com/package/@azure/event-hubs), open een opdrachtprompt met `npm` wijzigen in het pad, de map naar de map waar u uw voorbeelden hebt en voer deze opdracht

```shell
npm install @azure/event-hubs
```

Voor het installeren van de [npm-pakket voor de Event Processor Host](https://www.npmjs.com/package/@azure/event-processor-host), voert de onderstaande opdracht in plaats daarvan

```shell
npm install @azure/event-processor-host
```

## <a name="send-events"></a>Gebeurtenissen verzenden

Deze sectie leest u over het maken van een Node.js-toepassing die gebeurtenissen naar een event hub verzendt. 

1. Open uw favoriete editor, zoals [Visual Studio Code](https://code.visualstudio.com). 
2. Maak een bestand met de naam `send.js` en plak de onderstaande code naartoe.
    ```javascript
    const { EventHubClient } = require("@azure/event-hubs");

    // Define connection string and the name of the Event Hub
    const connectionString = "";
    const eventHubsName = "";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);

      for (let i = 0; i < 100; i++) {
        const eventData = {body: `Event ${i}`};
        console.log(`Sending message: ${eventData.body}`);
        await client.send(eventData);
      }

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Voer de verbindingsreeks en de naam van uw Event Hub in de bovenstaande code
4. Voer de opdracht `node send.js` in een opdrachtprompt voor het uitvoeren van dit bestand. Hiermee wordt de 100 gebeurtenissen verzonden naar uw Event Hub

Gefeliciteerd! U hebt nu gebeurtenissen verzonden naar een event hub.


## <a name="receive-events"></a>Gebeurtenissen ontvangen

Deze sectie leest u hoe u een Node.js-toepassing waarmee gebeurtenissen worden ontvangen van een enkele partitie van de standaardgroep voor consumenten in een event hub maken. 

1. Open uw favoriete editor, zoals [Visual Studio Code](https://code.visualstudio.com). 
2. Maak een bestand met de naam `receive.js` en plak de onderstaande code naartoe.
    ```javascript
    const { EventHubClient, delay } = require("@azure/event-hubs");

    // Define connection string and related Event Hubs entity name here
    const connectionString = "";
    const eventHubsName = "";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);
      const allPartitionIds = await client.getPartitionIds();
      const firstPartitionId = allPartitionIds[0];

      const receiveHandler = client.receive(firstPartitionId, eventData => {
        console.log(`Received message: ${eventData.body} from partition ${firstPartitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await receiveHandler.stop();

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Voer de verbindingsreeks en de naam van uw Event Hub in de bovenstaande code.
4. Voer de opdracht `node receive.js` in een opdrachtprompt voor het uitvoeren van dit bestand. Hiermee worden gebeurtenissen ontvangen van een van de partities van de standaardgroep voor consumenten in uw Event Hub

Gefeliciteerd! U hebt nu gebeurtenissen ontvangen van event hub.

## <a name="receive-events-using-event-processor-host"></a>Gebeurtenissen ontvangen met Eventprocessorhost

Deze sectie wordt beschreven hoe u gebeurtenissen ontvangen van een event hub met behulp van Azure [EventProcessorHost](event-hubs-event-processor-host.md) in een Node.js-toepassing. De EventProcessorHost (EPH) kunt u efficiënt gebeurtenissen ontvangen van een event hub door het maken van ontvangers voor alle partities in de consumentengroep van een event hub. Deze controlepunten metagegevens op de ontvangen berichten met regelmatige tussenpozen in een Azure Storage-Blob. Deze aanpak maakt het eenvoudig om door te gaan met het ontvangen van berichten van waar u gestopt op een later tijdstip bent.

1. Open uw favoriete editor, zoals [Visual Studio Code](https://code.visualstudio.com). 
2. Maak een bestand met de naam `receiveAll.js` en plak de onderstaande code naartoe.
    ```javascript
    const { EventProcessorHost, delay } = require("@azure/event-processor-host");

    // Define connection string and related Event Hubs entity name here
    const eventHubConnectionString = "";
    const eventHubName = "";
    const storageConnectionString = "";

    async function main() {
      const eph = EventProcessorHost.createFromConnectionString(
        "my-eph",
        storageConnectionString,
        "my-storage-container-name",
        eventHubConnectionString,
        {
          eventHubPath: eventHubName,
          onEphError: (error) => {
            console.log("[%s] Error: %O", error);
          }
        }
      );


      eph.start((context, eventData) => {
        console.log(`Received message: ${eventData.body} from partition ${context.partitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await eph.stop();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });

    ```
3. Voer in de verbindingsreeks en de naam van uw Event Hub in de bovenstaande code samen met de verbindingsreeks voor een Azure Blob-opslag
4. Voer de opdracht `node receiveAll.js` in een opdrachtprompt voor het uitvoeren van dit bestand.

Gefeliciteerd! U hebt nu gebeurtenissen ontvangen van event hub met behulp van Event Processor Host. Deze worden gebeurtenissen ontvangen van alle partities van de standaardgroep voor consumenten in uw Event Hub

## <a name="next-steps"></a>Volgende stappen
Lees de volgende artikelen:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Functies en de belangrijkste termen in de Azure Event Hubs](event-hubs-features.md)
- [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)
- Bekijk andere Node.js-voorbeelden voor [Event Hubs](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) en [Event Processor Host](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-processor-host/samples) op GitHub
