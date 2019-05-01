---
title: "Zelfstudie: Gegevens laden en query's uitvoeren in een Apache Spark-cluster in Azure HDInsight "
description: Leer hoe u gegevens laadt en interactieve query's uitvoert op Spark-clusters in Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.author: hrasheed
ms.date: 04/03/2019
ms.openlocfilehash: f480aeb7e126cb6ab8286bbfbfb8441fefeb07ef
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64716086"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Zelfstudie: Gegevens laden en query's uitvoeren in een Apache Spark-cluster in Azure HDInsight

In deze zelfstudie leert u hoe u een gegevensframe maakt van een CSV-bestand en hoe u interactieve Spark SQL-query's uitvoert op een [Apache Spark](https://spark.apache.org/)-cluster in Azure HDInsight. In Spark is een gegevensframe een gedistribueerde verzameling gegevens die zijn geordend in benoemde kolommen. Er ligt hetzelfde concept aan ten grondslag als aan een tabel in een relationele database of aan een gegevensframe in R/Python.
 
In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Een gegevensframe maken van een CSV-bestand
> * Query's uitvoeren op het gegevensframe

## <a name="prerequisites"></a>Vereisten

* De snelstart [Een Apache Spark-cluster maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) uitvoeren.

## <a name="create-a-dataframe-from-a-csv-file"></a>Een gegevensframe maken van een CSV-bestand

Toepassingen kunnen dataframes rechtstreeks maken vanuit bestanden of mappen op de externe opslag, zoals Azure Storage of Azure Data Lake Storage, vanuit een Hive-tabel, of vanuit andere gegevensbronnen die worden ondersteund door Apache Spark, zoals Cosmos DB, Azure SQL DB, DW enzovoort. In de volgende schermafbeelding ziet u een momentopname van het bestand HVAC.csv dat wordt gebruikt in deze zelfstudie. Het CSV-bestand wordt geleverd met alle HDInsight Spark-clusters. De gegevens hebben betrekking op de schommelingen in temperatuur van sommige gebouwen.
    
![Momentopname van gegevens voor interactieve Spark SQL-query](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Momentopname van gegevens voor interactieve Spark SQL-query")


1. Open de Jupyter-notebook die u hebt gemaakt in de sectie vereisten en een nieuwe notebook maken met PySpark.

    > [!NOTE]  
    > Door de PySpark-kernel te gebruiken voor het maken van een notebook, wordt de `spark`-sessie automatisch voor u gemaakt wanneer u de eerste cel met code uitvoert. U hoeft de sessie dus niet expliciet te maken.

2. Plak de volgende code in een lege cel van het notebook en druk op **Shift+Enter** om de code uit te voeren. Met de code importeert u de typen die voor dit scenario zijn vereist:

    ```python
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Wanneer u een interactieve query uitvoert in Jupyter, ziet u in het venster van de webbrowser of de titelbalk van het tabblad de status **(Busy)**, samen met de titel van het notebook. Ook ziet u een gevulde cirkel naast de **PySpark**-tekst in de rechterbovenhoek. Wanneer de taak is voltooid, verandert deze in een lege cirkel.

    ![Status van interactieve Spark SQL-query](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status van interactieve Spark SQL-query")

3. Voer de volgende code uit om een gegevensframe en een tijdelijke tabel (**hvac**) te maken. 

    ```python
    # Create a dataframe and table from sample data
    csvFile = spark.read.csv('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

## <a name="run-queries-on-the-dataframe"></a>Query's uitvoeren op het gegevensframe

Zodra de tabel is gemaakt, kunt u een interactieve query uitvoeren op de gegevens.

1. Voer de volgende code uit in een lege cel van de notebook:

    ```sql
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   De uitvoer wordt weergegeven in een tabel.

     ![Tabeluitvoer van resultaat van interactieve Spark-query](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabeluitvoer van resultaat van interactieve Spark-query")

2. U kunt de resultaten ook in andere visualisaties bekijken. Als u een vlakdiagram wilt zien voor dezelfde uitvoer, selecteert u **Area** en stelt u de andere waarden in zoals deze worden weergegeven.

    ![Gebiedsgrafiek van resultaat van interactieve Spark-query](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Gebiedsgrafiek van resultaat van interactieve Spark-query")

3. Navigeer in de menubalk van de notebook naar **bestand** > **opslaan en controlepunt**.

4. Als u direct verdergaat met de [volgende zelfstudie](apache-spark-use-bi-tools.md), laat u het notebook open. Als dit niet het geval is, sluit het notitieblok om de clusterresources vrij te geven: vanuit het notitieblok in de menubalk, navigeert u naar **bestand** >  **sluiten en stoppen**.

## <a name="clean-up-resources"></a>Resources opschonen

Met HDInsight worden uw gegevens en Jupyter-notitieblokken opgeslagen in Azure Storage of Azure Data Lake Storage, zodat u een cluster veilig kunt verwijderen wanneer dit niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt. Als u direct verder wilt met de zelfstudie, is het beter om het cluster te behouden.

Open het cluster in Azure Portal en selecteer **Verwijderen**.

![HDInsight-cluster verwijderen](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight-cluster verwijderen")

U kunt ook de naam van de resourcegroep selecteren om de pagina van de resourcegroep te openen en vervolgens **Resourcegroep verwijderen** selecteren. Als u de resourcegroep verwijdert, verwijdert u zowel het HDInsight Spark-cluster als het standaardopslagaccount.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:
> [!div class="checklist"]
> * Een Apache Spark-gegevensframe maken.
> * Spark SQL uitvoeren op het gegevensframe.

Ga naar het volgende artikel om te zien hoe de gegevens die u hebt geregistreerd in Apache Spark kunnen worden overgebracht naar een BI-hulpprogramma voor analyse zoals Power BI. 
> [!div class="nextstepaction"]
> [Gegevens analyseren met BI-tools](apache-spark-use-bi-tools.md)
