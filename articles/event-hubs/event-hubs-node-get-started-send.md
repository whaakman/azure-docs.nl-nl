---
title: Gebeurtenissen verzenden met behulp van Node.js - Azure Event Hubs | Microsoft Docs
description: Dit artikel bevat een overzicht voor het maken van een Node.js-toepassing die gebeurtenissen uit Azure Event Hubs verzendt.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: ec3182d11f1b2ffa31acd05fa1f2db695f3f2cf7
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447715"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Gebeurtenissen verzenden naar Azure Event Hubs met behulp van Node.js

Azure Event Hubs is een Big Data-platform en gebeurtenis een gebeurtenisopneemservice die kan worden ontvangen en verwerken miljoenen gebeurtenissen per seconde streamen. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

Deze zelfstudie wordt beschreven hoe u gebeurtenissen naar een event hub verzendt vanuit een toepassing die is geschreven in Node.js.

> [!NOTE]
> U kunt deze snelstart als voorbeeld downloaden van de [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client), de tekenreeksen `EventHubConnectionString` en `EventHubName` vervangen door uw event hub-waarden en deze uitvoeren. U kunt ook de stappen in deze zelfstudie volgen om uw eigen oplossing te maken.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie moet aan de volgende vereisten worden voldaan:

- Node.js versie 8.x en hoger. Download de nieuwste versie van de TNS van [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (aanbevolen) of een andere IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Een Event Hubs-naamruimte en een Event Hub maken
In de eerste stap gebruikt u [Azure Portal](https://portal.azure.com) om een naamruimte van het type Event Hubs te maken en de beheerreferenties te verkrijgen die de toepassing nodig heeft om met de Event Hub te communiceren. Als u wilt een naamruimte en een event hub maken, volgt u de procedure in [in dit artikel](event-hubs-create.md), gaat u verder met de volgende stappen in deze zelfstudie.

Haal de verbindingsreeks voor de naamruimte van de Event Hub op door de instructies in het artikel te volgen: [Verbindingstekenreeks ophalen](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). U gebruikt de verbindingsreeks later in deze zelfstudie.

## <a name="clone-the-sample-git-repository"></a>Kloon de voorbeeld-Git-opslagplaats
Kloon de voorbeeld-Git-opslagplaats van [GitHub](https://github.com/Azure/azure-event-hubs-node) op uw computer. 

## <a name="install-nodejs-package"></a>Node.js-pakket installeren
Node.js-pakket voor Azure Event Hubs op uw computer installeren. 

```shell
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>De Git-opslagplaats klonen
Download of kloon de [voorbeeld](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) vanuit GitHub. 

## <a name="send-events"></a>Gebeurtenissen verzenden
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


## <a name="review-the-sample-code"></a>De voorbeeldcode controleren 
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

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u berichten verzonden naar een event hub met behulp van Node.js. Zie voor meer informatie over gebeurtenissen ontvangen van een event hub met behulp van Node.js, [ontvangen van gebeurtenissen van event hub - Node.js](event-hubs-node-get-started-receive.md)

Bekijk andere Node.js-voorbeelden voor Event Hubs op [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
