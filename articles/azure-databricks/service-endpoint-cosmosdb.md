---
title: Azure Databricks implementeren met een Cosmos DB-eindpunt
description: In deze zelfstudie wordt beschreven hoe u Azure Databricks implementeren in een virtueel netwerk met een Service-eindpunt ingeschakeld voor Cosmos DB.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.topic: tutorial
ms.date: 04/17/2019
ms.openlocfilehash: 0d5442a63680227f3a6186330502666c92dc3129
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013142"
---
# <a name="tutorial-implement-azure-databricks-with-a-cosmos-db-endpoint"></a>Zelfstudie: Azure Databricks implementeren met een Cosmos DB-eindpunt

Deze zelfstudie wordt beschreven hoe u een VNet geïnjecteerde Databricks-omgeving met een Service-eindpunt ingeschakeld voor Cosmos DB implementeert.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte in een virtueel netwerk maken
> * Een Cosmos DB-service-eindpunt maken
> * Een Cosmos DB-account maken en gegevens importeren
> * Een Azure Databricks-cluster maken
> * Cosmos DB query uit een Azure Databricks-notebook

## <a name="prerequisites"></a>Vereisten

Voordat u begint, het volgende doen:

* Maak een [Azure Databricks-werkruimte in een virtueel netwerk](quickstart-create-databricks-workspace-vnet-injection.md).

* Download de [Spark-connector](https://search.maven.org/remotecontent?filepath=com/microsoft/azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.4/azure-cosmosdb-spark_2.4.0_2.11-1.3.4-uber.jar).

* Download de voorbeeldgegevens van de [NOAA National Centers voor omgevingsinformatie](https://www.ncdc.noaa.gov/stormevents/). Selecteer een staat of gebied en selecteer **zoeken**. Accepteer de standaardwaarden en selecteer op de volgende pagina **zoeken**. Selecteer vervolgens **CSV downloaden** aan de linkerkant van de pagina voor het downloaden van de resultaten.

* Download de [vooraf gecompileerde binaire](https://aka.ms/csdmtool) van de Azure Cosmos DB Data Migration Tool.

## <a name="create-a-cosmos-db-service-endpoint"></a>Een Cosmos DB-service-eindpunt maken

1. Wanneer u een Azure Databricks-werkruimte hebt geïmplementeerd met een virtueel netwerk, navigeert u naar het virtuele netwerk in de [Azure-portal](https://portal.azure.com). U ziet de openbare en particuliere subnetten die zijn gemaakt via de Databricks-implementatie.

   ![Virtuele subnetten](./media/service-endpoint-cosmosdb/virtual-network-subnets.png)

2. Selecteer de *openbaar subnet* en een Cosmos DB-service-eindpunt te maken. Vervolgens **opslaan**.
   
   ![Een Cosmos DB-service-eindpunt toevoegen](./media/service-endpoint-cosmosdb/add-cosmosdb-service-endpoint.png)

## <a name="create-a-cosmos-db-account"></a>Cosmos DB-account maken

1. Open Azure Portal. Selecteer aan de linkerbovenhoek van het scherm, **een resource maken > Databases > Azure Cosmos DB**.

2. Vul de **Exemplaardetails** op de **basisbeginselen** tabblad met de volgende instellingen:

   |Instelling|Value|
   |-------|-----|
   |Abonnement|*uw abonnement*|
   |Resourcegroep|*de resourcegroep*|
   |Accountnaam|db-vnet-service-endpoint|
   |API|Core (SQL)|
   |Location|US - west|
   |Geo-redundantie|Uitschakelen|
   |Schrijfbewerkingen voor meedere regio's|Inschakelen|

   ![Een Cosmos DB-service-eindpunt toevoegen](./media/service-endpoint-cosmosdb/create-cosmosdb-account-basics.png)

3. Selecteer de **netwerk** tabblad en uw virtuele netwerk configureren. 

   a. Kies het virtuele netwerk dat u hebt gemaakt als een vereiste en selecteer vervolgens *openbaar subnet*. U ziet dat *particuliere subnetten* de opmerking heeft *eindpunt 'Microsoft AzureCosmosDB' ontbreekt '*. Dit is omdat u alleen het Cosmos DB-service-eindpunt is ingeschakeld op de *openbaar subnet*.

   b. Zorg ervoor dat u hebt **zodat toegang vanaf Azure portal** ingeschakeld. Deze instelling kunt u toegang tot uw Cosmos DB-account vanuit Azure portal. Als deze optie is ingesteld op **weigeren**, ontvangt u fouten bij het toegang tot uw account. 

   > [!NOTE]
   > Het is niet nodig voor deze zelfstudie, maar u kunt ook inschakelen *toegang toestaan uit Mijn IP* als u wilt dat de mogelijkheid toegang tot uw Cosmos DB-account van uw lokale computer. Bijvoorbeeld, als u verbinding met uw account met behulp van de Cosmos DB SDK, moet u deze instelling wilt inschakelen. Als deze is uitgeschakeld, ontvangt u 'Toegang geweigerd'-fouten.

   ![Netwerkinstellingen voor cosmos DB-Account](./media/service-endpoint-cosmosdb/create-cosmosdb-account-network.png)

4. Selecteer **revisie + maken**, en vervolgens **maken** te maken van uw Cosmos DB-account in het virtuele netwerk.

5. Wanneer uw Cosmos DB-account is gemaakt, navigeert u naar **sleutels** onder **instellingen**. De primaire verbindingsreeks kopiëren en opslaan in een teksteditor voor later gebruik.

    ![Pagina voor cosmos DB-account sleutels](./media/service-endpoint-cosmosdb/cosmos-keys.png)

6. Selecteer **Data Explorer** en **nieuwe verzameling** een nieuwe database en verzameling toevoegen aan uw Cosmos DB-account.

    ![Nieuwe cosmos DB-verzameling](./media/service-endpoint-cosmosdb/new-collection.png)

## <a name="upload-data-to-cosmos-db"></a>Gegevens uploaden naar Cosmos DB

1. Open de grafische interface-versie van de [hulpprogramma voor gegevensmigratie voor Cosmos DB](https://aka.ms/csdmtool), **Dtui.exe**.

    ![Hulpprogramma voor gegevensmigratie cosmos DB](./media/service-endpoint-cosmosdb/cosmos-data-migration-tool.png)

2. Op de **brongegevens** tabblad **CSV-bestanden** in de **importeren uit** vervolgkeuzelijst. Selecteer vervolgens **bestanden toevoegen** en de gegevens van storm CSV die u hebt gedownload als een vereiste toevoegen.

    ![Informatie over cosmos DB-hulpprogramma voor gegevensmigratie](./media/service-endpoint-cosmosdb/cosmos-source-information.png)

3. Op de **doelgegevens** tabblad, Voer uw verbindingsreeks. De indeling van de verbindingsreeks is `AccountEndpoint=<URL>;AccountKey=<key>;Database=<database>`. De AccountEndpoint en AccountKey worden opgenomen in de primaire verbindingsreeks die u in de vorige sectie hebt opgeslagen. Toevoeg- `Database=<your database name>` aan het einde van de verbindingsreeks en selecteer **controleren**. Vervolgens voegt u de verzameling en partitie de sleutel.

    ![Cosmos DB-hulpprogramma voor gegevensmigratie doelinformatie](./media/service-endpoint-cosmosdb/cosmos-target-information.png)

4. Selecteer **volgende** totdat u op de pagina overzicht. Selecteer **importeren**.

## <a name="create-a-cluster-and-add-library"></a>Een cluster maken en de bibliotheek toevoegen

1. Navigeer naar uw Azure Databricks-service in de [Azure-portal](https://portal.azure.com) en selecteer **werkruimte starten**.

   ![Databricks-werkruimte starten](./media/service-endpoint-cosmosdb/launch-workspace.png)

2. Maak een nieuw cluster. Kies de naam van een Cluster en de overige standaardinstellingen accepteren.

   ![Nieuwe instellingen van cluster](./media/service-endpoint-cosmosdb/create-cluster.png)

3. Nadat het cluster is gemaakt, gaat u naar de pagina en selecteer de **bibliotheken** tabblad. Selecteer **installeren nieuwe** en upload het Spark-connector jar-bestand voor het installeren van de bibliotheek.

    ![Bibliotheek van Spark-connector installeren](./media/service-endpoint-cosmosdb/install-cosmos-connector-library.png)

    U kunt controleren dat de bibliotheek is geïnstalleerd op de **bibliotheken** tabblad.

    ![Tabblad bibliotheken van Databricks-cluster](./media/service-endpoint-cosmosdb/installed-library.png)

## <a name="query-cosmos-db-from-a-databricks-notebook"></a>Cosmos DB query uit een Databricks-notebook

1. Navigeer naar uw Azure Databricks-werkruimte en maak een nieuwe python-notebook.

    ![Nieuwe Databricks-notebook maken](./media/service-endpoint-cosmosdb/new-python-notebook.png)

2. Voer de volgende python code om in te stellen de configuratie van de Cosmos DB-verbinding. Wijzig de **eindpunt**, **hoofdsleutel**, **Database**, en **verzameling** dienovereenkomstig.

    ```python
    connectionConfig = {
      "Endpoint" : "https://<your Cosmos DB account name.documents.azure.com:443/",
      "Masterkey" : "<your Cosmos DB primary key>",
      "Database" : "<your database name>",
      "preferredRegions" : "West US 2",
      "Collection": "<your collection name>",
      "schema_samplesize" : "1000",
      "query_pagesize" : "200000",
      "query_custom" : "SELECT * FROM c"
    }
    ```

3. Gebruik de volgende python-code voor de gegevens worden geladen en een tijdelijke weergave maken.

    ```python
    users = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**connectionConfig).load()
    users.createOrReplaceTempView("storm")
    ```

4. Gebruik de volgende magic-opdracht voor het uitvoeren van een SQL-instructie waarmee gegevens worden geretourneerd.

    ```python
    %sql
    select * from storm
    ```

    U hebt uw VNet geïnjecteerd Databricks-werkruimte is verbonden met een service-eindpunt ingeschakeld Cosmos DB-resource. Voor meer informatie over hoe u verbinding maken met Cosmos DB, Zie [Azure Cosmos DB-Connector voor Apache Spark](https://github.com/Azure/azure-cosmosdb-spark).

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep, de Azure Databricks-werkruimte en alle gerelateerde resources. De taak wordt verwijderd, voorkomt u onnodig facturering. Als u van plan bent de Azure Databricks-werkruimte in de toekomst te gebruiken, kunt u het cluster stoppen en later opnieuw. Als u niet blijven gebruiken van deze Azure Databricks-werkruimte wilt, verwijdert u alle resources die u in deze zelfstudie hebt gemaakt met behulp van de volgende stappen uit:

1. Klik in het menu links in Azure portal op **resourcegroepen** en klik vervolgens op de naam van de resourcegroep die u hebt gemaakt.

2. Selecteer op de pagina van uw resourcegroep **verwijderen**, typ de naam van de resource wilt verwijderen in het tekstvak in en selecteer vervolgens **verwijderen** opnieuw.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure Databricks-werkruimte geïmplementeerd met een virtueel netwerk en de Cosmos DB Spark-connector gebruikt om query Cosmos DB-gegevens uit Databricks. Voor meer informatie over het werken met Azure Databricks in een virtueel netwerk, verder met de zelfstudie voor het gebruik van SQL Server met Azure Databricks.

> [!div class="nextstepaction"]
> [Zelfstudie: Query uitvoeren op een SQL Server Linux Docker-container in een virtueel netwerk van een Azure Databricks-notebook](vnet-injection-sql-server.md)