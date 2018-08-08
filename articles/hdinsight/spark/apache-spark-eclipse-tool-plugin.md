---
title: 'Azure Toolkit voor Eclipse: Maak Scala-toepassingen voor HDInsight Spark '
description: Gebruik HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen die zijn geschreven in Scala ontwikkelen en deze verzenden naar een HDInsight Spark-cluster, rechtstreeks vanuit de Eclipse IDE.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/30/2017
ms.author: jasonh
ms.openlocfilehash: 6ddfb7c23e05ea9528a462901fd87faade7371a7
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622266"
---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a>Azure Toolkit voor Eclipse gebruiken voor het maken van Spark-toepassingen voor een HDInsight-cluster

Gebruik HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen die zijn geschreven in Scala ontwikkelen en deze verzenden naar een Azure HDInsight Spark-cluster, rechtstreeks vanuit de Eclipse IDE. U kunt de HDInsight Tools-invoegtoepassing in een aantal verschillende manieren gebruiken:

* Om te ontwikkelen en verzenden van een Scala Spark-toepassing in een HDInsight Spark-cluster
* Voor toegang tot de resources van uw Azure HDInsight Spark-cluster
* Om te ontwikkelen en een Scala Spark-toepassing lokaal uitvoeren

> [!IMPORTANT]
> U kunt dit hulpprogramma gebruiken om te maken en verzenden van toepassingen alleen voor een HDInsight Spark-cluster op Linux.
> 
> 

## <a name="prerequisites"></a>Vereisten

* Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.
* Oracle Java Development Kit versie 8, die wordt gebruikt voor de Eclipse IDE-runtime. U kunt dit ook downloaden via de [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Eclipse IDE. In dit artikel wordt gebruikgemaakt van Eclipse Neon. U kunt installeren vanuit de [Eclipse website](https://www.eclipse.org/downloads/).



## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-the-scala-plug-in"></a>HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse en de Scala-invoegtoepassing installeren

### <a name="install-azure-toolkit-for-eclipse"></a>Azure Toolkit voor Eclipse installeren
HDInsight-hulpprogramma's voor Eclipse is beschikbaar als onderdeel van de Azure Toolkit voor Eclipse. Zie voor installatie-instructies [Azure Toolkit voor Eclipse installeren](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-installation).

### <a name="install-the-scala-plug-in"></a>Installeer de invoegtoepassing Scala
Wanneer u Eclipse opent, detecteert HDInsight-hulpprogramma automatisch of u de invoegtoepassing Scala geïnstalleerd. Selecteer **OK** blijven en volg de instructies voor het installeren van de invoegtoepassing van de Eclipse-Marketplace.

![Automatische installatie van de invoegtoepassing Scala](./media/apache-spark-eclipse-tool-plugin/auto-install-scala.png)

De gebruiker kan ofwel [aanmelden bij Azure-abonnement](#Sign-in-to-your-Azure-subscription), of [koppelt een HDInsight-cluster](#Link-a-cluster) met Ambari gebruikersnaam en wachtwoord met gekoppelde domeinen referentie om te starten. 

## <a name="sign-in-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement
1. Start de Eclipse IDE en Azure Explorer openen. Op de **venster** in het menu **weergave tonen**, en selecteer vervolgens **andere**. Vouw in het dialoogvenster dat wordt geopend, **Azure**, selecteer **Azure Explorer**, en selecteer vervolgens **OK**.

   ![Het dialoogvenster weergave tonen](./media/apache-spark-eclipse-tool-plugin/view-explorer-1.png)
1. Met de rechtermuisknop op de **Azure** knooppunt en selecteert u vervolgens **aanmelden**.
1. In de **Azure Sign In** dialoogvenster vak, selecteer de verificatiemethode, selecteer **aanmelden**, en voer uw Azure-referenties.
   
   ![Azure Sign In het dialoogvenster](./media/apache-spark-eclipse-tool-plugin/view-explorer-2.png)
1. Nadat u bent aangemeld, de **abonnementen selecteren** in het dialoogvenster geeft een lijst van alle de Azure-abonnementen die zijn gekoppeld aan de referenties. Klik op **Selecteer** om het dialoogvenster te sluiten.

   ![Selecteer in het dialoogvenster voor abonnementen](./media/apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
1. Op de **Azure Explorer** tabblad uit, vouw **HDInsight** om te zien van de HDInsight Spark-clusters in uw abonnement.
   
   ![HDInsight Spark-clusters in Azure Explorer](./media/apache-spark-eclipse-tool-plugin/view-explorer-3.png)
1. U kunt een clusterknooppunt naam om te zien van de resources (bijvoorbeeld storage-accounts) die zijn gekoppeld aan het cluster verder uitbreiden.
   
   ![De naam van een cluster om te zien van resources uitbreiden](./media/apache-spark-eclipse-tool-plugin/view-explorer-4.png)

## <a name="link-a-cluster"></a>Een cluster koppelen
U kunt een normale cluster koppelen met behulp van de gebruikersnaam van de Ambari beheerd. Op dezelfde manier voor een domein gekoppeld HDInsight-cluster kunt u koppelen met behulp van het domein en gebruikersnaam, zoals user1@contoso.com.

1. Selecteer **koppelen van een cluster** van **Azure Explorer**.

   ![koppeling cluster contextmenu](./media/apache-spark-intellij-tool-plugin/link-a-cluster-context-menu.png)

1. Voer **clusternaam**, **gebruikersnaam** en **wachtwoord**, klik vervolgens op de knop OK om te koppelen van de cluster. (Optioneel) Geef Storage-Account, opslagsleutel en selecteer vervolgens de Opslagcontainer voor de storage explorer om te werken in de structuurweergave links
   
   ![dialoogvenster van de cluster koppelen](./media/apache-spark-eclipse-tool-plugin/link-cluster-dialog.png)
   
   > [!NOTE]
   > We gebruiken de sleutel van een gekoppeld opslagaccount, gebruikersnaam en wachtwoord als het cluster zowel geregistreerd in Azure-abonnement en een cluster is gekoppeld.
   > ![Opslagverkenner in Eclipse](./media/apache-spark-eclipse-tool-plugin/storage-explorer-in-Eclipse.png)

1. Ziet u een cluster gekoppelde in **HDInsight** knooppunt na het klikken op de knop OK, als de ingevoerde gegevens correct zijn. U kunt nu een toepassing in dit cluster gekoppelde indienen.

   ![gekoppelde cluster](./media/apache-spark-intellij-tool-plugin/linked-cluster.png)

1. U kunt een cluster op basis van ook ontkoppelen **Azure Explorer**.
   
   ![niet-gekoppelde cluster](./media/apache-spark-intellij-tool-plugin/unlink.png)


## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Een Spark Scala-project voor een HDInsight Spark-cluster instellen

1. Selecteer in de werkruimte Eclipse IDE **bestand**, selecteer **nieuw**, en selecteer vervolgens **Project**. 
1. Vouw in de wizard Nieuw Project **HDInsight**, selecteer **Spark in HDInsight (Scala)**, en selecteer vervolgens **volgende**.

   ![De Spark in HDInsight (Scala) project selecteren](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
1. De wizard Scala-project maken detecteert automatisch of u de invoegtoepassing Scala geïnstalleerd. Selecteer **OK** blijven downloaden van de invoegtoepassing Scala en volg de instructies op te starten van Eclipse.

   ![Controle van scala](./media/apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
1. In de **nieuw Project voor een Scala HDInsight** in het dialoogvenster, geef de volgende waarden op en selecteer vervolgens **volgende**:
   * Voer een naam voor het project.
   * In de **JRE** gebied, zorg ervoor dat **gebruiken een uitvoeringsomgeving JRE** is ingesteld op **JavaSE 1.7** of hoger.
   * In de **Spark bibliotheek** gebied, kunt u **gebruik Maven om te configureren van Spark SDK** optie.  Onze hulpprogramma integreert de juiste versie voor Spark-SDK en Scala-SDK. U kunt ook **Spark SDK handmatig toevoegen** optie te downloaden en Spark-SDK door handmatig toevoegen.

   ![Dialoogvenster Nieuw Project voor HDInsight Scala](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
1. Selecteer in het volgende dialoogvenster **voltooien**. 
   
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Een Scala-toepassing voor een HDInsight Spark-cluster maken

1. Vouw in Package Explorer het project dat u eerder hebt gemaakt in de Eclipse IDE, met de rechtermuisknop op **src**, wijst u **nieuw**, en selecteer vervolgens **andere**.
1. In de **selecteert u een wizard** dialoogvenster Vouw **Scala Wizards**, selecteer **Scala Object**, en selecteer vervolgens **volgende**.
   
   ![Selecteer in het dialoogvenster wizard](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
1. In de **nieuw bestand maken** in het dialoogvenster, Geef een naam voor het object en selecteer vervolgens **voltooien**.
   
   ![Dialoogvenster Nieuw bestand maken](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
1. Plak de volgende code in de teksteditor:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows that have only one digit in the seventh column in the CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
1. De toepassing uitvoert op een HDInsight Spark-cluster:
   
   a. In Package Explorer met de rechtermuisknop op de projectnaam en selecteer vervolgens **Spark-toepassing verzenden naar HDInsight**.        
   b. In de **Spark inzending** in het dialoogvenster, geef de volgende waarden op en selecteer vervolgens **indienen**:
      
      * Voor **clusternaam**, selecteert u het HDInsight Spark-cluster waarop u wilt dat uw toepassing uit te voeren.
      * Selecteer een artefact in de Eclipse-project, of Selecteer een van een harde schijf. De standaardwaarde is afhankelijk van het item dat u met de rechtermuisknop in Package Explorer.
      * In de **de naam van de Main-klasse** vervolgkeuzelijst, de verzending van wizard worden alle objectnamen van uw project. Selecteer of typ een die u wilt uitvoeren. Als u een artefact van een harde schijf hebt geselecteerd, moet u de naam van de main-klasse handmatig invoeren. 
      * Omdat de toepassingscode in dit voorbeeld niet opdrachtregelargumenten of verwijzen naar JAR-bestanden of bestanden, kunt u de resterende tekstvakken leeg laten.
        
      ![In het dialoogvenster voor verzending van Spark](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
1. De **Spark inzending** tabblad moet beginnen met het weergeven van de voortgang. U kunt de toepassing stoppen door het selecteren van de rode knop in de **Spark inzending** venster. Ook vindt u de logboeken voor deze specifieke toepassing die wordt uitgevoerd door de hele wereld-pictogram (aangeduid met het blauwe vak in de afbeelding) te selecteren.
      
   ![Verzending van Spark-venster](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)


## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Toegang tot en beheer van HDInsight Spark-clusters met behulp van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse
U kunt verschillende bewerkingen uitvoeren met behulp van HDInsight-hulpprogramma's, waaronder toegang tot de taakuitvoer.

### <a name="access-the-job-view"></a>Toegang tot de Project-weergave
1. Vouw in Azure Explorer **HDInsight**, vouw de naam van het Spark-cluster, en selecteer vervolgens **taken**. 

   ![Taak weergeven knooppunt](./media/apache-spark-eclipse-tool-plugin/job-view-node.png)

1. Selecteer de **taken** knooppunt. Als u Java-versie is lager dan **1.8**, hulpprogramma's voor HDInsight automatisch herinnering die u installeert de **E (fx) clipse** invoegtoepassing. Selecteer **OK** blijven en voer vervolgens de wizard om te installeren vanuit de Eclipse-Marketplace en Eclipse opnieuw. 

   ![Clipse E (fx) installeren](./media/apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

1. Open de Project-weergave in de **taken** knooppunt. In het rechterdeelvenster de **Spark-taak weergeven** tabblad geeft alle toepassingen die zijn uitgevoerd op het cluster. Selecteer de naam van de toepassing waarvoor u wilt meer informatie.

   ![Toepassingsgegevens](./media/apache-spark-eclipse-tool-plugin/view-job-logs.png)

   Vervolgens kunt u een van deze acties uitvoeren:

   * Beweeg de muisaanwijzer op de taakgrafiek. De basisgegevens over de actieve taak wordt weergegeven. Selecteer de taakgrafiek en ziet u de fasen en gegevens die elke taak genereert.

     ![Taakdetails fase](./media/apache-spark-eclipse-tool-plugin/Job-graph-stage-info.png)

   * Selecteer de **Log** tabblad om regelmatig gebruikt zich aanmeldt, met inbegrip van **stuurprogramma Stderr**, **stuurprogramma Stdout**, en **Directory Info**.

     ![Logboekdetails](./media/apache-spark-eclipse-tool-plugin/Job-log-info.png)

   * Open de Spark-geschiedenis UI en de gebruikersinterface van YARN (op het toepassingsniveau van de) door te selecteren van de hyperlinks aan de bovenkant van het venster.

### <a name="access-the-storage-container-for-the-cluster"></a>Toegang tot de storage-container voor het cluster
1. Vouw in de Azure Explorer, de **HDInsight** hoofdknooppunt voor een overzicht van HDInsight Spark-clusters die beschikbaar zijn.
1. Vouw de naam van het cluster om de storage-account en de standaard-storage-container voor het cluster te bekijken.
   
   ![Storage-account en de standaard-opslagcontainer](./media/apache-spark-eclipse-tool-plugin/view-explorer-5.png)
1. Selecteer de naam van de container die zijn gekoppeld aan het cluster. Dubbelklik in het rechterdeelvenster op de **HVACOut** map. Open een van de **onderdeel -** bestanden om te zien van de uitvoer van de toepassing.

### <a name="access-the-spark-history-server"></a>Toegang tot de server van de geschiedenis van Spark
1. Met de rechtermuisknop op de naam van uw Spark-cluster in Azure Explorer, en selecteer vervolgens **Open Spark geschiedenis gebruikersinterface**. Wanneer u wordt gevraagd, typt u de beheerdersreferenties voor het cluster. U hebt opgegeven dat deze tijdens het inrichten van het cluster.
1. In het dashboard van de server in de Spark geschiedenis gebruikt u de naam van de toepassing om te zoeken naar de toepassing dat u zojuist hebt uitgevoerd. In de bovenstaande code stelt u de naam van de toepassing met behulp van `val conf = new SparkConf().setAppName("MyClusterApp")`. Dus de naam van uw Spark-toepassing is **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Start de Ambari-portal
1. Met de rechtermuisknop op de naam van uw Spark-cluster in Azure Explorer, en selecteer vervolgens **Open Management Portal van het Cluster (Ambari)**. 
1. Wanneer u wordt gevraagd, typt u de beheerdersreferenties voor het cluster. U hebt opgegeven dat deze tijdens het inrichten van het cluster.

### <a name="manage-azure-subscriptions"></a>Azure-abonnementen beheren
HDInsight-hulpprogramma in de Azure Toolkit voor Eclipse bevat standaard het Spark-clusters van al uw Azure abonnementen. U kunt indien nodig, de abonnementen waarvoor u toegang tot het cluster wilt opgeven. 

1. In Azure Explorer, met de rechtermuisknop op de **Azure** hoofd-knooppunt, en selecteer vervolgens **abonnementen beheren**. 
1. Schakel de selectievakjes uit voor het abonnement dat u niet wilt openen en selecteer vervolgens in het dialoogvenster **sluiten**. U kunt ook selecteren **Afmelden** als u wilt afmelden bij uw Azure-abonnement.

## <a name="run-a-spark-scala-application-locally"></a>Een Spark Scala-toepassing lokaal uitvoeren
U kunt HDInsight-hulpprogramma's gebruiken in Azure Toolkit voor Eclipse Spark Scala-toepassingen lokaal op uw werkstation uitvoeren. Normaal gesproken hebben voor deze toepassingen geen toegang tot clusterresources, zoals een opslagcontainer nodig en u kunt uitvoeren en lokaal te testen.

### <a name="prerequisite"></a>Vereiste
Terwijl u de lokale Spark Scala-toepassing op een Windows-computer uitvoert, krijgt u mogelijk een uitzondering, zoals wordt beschreven [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). Deze uitzondering treedt op omdat **WinUtils.exe** ontbreekt in Windows. 

U kunt deze fout oplossen, moet u [downloaden van het uitvoerbare bestand](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**, en voeg de omgevingsvariabele **HADOOP_HOME** en stel de waarde van de variabele **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Een lokale Spark Scala-toepassing uitvoeren
1. Start Eclipse en maak een project. In de **nieuw Project** in het dialoogvenster de volgende opties en selecteer vervolgens **volgende**.
   
   * Selecteer **HDInsight** in het linkerdeelvenster.
   * Selecteer in het rechter deelvenster **Spark in HDInsight lokaal uitvoeren voorbeeld (Scala)**.

   ![Het dialoogvenster Nieuw project](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
   
1. Het project om informatie te leveren, volg de stappen 3 tot en met 6 uit de vorige sectie [instellen van een Spark Scala-project voor een HDInsight Spark-cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster).

1. De sjabloon wordt een voorbeeld van code toegevoegd (**LogQuery**) onder de **src** map die u lokaal op uw computer uitvoeren kunt.
   
   ![Locatie van LogQuery](./media/apache-spark-eclipse-tool-plugin/local-app.png)
   
1. Met de rechtermuisknop op de **LogQuery** toepassing, wijs **uitvoeren als**, en selecteer vervolgens **1 Scala-toepassing**. Uitvoer zoals deze wordt weergegeven op de **Console** tabblad:
   
   ![Spark-toepassing lokaal uitvoeren van resultaat](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="known-problems"></a>Bekende problemen
Wanneer een cluster te koppelen, ik zou u de referenties van de opslag bieden voorstellen.

![Interactief aanmelden](./media/apache-spark-eclipse-tool-plugin/link-cluster-with-storage-credential-eclipse.png)

Er zijn twee modi voor het verzenden van de taken. Als storage-referentie is opgegeven, wordt de batchmodus worden gebruikt om de taak te verzenden. Anders, interactieve modus wordt gebruikt. Als het cluster bezet is, kunt u de onderstaande foutmelding krijgen.

![eclipse de foutmelding wanneer cluster bezet](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-upload.png)

![eclipse de foutmelding wanneer cluster bezet](./media/apache-spark-eclipse-tool-plugin/eclipse-interactive-cluster-busy-submit.png)

## <a name="feedback"></a>Feedback
Als u bepaalde feedback hebt, of als er andere problemen die optreden bij gebruik van dit hulpprogramma, stuur ons een e-mailbericht op hdivstool@microsoft.com.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Het maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure Toolkit voor IntelliJ gebruiken om te maken en verzenden van Spark Scala-toepassingen](apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via VPN-verbinding](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via SSH](../hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

