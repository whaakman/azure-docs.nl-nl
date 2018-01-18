---
title: Hoe Azure Table storage gebruiken met Node.js | Microsoft Docs
description: Sla gestructureerde gegevens op in de cloud met Azure Table Storage, een oplossing voor NoSQL-gegevensopslag.
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 0b412be8b93e1f871c09b7a4452141ac334d53ae
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Hoe Azure Table storage gebruiken met Node.js
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Overzicht
Dit onderwerp leest hoe u veelvoorkomende scenario's met de Azure Table-service in een Node.js-toepassing uitvoert.

De codevoorbeelden in dit onderwerp wordt ervan uitgegaan dat u hebt al een Node.js-toepassing. Zie voor informatie over het maken van een Node.js-toepassing in Azure, een van de volgende onderwerpen:

* [Een Node.js-web-app maken in Azure App Service](../app-service/app-service-web-get-started-nodejs.md)
* [Bouw en implementeer een Node.js-toepassing naar een Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (met behulp van Windows PowerShell)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-to-access-azure-storage"></a>Uw toepassing configureren voor toegang tot Azure Storage
Voor het gebruik van Azure Storage, moet u de Azure-opslag-SDK voor Node.js, waaronder een set van gemak bibliotheken die met de storage REST-services communiceren.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Knooppunt Package Manager (NPM) gebruiken voor het installeren van het pakket
1. Een opdrachtregelinterface gebruiken zoals **PowerShell** (Windows), **Terminal** (Mac) of **Bash** (Unix) en navigeer naar de map waar u uw toepassing hebt gemaakt.
2. Type **npm Installeer azure-opslag** in het opdrachtvenster. Uitvoer van de opdracht is vergelijkbaar met het volgende voorbeeld.

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
3. U kunt handmatig uitvoeren de **ls** opdracht om te controleren of een **knooppunt\_modules** map is gemaakt. In deze map vindt u de **azure-opslag** , dit pakket bevat de bibliotheken die u moet toegang hebben tot opslag.

### <a name="import-the-package"></a>Het pakket importeren
De volgende code toevoegen aan de bovenkant van de **server.js** bestand in uw toepassing:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Een Azure Storage-verbinding instellen
De azure-module leest de omgevingsvariabelen AZURE\_opslag\_ACCOUNT en AZURE\_opslag\_toegang\_sleutel of AZURE\_opslag\_verbinding\_tekenreeks voor de benodigde informatie om te verbinden met uw Azure storage-account. Als deze omgevingsvariabelen zijn niet ingesteld, moet u de accountgegevens opgeven bij het aanroepen van **TableService**.

## <a name="create-a-table"></a>Een tabel maken
De volgende code maakt een **TableService** object en gebruikt deze om een nieuwe tabel maken. Voeg de volgende boven aan **server.js**.

```nodejs
var tableSvc = azure.createTableService();
```

De aanroep van **createTableIfNotExists** wordt een nieuwe tabel maken met de opgegeven naam als deze niet al bestaat. Het volgende voorbeeld wordt een nieuwe tabel MijnTabel "' als deze niet al bestaat:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

De `result.created` worden `true` als een nieuwe tabel is gemaakt, en `false` als de tabel al bestaat. De `response` bevat informatie over de aanvraag.

### <a name="filters"></a>Filters
Optionele filteren bewerkingen kunnen worden toegepast op de bewerkingen die worden uitgevoerd met behulp van **TableService**. Bewerkingen voor het filteren kunt opnemen logboekregistratie, automatisch opnieuw, enzovoort. Filters zijn objecten die een methode met de handtekening implementeren:

```nodejs
function handle (requestOptions, next)
```

Hierna moet u de voorverwerking van de aanvraag-opties, de methode moet 'next', niet aanroepen voor het doorgeven van een retouraanroep met de volgende handtekening:

```nodejs
function (returnObject, finalCallback, next)
```

In deze retouraanroep en na het verwerken van de returnObject (de reactie van de aanvraag naar de server), moet de callback volgende aanroepen als deze bestaat als u wilt doorgaan met het verwerken van andere filters of gewoon finalCallback anders om te beëindigen van de aanroep van de service aanroepen.

Twee filters die Pogingslogica implementeren zijn opgenomen in de Azure SDK voor Node.js, **ExponentialRetryPolicyFilter** en **LinearRetryPolicyFilter**. De volgende code maakt een **TableService** -object dat gebruikmaakt van de **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Als u wilt een entiteit toevoegen, moet u eerst een object dat de entiteitseigenschappen van uw bepaalt maken. Alle entiteiten moeten bevatten een **PartitionKey** en **RowKey**, die de unieke id's voor de entiteit zijn.

* **PartitionKey** -bepaalt de partitie die de entiteit is opgeslagen in
* **RowKey** - unieke wijze identificeert de entiteit in de partitie

Beide **PartitionKey** en **RowKey** moet tekenreekswaarden. Zie voor meer informatie [inzicht in de tabel Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Hier volgt een voorbeeld van het definiëren van een entiteit. Houd er rekening mee dat **dueDate** is gedefinieerd als een soort **Edm.DateTime**. Op te geven is optioneel en typen zal worden afgeleid indien niet opgegeven.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> Er is ook een **tijdstempel** veld voor elke record, die door Azure wordt ingesteld wanneer een entiteit wordt ingevoegd of bijgewerkt.
>
>

U kunt ook de **entityGenerator** -entiteiten maken. Het volgende voorbeeld wordt de dezelfde taak entiteit met behulp van de **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

Voor een entiteit toevoegen aan de tabel, geeft u de entiteitsobject aan de **insertEntity** methode.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

Als de bewerking voltooid is, `result` bevat de [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) van de ingevoegde record en `response` bevat informatie over het opnieuw.

Voorbeeld van een antwoord:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Standaard **insertEntity** resulteert niet in de entiteit ingevoegd als onderdeel van de `response` informatie. Als u van plan bent andere bewerkingen uitvoert op deze entiteit of u wenst de informatie in de cache, kan het handig zijn om deze opgehaald als onderdeel van de `result`. U kunt dit doen door in te schakelen **echoContent** als volgt:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Bijwerken van een entiteit
Er zijn meerdere methoden beschikbaar voor het bijwerken van een bestaande entiteit:

* **replaceEntity** -updates van een bestaande entiteit door deze te vervangen
* **mergeEntity** -updates van een bestaande entiteit door de nieuwe eigenschapswaarden samenvoegt met de bestaande entiteit
* **insertOrReplaceEntity** -updates van een bestaande entiteit door deze te vervangen. Als er is geen entiteit bestaat, wordt een nieuwe ingevoegd
* **insertOrMergeEntity** -updates van een bestaande entiteit door nieuwe eigenschapswaarden in de bestaande samen te voegen. Als er is geen entiteit bestaat, wordt een nieuwe ingevoegd

Het volgende voorbeeld toont het bijwerken van een entiteit met **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Standaard wordt bijwerken van een entiteit niet gecontroleerd of de gegevens worden bijgewerkt eerder is gewijzigd door een ander proces. Ter ondersteuning van gelijktijdige updates:
>
> 1. Haal de ETag van het object wordt bijgewerkt. Dit wordt geretourneerd als onderdeel van de `response` voor elke entiteit-gerelateerde bewerking en kan worden opgehaald via `response['.metadata'].etag`.
> 2. Bij het uitvoeren van een updatebewerking op een entiteit toevoegen de ETag-informatie naar de nieuwe entiteit die eerder werden opgehaald. Bijvoorbeeld:
>
>       entity2 [.metadata] .etag = currentEtag;
> 3. De updatebewerking niet uitvoeren. Als de entiteit is gewijzigd sinds u de ETag-waarde opgehaald zoals een ander exemplaar van uw toepassing een `error` wordt geretourneerd met de mededeling dat de update-voorwaarde die is opgegeven in de aanvraag is niet voldaan aan.
>
>

Met **replaceEntity** en **mergeEntity**, als de entiteit die wordt bijgewerkt niet bestaat, mislukt de updatebewerking. Dus als u wilt opslaan van een entiteit ongeacht of deze al bestaat, gebruikt u **insertOrReplaceEntity** of **insertOrMergeEntity**.

De `result` voor geslaagde update operations bevat de **Etag** van de bijgewerkte entiteit.

## <a name="work-with-groups-of-entities"></a>Werken met groepen van entiteiten
Soms is het zinvol verzenden meerdere bewerkingen samen in een batch om ervoor te zorgen atomic verwerking door de server. Gebruik hiertoe die de **TableBatch** klasse voor het maken van een batch en gebruik vervolgens de **executeBatch** methode van **TableService** de batch-bewerkingen uit te voeren.

 Het volgende voorbeeld ziet u twee entiteiten in een batch te verzenden:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

Voor een geslaagde batchbewerkingen, `result` bevat informatie voor elke bewerking in de batch.

### <a name="work-with-batched-operations"></a>Werken met batchbewerkingen
Bewerkingen die zijn toegevoegd aan een batch kunnen worden gecontroleerd door de `operations` eigenschap. U kunt ook de volgende methoden gebruiken om te werken met bewerkingen:

* **Schakel** -Hiermee wist u alle bewerkingen van een batch
* **getOperations** -krijgt een bewerking van de batch
* **hasOperations** -retourneert true als de batch bewerkingen bevat
* **removeOperations** -Hiermee verwijdert u een bewerking
* **de grootte van** -retourneert het aantal bewerkingen in de batch

## <a name="retrieve-an-entity-by-key"></a>Een entiteit sleutel ophalen
Om te retourneren van een specifieke entiteit op basis van de **PartitionKey** en **RowKey**, gebruiken de **retrieveEntity** methode.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

Zodra deze bewerking voltooid is, `result` de entiteit bevat.

## <a name="query-a-set-of-entities"></a>Query uitvoeren op een verzameling entiteiten
Om te vragen een tabel, gebruikt u de **TableQuery** object voor het bouwen van een query-expressie met behulp van de volgende componenten:

* **Selecteer** -de velden moeten worden geretourneerd van de query
* **waar** -where component

  * **en** - een `and` where-voorwaarde
  * **of** - een `or` where-voorwaarde
* **Top** -het aantal items ophalen

Het volgende voorbeeld wordt een query de bovenste vijf items met een PartitionKey van 'hometasks retourneert'.

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Aangezien **Selecteer** niet wordt gebruikt, alle velden worden geretourneerd. Als u wilt uitvoeren van de query op een tabel, **queryEntities**. Het volgende voorbeeld maakt gebruik van deze query retourneren van entiteiten op "MijnTabel".

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

Als dit lukt, `result.entries` bevat een matrix van entiteiten die overeenkomen met de query. Als de query kan niet alle entiteiten retourneren `result.continuationToken` worden niet -*null* en kan worden gebruikt als de derde parameter van **queryEntities** meer resultaten ophalen. Gebruik voor de eerste query *null* voor de derde parameter.

### <a name="query-a-subset-of-entity-properties"></a>Een query uitvoeren op een subset van entiteitseigenschappen
Een query naar een tabel ophalen slechts enkele velden van een entiteit.
Dit verbruikt minder bandbreedte en kan de queryprestaties verbeteren, vooral bij grote entiteiten. Gebruik de **Selecteer** component en geef de namen van de velden moeten worden geretourneerd. De volgende query wordt bijvoorbeeld alleen de **beschrijving** en **dueDate** velden.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U kunt een entiteit met behulp van de sleutels van de partitie en rij verwijderen. In dit voorbeeld wordt de **task1** object bevat de **RowKey** en **PartitionKey** waarden van de entiteit moet worden verwijderd. Klik in het object wordt doorgegeven aan de **deleteEntity** methode.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> Overweeg het gebruik van ETags bij het verwijderen van items, om ervoor te zorgen dat het item niet is gewijzigd door een ander proces. Zie [bijwerken van een entiteit](#update-an-entity) voor informatie over het gebruik van ETags.
>
>

## <a name="delete-a-table"></a>Een tabel verwijderen
De volgende code wordt een tabel uit een opslagaccount verwijderd.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Als u niet zeker of de tabel bestaat weet, gebruikt u **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Voortzetting tokens gebruiken
Wanneer u in de tabellen voor grote hoeveelheden resultaten zoekt, zoekt u voortzetting tokens. Mogelijk zijn er grote hoeveelheden gegevens beschikbaar voor uw query die u niet beseft waarschijnlijk als u niet bouwen wilt herkennen als een vervolgtoken aanwezig is.

De resultaten object geretourneerd tijdens het opvragen van entiteiten stelt een `continuationToken` eigenschap bij dergelijke token aanwezig is. Vervolgens kunt u dit bij het uitvoeren van een query om te blijven over de partitie en tabel entiteiten verplaatst.

Wanneer een query uitvoert, kan een parameter continuationToken tussen het objectexemplaar query en de callback-functie worden opgegeven:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Als u de `continuationToken` object, vindt u eigenschappen zoals `nextPartitionKey`, `nextRowKey` en `targetLocation`, die kan worden gebruikt voor alle resultaten doorlopen.

Er is een voorbeeld van een voortzetting binnen de Azure Storage Node.js-opslagplaats op GitHub. Zoek naar `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Werken met handtekeningen voor gedeelde toegang
Shared access signatures (SAS) zijn geen veilige manier toegang te bieden gedetailleerde tot tabellen zonder dat de naam van het opslagaccount of sleutels. SAS worden vaak gebruikt voor beperkte toegang tot uw gegevens, zoals het toestaan van een mobiele app records wilt zoeken.

Een vertrouwde toepassing zoals een cloud-gebaseerde service genereert een SAS met de **generateSharedAccessSignature** van de **TableService**, en biedt dit aan een niet-vertrouwde of semi vertrouwde toepassing zoals een mobiele app. De SAS is gegenereerd met een beleid in, die beschrijft de begin- en einddatums gedurende welke de SAS geldig is, evenals het toegangsniveau verleend aan de SAS-houder.

Het volgende voorbeeld genereert een nieuw beleid voor gedeelde toegang die zorgen de SAS-houder aan query ('r') in de tabel dat ervoor en 100 minuten na het tijdstip waarop dat deze is gemaakt is verlopen.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

Houd er rekening mee dat de informatie over de host moet ook worden opgegeven als deze vereist is wanneer de SAS-houder probeert te krijgen van de tabel.

Vervolgens kunt u de clienttoepassing gebruikmaakt van de SAS met **TableServiceWithSAS** bewerkingen op de tabel uit te voeren. Het volgende voorbeeld maakt verbinding met de tabel en een query uitvoeren.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

Aangezien de SAS is gegenereerd met alleen query toegang als er een poging wilt invoegen, bijwerken of verwijderen van entiteiten zijn aangebracht, wordt een fout geretourneerd.

### <a name="access-control-lists"></a>Access Control Lists
U kunt ook een lijst met ACL (Access Control) het toegangsbeleid instellen voor een SAS. Dit is handig als u toestaan dat meerdere clients toegang tot de tabel wilt, maar bieden verschillende toegangsbeleid voor elke client.

Een ACL die is geïmplementeerd met behulp van een matrix van toegangsbeleid, met een ID die is gekoppeld aan elk beleid. Het volgende voorbeeld definieert twee beleidsregels, één voor 'gebruiker1' en één voor 'gebruiker2':

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Het volgende voorbeeld wordt de huidige ACL voor de **hometasks** tabel en voegt vervolgens het nieuwe beleid met behulp van **setTableAcl**. Met deze aanpak kunt:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Als de ACL is ingesteld, kunt u vervolgens een SAS op basis van de ID voor een beleid maken. Het volgende voorbeeld wordt een nieuwe SAS voor 'gebruiker2':

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is een gratis, zelfstandige app van Microsoft waarmee u visueel met Azure Storage-gegevens kunt werken in Windows, macOS en Linux.
* [Azure-opslag-SDK voor knooppunt](https://github.com/Azure/azure-storage-node) opslagplaats op GitHub.
* [Node.js Developer Center](/develop/nodejs/)
* [Maken en implementeren van een Node.js-toepassing naar een Azure-website](../app-service/app-service-web-get-started-nodejs.md)