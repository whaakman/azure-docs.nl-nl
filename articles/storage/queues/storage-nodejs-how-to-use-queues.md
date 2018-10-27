---
title: Hoe u Queue storage gebruiken met Node.js | Microsoft Docs
description: Informatie over het gebruik van de Azure Queue-service maken en verwijderen van wachtrijen, en invoegen, ophalen en verwijderen van berichten. Voorbeelden die zijn geschreven in Node.js.
services: storage
author: tamram
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.component: queues
ms.openlocfilehash: 38da370e8e3cd81e209d0fd592d6b2afa8c82e44
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138500"
---
# <a name="how-to-use-queue-storage-from-nodejs"></a>Queue Storage gebruiken met Node.js
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt beschreven hoe u algemene scenario's met behulp van de Microsoft Azure Queue-service uitvoert. De voorbeelden zijn geschreven met behulp van de Node.js-API. De behandelde scenario's zijn **invoegen**, **inspecteren**, **aan**, en **verwijderen** berichten in de wachtrij, evenals  **het maken en verwijderen van wachtrijen**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken
Maak een lege Node.js-toepassing. Zie voor instructies over het maken van een Node.js-toepassing [een Node.js-web-app maken in Azure App Service](../../app-service/app-service-web-get-started-nodejs.md), [bouwen en implementeren van een Node.js-toepassing in een Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) met Windows PowerShell of [ Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor opslag met leestoegang
Voor het gebruik van Azure storage, moet u de Azure Storage SDK voor Node.js, waaronder een set met gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken om het pakket te verkrijgen
1. Gebruik een opdrachtregelinterface als **PowerShell** (Windows), **Terminal** (Mac), of **Bash-** (Unix), gaat u naar de map waarin u de voorbeeldtoepassing hebt gemaakt.
2. Typ **npm install azure-storage** in het opdrachtvenster. De uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.
 
    ```bash
    azure-storage@0.5.0 node_modules\azure-storage
    +-- extend@1.2.1
    +-- xmlbuilder@0.4.3
    +-- mime@1.2.11
    +-- node-uuid@1.4.3
    +-- validator@3.22.2
    +-- underscore@1.4.4
    +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
    +-- xml2js@0.2.7 (sax@0.5.2)
    +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    ```

3. U kunt handmatig uitvoeren de **ls** opdracht uit om te controleren of een **knooppunt\_modules** map is gemaakt. In deze map vindt u het **azure-storage**-pakket. Dit pakket bevat de bibliotheken die u nodig hebt om toegang te krijgen tot opslag.

### <a name="import-the-package"></a>Het pakket importeren
In Kladblok of een andere teksteditor, Voeg het volgende toe boven de **server.js** -bestand van de toepassing waar u van plan bent opslag gebruiken:

```javascript
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen
De azure-module leest de omgevingsvariabelen AZURE\_opslag\_-ACCOUNT en AZURE\_opslag\_toegang\_KEY- of AZURE\_opslag\_verbinding\_ De tekenreeks voor de benodigde informatie om te verbinden met uw Azure storage-account. Als u deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **createQueueService**.

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken
De volgende code maakt een **QueueService** object, dat u kunt werken met wachtrijen.

```javascript
var queueSvc = azure.createQueueService();
```

Gebruik de **createQueueIfNotExists** methode, waarbij de opgegeven wachtrij geretourneerd als deze al bestaat of een nieuwe wachtrij met de opgegeven naam gemaakt als deze niet al bestaat.

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

Als de wachtrij is gemaakt, `result.created` is ingesteld op true. Als de wachtrij bestaat, `result.created` is ingesteld op false.

### <a name="filters"></a>Filters
Optionele filters gebruiken om bewerkingen kunnen worden toegepast op bewerkingen die worden uitgevoerd met behulp van **QueueService**. Filteren van bewerkingen kunt opnemen logboekregistratie, automatisch opnieuw wordt geprobeerd, enzovoort. Filters zijn objecten die een methode implementeren met de handtekening:

```javascript
function handle (requestOptions, next)
```

Hierna de voorverwerking van de Aanvraagopties, moet de methode 'volgende' doorgeven van een retouraanroep met de handtekening van de volgende aanroepen:

```javascript
function (returnObject, finalCallback, next)
```

In deze retouraanroep en na het verwerken van de returnObject (het antwoord van de aanvraag naar de server), moet de callback de volgende aanroepen als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon finalCallback anders als u wilt beëindigen van de service aanroepen aanroep.

Twee filters die logica implementeren voor nieuwe pogingen zijn opgenomen in de Azure SDK voor Node.js: **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende code maakt een **QueueService** object die gebruikmaakt van de **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Een bericht in een wachtrij invoegen
Voor het invoegen van een bericht in een wachtrij wordt geplaatst, gebruikt u de **createMessage** methode voor het maken van een nieuw bericht en toe te voegen aan de wachtrij.

```javascript
queueSvc.createMessage('myqueue', "Hello world!", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Het volgende bericht bekijken
U kunt het bericht vooraan in een wachtrij bekijken zonder deze te verwijderen uit de wachtrij door het aanroepen van de **peekMessages** methode. Standaard **peekMessages** geeft een enkel bericht weer.

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

De `result` het bericht bevat.

> [!NOTE]
> Met behulp van **peekMessages** wanneer er geen berichten in de wachtrij wordt geen fout geretourneerd, maar geen berichten worden geretourneerd.
> 
> 

## <a name="how-to-dequeue-the-next-message"></a>Procedure: Het volgende bericht uit de wachtrij verwijderen
Verwerken van een bericht is een proces in twee fasen:

1. Het bericht uit de wachtrij verwijderen.
2. Het bericht verwijderen.

Als u wilt een bericht uit de wachtrij verwijderen, gebruikt u **getMessages**. Hierdoor wordt de berichten in de wachtrij onzichtbaar, zodat ze kunnen worden verwerkt door geen andere clients. Wanneer uw toepassing een bericht is verwerkt, aanroepen **deleteMessage** te verwijderen uit de wachtrij. Het volgende voorbeeld wordt een bericht en vervolgens wordt verwijderd:

```javascript
queueSvc.getMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
    var message = results[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

> [!NOTE]
> Een bericht wordt standaard alleen verborgen gedurende 30 seconden, waarna deze zichtbaar voor andere clients is. U kunt een andere waarde opgeven met behulp van `options.visibilityTimeout` met **getMessages**.
> 
> [!NOTE]
> Met behulp van **getMessages** wanneer er geen berichten in de wachtrij wordt geen fout geretourneerd, maar geen berichten worden geretourneerd.
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in-place in de wachtrij met wijzigen **updateMessage**. De tekst van een bericht wordt bijgewerkt door het volgende voorbeeld:

```javascript
queueSvc.getMessages('myqueue', function(error, getResults, getResponse){
  if(!error){
    // Got the message
    var message = getResults[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, updateResults, updateResponse){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Het: Aanvullende opties voor eruit worden verwijderd voor berichten
Er zijn twee manieren u bericht dat wordt opgehaald uit een wachtrij kunt aanpassen:

* `options.numOfMessages` -Ophalen van een berichtenbatch (maximaal 32.)
* `options.visibilityTimeout` -Er is een time-out voor onzichtbaarheid langer of korter zijn ingesteld.

Het volgende voorbeeld wordt de **getMessages** methode voor het ophalen van 15 berichten in één aanroep. Vervolgens wordt verwerkt elke bericht met een for-lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor alle berichten die door deze methode wordt geretourneerd.

```javascript
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, results, getResponse){
  if(!error){
    // Messages retrieved
    for(var index in result){
      // text is available in result[index].messageText
      var message = results[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, deleteResponse){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

## <a name="how-to-get-the-queue-length"></a>Procedure: Lengte van de wachtrij ophalen
De **getQueueMetadata** retourneert metagegevens over de wachtrij, met inbegrip van het geschatte aantal berichten in de wachtrij.

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Procedure: Lijst met wachtrijen
U haalt een lijst met wachtrijen met **listQueuesSegmented**. Gebruiken om op te halen een lijst gefilterd op een bepaald voorvoegsel, **listQueuesSegmentedWithPrefix**.

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

Als alle wachtrijen kunnen niet worden geretourneerd, `result.continuationToken` kan worden gebruikt als de eerste parameter van **listQueuesSegmented** of de tweede parameter van **listQueuesSegmentedWithPrefix** om meer resultaten te halen.

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen
Als wilt verwijderen van een wachtrij en alle berichten die erin zijn opgenomen, roept de **deleteQueue** methode voor het object in de wachtrij.

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Schakel alle berichten uit een wachtrij zonder deze te verwijderen, gebruikt u **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Hoe: werken met handtekeningen voor gedeelde toegang
Shared Access Signatures (SAS) vormen een veilige manier voor gedetailleerde toegang tot wachtrijen zonder op te geven uw storage-accountnaam of sleutels. Beveiligingskoppelingen worden vaak gebruikt voor beperkte toegang tot uw wachtrijen, zoals het toestaan van een mobiele app om berichten te versturen.

Een betrouwbare toepassing, zoals een cloud-gebaseerde service genereert een SAS met de **generateSharedAccessSignature** van de **QueueService**, en biedt dit aan een niet-vertrouwde of semi vertrouwde-toepassing. Bijvoorbeeld: een mobiele app. De SAS wordt gegenereerd op basis van beleid, waarin de begin- en einddatum wordt vermeld voor de geldigheid van de SAS, evenals het toegangsniveau verleend aan de SAS-houder.

Het volgende voorbeeld genereert een nieuw beleid voor gedeelde toegang die ervoor zorgen dat de SAS-houder berichten toevoegen aan de wachtrij en verloopt 100 minuten na het moment dat deze is gemaakt.

```javascript
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};

var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
var host = queueSvc.host;
```

Houd er rekening mee dat de informatie over de host moet ook worden opgegeven als deze vereist is wanneer de SAS-houder probeert te krijgen tot de wachtrij.

Vervolgens kunt u de clienttoepassing maakt gebruik van de SAS met **QueueServiceWithSAS** bewerkingen op basis van de wachtrij uit te voeren. Het volgende voorbeeld maakt verbinding met de wachtrij en een bericht wordt gemaakt.

```javascript
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Omdat de SAS is gegenereerd met toegang tot het toevoegen, als er een poging om te lezen, bijwerken of verwijderen van berichten zijn aangebracht, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Toegangsbeheerlijsten
U kunt ook een ACL (Access Control List) gebruiken om het toegangsbeleid in te stellen voor een SAS. Dit is handig als u toestaan dat meerdere clients toegang krijgen tot de wachtrij wilt, maar bieden verschillende toegangsbeleid voor elke client.

Een ACL wordt geïmplementeerd met behulp van een matrix van toegangsbeleidregels, met een id die is gekoppeld aan elk beleid. Het volgende voorbeeld definieert twee beleidsregels; een voor 'gebruiker1' en een voor 'gebruiker2':

```javascript
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Het volgende voorbeeld wordt de huidige ACL voor **myqueue**, voegt u vervolgens de nieuwe beleidsregels met behulp van **setQueueAcl**. Deze aanpak biedt u de volgende mogelijkheid:

```javascript
var extend = require('extend');
queueSvc.getQueueAcl('myqueue', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Als de ACL is ingesteld, kunt u vervolgens een SAS op basis van de ID voor een beleid maken. In het volgende voorbeeld wordt een nieuwe SAS voor 'gebruiker2' gemaakt:

```javascript
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken.

* Ga naar de [Blog Azure Storage-Team][Azure Storage Team Blog].
* Ga naar de [Azure Storage SDK voor Node] [ Azure Storage SDK for Node] -bibliotheek op GitHub.



[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx

[Azure Portal]: https://portal.azure.com

[Een Node.js-web-app maken in Azure App Service](../../app-service/app-service-web-get-started-nodejs.md)

[Een Node.js-toepassing maken en implementeren in een Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)

[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/
