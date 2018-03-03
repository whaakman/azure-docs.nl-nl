---
title: Met behulp van MongoDB lezen voorkeur met de Azure Cosmos DB MongoDB API | Microsoft Docs
description: Informatie over het gebruik van MongoDB lezen voorkeur met de Azure-API voor MongoDB Cosmos DB
services: cosmos-db
documentationcenter: 
author: vidhoonv
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: 
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 02/26/2018
ms.author: viviswan
ms.openlocfilehash: b28285695f52d6dbcc33d9fb0efe06b43cdf1cab
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="how-to-globally-distribute-reads-using-read-preference-with-the-azure-cosmos-db-mongodb-api"></a>Het distribueren van globaal leest lezen voorkeur met met de Azure-API voor MongoDB Cosmos DB 

In dit artikel laat zien hoe globaal distribueren met behulp van leesbewerkingen [MongoDB lezen voorkeur](https://docs.mongodb.com/manual/core/read-preference/) instellingen met Azure Cosmos DB MongoDB-API. 

## <a name="prerequisites"></a>Vereisten 
Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

Neem deze [Quick Start](tutorial-global-distribution-mongodb.md) artikel voor instructies over het gebruik van de Azure-portal voor Azure DB die Cosmos-account met globale verdeling instellen en maak verbinding met MongoDB-API.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Open een git-terminalvenster zoals git bash en `cd` naar een werkmap.  

Voer de volgende opdrachten uit om de voorbeeldopslagplaats te klonen. Een van de volgende voorbeeld-opslagplaatsen op basis van uw platform voor het van belang zijn, gebruiken:

1. [.NET-voorbeeldtoepassing](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference)
2. [De voorbeeldtoepassing NodeJS]( https://github.com/Azure-Samples/azure-cosmos-db-mongodb-node-geo-readpreference)
3. [De voorbeeldtoepassing mongoose](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-mongoose-geo-readpreference)
4. [Voorbeeld van Java-toepassing](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference)
5. [De voorbeeldtoepassing SpringBoot](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-spring)


```bash
git clone <sample repo url>
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Afhankelijk van het platform dat wordt gebruikt, installeer de vereiste pakketten en de toepassing niet starten. Volg voor de installatie van afhankelijkheden, het Leesmij-bestand opgenomen in de opslagplaats van de toepassing voorbeeld. Gebruik bijvoorbeeld de volgende opdrachten in de voorbeeldtoepassing NodeJS voor het installeren van de vereiste pakketten en de toepassing niet starten.

```bash
cd mean
npm install
node index.js
```
De toepassing probeert verbinding maken met een bron MongoDB en mislukt, omdat de verbindingsreeks ongeldig is. Volg de stappen in de Leesmij-bestand de verbindingsreeks bijwerken `url`. Bovendien werken de `readFromRegion` tot een regio lezen in uw Azure DB die Cosmos-account. De volgende instructies zijn van de steekproef NodeJS:

```
* Next, substitute the `url`, `readFromRegion` in App.Config with your Cosmos DB account's values. 
```

Na deze stappen uitvoert, wordt de voorbeeldtoepassing wordt uitgevoerd en wordt de volgende uitvoer:

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

## <a name="read-using-read-preference-mode"></a>Voorkeur voor lezen-modus gelezen

MongoDB biedt de volgende lezen voorkeur modi voor clients gebruiken:

1. PRIMAIR
2. PRIMARY_PREFERRED
3. SECUNDAIR
4. SECONDARY_PREFERRED
5. DICHTSTBIJZIJNDE

Raadpleeg de gedetailleerde [MongoDB lezen voorkeur gedrag](https://docs.mongodb.com/manual/core/read-preference-mechanics/#replica-set-read-preference-behavior) documentatie voor meer informatie over het gedrag van elk van deze voorkeur modi lezen. In Azure Cosmos DB wijst primaire regio schrijven en secundaire is toegewezen aan lezen regio.

Op basis van veelvoorkomende scenario's, wordt u aangeraden de volgende instellingen:

1. Als **lage latentie leest** zijn vereist, gebruiken de **NEAREST** voorkeursmodus lezen. Deze instelling zorgt ervoor dat de leesbewerkingen naar de dichtstbijzijnde beschikbare regio. Houd er rekening mee dat als de dichtstbijzijnde regio de regio schrijven is, klikt u vervolgens deze bewerkingen worden omgeleid naar deze regio.
2. Als **hoge beschikbaarheid en geo-distributie van leesbewerkingen** zijn vereist (latentie is niet een beperking), gebruik vervolgens de **secundaire VOORKEUR** voorkeursmodus lezen. Deze instelling laat leesbewerkingen een beschikbare lezen regio. Als er geen regio lezen beschikbaar is, worden aanvragen omgeleid naar de regio schrijven.

Het volgende fragment van de voorbeeldtoepassing ziet u hoe DICHTSTBIJZIJNDE lezen voorkeur in NodeJS configureren:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.NEAREST});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromNearestfunc query completed!");
  });
```

Op deze manier ziet het onderstaande codefragment u hoe u de voorkeur voor het lezen van SECONDARY_PREFERRED in NodeJS configureert:

```javascript
  var query = {};
  var readcoll = client.db('regionDB').collection('regionTest', {readPreference: ReadPreference.SECONDARY_PREFERRED});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromSecondaryPreferredfunc query completed!");
  });
```

Raadpleeg de bijbehorende voorbeeld toepassing repo's voor andere platforms, zoals [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) en [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

## <a name="read-using-tags"></a>Lezen met tags

Naast de modus lezen voorkeur staat MongoDB het gebruik van codes voor directe leesbewerkingen. In Azure Cosmos DB voor MongoDB-API de `region` tag is standaard opgenomen als onderdeel van de `isMaster` antwoord:

```json
"tags": {
         "region": "West US"
      }
```

Daarom MongoClient kunt gebruiken de `region` samen met de naam van het gebied leesbewerkingen tot specifieke regio's direct tag. Voor Azure DB die Cosmos-accounts, regionamen vindt u in Azure-portal aan de linkerkant onder **instellingen -> replicagegevens globaal**. Deze instelling is nuttig voor het bereiken van **lezen isolatie** -aanvragen in welke clienttoepassing wilt Directe leesbewerkingen met alleen een specifieke regio. Deze instelling is ideaal voor niet-productie/analytics typt u scenario's, die op de achtergrond uitgevoerd en zijn niet productie essentiële services.

Het volgende fragment van de voorbeeldtoepassing ziet u de voorkeur lezen met labels in NodeJS configureren:

```javascript
 var query = {};
  var readcoll = client.db('regionDB').collection('regionTest',{readPreference: new ReadPreference(ReadPreference.SECONDARY_PREFERRED, {"region": "West US"})});
  readcoll.find(query).toArray(function(err, data) {
    assert.equal(null, err);
    console.log("readFromRegionfunc query completed!");
  });
```

Raadpleeg de bijbehorende voorbeeld toepassing repo's voor andere platforms, zoals [.NET](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-geo-readpreference) en [Java](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-geo-readpreference).

In dit artikel hebt u geleerd hoe globaal distribueren leesbewerkingen lezen voorkeur met op Azure Cosmos DB MongoDB-API.

## <a name="clean-up-resources"></a>Resources opschonen

Als u niet gaat u deze app blijven gebruiken, verwijdert u alle resources die zijn gemaakt in dit artikel wordt beschreven in de Azure portal met de volgende stappen:

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

* [MongoDB-gegevens importeren in Azure Cosmos DB](mongodb-migrate.md)
* [Een globaal gerepliceerde Azure DB die Cosmos-account instellen en deze gebruiken met MongoDB-API](tutorial-global-distribution-mongodb.md)
* [Lokaal ontwikkelen met de emulator](local-emulator.md)
