---
title: Een Azure Cosmos DB-grafiekdatabase maken met Java | Microsoft Docs
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
ms.topic: quickstart
ms.date: 10/20/2017
ms.author: denlee
ms.openlocfilehash: 4470b5adb52debce1492b084ce71100da77da046
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2017
---
# <a name="azure-cosmos-db-create-a-graph-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: een grafiek maken met behulp van Java en Azure Portal

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. Met behulp van Azure Cosmos DB, kunt u snel maken en query beheerd document, de tabel en de databases van de grafiek. 

Deze snelstartgids maakt een eenvoudige graph-database met behulp van de Azure portal-hulpprogramma's voor Azure Cosmos DB. In deze snelstartgids leest u ook hoe u snel een Java-console-app kunt maken via een grafiekdatabase met behulp van het OSS-stuurprogramma [Gremlin Java](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver). De instructies in deze snelstartgids kunnen worden uitgevoerd in elk besturingssysteem waarmee Java kan worden uitgevoerd. Deze snelstartgids familiarizes u met het maken en wijzigen van grafieken in de gebruikersinterface of via een programma, afhankelijk van wat is uw voorkeur. 

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Daarnaast doet u het volgende:

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.
    * Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.
* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair [Maven](http://maven.apache.org/)-archief
    * Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.
* [Git](https://www.git-scm.com/)
    * Op Ubuntu kunt u `sudo apt-get install git` uitvoeren om Git te installeren.

## <a name="create-a-database-account"></a>Een databaseaccount maken

Voordat u een grafiekdatabase kunt maken, moet u een Gremlin-databaseaccount (Graph) maken met Azure Cosmos DB.

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Een graaf toevoegen

U kunt nu het hulpprogramma Data Explorer in Azure Portal gebruiken om een grafiekdatabase te maken. 

1. Klik op **Gegevensverkenner** > **nieuwe grafiek**.

    De **grafiek toevoegen** gebied aan de rechterkant wordt weergegeven, moet u mogelijk scrollen rechts om het te bekijken.

    ![De Azure portal Data Explorer grafiek toevoegen-pagina](./media/create-graph-java/azure-cosmosdb-data-explorer-graph.png)

2. In de **grafiek toevoegen** pagina, voert u de instellingen voor de nieuwe grafiek.

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Database-id|voorbeelddatabase|Voer *-voorbeelddatabase* als de naam voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/ \ # ?` bevatten of eindigen op een spatie.
    Grafiek-id|voorbeeldgrafiek|Voer *voorbeeld grafiek* als de naam voor de nieuwe verzameling. Namen van de grafiek hebben dezelfde vereisten als de database-id's teken.
    Opslagcapaciteit|Vast (10 GB)|Wijzig de waarde in **vast (10 GB)**. Deze waarde is de opslagcapaciteit van de database.
    Doorvoer|400 RU‘s|Wijzig de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken.
    Partitiesleutel|Leeg laten|Laat het veld voor de partitiesleutel leeg omwille van deze snelstartgids.

3. Zodra het formulier is ingevuld, klikt u op **OK**.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we werken met code. We gaan een Graph API-app vanuit GitHub, de verbindingsreeks instellen en voer dit klonen. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken.  

1. Open een git-terminalvenster zoals git bash, en gebruik de `cd` opdracht om te wijzigen naar een map voor het installeren van de voorbeeld-app.  

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt u een kopie van de voorbeeld-app op uw computer. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources worden gemaakt in de code bent, kunt u de volgende codefragmenten bekijken. De codefragmenten zijn afkomstig uit de `Program.java` bestand in de map C:\git-samples\azure-cosmos-db-graph-java-getting-started\src\GetStarted. Anders kunt u verder gaan naar [bijwerken van de verbindingsreeks](#update-your-connection-string). 

* De Gremlin-`Client` wordt geïnitialiseerd vanuit de configuratie in `src/remote.yaml`.

    ```java
    cluster = Cluster.build(new File("src/remote.yaml")).create();
    ...
    client = cluster.connect();
    ```

* Een reeks Gremlin-stappen wordt uitgevoerd met behulp van de methode `client.submit`.

    ```java
    ResultSet results = client.submit(gremlin);

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```

## <a name="update-your-connection-information"></a>Werk uw verbindingsgegevens

Ga terug naar de Azure portal voor het verkrijgen van uw verbindingsgegevens en kopieer dit naar de app. Deze instellingen inschakelen voor uw app om te communiceren met uw gehoste-database.

1. In de [Azure-portal](http://portal.azure.com/), klikt u op **sleutels**. 

    Kopieer het eerste gedeelte van de URI-waarde.

    ![Weergeven en een toegangssleutel in de Azure portal, sleutels pagina kopiëren](./media/create-graph-java/keys.png)
2. Open het bestand src/remote.yaml en plak de waarde via `$name$` in `hosts: [$name$.graphs.azure.com]`.

    Regel 1 van remote.yaml nu zijn vergelijkbaar met 

    `hosts: [test-graph.graphs.azure.com]`

3. In de Azure portal kunt u de primaire sleutel kopiëren en plakken via `$masterKey$` in `password: $masterKey$`.

    Regel 4 van remote.yaml nu zijn vergelijkbaar met 

    `password: 2Ggkr662ifxz2Mg==`

4. Regel 3 van remote.yaml van wijzigen

    `username: /dbs/$database$/colls/$collection$`

    tot 

    `username: /dbs/sample-database/colls/sample-graph`

5. Sla het bestand remote.yaml.

## <a name="run-the-console-app"></a>De console-app uitvoeren

1. `cd` in het git-terminalvenster naar de map azure-cosmos-db-graph-java-getting-started.

    ```git
    cd "C:\git-samples\azure-cosmos-db-graph-java-getting-started"
    ```

2. Type `mvn package` in het git-terminalvenster om de vereiste Java-pakketten te installeren.

3. Voer in het terminalvenster git `mvn exec:java -D exec.mainClass=GetStarted.Program` om uw Java-toepassing te starten.

    In het terminalvenster ziet u de hoekpunten die worden toegevoegd aan de grafiek. Zodra het programma stopt, gaat u terug naar de Azure-portal in uw internetbrowser. 

<a id="add-sample-data"></a>
## <a name="review-and-add-sample-data"></a>Voorbeeldgegevens bekijken en toevoegen

U kunt nu teruggaan naar Data Explorer en de hoekpunten bekijken die zijn toegevoegd aan de grafiek. Ook kunt u extra hoekpunten toevoegen.

1. Klik op **Data Explorer**, vouw **voorbeeld grafiek**, klikt u op **grafiek**, en klik vervolgens op **Filter toepassen**. 

   ![Nieuwe documenten maken in Data Explorer in de Azure Portal](./media/create-graph-java/azure-cosmosdb-data-explorer-expanded.png)

2. In de lijst met **resultaten** ziet u de nieuwe gebruikers die zijn toegevoegd aan de grafiek. Als u **ben** selecteert, ziet u dat hij is verbonden met robin. U kunt de hoekpunten rond door slepen en neerzetten, in-en uitzoomen door het stuur van de muis schuiven en vouwt u de grootte van de grafiek met de dubbele pijl verplaatsen. 

   ![Nieuwe hoekpunten in de grafiek in Data Explorer in Azure Portal](./media/create-graph-java/azure-cosmosdb-graph-explorer-new.png)

3. Laten we enkele nieuwe gebruikers toevoegen. Klik op de knop **Nieuw hoekpunt** om gegevens toe te voegen aan uw grafiek.

   ![Nieuwe documenten maken in Data Explorer in de Azure Portal](./media/create-graph-java/azure-cosmosdb-data-explorer-new-vertex.png)

4. Voer een label van *persoon*.

5. Klik op **eigenschap toevoegen** om toe te voegen op elk van de volgende eigenschappen. U kunt unieke eigenschappen maken voor elke persoon in de grafiek. Alleen de id-sleutel is vereist.

    sleutel|waarde|Opmerkingen
    ----|----|----
    id|ashley|De unieke id voor het hoekpunt. Als u geen id opgeeft, wordt er een id voor u gegenereerd.
    geslacht|vrouwelijk| 
    technisch | java | 

    > [!NOTE]
    > In deze snelstartgids maken we een niet-gepartitioneerde verzameling. Als u echter een gepartitioneerde verzameling maakt door een partitiesleutel op te geven tijdens het maken van de verzameling, moet u de partitiesleutel opnemen als sleutel bij elk nieuw hoekpunt. 

6. Klik op **OK**. Mogelijk moet u het scherm groter maken om **OK** weer te geven onder aan het scherm.

7. Klik op **Nieuw hoekpunt** en voeg nog een nieuwe gebruiker toe. 

8. Voer een label van *persoon*.

9. Klik op **eigenschap toevoegen** om toe te voegen op elk van de volgende eigenschappen:

    sleutel|waarde|Opmerkingen
    ----|----|----
    id|rakesh|De unieke id voor het hoekpunt. Als u geen id opgeeft, wordt er een id voor u gegenereerd.
    geslacht|man| 
    school|MIT| 

10. Klik op **OK**. 

11. Klik op **Filter toepassen** met het standaard `g.V()` filter wilt weergeven van alle waarden in de grafiek. Alle gebruikers worden nu weergegeven in de lijst met **resultaten**. 

    Als u meer gegevens toevoegt, kunt u filters gebruiken om de resultaten te beperken. Data Explorer gebruikt standaard `g.V()` voor het ophalen van alle hoekpunten van een grafiek. U kunt dit wijzigen in een andere [grafiek query](tutorial-query-graph.md), zoals `g.V().count()`om te retourneren van een aantal van de hoekpunten in de grafiek in JSON-indeling. Als u het filter, wijzig het filter terug naar gewijzigd `g.V()` en klik op **Filter toepassen** om opnieuw alle resultaten weer te geven.

12. Nu kunnen we rakesh en ashley met elkaar verbinden. Selecteer **ashley** in de lijst met **resultaten** en klik vervolgens rechts onderaan naast **Doelen** op de knop Bewerken. Mogelijk moet u het scherm verbreden om het gedeelte **Eigenschappen** te kunnen zien.

   ![Het doel van een hoekpunt in een grafiek wijzigen](./media/create-graph-java/azure-cosmosdb-data-explorer-edit-target.png)

13. In de **doel** vak type *rakesh*, en in de **rand label** vak type *kent*, en klik vervolgens op de controle.

   ![Een verbinding tussen ashley en rakesh toevoegen in Data Explorer](./media/create-graph-java/azure-cosmosdb-data-explorer-set-target.png)

14. Selecteer nu **rakesh** in de lijst met resultaten en kijk of ashley en rakesh zijn verbonden. 

   ![Twee hoekpunten die zijn verbonden in Data Explorer](./media/create-graph-java/azure-cosmosdb-graph-explorer.png)

   De resource maken deel uit van deze zelfstudie is voltooid. U kunt blijven hoekpunten toevoegen aan uw grafiek, het wijzigen van de bestaande hoekpunten of het wijzigen van de query's. Nu gaan we leest de metrische gegevens Azure Cosmos DB biedt en vervolgens de resources opschonen. 

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een graaf gemaakt met Data Explorer en hebt u een app uitgevoerd. U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)

