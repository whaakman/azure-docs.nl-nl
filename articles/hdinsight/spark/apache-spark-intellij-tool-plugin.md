---
title: 'Azure Toolkit voor IntelliJ: Spark-toepassingen voor een HDInsight-cluster maken '
description: De Azure Toolkit voor IntelliJ gebruiken voor het ontwikkelen van Spark-toepassingen die zijn geschreven in Scala, en deze verzenden naar een HDInsight Spark-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: maxluk
ms.openlocfilehash: ff7cfcd56158bd38d031a29a21247fb9eb6b91f9
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289067"
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a>Azure Toolkit voor IntelliJ gebruiken voor het maken van Spark-toepassingen voor een HDInsight-cluster

De Azure Toolkit voor IntelliJ-invoegtoepassing gebruiken voor het ontwikkelen van Spark-toepassingen die zijn geschreven in Scala, en deze vervolgens verzenden naar een HDInsight Spark-cluster rechtstreeks vanuit de geïntegreerde ontwikkelomgeving (IDE) van IntelliJ. U kunt de invoegtoepassing gebruiken op een aantal manieren:

* Ontwikkelen en het verzenden van een Scala Spark-toepassing in een HDInsight Spark-cluster.
* Toegang tot de resources van uw Azure HDInsight Spark-cluster.
* Ontwikkelen en een Scala Spark-toepassing lokaal uitvoeren.

Voor het maken van uw project, geven de [Spark-toepassingen maken met de Azure Toolkit voor IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.

> [!IMPORTANT]
> U kunt deze invoegtoepassing gebruiken om te maken en verzenden van toepassingen alleen voor een HDInsight Spark-cluster op Linux.
> 

## <a name="prerequisites"></a>Vereisten

- Een Apache Spark-cluster in HDInsight Linux. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.
- Oracle Java Development Kit. U kunt installeren vanuit de [Oracle website](https://aka.ms/azure-jdks).
- IntelliJ IDEA. In dit artikel wordt versie 2017.1. U kunt installeren vanuit de [JetBrains website](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Azure Toolkit voor IntelliJ installeren
Zie voor installatie-instructies [Installeer Azure Toolkit voor IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).

## <a name="get-started"></a>Aan de slag
De gebruiker kan ofwel [aanmelden bij Azure-abonnement](#sign-in-to-your-azure-subscription), of [koppelt een HDInsight-cluster](#link-a-cluster) met Ambari gebruikersnaam en wachtwoord met gekoppelde domeinen referentie om te starten.


## <a name="sign-in-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

1. Start de IDE IntelliJ en Azure Explorer openen. Op de **weergave** in het menu **hulpprogramma Windows**, en selecteer vervolgens **Azure Explorer**.
       
   ![De Azure Explorer-koppeling](./media/apache-spark-intellij-tool-plugin/show-azure-explorer.png)

1. Met de rechtermuisknop op de **Azure** knooppunt en selecteert u vervolgens **aanmelden**.

1. In de **Azure Sign In** in het dialoogvenster, selecteer **aanmelden**, en voer uw Azure-referenties.

    ![In het dialoogvenster Azure-aanmelding](./media/apache-spark-intellij-tool-plugin/view-explorer-2.png)

1. Nadat u bent aangemeld, de **abonnementen selecteren** in het dialoogvenster geeft een lijst van alle Azure-abonnementen die gekoppeld aan de referenties zijn. Selecteer de **Selecteer** knop.

    ![Het dialoogvenster abonnementen selecteren](./media/apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

1. Op de **Azure Explorer** tabblad uit, vouw **HDInsight** om de HDInsight Spark-clusters die in uw abonnement zijn weer te geven.
   
    ![HDInsight Spark-clusters in Azure Explorer](./media/apache-spark-intellij-tool-plugin/view-explorer-3.png)

1. Als u de resources (bijvoorbeeld storage-accounts) die gekoppeld aan het cluster zijn, kunt u een naam van de cluster-knooppunt verder uitbreiden.
   
    ![Een uitgevouwen naam van de cluster-knooppunt](./media/apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="link-a-cluster"></a>Een cluster koppelen
U kunt een normale HDInsight-cluster koppelen met behulp van de gebruikersnaam van de Ambari beheerd. Op dezelfde manier voor een domein gekoppeld HDInsight-cluster kunt u koppelen met behulp van het domein en gebruikersnaam, zoals user1@contoso.com. U kunt ook Livy Service-cluster koppelen.

1. Selecteer **koppelen van een cluster** van **Azure Explorer**.

   ![koppeling cluster contextmenu](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

2. U hebt twee opties voor het koppelen van clusters. 

   * Voor het koppelen van HDInsight-cluster, kiest u **HDInsight-Cluster** in veld **Cluster Info**, voer **Cluster URL-Name**, **gebruikersnaam**, en **Wachtwoord**.

      ![dialoogvenster koppeling hdinsight-cluster](./media/apache-spark-intellij-tool-plugin/link-hdinsight-cluster-dialog.png)

   * Voor het koppelen van Livy Service-cluster, kiest u **Livy Service** in veld **Cluster Info**, voer **Livy eindpunt**, **clusternaam**. **Problemen met yarn eindpunt** is optioneel. In het veld **verificatie**, zijn er twee opties beschikbaar. Ze zijn **basisverificatie** en **geen verificatie**. Wanneer u selecteert **basisverificatie**, **gebruikersnaam** en **wachtwoord** moet worden opgegeven. U moet controleren de gebruikersnaam en het wachtwoord als de verificatie is mislukt.
      
      ![dialoogvenster livy-cluster koppelen](./media/apache-spark-intellij-tool-plugin/link-livy-cluster-dialog.png)
   
3. Ziet u een cluster gekoppelde in **HDInsight** knooppunt als de invoergegevens klopt. U kunt nu een toepassing in dit cluster gekoppelde indienen.

   ![gekoppelde cluster](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

4. U kunt een cluster op basis van ook ontkoppelen **Azure Explorer**.
   
   ![niet-gekoppelde cluster](./media/apache-spark-intellij-tool-plugin/unlink.png)

## <a name="create-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Een Spark Scala-toepassing in een HDInsight Spark-cluster maken

1. Start IntelliJ IDEA en maak een project. In de **nieuw Project** in het dialoogvenster, volg onderstaande stappen te volgen: 

   a. Selecteer **HDInsight** > **Spark in HDInsight (Scala)**.

   b. Selecteer in de lijst **Build-programma** een van de volgende opties, afhankelijk van uw behoeften:

      * **Maven**, voor de ondersteuning van de wizard Scala-project maken
      * **SBT**, voor het beheren van de afhankelijkheden en het maken van het Scala-project

    ![Het dialoogvenster Nieuw project](./media/apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

1. Selecteer **Volgende**.

1. Met de wizard Scala-project maken wordt automatisch gedetecteerd of u de Scala-invoegtoepassing hebt geïnstalleerd. Selecteer **Installeren**.

   ![Scala-invoegtoepassing controleren](./media/apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

1. Selecteer **OK** om de Scala-invoegtoepassing te downloaden. Volg de instructies om IntelliJ opnieuw te starten. 

   ![Het dialoogvenster Scala-invoegtoepassing installeren](./media/apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

1. Voer in het venster **Nieuw project** de volgende handelingen uit:  

    ![De Spark SDK selecteren](./media/apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Geef een projectnaam en locatie op.

   b. Selecteer in de vervolgkeuzelijst **Project SDK** de optie **Java 1.8** voor het Spark 2.x-cluster of selecteer **Java 1.7** voor het Spark 1.x-cluster.

   c. In de vervolgkeuzelijst **Spark-versie** wordt de juiste versie voor Spark SDK en Scala SDK geïntegreerd via de wizard Scala-project maken. Selecteer **Spark 1.x** als de Spark-clusterversie ouder is dan 2.0. Selecteer anders **Spark 2.x**. In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)** gebruikt.

1. Selecteer **Voltooien**.

1. Het Spark-project maakt automatisch een artefact voor u. Als u wilt het artefact weergeven, het volgende doen:

   a. Op de **bestand** in het menu **projectstructuur**.

   b. In de **projectstructuur** in het dialoogvenster, selecteer **artefacten** om weer te geven van de standaard-artefacten die is gemaakt. U kunt ook uw eigen artefact maken door het selecteren van het plusteken (**+**).

      ![Informatie over de artefact in het dialoogvenster](./media/apache-spark-intellij-tool-plugin/default-artifact.png)
      
1. De broncode van uw toepassing toevoegen door het volgende te doen:

   a. In Projectverkenner met de rechtermuisknop op **src**, wijst u **nieuw**, en selecteer vervolgens **Scala klasse**.
      
      ![Opdrachten voor het maken van een klasse Scala van Projectverkenner](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. In de **nieuwe Scala-klasse maken** dialoogvenster vak, Geef een naam op, selecteer **Object** in de **soort** vak en selecteer vervolgens **OK**.
      
      ![Dialoogvenster Nieuwe Scala-klasse maken](./media/apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. In de **MyClusterApp.scala** bestand, plak de volgende code. De code leest de gegevens van HVAC.csv (beschikbaar in alle HDInsight Spark-clusters), haalt de rijen waarvoor slechts één cijfer in de zevende kolom in het CSV-bestand op en schrijft de uitvoer naar **/HVACOut** onder de standaard-opslagcontainer voor het cluster.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Een Spark Scala-toepassing uitvoert op een HDInsight Spark-cluster
Na het maken van een Scala-toepassing, kunt u deze indient aan het cluster.

1. Zoek in de Projectverkenner, een Java- of Scala-bestand en selecteer vervolgens **Spark-toepassing verzenden naar HDInsight** in het snelmenu.
    
      ![De Spark-toepassing verzenden naar HDInsight-opdracht](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

2. In het dialoogvenster de configuratie van de volgende waarden opgeven, en klik vervolgens op **SparkJobRun**.

      ![Het dialoogvenster voor het indienen van Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)
      
    * Voor **Spark-clusters (alleen Linux)**, selecteert u het HDInsight Spark-cluster waarop u wilt dat uw toepassing uit te voeren.

    * Selecteer een artefact in het project IntelliJ of Selecteer een van de vaste schijf.

    * **De naam van de Main-klasse** veld: de standaardwaarde is de hoofdklasse van het geselecteerde bestand. U kunt de klasse wijzigen door het beletselteken (**...** ) en het kiezen van een andere klasse.   

    * **Taak configuraties** veld: de standaardwaarden zijn ingesteld als de afbeelding hierboven wordt weergegeven. U kunt de waarde wijzigen of toevoegen van nieuwe sleutel/waarde voor het verzenden van taken. Voor meer informatie: [Apache Livy REST-API](http://livy.incubator.apache.org./docs/latest/rest-api.html)

      ![De verzending van Spark dialoogvenster vak taak configuratie betekenis](./media/apache-spark-intellij-tool-plugin/submit-job-configurations.png)

    * **Opdrachtregelargumenten** veld: kunt u de argumenten waarden splitsen op basis van de ruimte voor de main-klasse, indien nodig.

    * **Waarnaar wordt verwezen JAR-bestanden** en **bestanden waarnaar wordt verwezen** velden: Geef de paden voor de waarnaar wordt verwezen, JAR-bestanden en bestanden indien van toepassing. Voor meer informatie: [configuratie van een Spark](https://spark.apache.org/docs/latest/configuration.html#runtime-environment) 

      ![Betekenis van de verzending van Spark dialoogvenster vak jar-bestanden](./media/apache-spark-intellij-tool-plugin/jar-files-meaning.png)

       > [!NOTE]
       > Als u wilt uploaden uw JARs waarnaar wordt verwezen en bestanden waarnaar wordt verwezen, Raadpleeg: [over het uploaden van resources aan het cluster](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-storage-explorer)
                         
    * **Pad uploaden**: kunt u de opslaglocatie voor het verzenden van Jar of Scala, een project resources aangeven. Er zijn drie opslagtypen ondersteund: **Azure Blob**, **interactieve sessie Spark gebruiken voor het uploaden van artefacten**, en **cluster standaardopslagaccount gebruiken**, en **ADLS Gen1**. In de schermafbeelding hieronder volgt een voorbeeld van Azure Blob.

        ![Het dialoogvenster voor het indienen van Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-upload-storage-types.png)

        ![Het dialoogvenster voor het indienen van Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-upload-storage-blob.png)

3. Klik op **SparkJobRun** indienen van uw project met het geselecteerde cluster. De **externe Spark-taak in Cluster** tabblad geeft de voortgang van de taak aan de onderkant. U kunt de toepassing door te klikken op de rode knop stoppen. Zie voor informatie over toegang tot de taakuitvoer gaat de ' toegang en HDInsight Spark-clusters beheren met behulp van Azure Toolkit voor IntelliJ ' verderop in dit artikel.
      
     ![Het indienen van Spark-venster](./media/apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)


## <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster"></a>Fouten opsporen in Spark toepassingen lokaal of extern op een HDInsight-cluster 
We raden u ook aan een andere manier om de Spark-toepassing met het cluster in te dienen. U kunt dit doen door in te stellen van de parameters in de **uitvoeren/Debug configuraties** IDE. Zie voor meer informatie, [fouten opsporen in Spark toepassingen lokaal of extern op een HDInsight-cluster met de Azure Toolkit voor IntelliJ via SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).



## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Toegang tot en beheer van HDInsight Spark-clusters met behulp van Azure Toolkit voor IntelliJ
U kunt verschillende bewerkingen uitvoeren met behulp van Azure Toolkit voor IntelliJ.

### <a name="access-the-job-view"></a>Toegang tot de Project-weergave
1. Vouw in Azure Explorer **HDInsight**, vouw de naam van het Spark-cluster, en selecteer vervolgens **taken**.  

    ![Taak weergeven knooppunt](./media/apache-spark-intellij-tool-plugin/job-view-node.png)

1. In het rechterdeelvenster de **Spark-taak weergeven** tabblad geeft alle toepassingen die zijn uitgevoerd op het cluster. Selecteer de naam van de toepassing waarvoor u wilt meer informatie.

    ![Toepassingsgegevens](./media/apache-spark-intellij-tool-plugin/view-job-logs.png)
    >Opmerking
    >

1. Als u wilt weergeven basisinformatie actieve taak, de muisaanwijzer over de taakgrafiek. Als u wilt weergeven van de fasen graph en de gegevens die elke taak genereert, selecteer een knooppunt van de taakgrafiek.

    ![Taakdetails fase](./media/apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

1. Om weer te geven vaak gebruikt u zich aanmeldt, zoals *stuurprogramma Stderr*, *stuurprogramma Stdout*, en *Directory Info*, selecteer de **Log** tabblad.

    ![Logboekdetails](./media/apache-spark-intellij-tool-plugin/Job-log-info.png)

1. U kunt ook het Spark-overzicht gebruikersinterface en de gebruikersinterface van YARN (op het toepassingsniveau van de) weergeven door een koppeling aan de bovenkant van het venster te selecteren.

### <a name="access-the-spark-history-server"></a>Toegang tot de server van de geschiedenis van Spark
1. Vouw in Azure Explorer **HDInsight**, met de rechtermuisknop op de naam van uw Spark-cluster en selecteer vervolgens **Open Spark geschiedenis gebruikersinterface**. 

1. Wanneer u wordt gevraagd, typt u de beheerdersreferenties van het cluster, dat u hebt opgegeven bij het instellen van het cluster.

1. Op het dashboard van de server in de Spark-geschiedenis, kunt u de naam van de toepassing om te zoeken naar de toepassing dat u zojuist hebt uitgevoerd. In de bovenstaande code stelt u de naam van de toepassing met behulp van `val conf = new SparkConf().setAppName("MyClusterApp")`. De naam van uw Spark-toepassing is daarom **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Start de Ambari-portal
1. Vouw in Azure Explorer **HDInsight**, met de rechtermuisknop op de naam van uw Spark-cluster en selecteer vervolgens **Open Management Portal van het Cluster (Ambari)**. 

1. Wanneer u wordt gevraagd, typt u de beheerdersreferenties voor het cluster. U hebt deze referenties hebt opgegeven tijdens het installatieproces van het cluster.

### <a name="manage-azure-subscriptions"></a>Azure-abonnementen beheren
Azure Toolkit voor IntelliJ bevat standaard het Spark-clusters van al uw Azure abonnementen. Indien nodig, kunt u de abonnementen die u wilt openen. 

1. In Azure Explorer, met de rechtermuisknop op de **Azure** hoofd-knooppunt, en selecteer vervolgens **abonnementen beheren**. 

1. Schakel de selectievakjes naast de abonnementen die u niet wilt openen en selecteer vervolgens in het dialoogvenster **sluiten**. U kunt ook selecteren **Afmelden** als u wilt afmelden bij uw Azure-abonnement.

## <a name="spark-console"></a>Spark-Console
U kunt Spark lokale Console(Scala) uitvoeren of Spark Livy interactieve sessie Console(Scala) uitvoeren.

### <a name="spark-local-consolescala"></a>Lokale Console(Scala) Spark
1. Instellen van de configuratie als u geen voordat hebt. In **uitvoeren/Debug configuraties** venster, klikt u op **+** -> **Azure HDInsight Spark**, selecteer tabblad **lokaal uitvoeren**en **op afstand uitvoeren in Cluster**, kiest u de main-klasse en klik vervolgens op **OK**.

    ![Configuratie van de lokale Console](./media/apache-spark-intellij-tool-plugin/console-set-configuration.png)
 
2. Open het bijbehorende main-klasse-bestand en klik met de rechtermuisknop **Spark Console**, klikt u vervolgens op **uitvoeren Spark lokale Console(Scala)**. Of Ga naar het menu **extra**->**Spark-Console**->**lokale Console(Scala) Spark uitgevoerd** om te starten van de console. Twee dialoogvensters wordt vervolgens weergegeven om aan te vragen u als u wilt dat automatisch de afhankelijkheden oplossen. Klik op knop **automatisch oplossen**.

    ![Spark automatisch Fix1](./media/apache-spark-intellij-tool-plugin/console-auto-fix1.png)

    ![Spark automatisch Fix2](./media/apache-spark-intellij-tool-plugin/console-auto-fix2.png)

    ![Spark lokale toegangspunt](./media/apache-spark-intellij-tool-plugin/spark-console-local-entry-script.png)

3. Nadat u hebt lokale console is gestart. Deze ziet eruit als hieronder. U kunt iets dat die u wilt doen. Voer bijvoorbeeld **sc.appName**, drukt u op ctrl + Enter, wordt het resultaat weergegeven. U kunt de lokale console door te klikken op de rode knop beëindigen.

    ![Resultaat van de lokale Console](./media/apache-spark-intellij-tool-plugin/local-console-result.png)


### <a name="spark-livy-interactive-session-consolescala"></a>Spark-Livy interactieve sessie Console(Scala)
Het wordt alleen ondersteund op IntelliJ 2018.2 en 2018.3.

1. Instellen van de configuratie als u geen voordat hebt. In **uitvoeren/Debug configuraties** venster, klikt u op **+** -> **Azure HDInsight Spark**, selecteer tabblad **op afstand uitvoeren in een Cluster** , kiest u de clusternaam en de main-klasse en klik vervolgens op **OK**.

    ![Interactieve Console configuratie-item toevoegen](./media/apache-spark-intellij-tool-plugin/interactive-console-add-config-entry.png)

    ![Configuratie van de interactieve Console](./media/apache-spark-intellij-tool-plugin/interactive-console-configuration.png)

2. Open het bestand overeenkomt uw main-klasse en klik met de rechtermuisknop **Spark Console**, klikt u vervolgens op **uitvoeren Spark Livy interactieve sessie Console(Scala)**. Of Ga naar het menu **hulpprogramma's**, klikt u vervolgens op **Spark Console**, klikt u vervolgens **uitvoeren Spark Livy interactieve sessie Console(Scala)** om te starten van de console.

3. Na het starten van de console is, kunt u iets dat die u wilt doen. Voer bijvoorbeeld **sc.appName**, drukt u op ctrl + Enter, wordt het resultaat weergegeven.

    ![Resultaat van interactieve Console](./media/apache-spark-intellij-tool-plugin/interactive-console-result.png)

### <a name="send-selection-to-spark-console"></a>Selectie naar Spark Console verzenden
Het is handig voor u forsee het scriptresultaat door sommige codes te sturen naar de lokale console of via Livy interactieve sessie Console(Scala). U kunt sommige codes in het bestand Scala markeren en vervolgens met de rechtermuisknop op **selectie verzenden naar Spark Console**. De geselecteerde codes worden verzonden naar de console en worden uitgevoerd. Het resultaat wordt na de codes in de console worden weergegeven. De console Controleer de fouten als bestaande. 

   ![Selectie naar Spark Console verzenden](./media/apache-spark-intellij-tool-plugin/send-selection-to-console.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a>Converteren van bestaande IntelliJ IDEA-toepassingen met Azure Toolkit voor IntelliJ
U kunt de bestaande Spark Scala converteren toepassingen die u hebt gemaakt in IntelliJ IDEA voor compatibiliteit met Azure-Toolkit voor IntelliJ. Vervolgens kunt u de invoegtoepassing voor het verzenden van de toepassingen naar een HDInsight Spark-cluster.

1. Voor een bestaande Spark Scala-toepassing die is gemaakt via IntelliJ IDEA, het bijbehorende .iml-bestand te openen.

1. In de hoofdmap niveau is een **module** element als volgt:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Bewerken van het element toe te voegen `UniqueKey="HDInsightTool"` zodat de **module** element ziet er als volgt uit:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

1. Sla de wijzigingen op. Nu moet uw toepassing compatibel zijn met de Azure Toolkit voor IntelliJ. U kunt dit testen met de rechtermuisknop op de naam van het project in Project Explorer. Het pop-upmenu heeft nu de optie **Spark-toepassing verzenden naar HDInsight**.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Fout bij lokaal uitvoeren: *gebruik een grotere heapgrootte*
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
* Create Scala-project (video): [Spark Scala-toepassingen maken](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Foutopsporing op afstand (video): [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand op HDInsight-Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-hulpprogramma's](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens gebruiken](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Het maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via VPN-verbinding](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via SSH](apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het Spark-toepassingen maken](apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
