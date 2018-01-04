---
title: Hoe Queue storage gebruiken met Node.js | Microsoft Docs
description: Informatie over het gebruik van de Azure Queue-service maken en verwijderen van wachtrijen, en invoegen, ophalen en verwijderen van berichten. Voorbeelden die zijn geschreven in Node.js.
services: storage
documentationcenter: nodejs
author: tamram
manager: timlt
editor: tysonn
ms.assetid: a8a92db0-4333-43dd-a116-28b3147ea401
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 97522abd05d60eeaa2cc8dd07d3ab81d7f1d5fb9
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="how-to-use-queue-storage-from-nodejs"></a>Queue Storage gebruiken met Node.js
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Overzicht
Deze handleiding wordt getoond hoe u veelvoorkomende scenario's met behulp van de Microsoft Azure Queue-service uitvoert. De voorbeelden zijn geschreven met behulp van de Node.js-API. De scenario's worden behandeld: **invoegen**, **inspecteren**, **ophalen**, en **verwijderen** wachtrij berichten, evenals **maken en verwijderen van wachtrijen**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Een Node.js-toepassing maken
Maak een lege Node.js-toepassing. Zie voor instructies over het maken van een Node.js-toepassing [een Node.js-web-app maken in Azure App Service](../../app-service/app-service-web-get-started-nodejs.md), [bouwen en implementeren van een Node.js-toepassing naar een Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) met Windows PowerShell of [bouwen en implementeren van een Node.js-web-app in Azure met behulp van Web Matrix](https://www.microsoft.com/web/webmatrix/).

## <a name="configure-your-application-to-access-storage"></a>Uw toepassing configureren voor toegang tot opslag
Voor het gebruik van Azure-opslag, moet u de Azure-opslag-SDK voor Node.js, waaronder een set van gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Knooppunt Package Manager (NPM) gebruiken om het pakket te verkrijgen
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac), of **Bash** (Unix), gaat u naar de map waar u uw voorbeeldtoepassing gemaakt.
2. Type **npm Installeer azure-opslag** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.
 
    ```
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

3. U kunt handmatig uitvoeren de **ls** opdracht om te controleren of een **knooppunt\_modules** map is gemaakt. In deze map vindt u de **azure-opslag** , dit pakket bevat de bibliotheken die u moet toegang hebben tot opslag.

### <a name="import-the-package"></a>Het pakket importeren
In Kladblok of een andere teksteditor en voeg het volgende toe boven de **server.js** -bestand van de toepassing waarin u van plan bent opslag gebruiken:

```
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Een Azure-opslag-verbinding instellen
De azure-module leest de omgevingsvariabelen AZURE\_opslag\_ACCOUNT en AZURE\_opslag\_toegang\_sleutel of AZURE\_opslag\_verbinding\_tekenreeks voor de benodigde informatie om te verbinden met uw Azure storage-account. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **createQueueService**.

Voor een voorbeeld van de omgevingsvariabelen instellen in de [Azure Portal](https://portal.azure.com) Zie [Node.js web-app met de Azure-tabel-Service] voor een Azure-Website.

## <a name="how-to-create-a-queue"></a>Procedure: Een wachtrij maken
De volgende code maakt een **QueueService** object, waarmee u werkt met wachtrijen.

```
var queueSvc = azure.createQueueService();
```

Gebruik de **createQueueIfNotExists** methode, die de opgegeven wachtrij retourneert als dit al bestaat of maakt een nieuwe wachtrij met de opgegeven naam als deze niet al bestaat.

```
queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
  if(!error){
    // Queue created or exists
  }
});
```

Als de wachtrij is gemaakt, `result.created` is ingesteld op true. Als de wachtrij bestaat, `result.created` is ingesteld op false.

### <a name="filters"></a>Filters
Optionele filteren bewerkingen kunnen worden toegepast op de bewerkingen die worden uitgevoerd met behulp van **QueueService**. Bewerkingen voor het filteren kunt opnemen logboekregistratie, automatisch opnieuw, enzovoort. Filters zijn objecten die een methode met de handtekening implementeren:

```
function handle (requestOptions, next)
```

Hierna moet u de voorverwerking van de aanvraag-opties, moet de methode aanroepen 'volgende' doorgeven van een retouraanroep met de volgende handtekening:

```
function (returnObject, finalCallback, next)
```

In deze retouraanroep en na het verwerken van de returnObject (de reactie van de aanvraag naar de server), moet de callback volgende aanroepen als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon finalCallback anders om uiteindelijk de aanroep van de service aanroepen.

Twee filters die Pogingslogica implementeren zijn opgenomen in de Azure SDK voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende code maakt een **QueueService** -object dat gebruikmaakt van de **ExponentialRetryPolicyFilter**:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Procedure: Een bericht in een wachtrij invoegen
Voor het invoegen van een bericht in een wachtrij, gebruikt u de **createMessage** methode voor het maken van een nieuw bericht en voeg deze toe aan de wachtrij.

```
queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a>Procedure: Bekijken van het volgende bericht
U kunt het bericht vooraan in een wachtrij bekijken zonder het te verwijderen uit de wachtrij door het aanroepen van de **peekMessages** methode. Standaard **peekMessages** geeft een enkel bericht weer.

```
queueSvc.peekMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
  }
});
```

De `result` het bericht bevat.

> [!NOTE]
> Met behulp van **peekMessages** wanneer er geen berichten in de wachtrij zijn wordt geen fout geretourneerd, maar er zijn geen berichten worden geretourneerd.
> 
> 

## <a name="how-to-dequeue-the-next-message"></a>Procedure: Het volgende bericht uit de wachtrij halen
Verwerken van een bericht is een proces in twee fasen:

1. Het bericht uit de wachtrij halen.
2. Verwijder het bericht.

Gebruiken om een bericht uit de wachtrij halen, **getMessages**. Dit maakt de berichten onzichtbaar in de wachtrij, zodat er geen andere clients kunnen verwerken. Wanneer een bericht is verwerkt door uw toepassing, aanroepen **deleteMessage** te verwijderen uit de wachtrij. Het volgende voorbeeld wordt een bericht en vervolgens wordt verwijderd:

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Message text is in messages[0].messageText
    var message = result[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

> [!NOTE]
> Standaard wordt een bericht alleen verborgen gedurende 30 seconden, waarna deze zichtbaar voor andere clients is. U kunt een andere waarde opgeven met behulp van `options.visibilityTimeout` met **getMessages**.
> 
> [!NOTE]
> Met behulp van **getMessages** wanneer er geen berichten in de wachtrij zijn wordt geen fout geretourneerd, maar er zijn geen berichten worden geretourneerd.
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Procedure: De inhoud van een bericht in de wachtrij wijzigen
U kunt de inhoud van een bericht in-place in de wachtrij met behulp wijzigen **updateMessage**. De tekst van een bericht wordt bijgewerkt door het volgende voorbeeld:

```
queueSvc.getMessages('myqueue', function(error, result, response){
  if(!error){
    // Got the message
    var message = result[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Procedure: Aanvullende opties voor waarbij berichten
Er zijn twee manieren u ophalen van berichten uit een wachtrij kunt aanpassen:

* `options.numOfMessages`-Ophalen van een batch met berichten (maximaal 32).
* `options.visibilityTimeout`-Er is een time-out voor onzichtbaarheid langer of korter zijn ingesteld.

Het volgende voorbeeld wordt de **getMessages** methode 15 berichten in één aanroep ophalen. Vervolgens wordt verwerkt elke bericht met een for-lus. De time-out voor onzichtbaarheid wordt ingesteld op vijf minuten voor alle berichten die zijn geretourneerd door deze methode.

```
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
  if(!error){
    // Messages retrieved
    for(var index in result){
      // text is available in result[index].messageText
      var message = result[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

## <a name="how-to-get-the-queue-length"></a>Procedure: De lengte van de wachtrij ophalen
De **getQueueMetadata** retourneert metagegevens over de wachtrij, waaronder het geschatte aantal berichten in de wachtrij.

```
queueSvc.getQueueMetadata('myqueue', function(error, result, response){
  if(!error){
    // Queue length is available in result.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Procedure: Lijst wachtrijen
Gebruik voor het ophalen van een lijst van wachtrijen, **listQueuesSegmented**. Gebruik voor het ophalen van een lijst gefilterd op een bepaald voorvoegsel **listQueuesSegmentedWithPrefix**.

```
queueSvc.listQueuesSegmented(null, function(error, result, response){
  if(!error){
    // result.entries contains the list of queues
  }
});
```

Als alle wachtrijen kunnen niet worden geretourneerd, `result.continuationToken` kan worden gebruikt als de eerste parameter van **listQueuesSegmented** of de tweede parameter van **listQueuesSegmentedWithPrefix** meer resultaten ophalen.

## <a name="how-to-delete-a-queue"></a>Procedure: Een wachtrij verwijderen
Aanroepen voor het verwijderen van een wachtrij en alle berichten hierin de **deleteQueue** methode voor het object in de wachtrij.

```
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Schakel alle berichten uit een wachtrij zonder het te verwijderen, gebruik **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>How to: werken met handtekeningen voor gedeelde toegang
Shared Access Signatures (SAS) zijn geen veilige manier toegang te bieden gedetailleerde tot wachtrijen zonder dat de naam van het opslagaccount of sleutels. SAS worden vaak gebruikt voor beperkte toegang tot uw wachtrijen, zoals het toestaan van een mobiele app om berichten te versturen.

Een vertrouwde toepassing zoals een cloud-gebaseerde service genereert een SAS met de **generateSharedAccessSignature** van de **QueueService**, en biedt dit aan een niet-vertrouwde of semi vertrouwde toepassing. Bijvoorbeeld: een mobiele app. De SAS is gegenereerd met een beleid in, die beschrijft de begin- en einddatums gedurende welke de SAS geldig is, evenals het toegangsniveau verleend aan de SAS-houder.

Het volgende voorbeeld genereert een nieuw beleid voor gedeelde toegang die ervoor zorgen dat de SAS-houder berichten toevoegen aan de wachtrij en 100 minuten na het tijdstip waarop dat deze is gemaakt is verlopen.

```
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

Houd er rekening mee dat de informatie over de host moet ook worden opgegeven als deze vereist is wanneer de SAS-houder probeert te krijgen van de wachtrij.

Vervolgens kunt u de clienttoepassing gebruikmaakt van de SAS met **QueueServiceWithSAS** bewerkingen op basis van de wachtrij uit te voeren. Het volgende voorbeeld maakt verbinding met de wachtrij en een bericht wordt gemaakt.

```
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Aangezien de SAS is gegenereerd met de toegang voor toevoegen als een poging is gedaan te lezen, bijwerken of verwijderen van berichten, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Toegangsbeheerlijsten
U kunt ook een lijst met ACL (Access Control) het toegangsbeleid instellen voor een SAS. Dit is handig als u toestaan dat meerdere clients toegang tot de wachtrij wilt, maar bieden verschillende toegangsbeleid voor elke client.

Een ACL die is geïmplementeerd met behulp van een matrix van toegangsbeleid, met een ID die is gekoppeld aan elk beleid. Het volgende voorbeeld definieert twee beleidsregels; een voor 'gebruiker1' en één voor 'gebruiker2':

```
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

Het volgende voorbeeld wordt de huidige ACL voor **rapportberichten**, voegt u vervolgens het nieuwe beleid met behulp van **setQueueAcl**. Met deze aanpak kunt:

```
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

Als de ACL is ingesteld, kunt u vervolgens een SAS op basis van de ID voor een beleid maken. Het volgende voorbeeld wordt een nieuwe SAS voor 'gebruiker2':

```
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisprincipes van queue storage hebt geleerd, volgt u deze koppelingen voor meer informatie over complexere opslagtaken.

* Ga naar de [Blog Azure Storage-Team][Azure Storage Team Blog].
* Ga naar de [Azure-opslag-SDK voor knooppunt] [ Azure Storage SDK for Node] opslagplaats op GitHub.



[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx

[Azure Portal]: https://portal.azure.com

[Een Node.js-web-app maken in Azure App Service](../../app-service/app-service-web-get-started-nodejs.md)

[Een Node.js-toepassing maken en implementeren in een Azure Cloud Service](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)

[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/
