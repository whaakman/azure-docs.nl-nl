---
title: 'Snelstartgids: Een Apache Spark-taak uitvoeren in Azure Databricks met behulp van Azure Portal'
description: De quickstart laat zien hoe er met Azure Portal een Azure Databricks-werkruimte en een Apache Spark-cluster kan worden gemaakt, en hoe een Spark-taak kan worden uitgevoerd.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.date: 05/08/2019
ms.custom: mvc
ms.openlocfilehash: 43133810c6f8b7cb9fdacb2503103e09f345acfc
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65551192"
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-portal"></a>Snelstart: Een Apache Spark-taak uitvoeren in Azure Databricks met behulp van Azure Portal

Deze quickstart laat zien hoe een Azure Databricks-werkruimte wordt gemaakt en hoe binnen die werkruimte een Apache Spark-cluster kan worden gemaakt. Tot slot leert u hoe u een Spark-taak op het Databricks-cluster kunt uitvoeren. Zie [Wat is Azure Databricks?](what-is-azure-databricks.md) voor meer informatie over Azure Databricks.

In deze snelstartgids analyseren als onderdeel van de Spark-taak u Boston veiligheidsgegevens voor meer inzicht in verschillende rapportagemethoden.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-databricks-workspace"></a>Een Azure Databricks-werkruimte maken

In deze sectie gaat u een Azure Databricks-werkruimte maken met behulp van Azure Portal.

1. Selecteer in Azure Portal **Een resource maken** > **Analyse** > **Azure Databricks**.

    ![Databricks in Azure Portal](./media/quickstart-create-databricks-workspace-portal/azure-databricks-on-portal.png "Databricks in Azure Portal")

2. Geef bij **Azure Databricks Service** de waarden op voor het maken van een Databricks-werkruimte.

    ![Een Azure Databricks-werkruimte maken](./media/quickstart-create-databricks-workspace-portal/create-databricks-workspace.png "Een Azure Databricks-werkruimte maken")

    Geef de volgende waarden op:
    
    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor uw Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **US - west 2**. Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor andere beschikbare regio's.        |
    |**Prijscategorie**     |  Kiezen tussen **Standard**, **Premium**, of **proefversie**. Bekijk de pagina [Prijzen voor Databricks](https://azure.microsoft.com/pricing/details/databricks/) voor meer informatie over deze categorieën.       |

    Selecteer **Vastmaken aan dashboard** en klik op **Maken**.

4. Het maken van de werkruimte duurt enkele minuten. Tijdens het maken van de werkruimte, vindt u de implementatiestatus in **meldingen**.

    ![Tegel Databricks-implementatie](./media/quickstart-create-databricks-workspace-portal/databricks-deployment-tile.png "Tegel Databricks-implementatie")

## <a name="create-a-spark-cluster-in-databricks"></a>Een Spark-cluster maken in Databricks

> [!NOTE]
> Als u een gratis account wilt gebruiken om het Azure Databricks-cluster te maken, gaat u voordat het cluster is gemaakt naar uw profiel en wijzigt u uw abonnement in **betalen per gebruik**. Zie [Gratis Azure-account](https://azure.microsoft.com/free/) voor meer informatie.

1. Ga in Azure Portal naar de Databricks-werkruimte die u hebt gemaakt en klik op **Werkruimte starten**.

2. U wordt omgeleid naar de Azure Databricks-portal. Klik in de portal op **nieuw Cluster**.

    ![Databricks in Azure](./media/quickstart-create-databricks-workspace-portal/databricks-on-azure.png "Databricks in Azure")

3. Op de pagina **Nieuw cluster** geeft u de waarden op waarmee een nieuw cluster wordt gemaakt.

    ![Een Databricks Spark-cluster maken in Azure](./media/quickstart-create-databricks-workspace-portal/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

    Accepteer alle andere standaardwaarden, anders dan de volgende:

   * Voer een naam in voor het cluster.
   * In dit artikel maakt u een cluster met **5.2** runtime.
   * Zorg ervoor dat u het selectievakje **Beëindigen na\_\_ minuten van inactiviteit** inschakelt. Geef een duur (in minuten) op waarna het cluster moet worden beëindigd als het niet wordt gebruikt.
    
     Selecteer **Cluster maken**. Zodra het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Spark-taken uitvoeren.

Zie [Een Spark-cluster maken in Azure Databricks](https://docs.azuredatabricks.net/user-guide/clusters/create.html) voor meer informatie over het maken van clusters.

## <a name="run-a-spark-sql-job"></a>Een Spark SQL-taak uitvoeren

De volgende taken uitvoeren om een notitieblok in Databricks te maken, configureer het notitieblok om te lezen van gegevens van een Azure Open-gegevenssets en voer vervolgens een Spark SQL-taak op de gegevens.

1. Selecteer in het linkerdeelvenster **Azure Databricks**. Uit de **algemene taken**, selecteer **nieuwe Notebook**.

    ![Een notitieblok maken in Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-create-notebook.png "Een notitieblok maken in Databricks")

2. In de **notitieblok maken** dialoogvenster vak, voer een naam in, selecteer **Python** als de taal en selecteer het Spark-cluster dat u eerder hebt gemaakt.

    ![Een notitieblok maken in Databricks](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-details.png "Een notitieblok maken in Databricks")

    Selecteer **Maken**.

3. In deze stap maakt u een Spark DataFrame met Boston veiligheidsgegevens uit [Azure Open gegevenssets](https://azure.microsoft.com/services/open-datasets/catalog/boston-safety-data/#AzureDatabricks), en SQL op de gegevens op te vragen.

   De volgende opdracht stelt de gegevens van de toegang tot Azure storage. Plak deze PySpark-code in de eerste cel en gebruik **Shift + Enter** de code uit te voeren.

   ```python
   blob_account_name = "azureopendatastorage"
   blob_container_name = "citydatacontainer"
   blob_relative_path = "Safety/Release/city=Boston"
   blob_sas_token = r"?st=2019-02-26T02%3A34%3A32Z&se=2119-02-27T02%3A34%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=XlJVWA7fMXCSxCKqJm8psMOh0W4h7cSYO28coRqF2fs%3D"
   ```

   De volgende opdracht manier kan Spark te lezen op afstand van Blob-opslag. Plak deze PySpark-code in de volgende cel en gebruik **Shift + Enter** de code uit te voeren.

   ```python
   wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
   spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)
   print('Remote blob path: ' + wasbs_path)
   ```

   De volgende opdracht maakt u een DataFrame. Plak deze PySpark-code in de volgende cel en gebruik **Shift + Enter** de code uit te voeren.

   ```python
   df = spark.read.parquet(wasbs_path)
   print('Register the DataFrame as a SQL temporary view: source')
   df.createOrReplaceTempView('source')
   ```

4. Uitvoeren van een SQL-instructie return de bovenste 10 rijen met gegevens uit de tijdelijke weergave met de naam **bron**. Plak deze PySpark-code in de volgende cel en gebruik **Shift + Enter** de code uit te voeren.

   ```python
   print('Displaying top 10 rows: ')
   display(spark.sql('SELECT * FROM source LIMIT 10'))
   ```

5. U ziet uitvoer in tabelvorm zoals weergegeven in de volgende schermafbeelding (alleen bepaalde kolommen worden weergegeven):

    ![Voorbeeldgegevens](./media/quickstart-create-databricks-workspace-portal/databricks-sample-csv-data.png "voorbeeld-JSON-gegevens")

6. U maakt nu een visuele representatie van deze gegevens om weer te geven hoeveel veiligheid gebeurtenissen worden gerapporteerd met behulp van de App voor burgers verbinding maken en de plaats Worker App in plaats van andere bronnen. Selecteer in de onderkant van de uitvoer in tabelvorm en de **staafdiagram** pictogram en klik vervolgens op **tekenopties**.

    ![Staafdiagram maken](./media/quickstart-create-databricks-workspace-portal/create-plots-databricks-notebook.png "Staafdiagram maken")

8. In **Tekening aanpassen** sleept en zet u de waarden neer zoals in de schermafbeelding wordt weergegeven.

    ![Cirkeldiagram aanpassen](./media/quickstart-create-databricks-workspace-portal/databricks-notebook-customize-plot.png "staafdiagram aanpassen")

   * Stel **sleutels** naar **bron**.
   * Stel **waarden** naar **< \id >**.
   * Stel **Aggregatie** in op **AANTAL**.
   * Stel **weergavetype** naar **cirkeldiagram**.

     Klik op **Toepassen**.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u het artikel hebt doorgenomen, kunt u het cluster beëindigen. Dit doet u door vanuit de Azure Databricks-werkruimte in het linkerdeelvenster **Clusters** te selecteren. Voor het cluster dat u wilt beëindigen, plaatst u de cursor op het weglatingsteken onder de kolom **Acties** en selecteert u het **beëindigingspictogram**.

![Een Databricks-cluster stopzetten](./media/quickstart-create-databricks-workspace-portal/terminate-databricks-cluster.png "Een Databricks-cluster stopzetten")

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na \_\_ minuten inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een Spark-cluster in Azure Databricks gemaakt en uitgevoerd van een Spark-taak met gegevens uit Azure Open-gegevenssets. U kunt ook zoeken op [Spark gegevensbronnen](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) voor meer informatie over het importeren van gegevens uit andere gegevensbronnen in Azure Databricks. Ga naar het volgende artikel voor informatie over het uitvoeren van een ETL-bewerking (Extraction, Transformation, and Loading) met behulp van Azure Databricks.

> [!div class="nextstepaction"]
>[Gegevens uitpakken, transformeren en laden met Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
