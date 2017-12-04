---
title: Verbinding maken met Azure HDInsight met behulp van Data Lake Tools voor Visual Studio | Microsoft Docs
description: Leer hoe u Data Lake Tools voor Visual Studio installeert en gebruikt om verbinding te maken met Hadoop-clusters in Azure HDInsight en Hive-query's uit te voeren.
keywords: hadoop-hulpprogramma's, hive-query, visual studio, visual studio hadoop
services: HDInsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ce9c572a-1e98-46bf-9581-13a9767f1fa5
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: jgao
ms.openlocfilehash: 1e9d5ca475424c99b30c62252f4b0abc9bd09078
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="connect-to-azure-hdinsight-and-run-hive-queries-using-data-lake-tools-for-visual-studio"></a>Verbinding maken met Azure HDInsight en Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio

Leer hoe u Data Lake Tools voor Visual Studio (ook wel Azure Data Lake en Stream Analytics-hulpprogramma's genoemd) gebruikt om verbinding te maken met Hadoop-clusters in [Azure HDInsight](../hdinsight-hadoop-introduction.md) en om Hive-query's uit te voeren. Zie [Inleiding tot HDInsight](../hdinsight-hadoop-introduction.md) en [Aan de slag met HDInsight](apache-hadoop-linux-tutorial-get-started.md) voor meer informatie over het gebruik van HDInsight. Zie [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](../storm/apache-storm-develop-csharp-visual-studio-topology.md) (C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Data Lake Tools voor Visual Studio) voor meer informatie over het maken van verbinding met een Storm-cluster.

Data Lake Tools voor Visual Studio kan worden gebruikt voor toegang tot Data Lake Analytics en HDInsight.  Zie voor informatie over Data Lake Tools de [Zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](../../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md).

**Vereisten**

Al u deze zelfstudie wilt voltooien en Data Lake Tools in Visual Studio wilt gebruiken, hebt u de volgende items nodig:

* Een Azure HDInsight-cluster: zie [Aan de slag met Hadoop in Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md) om een cluster te maken. Als u wilt leren hoe u interactieve Hive-query's uitvoert, hebt u een [HDInsight Interactive Query-cluster](../interactive-query/apache-interactive-query-get-started.md) nodig.
* Een werkstation met Visual Studio 2013/2015/2017.
    
    > [!NOTE]
    > Op dit moment wordt Data Lake Tools voor Visual Studio alleen geleverd bij de Engelse versie.
    > 
    > 

## <a name="install-and-upgrade-data-lake-tools-for-visual-studio"></a>Data Lake Tools voor Visual Studio installeren en bijwerken

Data Lake Tools is standaard geïnstalleerd voor Visual Studio 2017. Voor oudere versies van Visual Studio kunt u deze installeren via het [webplatforminstallatieprogramma](https://www.microsoft.com/web/downloads/). U moet de optie kiezen die overeenkomt met uw versie van Visual Studio. Als u Visual Studio niet hebt geïnstalleerd, kunt u de meest recente Visual Studio Community en Azure SDK installeren met het [webplatforminstallatieprogramma](https://www.microsoft.com/web/downloads/):

![Data Lake Tools voor Visual Studio webplatforminstallatieprogramma.](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png "Data Lake Tools voor Visual Studio installeren met het webplatforminstallatieprogramma")

**De hulpprogramma's bijwerken**
1. Open Visual Studio.
2. Klik in het menu **Extra** op **Extensies en updates**.
3. Vouw **Updates** uit en werk **Azure Data Lake en Stream Analytics-hulpprogramma's** bij (indien aanwezig).

> [!NOTE]
>
> Alleen in versie 2.3.0.0 of hoger wordt ondersteuning geboden voor het maken van verbinding met Interactive Query-clusters en het uitvoeren van interactieve Hive-query's.

## <a name="connect-to-azure-subscriptions"></a>Verbinding maken met Azure-abonnementen
Met Data Lake Tools voor Visual Studio kunt u verbinding maken met uw HDInsight-clusters, enkele eenvoudige beheerbewerkingen uitvoeren en Hive-query's uitvoeren.

> [!NOTE]
> Zie [Write and submit Hive queries using Visual Studio](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx) (Hive-query’s schrijven en verzenden met Visual Studio) voor informatie over het maken van verbinding met een algemeen Hadoop-cluster.
> 
> 

**Verbinding maken met uw Azure-abonnement**

1. Open Visual Studio.
2. Klik in het menu **Weergeven** op **Server Explorer** om het venster Server Explorer te openen.
3. Vouw **Azure** uit en vouw vervolgens **HDInsight** uit.
   
   > [!NOTE]
   > U ziet dat het venster **Taakoverzicht HDInsight** is geopend. Als het venster niet wordt weergegeven, klikt u op **Other Windows** (Overige vensters) in het menu **View** (Beeld) en klikt u vervolgens op **HDInsight Task List Window** (Lijstvenster met HDInsight-taken).  
   > 
   > 
4. Voer de referenties voor uw Azure-abonnement in en klik vervolgens op **Aanmelden**. Verificatie is alleen vereist als u op dit werkstation nooit verbinding hebt gemaakt met het Azure-abonnement vanuit Visual Studio.
5. In Server Explorer wordt een lijst met bestaande HDInsight-clusters weergegeven. Als u geen clusters hebt, kunt u een cluster maken door gebruik te maken van Azure Portal, Azure PowerShell of de HDInsight SDK. Zie [HDInsight-clusters maken](../hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie.
   
   ![Lijst met clusters in Server Explorer van Data Lake Tools voor Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png "Server Explorer van Data Lake Tools voor Visual Studio")
6. Een HDInsight-cluster uitbreiden. U ziet **Hive-databases**, een standaardopslagaccount, gekoppelde opslagaccounts en een **Hadoop Service-logboek**. U kunt de entiteiten verder uitbreiden.

Nadat u verbinding hebt gemaakt met het Azure-abonnement, kunt u de volgende taken uitvoeren:

**Verbinding maken met Azure Portal vanuit Visual Studio**

* Vouw in Server Explorer **Azure** > **HDInsight** uit, klik met de rechtermuisknop op een HDInsight-cluster en klik vervolgens op **Cluster beheren in Azure Portal**.

**Vragen stellen en feedback geven vanuit Visual Studio**

* Klik in het menu **Extra** op **HDInsight** en klik vervolgens op **MSDN-forum** om vragen te stellen of klik op **Feedback geven**.

## <a name="navigate-the-linked-resources"></a>Door de gekoppelde resources navigeren
In Server Explorer worden het standaardaccount voor opslag en alle gekoppelde opslagaccounts weergegeven. Als u het standaardopslagaccount uitvouwt, kunt u de containers op het opslagaccount zien. Het standaardopslagaccount en de standaardcontainer worden gemarkeerd. U kunt ook met de rechtermuisknop op een van de containers klikken om de inhoud weer te geven.

![Lijst met gekoppelde resources in Server Explorer van Data Lake Tools voor Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png "lijst met gekoppelde resources")

Nadat u een container hebt geopend, kunt u met de volgende knoppen blobs uploaden, verwijderen en downloaden:

![Blob-bewerkingen in Server Explorer van Data Lake Tools voor Visual Studio](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.blob.operations.png "blobs uploaden, verwijderen en downloaden")

## <a name="run-interactive-hive-queries"></a>Interactieve Hive-query's uitvoeren
[Apache Hive](http://hive.apache.org) is een datawarehouse-infrastructuur die is gebouwd op Hadoop om overzichten, query's en analyses te leveren. Data Lake Tools voor Visual Studio biedt ondersteuning voor het uitvoeren van Hive-query's vanuit Visual Studio. Zie [Hive gebruiken met HDInsight](hdinsight-use-hive.md) voor meer informatie over Hive.

[Interactive Query](../interactive-query/apache-interactive-query-get-started.md) maakt gebruik van [Hive op LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) in Apache Hive 2.1 en biedt interactiviteit voor complexe datawarehouse-query's voor grote opgeslagen gegevenssets. Het uitvoeren van Hive-query's in Interactive Query is veel sneller in vergelijking met de traditionele Hive-batchtaken.  Zie [Hive-batchtaken uitvoeren](#run-hive-batch-jobs) voor meer informatie over het uitvoeren van Hive-batchtaken.

> [!note]
>
> Het uitvoeren van interactieve Hive-query's wordt alleen ondersteund wanneer u verbinding maakt met een [HDInsight Interactive Query](../interactive-query/apache-interactive-query-get-started.md)-cluster.

Met Data Lake Tools voor Visual Studio kunnen gebruikers ook in de Hive-taak kijken door de YARN-logboeken van bepaalde Hive-taken te verzamelen en op te halen.

### <a name="view-the-hivesampletable"></a>De **hivesampletable** bekijken
Alle HDInsight-clusters bevatten een voorbeeld van een Hive-tabel met de naam*hivesampletable*. In deze Hive-tabel ziet u hoe u een lijst met Hive-tabellen maakt, en de tabelschema's en de rijen in de Hive-tabel weergeeft.

**Een lijst met Hive-tabellen maken en het Hive-tabelschema weergeven**

1. Vouw in **Server Explorer** achtereenvolgens **Azure** > **HDInsight** > het gewenste cluster > **Hive-databases** > **Standaard** > **hivesampletable** uit om het tabelschema weer te geven.
2. Klik met de rechtermuisknop op de **hivesampletable** en klik vervolgens op **View Top 100 Rows** (Top 100 rijen weergeven) om de rijen weer te geven. Dit is gelijk aan het uitvoeren van de volgende Hive-query met het Hive ODBC-stuurprogramma:
   
     SELECTEER * UIT hivesampletable LIMIET 100
   
   U kunt het aantal rijen aanpassen.
   
   ![Data Lake Tools: HDInsight Hive Visual Studio-schemaquery](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png "Resultaten van Hive-query")

### <a name="create-hive-tables"></a>Hive-tabellen maken
U kunt de GUI of Hive-query's gebruiken om een Hive-tabel te maken. Zie [Hive-query's uitvoeren](#run.queries) voor meer informatie over het gebruik van Hive-query's.

**Een Hive-tabel maken**

1. Vouw in **Server Explorer** achtereenvolgens **Azure** > **HDInsight-clusters** een HDInsight-cluster > **Hive-databases** uit. Klik vervolgens met de rechtermuisknop op **standaard** en klik op **Tabel maken**.
2. Configureer de tabel.
3. Klik op **Tabel maken** om de taak voor het maken van de nieuwe Hive-tabel te verzenden.
   
    ![Data Lake Tools: HDInsight Visual Studio Tools: Hive-tabel maken](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png "Hive-tabel maken")

### <a name="run.queries"></a>Hive-query’s valideren en uitvoeren
Er zijn twee manieren om Hive-query's te maken en uit te voeren:

* Ad-hocquery's maken
* Een Hive-toepassing maken

**Ad-hocquery's maken, valideren en uitvoeren**

1. Vouw in **Server Explorer** achtereenvolgens **Azure** en **HDInsight-clusters** uit.
2. Klik met de rechtermuisknop op het cluster waar u de query wilt uitvoeren en klik vervolgens op **Write a Hive Query** (Een Hive-query schrijven).
3. Voer de Hive-query's in Zoals u kunt zien, biedt de Hive-editor ondersteuning voor IntelliSense. Data Lake Tools voor Visual Studio biedt ondersteuning voor het laden van de externe metagegevens wanneer u het Hive-script bewerkt. Wanneer u bijvoorbeeld SELECT * FROM typt, geeft IntelliSense alle voorgestelde tabelnamen weer. Wanneer een tabelnaam is opgegeven, worden de kolomnamen door de IntelliSense vermeld. De hulpprogramma’s ondersteunen bijna alle DML-instructies, subquery's en ingebouwde UDF's van Hive.
   
    ![Data Lake Tools: HDInsight Visual Studio Tools IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png "U-SQL IntelliSense")
   
    ![Data Lake Tools: HDInsight Visual Studio Tools IntelliSense](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > Alleen de metagegevens van het cluster dat in HDInsight Toolbar is geselecteerd, wordt voorgesteld.
   > 
   > 
4. (Optioneel) Klik op **Script valideren** om het script te controleren op syntaxisfouten.
   
    ![Data Lake Tools: Data Lake Tools voor Visual Studio: lokale validatie](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png "Script valideren")
5. Klik op **Verzenden** of **Verzenden (geavanceerd)**. Met de optie voor geavanceerd verzenden configureert u de **Taaknaam**, **Argumenten**, **Aanvullende configuraties** en **Statusmap** voor het script:
   
    ![HDInsight Hadoop Hive-query](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Query's verzenden")
   
    Nadat u de taak hebt verzonden, ziet u een venster met een **samenvatting van de Hive-taak**.
   
    ![Samenvatting van een HDInsight Hadoop Hive-query](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png "Samenvatting van Hive-taak")
6. Gebruik de knop **Vernieuwen** om de status bij te werken totdat de status van de taak is gewijzigd in **Voltooid**.
7. Klik op de koppelingen onderaan om het volgende weer te geven: **Taakquery**, **Taakuitvoer**, **Logboek** of **Yarn-logboek**.

**Een Hive-oplossing maken en uitvoeren**

1. Klik in het menu **FILE** op **Nieuw** en klik vervolgens op **Project**.
2. Selecteer in het linkerdeelvenster **HDInsight** selecteer in het middelste venster **Hive-toepassing**, geef de eigenschappen op en klik vervolgens op **OK**.
   
    ![Data Lake Tools: HDInsight Visual Studio Tools: nieuw Hive-project](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Hive-toepassingen maken vanuit Visual Studio")
3. Dubbelklik in **Solution Explorer** op **Script.hql** om het script te openen.
4. Als u het Hive-script wilt valideren, klikt u op de knop **Script valideren** of klikt u met de rechtermuisknop in de Hive-editor en klikt u in het contextmenu vervolgens op **Script valideren**.

### <a name="view-hive-jobs"></a>Hive-taken weergeven
U kunt taakquery's, taakuitvoer, logboekbestanden van taken en Yarn-logboekbestanden voor Hive-taken weergeven. Zie de vorige schermafbeelding voor meer informatie.

Met de meest recente versie van de hulpprogramma’s kunt u in de Hive-taak kijken door de YARN-logboeken van bepaalde Hive-taken te verzamelen en op te halen. Een YARN-logboek kan u helpen om prestatieproblemen te onderzoeken. Zie [Programmacode gebruiken om toegang te krijgen tot HDInsight-toepassingslogboeken](../hdinsight-hadoop-access-yarn-app-logs.md) voor informatie over hoe HDInsight YARN-logboeken verzamelt.

**Hive-taken weergeven**

1. Vouw in **Server Explorer** achtereenvolgens **Azure** en **HDInsight** uit.
2. Klik met de rechtermuisknop op een HDInsight-cluster en klik vervolgens op **Taken weergeven**. U ziet een lijst met de Hive-taken die in het cluster zijn uitgevoerd.
3. Klik op een taak in de lijst om de taak te selecteren en gebruik vervolgens het venster **Hive Job Summary** (Samenvatting Hive-taak) om **Job Query** (Taakquery), **Job Output** (Taakuitvoer), **Job Log** (Logboekbestand van taak) of **Yarn log** (Yarn-logboekbestand) te openen.
   
    ![Data Lake Tools: Hive-taken in HDInsight Visual Studio Tools weergeven](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png "Hive-taken weergeven")

### <a name="faster-path-hive-execution-via-hiveserver2"></a>Sneller pad voor het uitvoeren van Hive via HiveServer2
> [!NOTE]
> Deze functie werkt alleen op HDInsight-cluster versie 3.2 en nieuwer.
> 
> 

De Data Lake Tools die worden gebruikt om Hive-taken via [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) te verzenden (ook wel bekend als Templeton). Het duurde lang om taakgegevens en informatie over de fout te retourneren.
De Data Lake Tools lossen dit probleem op door de Hive-taken rechtstreeks via HiveServer2 uit voeren in het cluster, zodat RDP/SSH wordt omzeild.
Naast betere prestaties kunnen gebruikers Hive ook weergeven op Tez-grafieken en de taakdetails weergeven.

Voor HDInsight-cluster versie 3.2 of hoger wordt de knop **Execute via HiveServer2** (Uitvoeren via HiveServer2) weergegeven:

![Data Lake Tools van Visual Studio uitvoeren via hiveserver2](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png "Hive-query's uitvoeren met behulp van HiveServer2")

U kunt bovendien zien hoe de logboeken in realtime terug worden gestreamd. Daarnaast kunt u de taakgrafieken bekijken als de Hive-query wordt uitgevoerd in Tez.

![Data Lake Tools van Visual Studio: snel pad voor het uitvoeren van hive](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png "taakgrafieken bekijken")

**Het verschil tussen het uitvoeren van query's via HiveServer2 en het verzenden van query's via WebHCat**

Het uitvoeren van query's via HiveServer2 biedt verschillende prestatievoordelen biedt, maar heeft ook enkele beperkingen. Sommige beperkingen zijn niet geschikt voor productiegebruik. In de volgende tabel kunt u de verschillen zien:

|  | Via HiveServer2 uitvoeren | Verzenden via WebHCat |
| --- | --- | --- |
| Query's uitvoeren |Hiermee wordt de overhead geëlimineerd in WebHCat (die MapReduce-taak met de naam TempletonControllerJob start). |Als een query wordt uitgevoerd met WebHCat, start in WebHCat een MapReduce-taak die resulteert in extra latentie. |
| Logboeken terugstreamen |Nagenoeg in realtime. |De logboekbestanden voor het uitvoeren van de taak zijn alleen beschikbaar wanneer de taak is voltooid. |
| Taakgeschiedenis weergeven |Als een query wordt uitgevoerd via HiveServer2, blijft de taakgeschiedenis (taaklogboek, taakuitvoer) niet behouden. De toepassing kan worden weergegeven in de gebruikersinterface van YARN met beperkte informatie. |Als een query wordt uitgevoerd met WebHCat, blijft de taakgeschiedenis (taaklogboek, taakuitvoer) bewaard en kan deze worden weergegeven met Visual Studio/HDInsight SDK/PowerShell. |
| Venster sluiten |Uitvoeren met HiveServer2 is een zogenaamde synchrone methode. Dit betekent dat de vensters geopend moeten blijven. Als u de vensters sluit, wordt de uitvoering van de query geannuleerd. |Verzenden via WebHCat is een 'asynchrone' methode. Dit betekent dat u de query via WebHCat kunt verzenden en Visual Studio kunt sluiten. U kunt op elk gewenst moment terugkeren om de resultaten te bekijken. |

### <a name="tez-hive-job-performance-graph"></a>Prestatiegrafiek Tez Hive-taak
Data Lake Tools bieden ondersteuning voor de weergave van de prestatiegrafieken voor de Hive-taken die worden uitgevoerd door de Tez-uitvoeringsengine. Zie [Hive in HDInsight gebruiken](hdinsight-use-hive.md) voor informatie over het inschakelen van Tez. Nadat u een Hive-taak in Visual Studio hebt ingediend, wordt in Visual Studio de grafiek weergegeven als de taak is voltooid.  Mogelijk moet u op de knop **Vernieuwen** klikken om de meest recente taakstatus op te halen.

> [!NOTE]
> Deze functie is alleen beschikbaar voor clusters in een hogere versies dan HDInsight 3.2.4.593 en werkt alleen voor voltooide taken (als u uw taak via WebHCat hebt verzonden). Deze grafiek wordt weergegeven wanneer u de query uitvoert met HiveServer2. 
> 
> 

![hadoop hive tez prestatiegrafiek](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png "Taakstatus")

De weergave Hive Operator is toegevoegd aan deze release voor meer inzicht in uw Hive-query. U hoeft alleen maar te dubbelklikken op de hoekpunten van de taakgrafiek om alle operators in het hoekpunt weer te geven. U kunt ook een bepaalde operator aanwijzen om meer informatie over deze operator weer te geven.

### <a name="task-execution-view-for-hive-on-tez-jobs"></a>Taakuitvoeringsweergave voor Hive op Tez-taken
De taakuitvoeringsweergave voor Hive op Tez-taken kan worden gebruikt om gestructureerde en gevisualiseerde informatie voor Hive-taken en meer taakdetails op te halen. Bij prestatieproblemen kunt u de weergave gebruiken om meer details op te vragen. Bijvoorbeeld hoe elke taak functioneert en de gedetailleerde informatie over elke taak (gegevens lezen/schrijven, start-/eindtijd van het schema enzovoort), zodat u de taakconfiguraties of de systeemarchitectuur op basis van de gevisualiseerde informatie kunt optimaliseren.

![Data Lake Visual Studio Tools taakuitvoeringsweergave](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png "Taakuitvoeringsweergave")

## <a name="run-hive-batch-jobs"></a>Hive-batchtaken uitvoeren
[Apache Hive](http://hive.apache.org) is een datawarehouse-infrastructuur die is gebouwd op Hadoop om overzichten, query's en analyses te leveren. Data Lake Tools voor Visual Studio biedt ondersteuning voor het uitvoeren van Hive-query's vanuit Visual Studio. Zie [Hive gebruiken met HDInsight](hdinsight-use-hive.md) voor meer informatie over Hive.

Het kost veel tijd om een Hive-script te testen in een HDInsight-cluster, met uitzondering van het Interactive Query-cluster. Dit kan enkele minuten of langer duren. Data Lake Tools voor Visual Studio kan het Hive-script lokaal valideren zonder dat er verbinding hoeft te worden gemaakt met een live cluster. Zie [Interactieve Hive-query's uitvoeren](#run-interactive-hive-queries) voor meer informatie over het uitvoeren van interactieve query's.

Met Data Lake Tools voor Visual Studio kunnen gebruikers ook in de Hive-taak kijken door de YARN-logboeken van bepaalde Hive-taken te verzamelen en op te halen.

Zie de sectie [Interactieve Hive-query's uitvoeren](#run-interactive-hive-queries) voor meer informatie over het uitvoeren van Hive-batchtaken. De informatie in deze sectie is van toepassing op het uitvoeren van langdurigere Hive-batchtaken.

## <a name="run-pig-scripts"></a>Pig-scripts uitvoeren
Data Lake Tools voor Visual Studio ondersteunt het maken en verzenden van Pig-scripts naar HDInsight-clusters. Gebruikers kunnen een Pig-project maken op basis van een sjabloon en het script vervolgens verzenden naar HDInsight-clusters.

## <a name="feedbacks--known-issues"></a>Feedback en bekende problemen
* Momenteel worden de HiveServer2-resultaten weergegeven als tekst. Dit is niet ideaal. Microsoft werkt aan een oplossing hiervoor.
* Als de resultaten zijn gestart met NULL-waarden, worden de resultaten momenteel niet weergegeven. Dit probleem is opgelost. Als u desondanks vastloopt op dit probleem, neemt u contact op met het ondersteuningsteam.
* Het HQL-script dat door Visual Studio wordt gemaakt, word gecodeerd op basis van de lokale regio-instellingen van de gebruiker. Het script wordt mogelijk niet correct uitgevoerd als de gebruiker het script als een binair bestand uploadt naar het cluster.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u de Data Lake (HDInsight) Tools kunt gebruiken om vanuit Visual Studio verbinding te maken met HDInsight-clusters en hoe u een Hive-query uitvoert. Zie voor meer informatie:

* [Hadoop Hive in HDInsight gebruiken](hdinsight-use-hive.md)
* [Aan de slag met Hadoop in HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Hadoop-taken opgeven in HDInsight](submit-apache-hadoop-jobs-programmatically.md)
* [Twitter-gegevens met Hadoop analyseren in HDInsight](../hdinsight-analyze-twitter-data.md)

