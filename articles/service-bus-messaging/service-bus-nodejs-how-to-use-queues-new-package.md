---
title: Over het gebruik van Azure Service Bus-wachtrijen in Node.js - azure/service-bus | Microsoft Docs
description: Informatie over het gebruik van Service Bus-wachtrijen in Azure vanuit een Node.js-app.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 7aacefde9c037fcce64d9256e35082eb04e0a2f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65988352"
---
# <a name="how-to-use-service-bus-queues-with-nodejs-and-the-azureservice-bus-package"></a>Het gebruik van Service Bus-wachtrijen met Node.js en de azure-servicebus-pakket
> [!div class="op_multi_selector" title1="Programmeertaal" title2="Node.js pacakge"]
> - [(Node.js | azure-sb)](service-bus-nodejs-how-to-use-queues.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-queues-new-package.md)

In deze zelfstudie leert u hoe u een Nodejs-programma voor het verzenden van berichten naar en ontvangen van berichten van een Service Bus-wachtrij met de nieuwe schrijven [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pakket. Dit pakket maakt gebruik van de snellere [AMQP 1.0-protocol](service-bus-amqp-overview.md) terwijl de oudere [azure sb](https://www.npmjs.com/package/azure-sb) pakket gebruikt [runtime-API's van Service Bus REST](/rest/api/servicebus/service-bus-runtime-rest). De voorbeelden zijn geschreven in JavaScript.

## <a name="prerequisites"></a>Vereisten
- Een Azure-abonnement. U hebt een Azure-account nodig om deze zelfstudie te voltooien. U kunt uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) zich ook aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Als u een wachtrij om te werken met geen hebt, voert u de stappen de [gebruik Azure portal voor het maken van een Service Bus-wachtrij](service-bus-quickstart-portal.md) artikel om een wachtrij te maken. Noteer de verbindingsreeks voor uw Service Bus-exemplaar en de naam van de wachtrij die u hebt gemaakt. We gebruiken deze waarden in de voorbeelden.

> [!NOTE]
> - In deze zelfstudie werkt met voorbeelden die u kunt kopiëren en uitgevoerd met behulp van [Nodejs](https://nodejs.org/). Zie voor instructies over het maken van een Node.js-toepassing [maken en implementeren van een Node.js-toepassing naar een Azure-Website](../app-service/app-service-web-get-started-nodejs.md), of [Node.js-cloudservice met behulp van Windows PowerShell](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - De nieuwe [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pakket biedt geen ondersteuning voor het maken van wachtrijen nog. Gebruik de [ @azure/arm-servicebus ](https://www.npmjs.com/package/@azure/arm-servicebus) pakket als u wilt deze via een programma te maken.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Node Package Manager (NPM) gebruiken om het pakket te installeren
Voor het installeren van de npm-pakket voor Service Bus, open een opdrachtprompt met `npm` wijzigen in het pad, de map naar de map waar u uw voorbeelden hebt en voer deze opdracht.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij
Interactie met een Service Bus queue begint met het instantiëren van de [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) klasse en het gebruik van het exemplaar maken van de [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) klasse. Zodra u de wachtrij-client hebt, u kunt maken van een afzender en gebruik een [verzenden](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) of [sendBatch](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) methode voor het verzenden van berichten.

1. Open uw favoriete editor, zoals [Visual Studio Code](https://code.visualstudio.com/)
2. Maak een bestand met de naam `send.js` en plak de onderstaande code naartoe. Deze code wordt 10 berichten verzenden naar de wachtrij.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const sender = queueClient.createSender();
      
      try {
        for (let i = 0; i < 10; i++) {
          const message= {
            body: `Hello world! ${i}`,
            label: `test`,
            userProperties: {
                myCustomPropertyName: `my custom property value ${i}`
           }
          };
          console.log(`Sending message: ${message.body}`);
          await sender.send(message);
        }
        
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Voer de verbindingsreeks en de naam van uw wachtrij in de bovenstaande code.
4. Voer de opdracht `node send.js` in een opdrachtprompt voor het uitvoeren van dit bestand.

Gefeliciteerd! U kunt alleen berichten verzonden naar een Service Bus-wachtrij.

Berichten zijn sommige standaardeigenschappen zoals `label` en `messageId` die u kunt bij het verzenden van instellen. Als u alle aangepaste eigenschappen instellen wilt, gebruikt u de `userProperties`, dit is een json-object die sleutel / waarde-paren van uw aangepaste gegevens kan bevatten.

Service Bus-wachtrijen ondersteunen een maximale berichtgrootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). Er is geen limiet voor het aantal berichten in een wachtrij, maar er is een limiet voor de totale grootte van de berichten door een wachtrij. De grootte van de wachtrij wordt gedefinieerd tijdens het aanmaken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota](service-bus-quotas.md).

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangen van een wachtrij
Interactie met een Service Bus queue begint met het instantiëren van de [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) klasse en het gebruik van het exemplaar maken van de [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) klasse. Zodra u de wachtrij-client hebt, kunt u een ontvanger maken en gebruiken [receiveMessages](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) of [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) methode om berichten te ontvangen.

1. Open uw favoriete editor, zoals [Visual Studio Code](https://code.visualstudio.com/)
2. Maak een bestand met de naam `recieve.js` en plak de onderstaande code naartoe. Deze code probeert 10 om berichten te ontvangen uit de wachtrij. Het werkelijke aantal u ontvangt afhankelijk van het aantal berichten in de wachtrij en netwerk latentie.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const receiver = queueClient.createReceiver(ReceiveMode.ReceiveAndDelete);
      try {
        const messages = await receiver.receiveMessages(10)
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
         
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Voer de verbindingsreeks en de naam van uw wachtrij in de bovenstaande code.
4. Voer de opdracht `node receiveMessages.js` in een opdrachtprompt voor het uitvoeren van dit bestand.

Gefeliciteerd! U kunt alleen berichten ontvangen van een Service Bus-wachtrij.

De [createReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient#createreceiver-receivemode-) methode gebruikt een `ReceiveMode` is een enum-waarde met waarden [ReceiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) en [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations). Houd er rekening mee te [vereffenen uw berichten](message-transfers-locks-settlement.md#settling-receive-operations) als u de `PeekLock` modus met behulp van `complete()`, `abandon()`, `defer()`, of `deadletter()` methoden voor het bericht.

> [!NOTE]
> U kunt Service Bus-resources beheren [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/). De Service Bus Explorer kunnen gebruikers verbinding maken met een Service Bus-naamruimte en berichtentiteiten op een eenvoudige manier te beheren. Het hulpprogramma biedt geavanceerde functies zoals import/export-functionaliteit of de mogelijkheid om te testen, onderwerp, wachtrijen, abonnementen, relayservices, notification hubs en gebeurtenissen hubs. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie.
- [Wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md)
- Bekijk andere [Nodejs-voorbeelden voor Service Bus op GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript)
- [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/)

