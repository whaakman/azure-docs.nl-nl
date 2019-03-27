---
title: Verbinding maken met Apache Hadoop met Data Lake Tools voor Visual Studio - Azure HDInsight
description: Informatie over het installeren en gebruiken van Data Lake Tools voor Visual Studio om te verbinden met Apache Hadoop-clusters in Azure HDInsight en vervolgens Hive-query's worden uitgevoerd.
keywords: hadoop-hulpprogramma's, hive-query, visual studio, visual studio hadoop
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: cf392bb254e38b2e07a92e87927b12e144b26f16
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58447479"
---
# <a name="use-data-lake-tools-for-visual-studio-to-connect-to-azure-hdinsight-and-run-apache-hive-queries"></a>Data Lake Tools voor Visual Studio gebruiken om te verbinden met Azure HDInsight en Apache Hive-query's uitvoeren

Meer informatie over het gebruik van [Microsoft Azure Data Lake en Stream Analytics-hulpprogramma's voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504) (ook wel genoemd Data Lake Tools) verbinding maken met Apache Hadoop-clusters in [Azure HDInsight](../hdinsight-hadoop-introduction.md) en het verzenden van Hive-query's.  

Zie [Inleiding tot HDInsight](../hdinsight-hadoop-introduction.md) en [Aan de slag met HDInsight](apache-hadoop-linux-tutorial-get-started.md) voor meer informatie over het gebruik van HDInsight.  

Zie voor meer informatie over het verbinden met een Apache Storm-cluster [ontwikkelen C# topologieën voor Apache Storm op HDInsight met behulp van Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md).

U kunt Data Lake Tools voor Visual Studio gebruiken voor toegang tot Azure Data Lake Analytics en HDInsight. Zie voor informatie over Data Lake Tools [Zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

## <a name="prerequisites"></a>Vereisten

Al u deze zelfstudie wilt voltooien en Data Lake Tools voor Visual Studio wilt gebruiken, hebt u de volgende items nodig:

* Een Azure HDInsight-cluster. Zie voor het maken van een HDInsight-cluster, [aan de slag met behulp van Apache Hadoop in Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md). Om de interactieve Apache Hive-query's uitvoeren, moet u een [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md) cluster.  

* [Visual Studio](https://visualstudio.microsoft.com/downloads/) (2013 of hoger).  De [Visual Studio Community edition](https://visualstudio.microsoft.com/vs/community/) is gratis.  Zie ook [Visual Studio 2017 installeren](https://docs.microsoft.com/visualstudio/install/install-visual-studio).

  > [!IMPORTANT]  
  > Data Lake Tools is niet meer ondersteund voor Visual Studio 2013. 

## <a name="install-data-lake-tools-for-visual-studio"></a>Data Lake Tools voor Visual Studio installeren  
<a name="install-or-update-data-lake-tools-for-visual-studio"></a>

* Visual Studio 2017  
  Tijdens de installatie, zorg ervoor dat u ten minste opnemen Workloads **Azure-ontwikkeling** of **gegevensopslag en verwerking**.  

  Voor bestaande installaties in de menubalk, navigeer naar **extra** > **hulpprogramma's en onderdelen...**  installatieprogramma voor Visual Studio openen.  Selecteer ten minste Workloads **Azure-ontwikkeling** of **gegevensopslag en verwerking**.

  ![Schermopname van het installatieprogramma voor Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/vs2017_installation.png)

* Visual Studio 2013 en 2015  
  [Data Lake Tools downloaden](https://www.microsoft.com/download/details.aspx?id=49504). Kies de versie van Data Lake Tools die overeenkomt met uw versie van Visual Studio.  

> [!NOTE]  
> Momenteel is alleen de Engelse versie van Data Lake Tools voor Visual Studio beschikbaar.

## <a name="update-data-lake-tools-for-visual-studio"></a>Data Lake Tools voor Visual Studio bijwerken  

1. Open Visual Studio.

2. In de menubalk, gaat u naar **extra** > **extensies en Updates...** .

3. Uit de **extensies en Updates** venster, vouw **Updates** aan de linkerkant.

4. Als een update beschikbaar is, **Azure Data Lake en Stream Analytics-hulpprogramma's** wordt weergegeven in het hoofdvenster.  Selecteer **Update**.

> [!NOTE]  
> U kunt alleen Data Lake Tools versie 2.3.0.0 of hoger gebruiken om verbinding te maken met Interactive Query-clusters en interactieve Hive-query's uit te voeren.

## <a name="connect-to-azure-subscriptions"></a>Verbinding maken met Azure-abonnementen
U kunt Data Lake Tools voor Visual Studio gebruiken om verbinding te maken met uw HDInsight-clusters, enkele eenvoudige beheerbewerkingen uit te voeren en Hive-query's uit te voeren.

> [!NOTE]  
> Zie [Write and submit Hive queries by using Visual Studio](https://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx) (Hive-query's schrijven en verzenden met behulp van Visual Studio) voor informatie over het maken van verbinding met een algemeen Hadoop-cluster.

Verbinding maken met uw Azure-abonnement:

1. Open Visual Studio.

2. In de menubalk, gaat u naar **weergave** > **Server Explorer**.

3. In Server Explorer met de rechtermuisknop op **Azure**, selecteer **verbinding maken met Microsoft Azure-abonnement...** , en het aanmeldingsproces te voltooien.

4. In Server Explorer wordt een lijst met bestaande HDInsight-clusters weergegeven. Als u geen clusters hebt, kunt u een cluster maken door gebruik te maken van Azure Portal, Azure PowerShell of de HDInsight SDK. Zie [HDInsight-clusters maken](../hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie.

   ![Schermafbeelding van de lijst met clusters in Server Explorer van Data Lake Tools voor Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Lijst met clusters in Server Explorer van Data Lake Tools voor Visual Studio")

5. Een HDInsight-cluster uitbreiden. **Hive-databases**, een standaardopslagaccount, gekoppelde opslagaccounts en een **Hadoop Service-logboek** worden weergegeven. U kunt de entiteiten verder uitbreiden.

Nadat u met uw Azure-abonnement verbonden bent, kunt u de volgende taken uitvoeren.

Verbinding maken met Azure Portal vanuit Visual Studio:

1. In Server Explorer, gaat u naar **Azure** > **HDInsight** en selecteert u uw cluster.

2. Met de rechtermuisknop op een HDInsight-cluster en selecteer **Cluster beheren in Azure portal**.

Stel vragen en/of feedback geven vanuit Visual Studio:

1. In Server Explorer, gaat u naar **Azure** > **HDInsight**.

2. Met de rechtermuisknop op **HDInsight** en selecteert u **MSDN-Forum** vragen, of **Feedback geven** om feedback te geven.

## <a name="explore-linked-resources"></a>Gekoppelde resources verkennen
In Server Explorer worden het standaardaccount voor opslag en alle gekoppelde opslagaccounts weergegeven. Als u het standaardopslagaccount uitvouwt, kunt u de containers op het opslagaccount zien. Het standaardopslagaccount en de standaardcontainer worden gemarkeerd. Klik met de rechtermuisknop op een van de containers om de inhoud ervan weer te geven.

![Schermafbeelding van de lijst met gekoppelde resources in Server Explorer van Data Lake Tools voor Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "Lijst met gekoppelde resources")

Nadat u een container hebt geopend, kunt u met de volgende knoppen blobs uploaden, verwijderen en downloaden:

![Schermafbeelding van Blob-bewerkingen in Server Explorer van Data Lake Tools voor Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "Blobs uploaden, verwijderen en downloaden in Server Explorer")

## <a name="run-interactive-apache-hive-queries"></a>Interactieve Apache Hive-query's uitvoeren
[Apache Hive](https://hive.apache.org) is een datawarehouse-infrastructuur die is gebouwd op Hadoop. Hive wordt gebruikt voor gegevenssamenvatting, query's en analyse. U kunt Data Lake Tools voor Visual Studio gebruiken om Hive-query's uit te voeren vanuit Visual Studio. Zie voor meer informatie over Hive [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md).

[Interactive Query](../interactive-query/apache-interactive-query-get-started.md) gebruikt [Hive op LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) in Apache Hive 2.1. Interactive Query brengt interactiviteit naar complexe datawarehouse-achtige query's op grote opgeslagen gegevenssets. Het uitvoeren van Hive-query's in Interactive Query is veel sneller dan de traditionele Hive-batchtaken. Zie voor meer informatie, voeren Apache Hive-batchtaken.

> [!NOTE]  
> U kunt alleen interactieve Hive-query's uitvoeren wanneer u verbinding maakt met een [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md)-cluster.

U kunt Data Lake Tools voor Visual Studio ook gebruiken om te zien wat er in een Hive-taak zit. Data Lake Tools voor Visual Studio verzamelt de Yarn-logboeken van bepaalde Hive-taken en maakt ze zichtbaar.

In Server Explorer, gaat u naar **Azure** > **HDInsight** en selecteert u uw cluster.  Dit is het beginpunt in Server Explorer van de secties te volgen.

### <a name="view-hivesampletable"></a>Weergave hivesampletable
Alle HDInsight-clusters hebben een standaard voorbeeld Hive-tabel met de naam `hivesampletable`.  

Vanuit het cluster, navigeert u naar **Hive-Databases** > **standaard** > **hivesampletable**.

* Om weer te geven `hivesampletable` schema:  
Vouw **hivesampletable**.

* Om weer te geven `hivesampletable` gegevens:  
Met de rechtermuisknop op **hivesampletable**, en selecteer **View Top 100 rijen**.  Dit is equivalent aan het uitvoeren van de volgende Hive-query met behulp van het Hive ODBC-stuurprogramma:

   `SELECT * FROM hivesampletable LIMIT 100`

  U kunt het aantal rijen aanpassen.

  ![Schermafbeelding van een HDInsight Hive Visual Studio-schemaquery](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Resultaten van Hive-query")

### <a name="create-hive-tables"></a>Hive-tabellen maken
U een Hive-tabel maken door de GUI te gebruiken of door Hive-query's te gebruiken. Zie voor meer informatie over het gebruik van Hive-query's [uitvoeren Apache Hive-query's](#run.queries).

1. Vanuit het cluster, navigeert u naar **Hive-Databases** > **standaard**.

2. Met de rechtermuisknop op **standaard**, en selecteer **Create Table**.

3. De tabel naar wens configureren.  

4. Selecteer **Tabel maken** om de taak voor het maken van de nieuwe Hive-tabel te verzenden.

    ![Schermafbeelding van het venster Tabel maken van HDInsight Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Hive-tabel maken")

### <a name="run.queries"></a>Maken en uitvoeren van Hive-query 's
U hebt twee opties voor het maken en uitvoeren van Hive-query's:

* Ad-hocquery's maken
* Een Hive-toepassing maken

Maken en uitvoeren van ad-hocquery's:

1. Met de rechtermuisknop op het cluster waar u de query uitvoert, en selecteer **een Hive-Query schrijven**.  

2. Voer de Hive-query's in  

    De Hive-editor ondersteunt IntelliSense. Data Lake Tools voor Visual Studio biedt ondersteuning voor het laden van externe metagegevens wanneer u het Hive-script bewerkt. Als u bijvoorbeeld `SELECT * FROM`, geeft IntelliSense alle voorgestelde tabelnamen. Wanneer een tabelnaam wordt opgegeven, geeft IntelliSense de kolomnamen weer. De hulpprogramma's ondersteunen de meeste DML-instructies, subquery's en ingebouwde UDF's van Hive.

    ![Schermafbeelding van een HDInsight Visual Studio Tools IntelliSense-voorbeeld 1](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")

    ![Schermafbeelding van een HDInsight Visual Studio Tools IntelliSense-voorbeeld 2](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")

   > [!NOTE]  
   > IntelliSense suggereert alleen de metagegevens van het cluster dat in de HDInsight-werkbalk is geselecteerd.

3. Kies de uitvoeringsmodus:

    * **Interactief**  

      Zorg ervoor dat **interactief** is geselecteerd en selecteer vervolgens **Execute**.

      ![Schermafbeelding van de query en uit te voeren](./media/apache-hadoop-visual-studio-tools-get-started/execute.png)  

    * **Batch**  

      Zorg ervoor dat **Batch** is geselecteerd en selecteer vervolgens **indienen**.  Als u de optie voor Geavanceerd verzenden selecteert, configureert u **taaknaam**, **argumenten**, **aanvullende configuraties**, en **Statusmap**voor het script.

      ![Schermafbeelding van de query en batch](./media/apache-hadoop-visual-studio-tools-get-started/batch.png)  

      ![Schermafbeelding van een HDInsight Hadoop Hive-query](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Query's verzenden")

      > [!NOTE]  
      > U kunt niet indienen batches met Interactive Query-clusters.  U moet de interactieve modus gebruiken.

Een Hive-oplossing maken en uitvoeren:

1. In de menubalk, gaat u naar **bestand** > **nieuw** > **Project...** .

2. Navigeer in het linkerdeelvenster naar **geïnstalleerde** > **Azure Data Lake** > **HIVE (HDInsight)**.  

3. Selecteer **Hive-toepassing** in het middelste deelvenster. Voer de eigenschappen in en selecteer **OK**.

    ![Schermafbeelding van een nieuw Hive-project in HDInsight Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Hive-toepassingen maken vanuit Visual Studio")

4. Dubbelklik in **Solution Explorer** op **Script.hql** om het script te openen.

### <a name="view-job-summary-and-output"></a>Overzicht van de taak weergeven en uitvoer

Het taakoverzicht verschilt enigszins tussen **Batch** en **interactief** modus.

![Taaksamenvatting](./media/apache-hadoop-visual-studio-tools-get-started/jobSummary.png "samenvatting Hive-taak")

Gebruik de **vernieuwen** knop om bij te werken van de status totdat de taak de status gewijzigd in **voltooid**.  

* Voor de taakdetails van de van **Batch** modus, selecteert u de koppelingen onderaan om te zien **Taakquery**, **Taakuitvoer**, **taaklogboek**, of **Yarn-logboek**.

* Voor de taakdetails van de van **interactief** modus, Zie tabbladen **uitvoer** en **HiveServer2 uitvoer**.

  ![Taakgegevens](./media/apache-hadoop-visual-studio-tools-get-started/tabs.png "taakdetails Hive")

### <a name="view-job-graph"></a>Grafiek van de taak weergeven

Op dit moment worden alleen taakgrafieken weergegeven voor Hive-taken die Tez als de engine voor het uitvoeren gebruiken.  Zie voor meer informatie over het inschakelen van Tez [gebruik Apache Hive in HDInsight](hdinsight-use-hive.md).  Zie ook [Apache Tez gebruiken in plaats van Mapreduce](../hdinsight-hadoop-optimize-hive-query.md#use-apache-tez-instead-of-map-reduce).  

Als u alle operators binnen het hoekpunt wilt bekijken, dubbelklikt u op de hoekpunten van de taakgrafiek. U kunt ook een bepaalde operator aanwijzen voor meer informatie over de operator.

De taakgrafiek mogelijk niet weergegeven, zelfs als Tez is opgegeven als de engine voor het uitvoeren als er geen Tez-toepassing wordt gestart.  Dit gebeurt mogelijk omdat de taak geen DML-instructies bevat of de DML-instructies retourneren kunnen zonder een Tez-toepassing te starten. Bijvoorbeeld, `SELECT * FROM table1` de Tez-toepassing niet gestart.

![Taakgrafiek](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "samenvatting Hive-taak")


### <a name="task-execution-detail"></a>Details van de taak kan worden uitgevoerd

U kunt selecteren uit de taakgrafiek **details van de uitvoering van taak** om op te halen gestructureerde en gevisualiseerde informatie voor Hive-taken. U kunt ook meer taakgegevens ophalen. Als er prestatieproblemen optreden, kunt u de weergave gebruiken om meer informatie over het probleem op te vragen. U kunt bijvoorbeeld informatie krijgen over hoe elke taak werkt, en gedetailleerde informatie over elke taak (gegevens lezen/schrijven, geplande/start-/eindtijd, enzovoort). Gebruik de informatie om taakconfiguraties of systeemarchitectuur bij te stellen op basis van de gevisualiseerde informatie.

![Schermafbeelding van het venster Taakuitvoeringsweergave van Data Lake Visual Studio Tools ](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Taakuitvoeringsweergave")


### <a name="view-hive-jobs"></a>Hive-taken weergeven
U kunt taakquery's, taakuitvoer, logboekbestanden van taken en Yarn-logboekbestanden voor Hive-taken weergeven.

In de meest recente versie van de hulpprogramma's kunt u in uw Hive-taken kijken door Yarn-logboeken te verzamelen en zichtbaar te maken. Een Yarn-logboek kan u helpen bij het onderzoeken van prestatieproblemen. Zie [Programmacode gebruiken om toegang te krijgen tot HDInsight-toepassingslogboeken](../hdinsight-hadoop-access-yarn-app-logs-linux.md) voor informatie over hoe HDInsight Yarn-logboeken verzamelt.

Hive-taken weergeven:

1. Met de rechtermuisknop op een HDInsight-cluster en selecteer **taken weergeven**. Er wordt een lijst weergegeven met de Hive-taken die op het cluster zijn uitgevoerd.  

2. Selecteer een taak. Selecteer in het venster **Samenvatting van Hive-taak** een van de volgende opties:
   - **Taakquery**
   - **Taakuitvoer**
   - **Takenlogboek**  
   - **Yarn-logboek**

     ![Schermafbeelding van het venster Hive-taken in HDInsight Visual Studio Tools](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Hive-taken weergeven")


## <a name="run-apache-pig-scripts"></a>Apache Pig-scripts uitvoeren

1. In de menubalk, gaat u naar **bestand** > **nieuw** > **Project...** .

2. Navigeer in het linkerdeelvenster naar **geïnstalleerde** > **Azure Data Lake** > **Pig (HDInsight)**.  

3. Selecteer in het middelste deelvenster **Pig toepassing**. Voer de eigenschappen in en selecteer **OK**.

4. In **Solution Explorer**, dubbelklikt u op **Script.pig** openen van het script.

## <a name="feedback-and-known-issues"></a>Feedback en bekende problemen
* Een probleem waardoor resultaten beginnend met null-waarden niet worden weergegeven, is opgelost. Neem contact op met het ondersteuningsteam als u vastloopt op dit probleem.
* Het HQL-script dat door Visual Studio wordt gemaakt, word gecodeerd op basis van de lokale regio-instellingen van de gebruiker. Het script wordt niet correct uitgevoerd als u het script als binair bestand naar een cluster uploadt.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u hoe geleerd u het Data Lake Tools-pakket voor Visual Studio kunt gebruiken om verbinding te maken met HDInsight-clusters vanuit Visual Studio. U hebt ook geleerd hoe u een Hive-query uitvoert. Raadpleeg voor meer informatie de volgende artikelen:

* [Apache Hive-query's uitvoeren met behulp van de Data Lake-tools voor Visual Studio](apache-hadoop-use-hive-visual-studio.md)
* [Hadoop Hive in HDInsight gebruiken](hdinsight-use-hive.md)
* [Aan de slag met Apache Hadoop in HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Apache Hadoop-taken in HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Twitter-gegevens met Apache Hadoop in HDInsight analyseren](../hdinsight-analyze-twitter-data-linux.md)

