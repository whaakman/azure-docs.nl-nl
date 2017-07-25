---
title: Een Azure Cosmos DB Java-toepassing ontwikkelen met behulp van de Graph API | Microsoft Docs
description: Biedt een voorbeeld van Java-code dat u kunt gebruiken om verbinding te maken met gegevens over grafen in Azure Cosmos DB met behulp van Gremlin.
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 8eac406c6ef96d7ae8dd5f4931c7d16edb723be8
ms.contentlocale: nl-nl
ms.lasthandoff: 07/17/2017

---
# <a name="azure-cosmos-db-build-a-java-application-using-the-graph-api"></a>Azure Cosmos DB: een Java-toepassing ontwikkelen met de Graph API

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafen en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u met Azure Portal een Azure Cosmos DB-account voor Graph API (preview), een database en een graaf kunt maken. U gaat vervolgens een console-app ontwikkelen en uitvoeren met behulp van het OSS-stuurprogramma [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver).  

## <a name="prerequisites"></a>Vereisten

* Voordat u met dit voorbeeld aan de slag gaat, moet u aan de volgende vereisten voldoen:
   * JDK 1.7 + (voer `apt-get install default-jdk` uit als u niet beschikt over JDK) en omgevingsvariabelen instellen, zoals`JAVA_HOME`
   * Maven (voer `apt-get install maven` uit als u niet over Maven beschikt)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Een graaf toevoegen

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Graph API-app (preview) klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een venster in een git-terminal zoals git bash en `cd` naar een werkmap.  

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Als u het bestand `Program.java` opent kunt u zien wat deze regels code precies doen. 

* De Gremlin-`Client` wordt geïnitialiseerd vanuit de configuratie in `src/remote.yaml`.

    ```java
    Cluster cluster = Cluster.build(new File("src/remote.yaml")).create();
    
    Client client = cluster.connect();
    ```

* Een reeks Gremlin-stappen wordt uitgevoerd met behulp van de methode `client.submit`.

    ```java
    ResultSet results = client.submit("g.V()");

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```
## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

1. Open het bestand src/remote.yaml. 

3. Vul uw *host-*, *poort-*, *gebruikersnaam-*, *wachtwoord-*, *connectionPool-* en *serializer-* configuraties in het bestand src/remote.yaml in:

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Hosts|[***.graphs.azure.com]|Zie de onderstaande schermafbeelding. Dit is de Gremlin URI-waarde op de pagina Overzicht van Azure Portal tussen vierkante haken, waarbij de afsluitende: 443/ is verwijderd.<br><br>Deze waarde kan ook worden opgehaald van het tabblad Sleutels met behulp van de URI-waarde door https:// te verwijderen, documenten in grafieken te wijzigen en de afsluitende: 443/ te verwijderen.
    Poort|443|Ingesteld op 443.
    Gebruikersnaam|*Uw gebruikersnaam*|De bron van het formulier `/dbs/<db>/colls/<coll>` waar `<db>` de naam van uw database is en `<coll>` de naam van uw verzameling is.
    Wachtwoord|*Uw primaire hoofdsleutel*|Zie de tweede onderstaande schermafbeelding. Dit is uw primaire sleutel, die u kunt ophalen van de pagina Sleutels in Azure Portal, in het vak Primaire sleutel. Gebruik de kopieerknop aan de linkerkant van het vak om de waarde te kopiëren.
    ConnectionPool|{enableSsl: true}|De instelling van de verbindingsgroep voor SSL.
    Serializer|{ className: org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Instellen op deze waarden en eventuele `\n`-regeleinden verwijderen bij het plakken in de waarde.

    Kopieer voor de waarde voor hosts de **Gremlin URI**-waarde van de pagina **Overzicht**: ![De Gremlin-URI-waarde bekijken en kopiëren op de pagina Overzicht van Azure Portal](./media/create-graph-java/gremlin-uri.png)

    Kopieer voor de wachtwoordwaarde de **Primaire sleutel** van de pagina **Sleutels**: ![Uw primaire sleutel bekijken en kopiëren in Azure Portal op de pagina Sleutels](./media/create-graph-java/keys.png)

## <a name="run-the-console-app"></a>De app console uitvoeren

1. Voer `mvn package` uit op een terminal om de vereiste Java-pakketten te installeren.

2. Voer `mvn exec:java -D exec.mainClass=GetStarted.Program` uit op een terminal om uw Java-toepassing te starten.

U kunt nu teruggaan naar Data Explorer en deze nieuwe gegevens bekijken, wijzigen, een query erop uitvoeren of er iets anders mee doen. 

## <a name="browse-using-the-data-explorer"></a>Bladeren met Data Explorer

U kunt nu teruggaan naar Data Explorer in Azure Portal en door uw nieuwe graafgegevens bladeren en er query’s op uitvoeren.

* De nieuwe database wordt in Data Explorer weergegeven in het deelvenster Verzamelingen. Vouw **graphdb**, **graphcoll** uit en klik vervolgens op **Graaf**.

    De gegevens die worden gegenereerd door de voorbeeldapp worden weergegeven in het deelvenster Grafen.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal: 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een graaf gemaakt met Data Explorer en hebt u een app uitgevoerd. U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)


