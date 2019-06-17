---
title: Over het gebruik van Azure Service Bus-onderwerpen en abonnementen met behulp van Node.js | Microsoft Docs
description: Meer informatie over het gebruik van Service Bus-onderwerpen en abonnementen in Azure vanuit een Node.js-app.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: 7686014adb989494e6df277de4137b76c3125696
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65992137"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azureservice-bus-package"></a>Over het gebruik van Service Bus-onderwerpen en abonnementen met Node.js en de azure-servicebus-pakket
> [!div class="op_multi_selector" title1="Programmeertaal" title2="Node.js pacakge"]
> - [(Node.js | azure-sb)](service-bus-nodejs-how-to-use-topics-subscriptions.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-topics-subscriptions-new-package.md)

In deze zelfstudie leert u hoe u een Node.js-programma voor het verzenden van berichten naar een Service Bus-onderwerp en ontvangen van berichten van een Service Bus-abonnement met behulp van de nieuwe schrijven [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pakket. Dit pakket maakt gebruik van de snellere [AMQP 1.0-protocol](service-bus-amqp-overview.md) terwijl de oudere [azure sb](https://www.npmjs.com/package/azure-sb) pakket gebruikt [runtime-API's van Service Bus REST](/rest/api/servicebus/service-bus-runtime-rest). De voorbeelden zijn geschreven in JavaScript.

## <a name="prerequisites"></a>Vereisten
- Een Azure-abonnement. U hebt een Azure-account nodig om deze zelfstudie te voltooien. U kunt uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) zich ook aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Als u een onderwerp en een abonnement om te werken met geen hebt, voert u de stappen de [gebruik Azure portal voor het maken van een Service Bus-onderwerpen en abonnementen](service-bus-quickstart-topics-subscriptions-portal.md) artikel om ze te maken. Noteer de verbindingsreeks voor uw Service Bus-exemplaar en de namen van het onderwerp en abonnement die u hebt gemaakt. We gebruiken deze waarden in de voorbeelden.

> [!NOTE]
> - In deze zelfstudie werkt met voorbeelden die u kunt kopiëren en uitgevoerd met behulp van [Nodejs](https://nodejs.org/). Zie voor instructies over het maken van een Node.js-toepassing [maken en implementeren van een Node.js-toepassing naar een Azure-Website](../app-service/app-service-web-get-started-nodejs.md), of [Node.js-Cloudservice met behulp van Windows PowerShell](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - De nieuwe [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pakket biedt geen ondersteuning voor het maken van topcis en -abonnementen nog. Gebruik de [ @azure/arm-servicebus ](https://www.npmjs.com/package/@azure/arm-servicebus) pakket als u wilt deze via een programma te maken.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Node Package Manager (NPM) gebruiken om het pakket te installeren
Voor het installeren van de npm-pakket voor Service Bus, open een opdrachtprompt met `npm` wijzigen in het pad, de map naar de map waar u uw voorbeelden hebt en voer deze opdracht.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp
Interactie met een Service Bus onderwerp begint met het instantiëren van de [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) klasse en het gebruik van het exemplaar maken van de [TopicClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/topicclient) klasse. Zodra u de client onderwerp hebt, kunt u een afzender maken en gebruiken [verzenden](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) of [sendBatch](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) methode voor het verzenden van berichten.

1. Open uw favoriete editor, zoals [Visual Studio Code](https://code.visualstudio.com/)
2. Maak een bestand met de naam `send.js` en plak de onderstaande code naartoe. Deze code wordt 10 berichten verzenden naar het onderwerp.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const topicClient = sbClient.createTopicClient(topicName);
      const sender = topicClient.createSender();
      
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

            await topicClient.close();
          } finally {
            await sbClient.close();
          }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Voer de verbindingsreeks en de naam van uw onderwerp in de bovenstaande code.
4. Voer de opdracht `node send.js` in een opdrachtprompt voor het uitvoeren van dit bestand. 

Gefeliciteerd! U kunt alleen berichten verzonden naar een Service Bus-wachtrij.

Berichten zijn sommige standaardeigenschappen zoals `label` en `messageId` die u kunt bij het verzenden van instellen. Als u alle aangepaste eigenschappen instellen wilt, gebruikt u de `userProperties`, dit is een json-object die sleutel / waarde-paren van uw aangepaste gegevens kan bevatten.

Service Bus-onderwerpen ondersteunen een maximale grootte van 256 kB in de [Standard-laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). Er is geen limiet voor het aantal berichten in een onderwerp, maar er is een limiet voor de totale grootte van de berichten op een onderwerp. De grootte van het onderwerp wordt gedefinieerd tijdens het maken, met een bovengrens van 5 GB. Zie voor meer informatie over quota [Service Bus-quota](service-bus-quotas.md).

## <a name="receive-messages-from-a-subscription"></a>Berichten ontvangen van een abonnement
Interactie met een Service Bus abonnement begint met het instantiëren van de [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) klasse en het gebruik van het exemplaar maken van de [SubscriptionClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient) klasse. Zodra u de abonnementsclient hebt, kunt u een ontvanger maken en gebruiken [receiveMessages](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) of [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) methode om berichten te ontvangen.

1. Open uw favoriete editor, zoals [Visual Studio Code](https://code.visualstudio.com/)
2. Maak een bestand met de naam `recieve.js` en plak de onderstaande code naartoe. Deze code probeert te 10 berichten ontvangen van uw abonnement. Het werkelijke aantal dat u ontvangt, is afhankelijk van het aantal berichten in het abonnement en de netwerklatentie.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    const subscriptionName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const subscriptionClient = sbClient.createSubscriptionClient(topicName, subscriptionName);
      const receiver = subscriptionClient.createReceiver(ReceiveMode.ReceiveAndDelete);
      
      try {
        const messages = await receiver.receiveMessages(10);
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
        
        await subscriptionClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Voer de verbindingsreeks en de namen van uw onderwerp en een abonnement in de bovenstaande code.
4. Voer de opdracht `node receiveMessages.js` in een opdrachtprompt voor het uitvoeren van dit bestand.

Gefeliciteerd! U kunt alleen berichten ontvangen van een Service Bus-abonnement.

De [createReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient#createreceiver-receivemode-) methode gebruikt een `ReceiveMode` is een enum-waarde met waarden [ReceiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) en [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations). Houd er rekening mee te [vereffenen uw berichten](message-transfers-locks-settlement.md#settling-receive-operations) als u de `PeekLock` modus met behulp van `complete()`, `abandon()`, `defer()`, of `deadletter()` methoden voor het bericht.

## <a name="subscription-filters-and-actions"></a>Abonnementfilters en acties
Service Bus ondersteunt [filters en acties op abonnementen](topic-filters.md), waarmee u voor het filteren van de binnenkomende berichten naar een abonnement en hun eigenschappen te bewerken.

Zodra u een exemplaar van hebt een `SubscriptionClient` kunt u de onderstaande methoden erop om op te halen, toevoegen en verwijderen van de regels voor het abonnement voor het beheren van de filters en acties.

- getRules
- addRule
- removeRule

Elk abonnement heeft een standaardregel waarmee de waarde true filter gebruikt waarmee alle binnenkomende berichten. Wanneer u een nieuwe regel toevoegt, moet u het standaardfilter te verwijderen in volgorde voor het filter in de nieuwe regel te werken. Als een abonnement geen regels heeft, wordt er geen berichten ontvangt.

> [!NOTE]
> U kunt Service Bus-resources beheren [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/). De Service Bus Explorer kunnen gebruikers verbinding maken met een Service Bus-naamruimte en berichtentiteiten op een eenvoudige manier te beheren. Het hulpprogramma biedt geavanceerde functies zoals import/export-functionaliteit of de mogelijkheid om te testen, onderwerp, wachtrijen, abonnementen, relayservices, notification hubs en gebeurtenissen hubs. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie.

- [Wachtrijen, onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md)
- Bekijk andere [Nodejs-voorbeelden voor Service Bus op GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript)
- [Node.js Developer Center](https://azure.microsoft.com/develop/nodejs/)


