---
title: "Zelfstudie Azure Cosmos DB: gebruik de Gremlin-console om objecten te maken, query’s uit te voeren en door grafen te gaan| Microsoft Docs"
description: Een Azure Cosmos DB Quick Start voor het maken van hoekpunten, randen en query&quot;s met behulp van de Graph API van Azure Cosmos DB.
services: cosmosdb
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: terminal
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: anhoh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fb27ba1a70959ba92fbd021e9e42438081000e45
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-create-query-and-traverse-a-graph-in-the-gremlin-console"></a>Zelfstudie Azure Cosmos DB: gebruik de Gremlin-console om objecten te maken, query’s op grafen uit te voeren en door grafen te gaan

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafen en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u met Azure Portal een Azure Cosmos DB-account, een database en een graaf (container) kunt maken, en vervolgens de [Gremlin-console](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) kunt gebruiken om met gegevens van de Graph API (preview) te werken. In deze zelfstudie gaat u hoekpunten en randen maken, er query’s op uitvoeren, een eigenschap van een hoekpunt bijwerken, query’s uitvoeren op hoekpunten, door de graaf gaan en een hoekpunt verwijderen.

![Azure Cosmo DB vanuit de Apache Gremlin-console](./media/create-graph-gremlin-console/gremlin-console.png)

De Gremlin-console is op Groovy/Java gebaseerd en wordt uitgevoerd op Linux, Mac en Windows. U kunt de console downloaden van de [Apache TinkerPop-site](https://www.apache.org/dyn/closer.lua/tinkerpop/3.2.4/apache-tinkerpop-gremlin-console-3.2.4-bin.zip).

## <a name="prerequisites"></a>Vereisten

U moet een Azure-abonnement hebben om een Azure Cosmos DB-account volgens deze Quick Start te kunnen maken.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

U moet ook de [Gremlin-console](http://tinkerpop.apache.org/) installeren. Gebruik versie 3.2.4 of hoger.

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Een graaf toevoegen

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## <a id="ConnectAppService"></a>Verbinding maken met uw app service
1. Voordat u de Gremlin-console start, moet u het configuratiebestand *remote-secure.yaml* in uw map *apache-tinkerpop-gremlin-console-3.2.4/conf* maken of wijzigen.
2. Vul uw *host-*, *poort-*, *gebruikersnaam-*, *wachtwoord-*, *connectionPool-* en *serializer-*configuraties in:

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Hosts|***.graphs.azure.com|Uw graafservice-URI, die u uit Azure Portal kunt ophalen
    Poort|443|Ingesteld op 443
    Gebruikersnaam|*Uw gebruikersnaam*|De resource van het formulier `/dbs/<db>/colls/<coll>`.
    Wachtwoord|*Uw primaire hoofdsleutel*|Uw primaire hoofdsleutel voor de Azure Cosmos DB
    ConnectionPool|{enableSsl: true}|De instelling van de verbindingsgroep voor SSL
    Serializer|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Op deze waarde ingesteld

3. Voer in de terminal *bin/gremlin.bat* of *bin/gremlin.sh* uit om de [Gremlin-console](http://tinkerpop.apache.org/docs/3.2.4/tutorials/getting-started/) te starten.
4. Voer *:remote connect tinkerpop.server conf/remote-secure.yaml* uit om verbinding te maken met uw app-service.

Goed gedaan. Nu we configuratie hebben voltooid, gaan we een aantal consoleopdrachten uitvoeren.

## <a name="create-vertices-and-edges"></a>Hoekpunten en randen maken

Laten we beginnen met het toevoegen van vier hoekpunten voor de personen *Thomas*, *Marije*, *Robin* en *Ben*.

Invoer (Thomas):

```
:> g.addV('person').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Uitvoer:

```
==>[id:1eb91f79-94d7-4fd4-b026-18f707952f21,label:person,type:vertex,properties:[firstName:[[id:ec5fcfbe-040e-48c3-b961-31233c8b1801,value:Thomas]],lastName:[[id:86e5b580-0bca-4bc2-bc53-a46f92c1a182,value:Andersen]],age:[[id:2caeab3c-c66d-4098-b673-40a8101bb72a,value:44]]]]
```
Invoer (Marije):

```
:> g.addV('person').property('firstName', 'Mary Kay').property('lastName', 'Andersen').property('age', 39)
```

Uitvoer:

```
==>[id:899a9d37-6701-48fc-b0a1-90950be7e0f4,label:person,type:vertex,properties:[firstName:[[id:c79c5599-8646-47d1-9a49-3456200518ce,value:Mary Kay]],lastName:[[id:c1362095-9dcc-479d-ab21-86c1b6d4ffc1,value:Andersen]],age:[[id:0b530408-bfae-4e8f-98ad-c160cd6e6a8f,value:39]]]]
```

Invoer (Robin):

```
:> g.addV('person').property('firstName', 'Robin').property('lastName', 'Wakefield')
```

Uitvoer:

```
==>[id:953aefd9-5a54-4033-9b3a-d4dc3049f720,label:person,type:vertex,properties:[firstName:[[id:bbda02e0-8a96-4ca1-943e-621acbb26824,value:Robin]],lastName:[[id:f0291ad3-05a3-40ec-aabb-6538a7c331e3,value:Wakefield]]]]
```

Invoer (Ben):

```
:> g.addV('person').property('firstName', 'Ben').property('lastName', 'Miller')
```

Uitvoer:

```
==>[id:81c891d9-beca-4c87-9009-13a826c9ed9a,label:person,type:vertex,properties:[firstName:[[id:3a3b53d3-888c-46da-bb54-1c42194b1e18,value:Ben]],lastName:[[id:48c6dd50-79c4-4585-ab71-3bf998061958,value:Miller]]]]
```

Laten we nu randen toevoegen om relaties tussen deze personen aan te geven.

Invoer (Thomas -> Marije):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Mary Kay'))
```

Uitvoer:

```
==>[id:c12bf9fb-96a1-4cb7-a3f8-431e196e702f,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:0d1fa428-780c-49a5-bd3a-a68d96391d5c,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Invoer (Thomas -> Robin):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Robin'))
```

Uitvoer:

```
==>[id:58319bdd-1d3e-4f17-a106-0ddf18719d15,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:3e324073-ccfc-4ae1-8675-d450858ca116,outV:1ce821c6-aa3d-4170-a0b7-d14d2a4d18c3]
```

Invoer (Robin -> Ben):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').addE('knows').to(g.V().hasLabel('person').has('firstName', 'Ben'))
```

Uitvoer:

```
==>[id:889c4d3c-549e-4d35-bc21-a3d1bfa11e00,label:knows,type:edge,inVLabel:person,outVLabel:person,inV:40fd641d-546e-412a-abcc-58fe53891aab,outV:3e324073-ccfc-4ae1-8675-d450858ca116]
```

## <a name="update-a-vertex"></a>Een hoekpunt bijwerken

We gaan nu de hoekpunt van *Thomas* bijwerken met een nieuwe leeftijd van *45*.

Invoer:
```
:> g.V().hasLabel('person').has('firstName', 'Thomas').property('age', 45)
```
Uitvoer:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

## <a name="query-your-graph"></a>Query uitvoeren op uw graaf

Nu gaan we verschillende query's uitvoeren op de graaf.

Eerst gaan we met een filter een query uitvoeren om als resultaat mensen die ouder dan 40 jaar zijn te retourneren.

Invoer (filterquery):

```
:> g.V().hasLabel('person').has('age', gt(40))
```

Uitvoer:

```
==>[id:ae36f938-210e-445a-92df-519f2b64c8ec,label:person,type:vertex,properties:[firstName:[[id:872090b6-6a77-456a-9a55-a59141d4ebc2,value:Thomas]],lastName:[[id:7ee7a39a-a414-4127-89b4-870bc4ef99f3,value:Andersen]],age:[[id:a2a75d5a-ae70-4095-806d-a35abcbfe71d,value:45]]]]
```

Vervolgens gaan we voor de mensen die ouder zijn dan 40 jaar de voornaam projecteren.

Invoer (filter + projectiequery):

```
:> g.V().hasLabel('person').has('age', gt(40)).values('firstName')
```

Uitvoer:

```
==>Thomas
```

## <a name="traverse-your-graph"></a>Door uw graaf gaan

Laten we door de graaf gaan om alle vrienden van Thomas als resultaat te retourneren.

Invoer (vrienden van Thomas):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person')
```

Uitvoer: 

```
==>[id:f04bc00b-cb56-46c4-a3bb-a5870c42f7ff,label:person,type:vertex,properties:[firstName:[[id:14feedec-b070-444e-b544-62be15c7167c,value:Mary Kay]],lastName:[[id:107ab421-7208-45d4-b969-bbc54481992a,value:Andersen]],age:[[id:4b08d6e4-58f5-45df-8e69-6b790b692e0a,value:39]]]]
==>[id:91605c63-4988-4b60-9a30-5144719ae326,label:person,type:vertex,properties:[firstName:[[id:f760e0e6-652a-481a-92b0-1767d9bf372e,value:Robin]],lastName:[[id:352a4caa-bad6-47e3-a7dc-90ff342cf870,value:Wakefield]]]]
```

Laten we nu de volgende laag met hoekpunten ophalen. Ga door de graaf om alle vrienden van de vrienden van Thomas als resultaat te retourneren.

Invoer (vrienden van vrienden van Thomas):

```
:> g.V().hasLabel('person').has('firstName', 'Thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```
Uitvoer:

```
==>[id:a801a0cb-ee85-44ee-a502-271685ef212e,label:person,type:vertex,properties:[firstName:[[id:b9489902-d29a-4673-8c09-c2b3fe7f8b94,value:Ben]],lastName:[[id:e084f933-9a4b-4dbc-8273-f0171265cf1d,value:Miller]]]]
```

## <a name="drop-a-vertex"></a>Een hoekpunt verwijderen

Nu gaan we een hoekpunt verwijderen uit de graafdatabase.

Invoer (hoekpunt Robin verwijderen):

```
:> g.V().hasLabel('person').has('firstName', 'Robin').drop()
```

## <a name="clear-your-graph"></a>Uw graaf wissen

Ten slotte gaan we de database met alle hoekpunten en randen wissen.

Invoer:

```
:> g.V().drop()
```

Gefeliciteerd. U hebt deze Cosmos Azure DB-zelfstudie over de Graph API voltooid.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal:  

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, een graaf kunt maken met Data Explorer, hoekpunten en randen kunt maken en door de graaf kunt gaan met behulp van de Gremlin-console. U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)

