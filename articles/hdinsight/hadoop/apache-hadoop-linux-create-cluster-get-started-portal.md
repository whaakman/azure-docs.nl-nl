---
title: 'Snelstart: Aan de slag met Apache Hadoop en Apache Hive met behulp van Azure Portal - Azure HDInsight'
description: In deze snelstartgids gebruikt u de Azure portal om een HDInsight Hadoop-cluster te maken
keywords: aan de slag met hadoop, hadoop linux, hadoop quickstart, aan de slag met hive, hive quickstart
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.topic: quickstart
ms.date: 07/02/2019
ms.author: hrasheed
ms.openlocfilehash: f92cb247afb25562a96373c28534549a2f16d8c9
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805598"
---
# <a name="quickstart-create-apache-hadoop-cluster-in-azure-hdinsight-using-azure-portal"></a>Quickstart: Apache Hadoop-cluster maken in Azure HDInsight met behulp van Azure portal

In dit artikel leert u hoe u [Apache Hadoop](https://hadoop.apache.org/)-clusters maakt in HDInsight met behulp van de Azure-portal en vervolgens Apache Hive-taken uitvoert in HDInsight. De meeste Hadoop-taken zijn batchtaken. U maakt een cluster, voert enkele taken uit en verwijdert het cluster vervolgens. In dit artikel gaat u al deze drie taken uitvoeren.

In deze snelstartgids gebruikt u Azure Portal voor het maken van een Hadoop-cluster in HDInsight. U kunt ook een cluster maken met behulp van een [Azure Resource Manager-sjabloon](apache-hadoop-linux-tutorial-get-started.md).

Op dit moment wordt HDInsight geleverd met [zeven verschillende clustertypen](./apache-hadoop-introduction.md#cluster-types-in-hdinsight). Elk clustertype ondersteunt een andere set onderdelen. Alle clustertypen ondersteunen Hive. Zie [Wat is er nieuw in de Apache Hadoop-clusterversies geleverd door HDInsight?](../hdinsight-component-versioning.md) voor een lijst met ondersteunde onderdelen in HDInsight.  

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-an-apache-hadoop-cluster"></a>Een Apache Hadoop-cluster maken

In deze sectie maakt u een Hadoop-cluster in HDInsight met behulp van Azure Portal.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).

1. Vanuit de Azure-portal, gaat u naar **een resource maken** > **Analytics** > **HDInsight**.

    ![Databricks in Azure Portal](./media/apache-hadoop-linux-create-cluster-get-started-portal/create-hdinsight.png "Databricks in Azure Portal")

1. Onder **HDInsight** > **snelle invoer** > **basisbeginselen**Typ of Selecteer de volgende waarden:

    |Eigenschap  |Description  |
    |---------|---------|
    |Clusternaam   | Voer een naam in voor het Hadoop-cluster. Omdat alle clusters in HDInsight dezelfde DNS-naamruimte delen, moet deze naam uniek zijn. De naam kan bestaan uit maximaal 59 tekens, inclusief letters, cijfers en afbreekstreepjes bevatten. De eerste en laatste tekens van de naam mogen geen streepjes zijn. |
    |Subscription    |  Selecteer uw Azure-abonnement. |
    |Clustertype     | Deze stap kunt u nu overslaan. U geeft het type cluster op in de volgende stap van deze procedure.|
    |Gebruikersnaam en wachtwoord    | De standaardaanmeldingsnaam is **admin**. Het wachtwoord moet uit minstens tien tekens bestaan en moet minstens één cijfer, één hoofdletter, één kleine letter en één niet-alfanumeriek teken bevatten (uitgezonderd ' " ` \). Zorg ervoor dat u **geen makkelijk te raden** wachtwoorden gebruikt, zoals 'Pass@word1'.|
    |SSH-gebruikersnaam (Secure Shell) | De standaardgebruikersnaam is **sshuser**.  U kunt hier echter een andere naam opgeven als u dat wilt. |
    |Wachtwoord voor clusteraanmelding voor SSH gebruiken| Schakel dit selectievakje in als u voor de SSH-gebruiker het wachtwoord wilt gebruiken dat u hebt opgegeven voor Wachtwoord voor clusteraanmeldgegevens.|
    |Resource group     | Maak een resourcegroep of selecteer een bestaande resourcegroep.  Een resourcegroep is een container met Azure-onderdelen.  In dit geval bevat de resourcegroep het HDInsight-cluster en het afhankelijke Azure Storage-account. |
    |Location    | Selecteer een Azure-locatie waar u het cluster wilt maken.  Kies een locatie zo dicht mogelijk bij u in de buurt voor betere prestaties. |

    ![HDInsight Linux aan de slag basiswaarden voor cluster opgeven](./media/apache-hadoop-linux-create-cluster-get-started-portal/quick-create-basics.png "Geef basiswaarden op voor het maken van een HDInsight-cluster")

1. Selecteer **clustertype** openen de **clusterconfiguratie** pagina en geef vervolgens de volgende waarden:

    |Eigenschap  |Description  |
    |---------|---------|
    |Clustertype     | Selecteer **Hadoop**. |
    |Version     | Selecteer **Hadoop 2.7.3 (HDI 3.6)**|

    ![HDInsight Linux aan de slag basiswaarden voor cluster opgeven](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-configuration-hadoop.png "Geef basiswaarden op voor het maken van een HDInsight-cluster")

    Selecteer **Selecteer** en selecteer vervolgens **volgende** om naar de opslaginstellingen te gaan.

1. Uit de **opslag** tabblad, geeft u de volgende waarden:

    |Eigenschap  |Description  |
    |---------|---------|
    |Type primaire opslag    | In dit artikel Azure storage gebruik van Azure Storage-Blob als het standaardopslagaccount te selecteren. U kunt ook Azure Data Lake Storage gebruiken als standaardopslag. |
    |Methode selecteren     |  Selecteer voor dit artikel **Mijn abonnementen** om een opslagaccount uit uw Azure-abonnement te gebruiken. Als u een opslagaccount uit andere abonnementen wilt gebruiken, selecteert u **Toegangssleutel** en geeft u vervolgens de toegangssleutel voor dat account op. |
    |Selecteer een opslagaccount   | Selecteer **selecteert u een opslagaccount** naar selecteert u een bestaand opslagaccount of **nieuw**. Als u een nieuw account maakt, wordt de naam moet tussen 3 en 24 tekens lang en kan alleen cijfers en kleine letters bevatten.|

    Accepteer alle overige standaardwaarden en selecteer vervolgens **volgende** om door te gaan naar de pagina overzicht.

    ![HDInsight Linux aan de slag waarden voor clusteropslag opgeven](./media/apache-hadoop-linux-create-cluster-get-started-portal/quick-create-storage.png "Geef opslagwaarden op voor het maken van een HDInsight-cluster")

1. Uit de **samenvatting** tabblad, controleert u of de waarden die u hebt geselecteerd in de eerdere stappen.

    ![HDInsight Linux aan de slag cluster overzicht](./media/apache-hadoop-linux-create-cluster-get-started-portal/quick-create-summary.png "HDInsight Linux aan de slag cluster overzicht")

1. Selecteer **Maken**. Het duurt ongeveer 20 minuten om een cluster te maken.

1. Zodra het cluster is gemaakt, ziet u de overzichtspagina van het cluster in Azure Portal.

    ![Aan de slag met clusterinstellingen in HDInsight op basis van Linux](./media/apache-hadoop-linux-create-cluster-get-started-portal/cluster-overview.png "HDInsight-clustereigenschappen")    

    Elk cluster is afhankelijk van een [Azure Storage-account](../hdinsight-hadoop-use-blob-storage.md) of een [Azure Data Lake-account](../hdinsight-hadoop-use-data-lake-store.md). Dit wordt het standaardopslagaccount genoemd. HDInsight-cluster en het standaardopslagaccount moeten in dezelfde Azure-regio worden geplaatst. Het opslagaccount wordt niet verwijderd wanneer er clusters worden verwijderd.

    > [!NOTE]  
    > Voor andere methoden voor het maken van cluster en inzicht in de eigenschappen die in deze Quick Start wordt gebruikt, Zie [maken van HDInsight-clusters](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="run-apache-hive-queries"></a>Apache Hive-query's uitvoeren

[Apache Hive](hdinsight-use-hive.md) is het meest populaire onderdeel dat in HDInsight wordt gebruikt. Er zijn veel manieren om Hive-taken uit te voeren in HDInsight. In deze snelstartgids gebruikt u de Ambari Hive-weergave in de portal. Voor andere methoden voor het indienen van Hive-taken raadpleegt u [Hive gebruiken in HDInsight](hdinsight-use-hive.md).

1. Als u Ambari wilt openen, selecteert u **Clusterdashboard** in de vorige schermafbeelding.  U kunt ook bladeren naar `https://ClusterName.azurehdinsight.net`, waarbij `ClusterName` is van het cluster dat u in de vorige sectie hebt gemaakt.

    ![HDInsight Linux aan de slag clusterdashboard](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-open-cluster-dashboard.png "HDInsight Linux aan de slag clusterdashboard")

2. Voer de gebruikersnaam en het wachtwoord voor Hadoop in die u hebt opgegeven tijdens het maken van het cluster. De standaardgebruikersnaam **admin**.

3. Open **Hive-weergave** zoals weergegeven in de volgende schermafbeelding:

    ![Ambari-weergaven selecteren](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "HDInsight Hive Weergave-menu")

4. Plak in het tabblad **QUERY** de volgende HiveQL-instructies in het werkblad:

    ```sql
    SHOW TABLES;
    ```

    ![HDInsight Hive-weergaven](./media/apache-hadoop-linux-tutorial-get-started/hiveview-1.png "HDInsight Hive-weergave Query Editor")

5. Selecteer **Uitvoeren**. Er wordt een tabblad **RESULTATEN** weergegeven onder het tabblad **QUERY** met informatie over de taak. 

    Nadat de query is voltooid, worden de resultaten van de bewerking weergegeven op het tabblad **QUERY**. U ziet één tabel met de naam **hivesampletable**. Deze Hive-voorbeeldtabel is bij alle HDInsight-clusters inbegrepen.

    ![HDInsight Hive-weergaven](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "HDInsight Hive-weergave Query Editor")

6. Herhaal stap 4 en 5 om de volgende query uit te voeren:

    ```sql
    SELECT * FROM hivesampletable;
    ```

7. U kunt de resultaten van de query ook opslaan. Selecteer de menuknop aan de rechterkant en geef aan of u de resultaten wilt downloaden als een CSV-bestand of deze wilt opslaan in het opslagaccount dat aan het cluster is gekoppeld.

    ![Resultaat van Hive-query opslaan](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-hive-view-save-results.png "Resultaat van Hive-query opslaan")

Nadat u een Hive-taak hebt voltooid, kunt u [de resultaten exporteren naar een Azure SQL-database of een SQL Server-database](apache-hadoop-use-sqoop-mac-linux.md). U kunt ook [de resultaten weergeven in Excel](apache-hadoop-connect-excel-power-query.md). Zie [Apache Hive en HiveQL gebruiken met Apache Hadoop in HDInsight voor het analyseren van een voorbeeldbestand van de Apache-log4j](hdinsight-use-hive.md) voor meer informatie over het gebruik van Hive in HDInsight.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de Quick Start hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

> [!NOTE]  
> Als u *onmiddellijk* u doorgaat met het volgende artikel voor meer informatie over het uitvoeren van ETL-bewerkingen met Hadoop in HDInsight, kunt u dat het cluster uitgevoerd. Dit komt doordat in de zelfstudie hebt u een Hadoop-cluster opnieuw maken. Als u niet meer via het volgende artikel meteen, moet u nu een het cluster verwijderen.

### <a name="to-delete-the-cluster-andor-the-default-storage-account"></a>Het cluster en/of het standaardopslagaccount verwijderen

1. Ga terug naar het browsertabblad voor Azure Portal. U komt terecht op de overzichtspagina voor het cluster. Selecteer **Verwijderen** als u alleen het cluster wilt verwijderen maar het standaardopslagaccount wilt behouden.

    ![HDInsight delete cluster](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "HDInsight-cluster verwijderen")

2. Als u het cluster en het standaardopslagaccount wilt verwijderen, selecteert u de naam van de resourcegroep (gemarkeerd in de vorige schermafbeelding) om de pagina van de resourcegroep te openen.

3. Selecteer **Resourcegroep verwijderen** om de resourcegroep te verwijderen. De groep bevat zowel het cluster als het standaardopslagaccount. Als u de resourcegroep verwijdert, wordt ook het opslagaccount verwijderd. Als u het opslagaccount wilt behouden, verwijdert u alleen het cluster.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u een Linux gebaseerde HDInsight-cluster met behulp van Resource Manager-sjabloon maken en hoe u eenvoudige Hive-query's uitvoert. In het volgende artikel leert u hoe u een ETL-bewerking (Extraction, Transformation, Loading) uitvoert met behulp van Hadoop in HDInsight.

> [!div class="nextstepaction"]
>[Uitpakken, transformeren en laden van gegevens met behulp van interactieve Query in HDInsight](../interactive-query/interactive-query-tutorial-analyze-flight-data.md)
