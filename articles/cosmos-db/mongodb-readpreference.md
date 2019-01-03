---
title: MongoDB-Leesvoorkeur gebruiken met de Azure Cosmos DB-API voor MongoDB
description: Informatie over het gebruik van MongoDB Leesvoorkeur met de Azure Cosmos DB-API voor MongoDB
services: cosmos-db
author: vidhoonv
ms.author: sclyon
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 02/26/2018
ms.openlocfilehash: 25c1872a677b05980899307a8de9f9b51fa749f9
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53787832"
---
# <a name="how-to-globally-distribute-reads-using-azure-cosmos-dbs-api-for-mongodb"></a>Wereldwijd distribueren wordt gelezen met behulp van Azure Cosmos DB-API voor MongoDB

Dit artikel wordt beschreven hoe u wereldwijd distribueren leesbewerkingen met [MongoDB Leesvoorkeur](https://docs.mongodb.com/manual/core/read-preference/) instellingen met behulp van Azure Cosmos DB-API voor MongoDB.

## <a name="prerequisites"></a>Vereisten 
Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Verwijzen naar dit [snelstartgids](tutorial-global-distribution-mongodb.md) voor instructies over het gebruik van Azure portal om in te stellen-up maken van een Cosmos-account met globale distributie en maak verbinding met het artikel.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Open een git-terminalvenster zoals git bash en `cd` naar een werkmap.  

Voer de volgende opdrachten uit om de voorbeeldopslagplaats te klonen. Op basis van het platform van belang zijn, gebruik een van de opslagplaatsen van het volgende voorbeeld:

1. [Voorbeeld van .NET-toepassing](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference)
2. [Voorbeeld van NodeJS-toepassing]( https://github.com/Azure-Samples/azure-cosmos-db-mongodb-node-geo-readpreference)
3. [Voorbeeld van mongoose-toepassing](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-mongoose-geo-readpreference)
4. [Voorbeeld van Java-toepassing](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)
5. [SpringBoot-voorbeeldtoepassing](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-spring)


```bash
git clone <sample repo url>
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Installeer de vereiste pakketten, afhankelijk van het platform dat wordt gebruikt, en start de toepassing. Volg het Leesmij-bestand opgenomen in de opslagplaats van de voorbeeld-toepassing voor het installeren van afhankelijkheden. Gebruik bijvoorbeeld de volgende opdrachten in de voorbeeldtoepassing NodeJS aan de vereiste pakketten installeren en start de toepassing.

```bash
cd mean
npm install
node index.js
```
De toepassing probeert verbinding te maken met een MongoDB-bron en is mislukt omdat de verbindingsreeks ongeldig is. Volg de stappen in het Leesmij-bestand om bij te werken van de verbindingsreeks `url`. Ook, werken de `readFromRegion` naar een leesregio in uw Cosmos-account. De volgende instructies zijn van de NodeJS-voorbeeld:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos account's values. 
```

Na deze stappen uitvoert, wordt de voorbeeldtoepassing wordt uitgevoerd en wordt de volgende uitvoer gegenereerd:

```
connected!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromNearestfunc query completed!
readFromRegionfunc query completed!
readDefaultfunc query completed!
readFromSecondaryfunc query completed!
```

## <a name="read-using-read-preference-mode"></a>Leesvoorkeur modus gelezen

MongoDB-protocol biedt de volgende modi Leesvoorkeur voor clients om te gebruiken:

1. PRIMAIR
2. PRIMARY_PREFERRED
3. SECUNDAIRE
4. SECONDARY_PREFERRED
5. DICHTSTBIJZIJNDE

Raadpleeg de gedetailleerde [MongoDB Leesvoorkeur gedrag](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) documentatie voor meer informatie over het gedrag van elk van deze voorkeursinstelling leveringsmethoden lezen. In Cosmos DB toegewezen primaire regio voor schrijven en secundaire is toegewezen aan de leesregio.

Op basis van algemene scenario's, wordt u aangeraden de volgende instellingen:

1. Als **lage latentie leest** zijn vereist, gebruikt u de **NEAREST** voor de voorkeursmodus lezen. Deze instelling zorgt ervoor dat de leesbewerkingen naar de dichtstbijzijnde beschikbare regio. Houd er rekening mee dat als de dichtstbijzijnde regio de schrijfregio is, klikt u vervolgens deze bewerkingen worden omgeleid naar deze regio.
2. Als **hoge beschikbaarheid en geo-distributie van leesbewerkingen** zijn vereist (latentie is geen beperking), gebruik vervolgens de **secundaire VOORKEUR** voor de voorkeursmodus lezen. Deze instelling zorgt ervoor dat de leesbewerkingen naar een beschikbare lezen regio. Als er geen leesregio beschikbaar is, worden aanvragen omgeleid naar de schrijfregio.

Het volgende fragment van de voorbeeldtoepassing laat zien hoe het configureren van de DICHTSTBIJZIJNDE Leesvoorkeur in NodeJS:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.NEAREST});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromNearestfunc query completed!");
  });
```

Op deze manier is het onderstaande codefragment laat zien hoe de Leesvoorkeur SECONDARY_PREFERRED in NodeJS configureren:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.SECONDARY_PREFERRED});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromSecondaryPreferredfunc query completed!");
  });
```

De Leesvoorkeur kan ook worden ingesteld door door te geven `readPreference` als een parameter in de connection string URI opties:

```javascript
const MongoClient = require('mongodb').MongoClient;
const assert = require('assert');

// Connection URL
const url = 'mongodb://localhost:27017?ssl=true&replicaSet=globaldb&readPreference=nearest';

// Database Name
const dbName = 'myproject';

// Use connect method to connect to the Server
MongoClient.connect(url, function(err, client) {
  console.log("Connected correctly to server");

  const db = client.db(dbName);

  client.close();
});
```

Raadpleeg de bijbehorende voorbeeld toepassing opslagplaatsen voor andere platforms, zoals [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) en [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

## <a name="read-using-tags"></a>Lezen met behulp van tags

MongoDB dit protocol kunnen naast de Leesvoorkeur-modus, het gebruik van tags om te leiden van leesbewerkingen. In Cosmos DB API voor MongoDB, de `region` tag is standaard opgenomen als onderdeel van de `isMaster` reactie:

```json
"tags": {
         "region": "West US"
      }
```

Daarom kan de MongoClient kunt gebruiken de `region` code samen met de naam van het regio om te leiden van leesbewerkingen naar specifieke regio's. Voor Cosmos-accounts, regionamen vindt u in Azure portal aan de linkerkant onder **instellingen -> replicagegevens wereldwijd**. Deze instelling is nuttig voor het bereiken van **lezen isolatie** -aanvragen in welke clienttoepassing wilt Directe leesbewerkingen in slechts een specifieke regio. Deze instelling is ideaal voor niet-productie en analyse typt u scenario's, die op de achtergrond uitgevoerd en zijn niet productie kritieke services.

Het volgende fragment van de voorbeeldtoepassing laat zien hoe het configureren van de Leesvoorkeur met tags in NodeJS:

```javascript
 var query = {};
  var readcoll = client.db('regionDB').collection('regionTest',{readPreference: new ReadPreference(ReadPreference.SECONDARY_PREFERRED, {"region": "West US"})});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromRegionfunc query completed!");
  });
```

Raadpleeg de bijbehorende voorbeeld toepassing opslagplaatsen voor andere platforms, zoals [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) en [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

In dit artikel hebt u geleerd hoe u wereldwijd distribueren leesbewerkingen Leesvoorkeur gebruiken met Azure Cosmos DB-API voor MongoDB.

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet verder om door te gaan deze app wilt gebruiken, verwijdert u alle resources die zijn gemaakt door in dit artikel in de Azure-portal met de volgende stappen uit:

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* [MongoDB-gegevens importeren in Azure Cosmos DB](mongodb-migrate.md)
* [Instellen van een globaal gedistribueerde database met Azure Cosmos DB-API voor MongoDB](tutorial-global-distribution-mongodb.md)
* [Lokaal ontwikkelen met de Azure Cosmos DB-emulator](local-emulator.md)
