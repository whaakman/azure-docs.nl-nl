---
title: Gebeurtenissen ontvangen van Azure Event Hubs met behulp van Node.js | Microsoft Docs
description: Informatie over het ontvangen van gebeurtenissen van Event Hubs met behulp van Node.js.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 09/18/2018
ms.author: shvija
ms.openlocfilehash: 6d5b52c8a5dd0306a349cac5e67eecc809005c6f
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49429181"
---
# <a name="receive-events-from-azure-event-hubs-using-nodejs"></a>Gebeurtenissen ontvangen van Azure Event Hubs met behulp van Node.js

Azure Event Hubs is een uiterst schaalbare gebeurtenissen beheersysteem dat miljoenen verwerken kan gebeurtenissen per seconde, zodat toepassingen om te verwerken en analyseren van enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en andere systemen. Zodra de verzameld in een event hub, u kunt ontvangen en verwerken van gebeurtenissen met in-process-handlers of doorsturen naar andere systemen analytics. U kunt ook de gegevens van de gebeurtenis in Azure Storage of Azure Data Lake Store vastleggen voordat deze wordt verwerkt.  

Zie voor meer informatie over Event Hubs, de [Event Hubs-overzicht](event-hubs-about.md).

Deze zelfstudie laat zien hoe u gebeurtenissen ontvangen van een event hub met behulp van Azure [EventProcessorHost](event-hubs-event-processor-host.md) in een Node.js-toepassing. De EventProcessorHost (EPH) kunt u efficiënt gebeurtenissen ontvangen van een event hub door het maken van ontvangers voor alle partities in de consumentengroep van een event hub. Deze controlepunten metagegevens op de ontvangen berichten met regelmatige tussenpozen in een Azure Storage-Blob. Deze aanpak maakt het eenvoudig om door te gaan met het ontvangen van berichten van waar u gestopt op een later tijdstip bent.

De code voor deze snelstartgids is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor).

> [!NOTE]
>  Zie voor het verzenden van gebeurtenissen naar Event Hubs met behulp van Node.js, in dit artikel: [gebeurtenissen verzenden naar Azure Event Hubs met behulp van Node.js](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Node.js versie 8.x en hoger. Download de nieuwste versie van de TNS van [ https://nodejs.org ](https://nodejs.org). Gebruik geen oudere LTS-versie van node.js. 
- Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][] aan voordat u begint.

## <a name="create-a-namespace-and-event-hub"></a>Een naamruimte en event hub maken
De eerste stap is het gebruik van Azure portal een Event Hubs-naamruimte maken met een event hub. Als u een bestaand account hebt, kunt u deze entiteiten maken door de instructies in [maken van een Event Hubs-naamruimte en een event hub met behulp van de Azure-portal](event-hubs-create.md).

## <a name="create-a-storage-account-and-container"></a>Een storage-account en een container maken
U moet een Azure Storage-account hebben voor het gebruik van de EventProcessorHost. De informatie over de status, zoals leases op partities en de controlepunten in de gebeurtenisstroom worden gedeeld tussen ontvangers met behulp van een Azure Storage-container. U kunt een Azure Storage-account maken door de instructies in [in dit artikel](../storage/common/storage-quickstart-create-account.md).

## <a name="clone-the-git-repository"></a>De Git-opslagplaats klonen
Download of kloon de [voorbeeld](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) vanuit Github. 

## <a name="install-the-eventprocessorhost"></a>Installeren van de EventProcessorHost
Installeer de EventProcessorHost voor Event Hubs-module. 

```nodejs
npm install @azure/event-processor-host
```

## <a name="receive-events-using-eventprocessorhost"></a>Gebeurtenissen ontvangen met EventProcessorHost
De SDK die u hebt gekloond bevat meerdere voorbeelden die laten zien u hoe gebeurtenissen ontvangen van een event hub met behulp van Node.js. In deze snelstartgids gebruikt u de **singleEPH.js** voorbeeld. U ziet dat gebeurtenissen worden ontvangen, opent u een andere terminal en verzenden van gebeurtenissen met behulp van de [steekproef te verzenden](event-hubs-node-get-started-send.md).

1. Open het project in Visual Studio Code. 
2. Maak een bestand met de naam **.env** onder de **processor** map. Kopieer en plak de omgevingsvariabelen voorbeeld van de **sample.env** in de hoofdmap.
3. Uw event hub-verbindingsreeks, de event hub-naam en de storage-eindpunt configureren. U kunt de verbindingsreeks kopiëren voor uw event hub uit **verbinding verbindingsreeks-primaire** key onder **RootManageSharedAccessKey** op de Event Hub-pagina in de Azure-portal. Zie voor gedetailleerde stappen [-verbindingsreeks ophalen](event-hubs-create.md#create-an-event-hubs-namespace).
4. Op uw Azure-CLI, gaat u naar de **processor** mappad. Knooppunt-pakketten installeren en bouw het project door het uitvoeren van de volgende opdrachten:

    ```nodejs
    npm i
    npm run build
    ```
5. Gebeurtenissen ontvangen met de event processor host met de volgende opdracht:

    ```nodejs
    node dist/examples/singleEph.js
    ```

## <a name="review-the-sample-code"></a>De voorbeeldcode controleren 
Hier volgt de voorbeeldcode voor het ontvangen van gebeurtenissen van een event hub met behulp van node.js. U kunt handmatig een sampleEph.js-bestand maken en uitvoeren om te ontvangen van gebeurtenissen naar een event hub. 

  ```nodejs
  const { EventProcessorHost, delay } = require("@azure/event-processor-host");

  const path = process.env.EVENTHUB_NAME;
  const storageCS = process.env.STORAGE_CONNECTION_STRING;
  const ehCS = process.env.EVENTHUB_CONNECTION_STRING;
  const storageContainerName = "test-container";
  
  async function main() {
    // Create the Event Processo Host
    const eph = EventProcessorHost.createFromConnectionString(
      EventProcessorHost.createHostName("my-host"),
      storageCS,
      storageContainerName,
      ehCS,
      {
        eventHubPath: path
      },
      onEphError: (error) => {
        console.log("This handler will notify you of any internal errors that happen " +
        "during partition and lease management: %O", error);
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

```
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

U vindt meer voorbeelden [hier](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples).


## <a name="next-steps"></a>Volgende stappen

Ga naar de volgende pagina's voor meer informatie over Event Hubs:

* [Gebeurtenissen verzenden met behulp van Node.js](event-hubs-go-get-started-send.md)
* [Event Hubs-voorbeelden](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/)
* [Gebeurtenissen vastleggen naar Azure Storage of Data Lake Store](event-hubs-capture-overview.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Links -->
[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
