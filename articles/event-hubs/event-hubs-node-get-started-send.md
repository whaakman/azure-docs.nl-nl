---
title: Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Node.js | Microsoft Docs
description: Aan de slag verzenden van gebeurtenissen naar Event Hubs met behulp van Node.js.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 09/18/2018
ms.author: shvija
ms.openlocfilehash: bb5a7b477b2d19c74cc645a15cc3d891c76f28c5
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427192"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Node.js

Azure Event Hubs is een uiterst schaalbare gebeurtenissen beheersysteem dat miljoenen verwerken kan gebeurtenissen per seconde, zodat toepassingen om te verwerken en analyseren van enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en andere systemen. Zodra de verzameld in een event hub, u kunt ontvangen en verwerken van gebeurtenissen met in-process-handlers of doorsturen naar andere systemen analytics.

Zie voor meer informatie over Event Hubs, de [Event Hubs-overzicht](event-hubs-about.md).

Deze zelfstudie wordt beschreven hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Node.js. Zie voor het ontvangen van gebeurtenissen met behulp van het pakket Node.js Event Processor Host [de betreffende ontvangen artikel](event-hubs-node-get-started-receive.md).

Code voor deze snelstartgids is beschikbaar op [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client). 

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Node.js versie 8.x en hoger. Download de nieuwste versie van de TNS van [ https://nodejs.org ](https://nodejs.org).
- Een actief Azure-account. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][] aan voordat u begint.
- Visual Studio Code (aanbevolen) of een andere IDE

## <a name="create-a-namespace-and-event-hub"></a>Een naamruimte en event hub maken
De eerste stap is het gebruik van Azure portal een Event Hubs-naamruimte maken met een event hub. Als u een bestaand account hebt, kunt u deze entiteiten maken door de instructies in [maken van een Event Hubs-naamruimte en een event hub met behulp van de Azure-portal](event-hubs-create.md).

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

Zie de volgende artikelen voor meer informatie over Event Hubs:

* [Gebeurtenissen ontvangen met Node.js](event-hubs-node-get-started-receive.md)
* [Voorbeelden op GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Links -->
[gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
