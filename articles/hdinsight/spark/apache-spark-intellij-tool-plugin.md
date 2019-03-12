---
title: 'Azure Toolkit voor IntelliJ: Spark-toepassingen voor een HDInsight-cluster maken '
description: De Azure Toolkit voor IntelliJ gebruiken voor het ontwikkelen van Spark-toepassingen die zijn geschreven in Scala, en deze verzenden naar een HDInsight Spark-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: maxluk
ms.openlocfilehash: 272ec7298785128caebd850dc488ca01937a3960
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57548504"
---
# <a name="use-azure-toolkit-for-intellij-to-create-apache-spark-applications-for-an-hdinsight-cluster"></a>Azure Toolkit voor IntelliJ gebruiken voor het maken van Apache Spark-toepassingen voor een HDInsight-cluster

Gebruik de Azure Toolkit voor IntelliJ-invoegtoepassing voor het ontwikkelen van [Apache Spark](https://spark.apache.org/) toepassingen die zijn geschreven [Scala](https://www.scala-lang.org/), en deze vervolgens verzenden naar een HDInsight Spark-cluster rechtstreeks vanuit de geïntegreerde IntelliJ-ontwikkeling Environment (IDE). U kunt de invoegtoepassing gebruiken op een aantal manieren:

* Ontwikkelen en het verzenden van een Scala Spark-toepassing in een HDInsight Spark-cluster.
* Toegang tot de resources van uw Azure HDInsight Spark-cluster.
* Ontwikkelen en een Scala Spark-toepassing lokaal uitvoeren.

## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.
* [Oracle Java Development Kit](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).  In deze zelfstudie wordt gebruikgemaakt van Java-versie 8.0.202.
* IntelliJ IDEA. In dit artikel wordt gebruikgemaakt van [IntelliJ IDEA Community-versie  2018.3.4](https://www.jetbrains.com/idea/download/).
* Azure-toolkit voor IntelliJ.  Zie [Installing the Azure Toolkit for IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-installation?view=azure-java-stable) (De Azure Toolkit voor IntelliJ installeren).
* WINUTILS. EXE.  Zie [problemen bij het uitvoeren van Hadoop op Windows](https://wiki.apache.org/hadoop/WindowsProblems).

## <a name="install-scala-plugin-for-intellij-idea"></a>Scala-invoegtoepassing voor IntelliJ IDEA installeren
Voer de volgende stappen uit om de Scala-invoegtoepassing te installeren:

1. Open IntelliJ IDEA.

2. Ga op het welkomstscherm naar **Configure** > **Plugins** om het venster **Plugins** te openen.
   
    ![Scala-invoegtoepassing inschakelen](./media/apache-spark-intellij-tool-plugin/enable-scala-plugin.png)

3. Select **Install** voor de Scala-invoegtoepassing die in het nieuwe venster wordt weergegeven.  

    ![Scala-invoegtoepassing installeren](./media/apache-spark-intellij-tool-plugin/install-scala-plugin.png)

4. Als de invoegtoepassing is geïnstalleerd, moet u de IDE opnieuw starten.


## <a name="create-a-spark-scala-application-for-an-hdinsight-spark-cluster"></a>Een Spark Scala-toepassing voor een HDInsight Spark-cluster maken

1. Start IntelliJ IDEA en selecteer **Create New Project** om het venster **New Project** te openen.

2. Selecteer **Azure Spark/HDInsight** in het linkerdeelvenster.

3. Selecteer **Spark Project (Scala)** in het hoofdvenster.

4. Selecteer in de vervolgkeuzelijst **Build tool** een van de volgende opties:
      * **Maven**, voor de ondersteuning van de wizard Scala-project maken.
      * **SBT**, voor het beheren van de afhankelijkheden en het maken van het Scala-project.

    ![Het dialoogvenster Nieuw project](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

5. Selecteer **Volgende**.

6. Geef in het venster **New project** de volgende gegevens op:  

    |  Eigenschap   | Description   |  
    | ----- | ----- |  
    |Projectnaam| Voer een naam in.  In deze zelfstudie wordt `myApp` gebruikt.|  
    |Project&nbsp;location| Voer de gewenste locatie in om uw project in op te slaan.|
    |Project SDK| Dit kan zijn leeg op het eerste gebruik van IDEE.  Selecteer **New...** en ga naar uw JDK.|
    |Spark-versie|De wizard voor het maken van het project integreert de juiste versie voor Spark SDK en Scala SDK. Selecteer **Spark 1.x** als de Spark-clusterversie ouder is dan 2.0. Selecteer anders **Spark 2.x**. In dit voorbeeld wordt **Spark 2.3.0 (Scala 2.11.8)** gebruikt.|

    ![De Spark SDK selecteren](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

7. Selecteer **Voltooien**.  Het kan enkele minuten duren voordat het project beschikbaar is.

8. Het Spark-project maakt automatisch een artefact voor u. Als u wilt het artefact weergeven, het volgende doen:

   a. In de menubalk, gaat u naar **bestand** > **projectstructuur...** .

   b. Uit de **projectstructuur** venster **artefacten**.  

   c. Selecteer **annuleren** na die het artefact weergeven.

      ![Informatie over de artefact in het dialoogvenster](./media/apache-spark-intellij-tool-plugin/default-artifact.png)

9. De broncode van uw toepassing toevoegen door het volgende te doen:

    a. Vanuit het Project, navigeert u naar **Mijntoep** > **src** > **belangrijkste** > **scala**.  

    b. Met de rechtermuisknop op **scala**, en navigeer vervolgens naar **nieuw** > **Scala klasse**.

   ![Opdrachten voor het maken van een klasse Scala van Project](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   c. In de **nieuwe Scala-klasse maken** dialoogvenster vak, Geef een naam op, selecteer **Object** in de **soort** vervolgkeuzelijst en selecteer vervolgens **OK**.

     ![Dialoogvenster Nieuwe Scala-klasse maken](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   d. De **myApp.scala** bestand vervolgens geopend in de belangrijkste weergave. De standaardcode vervangen door de code hieronder gevonden:  

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object myApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("myApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasbs:///HVACOut")
            }
    
        }

    De code leest de gegevens van HVAC.csv (beschikbaar in alle HDInsight Spark-clusters), haalt de rijen waarvoor slechts één cijfer in de zevende kolom in het CSV-bestand op en schrijft de uitvoer naar `/HVACOut` onder de standaard-storage-container voor het cluster.

## <a name="connect-to-your-hdinsight-cluster"></a>Verbinding maken met uw HDInsight-cluster
De gebruiker kan ofwel [aanmelden bij Azure-abonnement](#sign-in-to-your-azure-subscription), of [koppelt een HDInsight-cluster](#link-a-cluster) met Ambari gebruikersnaam en wachtwoord met gekoppelde domeinen referentie voor de verbinding met uw HDInsight-cluster.

### <a name="sign-in-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

1. In de menubalk, gaat u naar **weergave** > **hulpprogramma Windows** > **Azure Explorer**.
       
   ![De Azure Explorer-koppeling](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. In Azure Explorer met de rechtermuisknop op de **Azure** knooppunt en selecteert u vervolgens **aanmelden**.

3. In de **Azure Sign In** in het dialoogvenster, selecteer **aanmelden**, en voer uw Azure-referenties.

    ![In het dialoogvenster Azure-aanmelding](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. Nadat u bent aangemeld, de **abonnementen selecteren** in het dialoogvenster geeft een lijst van alle Azure-abonnementen die gekoppeld aan de referenties zijn. Selecteer uw abonnement en selecteer vervolgens de **Selecteer** knop.

    ![Het dialoogvenster abonnementen selecteren](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. Van **Azure Explorer**, vouw **HDInsight** om de HDInsight Spark-clusters die in uw abonnement zijn weer te geven.

    ![HDInsight Spark-clusters in Azure Explorer](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. Als u de resources (bijvoorbeeld storage-accounts) die gekoppeld aan het cluster zijn, kunt u een naam van de cluster-knooppunt verder uitbreiden.

    ![Een uitgevouwen naam van de cluster-knooppunt](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

### <a name="link-a-cluster"></a>Een cluster koppelen
U kunt een HDInsight-cluster koppelen met behulp van de gebruikersnaam van de Apache Ambari beheerd. Op dezelfde manier voor een domein gekoppeld HDInsight-cluster kunt u koppelen met behulp van het domein en gebruikersnaam, zoals user1@contoso.com. U kunt ook Livy Service-cluster koppelen.

1. In de menubalk, gaat u naar **weergave** > **hulpprogramma Windows** > **Azure Explorer**.

2. In Azure Explorer met de rechtermuisknop op de **HDInsight** knooppunt en selecteert u vervolgens **Link A Cluster**.

   ![koppeling cluster contextmenu](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

3. De beschikbare opties in de **Link A Cluster** venster varieert, afhankelijk van welke waarde u kiezen uit de **koppeling resourcetype** vervolgkeuzelijst.  Voer de waarden in en selecteer vervolgens **OK**.

    * **HDInsight Cluster**  
  
        |Eigenschap |Value |
        |----|----|
        |Resource-koppelingstype|Selecteer **HDInsight-Cluster** uit de vervolgkeuzelijst.|
        |De naam van de cluster-/ URL| Voer de clusternaam van het.|
        |Authenticatietype| Laat de **basisverificatie**|
        |Gebruikersnaam| Voer de naam van de cluster-gebruiker, standaard is de beheerder.|
        |Wachtwoord| Wachtwoord opgeven voor de gebruikersnaam van de.|
    
        ![dialoogvenster koppeling hdinsight-cluster](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

    * **Livy-Service**  
  
        |Eigenschap |Value |
        |----|----|
        |Resource-koppelingstype|Selecteer **Livy Service** uit de vervolgkeuzelijst.|
        |Livy-eindpunt| Voer Livy-eindpunt|
        |Clusternaam| Voer de clusternaam van het.|
        |Yarn-eindpunt|Optioneel.|
        |Authenticatietype| Laat de **basisverificatie**|
        |Gebruikersnaam| Voer de naam van de cluster-gebruiker, standaard is de beheerder.|
        |Wachtwoord| Wachtwoord opgeven voor de gebruikersnaam van de.|

        ![dialoogvenster livy-cluster koppelen](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)

4. U ziet uw gekoppelde cluster op basis van de **HDInsight** knooppunt.

   ![gekoppelde cluster](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

5. U kunt een cluster op basis van ook ontkoppelen **Azure Explorer**.

   ![niet-gekoppelde cluster](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Een Spark Scala-toepassing uitvoert op een HDInsight Spark-cluster
Na het maken van een Scala-toepassing, kunt u deze indient aan het cluster.

1. Vanuit het Project, navigeert u naar **Mijntoep** > **src** > **belangrijkste** > **scala**  >  **Mijntoep**.  Met de rechtermuisknop op **Mijntoep**, en selecteer **Spark-toepassing indienen** (deze waarschijnlijk in de rechterbenedenhoek van de lijst).
    
      ![De Spark-toepassing verzenden naar HDInsight-opdracht](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. In de **Spark-toepassing indienen** dialoogvenster, selecteer **1. Spark in HDInsight**.

3. In de **Upravit konfiguraci** venster, geef de volgende waarden op en selecteer vervolgens **OK**:

    |Eigenschap |Value |
    |----|----|
    |Spark-clusters (alleen Linux)|Selecteer het HDInsight Spark-cluster waarop u wilt dat uw toepassing uit te voeren.|
    |Selecteer een artefact om in te dienen|Laat de standaardinstelling.|
    |De naam van de Main-klasse|De standaardwaarde is de hoofdklasse van het geselecteerde bestand. U kunt de klasse wijzigen door het beletselteken (**...** ) en het kiezen van een andere klasse.|
    |Configuraties van taak|U kunt de standaardsleutels en/of de waarden wijzigen. Zie voor meer informatie, [Apache Livy REST-API](https://livy.incubator.apache.org./docs/latest/rest-api.html).|
    |Opdrachtregelargumenten|U kunt argumenten gescheiden door ruimte voor de main-klasse, indien nodig kunt invoeren.|
    |Waarnaar wordt verwezen, JAR-bestanden en bestanden waarnaar wordt verwezen|Indien van toepassing, kunt u de paden voor de waarnaar wordt verwezen, JAR-bestanden en bestanden. Voor meer informatie: [Apache Spark Configuration](https://spark.apache.org/docs/latest/configuration.html#runtime-environment).  Zie ook [over het uploaden van resources aan het cluster](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer).|
    |Taakopslag uploaden|Uitvouwen om extra opties weer te geven.|
    |Opslagtype|Selecteer **gebruikt Azure Blob uploaden** uit de vervolgkeuzelijst.|
    |Opslagaccount|Voer uw storage-account.|
    |Opslagsleutel|Voer de opslagsleutel van uw.|
    |Opslagcontainer|Uw storage-container keer selecteren uit de vervolgkeuzelijst **Opslagaccount** en **opslagsleutel** is ingevoerd.|

    ![Het dialoogvenster voor het indienen van Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

4. Selecteer **SparkJobRun** indienen van uw project met het geselecteerde cluster. De **externe Spark-taak in Cluster** tabblad geeft de voortgang van de taak aan de onderkant. U kunt de toepassing door te klikken op de rode knop stoppen. Zie voor informatie over toegang tot de taakuitvoer gaat de ' toegang en HDInsight Spark-clusters beheren met behulp van Azure Toolkit voor IntelliJ ' verderop in dit artikel.  
      
    ![Het indienen van Spark-venster](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)

## <a name="debug-apache-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Fouten opsporen in Apache Spark toepassingen lokaal of extern op een HDInsight-cluster 
We raden u ook aan een andere manier om de Spark-toepassing met het cluster in te dienen. U kunt dit doen door in te stellen van de parameters in de **uitvoeren/Debug configuraties** IDE. Zie voor meer informatie, [Debug Apache Spark-toepassingen lokaal of extern op een HDInsight-cluster met de Azure Toolkit voor IntelliJ via SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Toegang tot en beheer van HDInsight Spark-clusters met behulp van Azure Toolkit voor IntelliJ
U kunt verschillende bewerkingen uitvoeren met behulp van Azure Toolkit voor IntelliJ.  De meeste bewerkingen geïnitieerd worden vanuit **Azure Explorer**.  In de menubalk, gaat u naar **weergave** > **hulpprogramma Windows** > **Azure Explorer**.

### <a name="access-the-job-view"></a>Toegang tot de Project-weergave

1. Vanuit Azure Explorer, navigeert u naar **HDInsight** > \<uw Cluster >> **taken**.

    ![Taak weergeven knooppunt](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

2. In het rechterdeelvenster de **Spark-taak weergeven** tabblad geeft alle toepassingen die zijn uitgevoerd op het cluster. Selecteer de naam van de toepassing waarvoor u wilt meer informatie.

    ![Toepassingsgegevens](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)

3. Als u wilt weergeven basisinformatie actieve taak, de muisaanwijzer over de taakgrafiek. Als u wilt weergeven van de fasen graph en de gegevens die elke taak genereert, selecteer een knooppunt van de taakgrafiek.

    ![Taakdetails fase](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. Om weer te geven vaak gebruikt u zich aanmeldt, zoals *stuurprogramma Stderr*, *stuurprogramma Stdout*, en *Directory Info*, selecteer de **Log** tabblad.

    ![Logboekdetails](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

5. U kunt ook het Spark-overzicht gebruikersinterface en de gebruikersinterface van YARN (op het toepassingsniveau van de) weergeven door een koppeling aan de bovenkant van het venster te selecteren.

### <a name="access-the-spark-history-server"></a>Toegang tot de server van de geschiedenis van Spark

1. Vouw in Azure Explorer **HDInsight**, met de rechtermuisknop op de naam van uw Spark-cluster en selecteer vervolgens **Open Spark geschiedenis gebruikersinterface**.  
2. Wanneer u wordt gevraagd, typt u de beheerdersreferenties van het cluster, dat u hebt opgegeven bij het instellen van het cluster.

3. Op het dashboard van de server in de Spark-geschiedenis, kunt u de naam van de toepassing om te zoeken naar de toepassing dat u zojuist hebt uitgevoerd. In de bovenstaande code stelt u de naam van de toepassing met behulp van `val conf = new SparkConf().setAppName("myApp")`. De naam van uw Spark-toepassing is daarom **Mijntoep**.

### <a name="start-the-ambari-portal"></a>Start de Ambari-portal

1. Vouw in Azure Explorer **HDInsight**, met de rechtermuisknop op de naam van uw Spark-cluster en selecteer vervolgens **Open Cluster Management Portal(Ambari)**.  

2. Wanneer u wordt gevraagd, typt u de beheerdersreferenties voor het cluster. U hebt deze referenties hebt opgegeven tijdens het installatieproces van het cluster.

### <a name="manage-azure-subscriptions"></a>Azure-abonnementen beheren
Azure Toolkit voor IntelliJ bevat standaard het Spark-clusters van al uw Azure abonnementen. Indien nodig, kunt u de abonnementen die u wilt openen.  

1. In Azure Explorer met de rechtermuisknop op de **Azure** hoofd-knooppunt, en selecteer vervolgens **abonnementen selecteren**.  

2. Uit de **abonnementen selecteren** venster, schakel de selectievakjes naast de abonnementen die u niet wilt openen en selecteer vervolgens **sluiten**.

## <a name="spark-console"></a>Spark Console
U kunt Spark lokale Console(Scala) uitvoeren of Spark Livy interactieve sessie Console(Scala) uitvoeren.

### <a name="spark-local-consolescala"></a>Spark Local Console(Scala)
Zorg ervoor dat u hebt voldaan aan de WINUTILS. EXE vereiste.

1. In de menubalk, gaat u naar **uitvoeren** > **configuraties bewerken...** .

2. Uit de **uitvoeren/Debug configuraties** in het linkerdeelvenster van het venster gaat u naar **Apache Spark in HDInsight** > **[Spark in HDInsight] Mijntoep**.

3. Selecteer in het hoofdvenster de **lokaal uitvoeren** tabblad.

4. Geef de volgende waarden op en selecteer vervolgens **OK**:

    |Eigenschap |Value |
    |----|----|
    |Taak main-klasse|De standaardwaarde is de hoofdklasse van het geselecteerde bestand. U kunt de klasse wijzigen door het beletselteken (**...** ) en het kiezen van een andere klasse.|
    |Omgevingsvariabelen|Zorg ervoor dat de waarde voor HADOOP_HOME juist is.|
    |WINUTILS.exe locatie|Zorg ervoor dat het pad juist is.|

    ![Configuratie van de lokale Console](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)

5. Vanuit het Project, navigeert u naar **Mijntoep** > **src** > **belangrijkste** > **scala**  >  **Mijntoep**.  

6. In de menubalk, gaat u naar **extra** > **Spark Console** > **lokale Console(Scala) Spark uitgevoerd**.

7. Twee dialoogvensters kunnen vervolgens weergegeven om aan te vragen u als u wilt automatisch afhankelijkheden oplossen. Als dit het geval is, selecteert u **automatisch oplossen**.

    ![Spark Auto Fix1](./media/apache-spark-intellij-tool-plugin/console-auto-fix1.png)

    ![Spark Auto Fix2](./media/apache-spark-intellij-tool-plugin/console-auto-fix2.png)

8. De console moet eruitzien als op de volgende afbeelding. Typ in de console-venster `sc.appName`, en druk op ctrl + Enter.  Het resultaat wordt weergegeven. U kunt de lokale console door te klikken op de rode knop beëindigen.

    ![Resultaat van de lokale Console](./media/apache-spark-intellij-tool-plugin/local-console-result.png)


### <a name="spark-livy-interactive-session-consolescala"></a>Spark Livy Interactive Session Console(Scala)
Het wordt alleen ondersteund op IntelliJ 2018.2 en 2018.3.

1. In de menubalk, gaat u naar **uitvoeren** > **configuraties bewerken...** .

2. Uit de **uitvoeren/Debug configuraties** in het linkerdeelvenster van het venster gaat u naar **Apache Spark in HDInsight** > **[Spark in HDInsight] Mijntoep**.

3. Selecteer in het hoofdvenster de **op afstand uitvoeren in Cluster** tabblad.

4. Geef de volgende waarden op en selecteer vervolgens **OK**:

    |Eigenschap |Value |
    |----|----|
    |Spark-clusters (alleen Linux)|Selecteer het HDInsight Spark-cluster waarop u wilt dat uw toepassing uit te voeren.|
    |De naam van de Main-klasse|De standaardwaarde is de hoofdklasse van het geselecteerde bestand. U kunt de klasse wijzigen door het beletselteken (**...** ) en het kiezen van een andere klasse.|

    ![Configuratie van de interactieve Console](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

5. Vanuit het Project, navigeert u naar **Mijntoep** > **src** > **belangrijkste** > **scala**  >  **Mijntoep**.  

6. In de menubalk, gaat u naar **extra** > **Spark Console** > **uitvoeren Spark Livy interactieve sessie Console(Scala)**.

7. De console moet eruitzien als op de volgende afbeelding. Typ in de console-venster `sc.appName`, en druk op ctrl + Enter.  Het resultaat wordt weergegeven. U kunt de lokale console door te klikken op de rode knop beëindigen.

    ![Resultaat van interactieve Console](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Selectie naar Spark Console verzenden

Het is handig voor u te voorzien van het scriptresultaat door code te sturen naar de lokale console of via Livy interactieve sessie Console(Scala). U kunt code in het bestand Scala markeren en vervolgens met de rechtermuisknop op **selectie verzenden naar Spark Console**. De geselecteerde code verzonden naar de console en worden uitgevoerd. Het resultaat wordt weergegeven na de code in de console. De console Controleer de fouten als bestaande.  

   ![Selectie naar Spark Console verzenden](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Converteren van bestaande IntelliJ IDEA-toepassingen met Azure Toolkit voor IntelliJ

U kunt de bestaande Spark Scala converteren toepassingen die u hebt gemaakt in IntelliJ IDEA voor compatibiliteit met Azure-Toolkit voor IntelliJ. Vervolgens kunt u de invoegtoepassing voor het verzenden van de toepassingen naar een HDInsight Spark-cluster.

1. Voor een bestaande Spark Scala-toepassing die is gemaakt via IntelliJ IDEA, het bijbehorende .iml-bestand te openen.

1. In de hoofdmap niveau is een **module** element als volgt:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Bewerken van het element toe te voegen `UniqueKey="HDInsightTool"` zodat de **module** element ziet er als volgt uit:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

1. Sla de wijzigingen op. Nu moet uw toepassing compatibel zijn met de Azure Toolkit voor IntelliJ. U kunt dit testen met de rechtermuisknop op de naam van het project in Project. Het pop-upmenu heeft nu de optie **Spark-toepassing verzenden naar HDInsight**.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="error-in-local-run-use-a-larger-heap-size"></a>Fout bij lokaal uitvoeren: *Gebruik een grotere heapgrootte*
Als u een 32-bits-SDK voor Java bij lokaal uitvoeren, kunt u in Spark 1.6, de volgende fouten optreden:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Deze fouten optreden omdat de heapgrootte is niet groot genoeg zijn voor Spark om uit te voeren. Spark vereist ten minste 471 MB. (Zie voor meer informatie, [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) Een eenvoudige oplossing is het gebruik van een 64-bits Java SDK. U kunt ook de JVM-instellingen in IntelliJ wijzigen door toe te voegen van de volgende opties:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Opties toe te voegen aan het vak 'VM-opties' in IntelliJ](./media/apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>Veelgestelde vragen
Als het cluster bezet is, kunt u de onderstaande foutmelding krijgen.

![Intellij de foutmelding wanneer cluster bezet](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-upload.png)

![Intellij de foutmelding wanneer cluster bezet](./media/apache-spark-intellij-tool-plugin/intellij-interactive-cluster-busy-submit.png)

## <a name="feedback-and-known-issues"></a>Feedback en bekende problemen
Op dit moment wordt bekijken Spark uitvoer rechtstreeks niet ondersteund.

Als u suggesties of feedback hebt, of als u problemen ondervindt wanneer u deze invoegtoepassing gebruikt, een e-mail verzenden naar hdivstool@microsoft.com.

## <a name="seealso"></a>Volgende stappen
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demo
* Maak Scala-project (video): [Apache Spark Scala-toepassingen maken](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Externe foutopsporing (video): [Azure Toolkit voor IntelliJ voor foutopsporing van Apache Spark-toepassingen op afstand op HDInsight-Cluster gebruiken](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met BI: Interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-hulpprogramma 's](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Het maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure Toolkit voor IntelliJ voor foutopsporing van Apache Spark-toepassingen op afstand via VPN-verbinding gebruiken](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure Toolkit voor IntelliJ gebruiken voor foutopsporing van Apache Spark-toepassingen op afstand via SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het maken van Apache Spark-toepassingen](apache-spark-eclipse-tool-plugin.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
