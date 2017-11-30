---
title: 'Azure Toolkit voor Eclipse: toepassingen voor HDInsight Spark Scala maken | Microsoft Docs'
description: Het gebruik van HDInsight Tools in Azure Toolkit voor Eclipse Spark scala-toepassingen die zijn geschreven in Scala ontwikkelen en deze rechtstreeks vanuit de Eclipse IDE verzenden naar een HDInsight Spark-cluster.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f6c79550-5803-4e13-b541-e86c4abb420b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: nitinme
ms.openlocfilehash: ede1a974b32227edf44464ed56ae85a1ea7ee97b
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a>Gebruik van Azure Toolkit voor Eclipse Spark-toepassingen voor een HDInsight-cluster maken

Het gebruik van HDInsight Tools in Azure Toolkit voor Eclipse Spark scala-toepassingen die zijn geschreven in Scala ontwikkelen en deze rechtstreeks vanuit de Eclipse IDE verzenden naar een Azure HDInsight Spark-cluster. U kunt de HDInsight Tools-invoegtoepassing op verschillende manieren gebruiken:

* Te ontwikkelen en verzenden van een Scala Spark-toepassing op een HDInsight Spark-cluster
* Voor toegang tot de resources van uw Azure HDInsight Spark-cluster
* Te ontwikkelen en een Scala Spark-toepassing lokaal uitvoeren

> [!IMPORTANT]
> U kunt dit hulpprogramma gebruiken voor het maken en verzenden van toepassingen alleen voor een HDInsight Spark-cluster op Linux.
> 
> 

## <a name="prerequisites"></a>Vereisten

* Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Oracle Java Development Kit versie 8, dat wordt gebruikt voor de Eclipse IDE-runtime. U kunt downloaden van de [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Eclipse IDE. In dit artikel maakt gebruik van Eclipse Neon. U kunt installeren vanuit de [Eclipse website](https://www.eclipse.org/downloads/).



## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-the-scala-plug-in"></a>HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse en de Scala invoegtoepassing installeren
### <a name="install-hdinsight-toolsazure-toolkit-for"></a>HDInsight Toolsazure toolkit-voor installeren
HDInsight Tools voor Eclipse is beschikbaar als onderdeel van Azure Toolkit voor Eclipse. Zie voor installatie-instructies [Azure Toolkit installeren voor Eclipse](https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse-installation).
### <a name="install-the-scala-plug-in"></a>De invoegtoepassing Scala installeren
Wanneer u Eclipse opent, detecteert HDInsight-hulpprogramma automatisch of u de invoegtoepassing Scala geïnstalleerd. Selecteer **OK** blijven en volg de instructies voor het installeren van de invoegtoepassing van de Marketplace Eclipse.

![Automatische installatie van de invoegtoepassing Scala](./media/apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement
1. Start de Eclipse IDE en Azure Explorer te openen. Op de **venster** selecteert u **weergave tonen**, en selecteer vervolgens **andere**. Vouw in het dialoogvenster **Azure**, selecteer **Azure Explorer**, en selecteer vervolgens **OK**.

   ![Het dialoogvenster weergave weergeven](./media/apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Met de rechtermuisknop op de **Azure** knooppunt en selecteer vervolgens **aanmelden**.
3. In de **Azure aanmelden** dialoogvenster vak, selecteer de verificatiemethode, selecteert u **aanmelden**, en voer uw Azure-referenties.
   
   ![Azure Sign In het dialoogvenster](./media/apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. Nadat u bent aangemeld, de **abonnementen Selecteer** in het dialoogvenster geeft een lijst van alle de Azure-abonnementen die zijn gekoppeld aan de referenties. Klik op **Selecteer** om het dialoogvenster te sluiten.

   ![Selecteer in het dialoogvenster voor abonnementen](./media/apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. Op de **Azure Explorer** tabblad uit, vouw **HDInsight** om te zien van de HDInsight Spark-clusters in uw abonnement.
   
   ![HDInsight Spark-clusters in Azure Explorer](./media/apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. U kunt een clusterknooppunt naam om te zien van de bronnen (bijvoorbeeld opslagaccounts) die zijn gekoppeld aan het cluster verder uitbreiden.
   
   ![De clusternaam van een om te zien van resources uitbreiden](./media/apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>Een Spark Scala-project voor een HDInsight Spark-cluster instellen

1. Selecteer in de werkruimte Eclipse IDE **bestand**, selecteer **nieuw**, en selecteer vervolgens **Project**. 
2. Vouw in de wizard Nieuw Project **HDInsight**, selecteer **Spark in HDInsight (Scala)**, en selecteer vervolgens **volgende**.

   ![De Spark in HDInsight (Scala) project selecteren](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. De wizard Scala project maken detecteert automatisch of u de invoegtoepassing Scala geïnstalleerd. Selecteer **OK** blijven downloaden van de invoegtoepassing Scala en volg de instructies Eclipse opnieuw opstarten.

   ![controle van scala](./media/apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. In de **nieuw HDInsight Scala Project** in het dialoogvenster bieden de volgende waarden en selecteer vervolgens **volgende**:
   * Voer een naam voor het project.
   * In de **JRE** gebied, zorg ervoor dat **gebruik van een uitvoeringsomgeving JRE** is ingesteld op **JavaSE 1.7** of hoger.
   * In de **Spark bibliotheek** gebied kunt u **Maven gebruiken voor het configureren van Spark SDK** optie.  Onze hulpprogramma integreert de juiste versie voor Spark SDK en Scala SDK. U kunt ook **Spark SDK handmatig toevoegen** optie, downloaden en Spark SDK door handmatig toevoegen.

   ![Het dialoogvenster New Project van HDInsight Scala](./media/apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5. Selecteer in het volgende dialoogvenster **voltooien**. 
   
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>Een Scala-toepassing voor een HDInsight Spark-cluster maken

1. In de IDE Eclipse vanuit de Explorer-pakket, vouwt u het project dat u eerder hebt gemaakt, met de rechtermuisknop op **src**, wijs **nieuw**, en selecteer vervolgens **andere**.
2. In de **een wizard selecteren** dialoogvenster Vouw **Scala Wizards**, selecteer **Scala Object**, en selecteer vervolgens **volgende**.
   
   ![Selecteer in het dialoogvenster wizard](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. In de **nieuw bestand maken** in het dialoogvenster een naam voor het object en selecteer vervolgens **voltooien**.
   
   ![Dialoogvenster nieuwe bestanden maken](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Plak de volgende code in de teksteditor:
   
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
5. Voer de toepassing op een HDInsight Spark-cluster:
   
   a. Vanuit de Explorer-pakket met de rechtermuisknop op de projectnaam en selecteer vervolgens **Spark toepassing verzenden naar HDInsight**.        
   b. In de **Spark verzending** in het dialoogvenster bieden de volgende waarden en selecteer vervolgens **indienen**:
      
      * Voor **clusternaam**, selecteert u het HDInsight Spark-cluster waarop u wilt dat uw toepassing uit te voeren.
      * Selecteer een artefact in de Eclipse-project of Selecteer een vanaf een vaste schijf. De standaardwaarde is afhankelijk van het item dat u met de rechtermuisknop in Explorer-pakket.
      * In de **Main klassenaam** vervolgkeuzelijst, de verzending van wizard geeft alle objectnamen van uw project. Selecteer of typ een die u wilt uitvoeren. Als u een artefact van een harde schijf hebt geselecteerd, moet u de naam van de belangrijkste klasse handmatig invoeren. 
      * Omdat de toepassingscode die in dit voorbeeld geen opdrachtregelargumenten vereist of verwijzen naar potten of bestanden, kunt u de resterende tekstvakken leeg laten.
        
      ![Dialoogvenster Spark verzending](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
6. De **Spark verzending** tabblad moet worden gestart om de voortgang weer te geven. U kunt de toepassing stoppen door het selecteren van de rode knop in de **Spark verzending** venster. U kunt ook de logboeken voor deze specifieke toepassing die wordt uitgevoerd door het selecteren van het pictogram van globe (aangeduid met het blauw in de afbeelding) weergeven.
      
   ![Verzending van Spark-venster](./media/apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Toegang tot en HDInsight Spark-clusters beheren met behulp van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse
U kunt verschillende bewerkingen kunt uitvoeren met behulp van HDInsight-hulpprogramma's, waaronder toegang tot de taakuitvoer van de.

### <a name="access-the-job-view"></a>Toegang tot de taakweergave van de
1. Vouw in Azure Explorer **HDInsight**, vouw de naam van het Spark-cluster en selecteer vervolgens **taken**. 

   ![Taak weergave knooppunt](./media/apache-spark-eclipse-tool-plugin/job-view-node.png)

2. Selecteer de **taken** knooppunt. Als u Java-versie is lager dan **1.8**, HDInsight Tools automatisch herinnering die u installeert de **E (fx) clipse** invoegtoepassing. Selecteer **OK** blijven en voer vervolgens de wizard om te installeren vanaf de Marketplace Eclipse en Eclipse opnieuw opstarten. 

   ![Clipse E (fx) installeren](./media/apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Open de taak van de **taken** knooppunt. In het rechterdeelvenster de **Spark taakweergave** tabblad geeft alle toepassingen die op het cluster worden uitgevoerd. Selecteer de naam van de toepassing waarvoor u wilt voor meer informatie.

   ![Toepassingsgegevens](./media/apache-spark-eclipse-tool-plugin/view-job-logs.png)

   Vervolgens kunt u een van deze acties uitvoeren:

   * Beweeg de muisaanwijzer op de taakgrafiek. Basisinformatie over de actieve taak wordt weergegeven. Selecteer de taakgrafiek en ziet u de fasen en gegevens die elke taak genereert.

     ![Fase taakdetails](./media/apache-spark-eclipse-tool-plugin/Job-graph-stage-info.png)

   * Selecteer de **logboek** tabblad om regelmatig gebruikt zich aanmeldt, met inbegrip van **stuurprogramma Stderr**, **stuurprogramma Stdout**, en **Directory Info**.

     ![Logboekgegevens](./media/apache-spark-eclipse-tool-plugin/Job-log-info.png)

   * Open de geschiedenis Spark gebruikersinterface en de gebruikersinterface van YARN (op het toepassingsniveau) door het selecteren van de hyperlinks aan de bovenkant van het venster.

### <a name="access-the-storage-container-for-the-cluster"></a>Toegang tot de storage-container voor het cluster
1. Vouw in Azure Explorer de **HDInsight** hoofdknooppunt voor een overzicht van HDInsight Spark-clusters die beschikbaar zijn.
2. Vouw de naam van het cluster om te zien van het opslagaccount en de standaardcontainer opslag voor het cluster.
   
   ![Storage-account en het standaard storage-container](./media/apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Selecteer de naam van de opslag-container die zijn gekoppeld aan het cluster. Dubbelklik in het rechterdeelvenster op de **HVACOut** map. Open een van de **onderdeel -** bestanden om te zien van de uitvoer van de toepassing.

### <a name="access-the-spark-history-server"></a>Toegang tot de server van de geschiedenis van Spark
1. Met de rechtermuisknop op de naam van uw Spark-cluster in Azure Explorer en selecteer vervolgens **Open Spark geschiedenis UI**. Wanneer u wordt gevraagd, typt u de beheerdersreferenties voor het cluster. U hebt opgegeven dat deze tijdens het inrichten van het cluster.
2. In het Spark geschiedenis serverdashboard gebruikt u de naam van de toepassing op zoek naar de toepassing dat u zojuist hebt uitgevoerd. In de bovenstaande code stelt u de naam van de toepassing met behulp van `val conf = new SparkConf().setAppName("MyClusterApp")`. Ja, de naam van uw toepassing Spark is **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Start de Ambari-portal
1. Met de rechtermuisknop op de naam van uw Spark-cluster in Azure Explorer en selecteer vervolgens **beheerportal Open Cluster (Ambari)**. 
2. Wanneer u wordt gevraagd, typt u de beheerdersreferenties voor het cluster. U hebt opgegeven dat deze tijdens het inrichten van het cluster.

### <a name="manage-azure-subscriptions"></a>Azure-abonnementen beheren
HDInsight-hulpprogramma in de werkset van Azure voor Eclipse bevat standaard het Spark-clusters van alle uw Azure-abonnementen. Indien nodig, kunt u de abonnementen waarvoor u toegang tot het cluster wilt opgeven. 

1. In Azure Explorer met de rechtermuisknop op de **Azure** de hoofd-knooppunt en selecteer vervolgens **abonnementen beheren**. 
2. Schakel de selectievakjes uit voor het abonnement dat u niet wilt openen, en selecteer vervolgens in het dialoogvenster **sluiten**. U kunt ook selecteren **Afmelden** als u wilt afmelden bij uw Azure-abonnement.

## <a name="run-a-spark-scala-application-locally"></a>Een Spark Scala-toepassing lokaal uitvoeren
U kunt in Azure Toolkit voor Eclipse HDInsight Tools Spark Scala-toepassingen lokaal uitvoeren op uw werkstation. Normaal gesproken deze toepassingen hebben geen toegang tot clusterbronnen, zoals een opslagcontainer nodig en u kunt uitvoeren en lokaal te testen.

### <a name="prerequisite"></a>Vereiste
Terwijl u de lokale Spark Scala-toepassingen op een Windows-computer uitvoert, krijgt u mogelijk een uitzondering zoals toegelicht in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). Deze uitzondering doet zich voor omdat **WinUtils.exe** ontbreekt in Windows. 

U lost deze fout, moet u [downloaden van het uitvoerbare bestand](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**, en voeg vervolgens de omgevingsvariabele **HADOOP_HOME** en stel de waarde van de variabele **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>Een lokale Spark Scala-toepassing uitvoeren
1. Start Eclipse en maak een project. In de **nieuw Project** in het dialoogvenster de volgende keuzen en selecteer vervolgens **volgende**.
   
   * Selecteer in het linkerdeelvenster **HDInsight**.
   * Selecteer in het rechterdeelvenster **Spark in HDInsight lokale uitvoeren voorbeeld (Scala)**.

   ![Het dialoogvenster Nieuw project](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
   
2. Het om projectdetails te verstrekken, volg de stappen 3 tot en met 6 uit de vorige sectie [instellen van een Spark Scala-project voor een HDInsight Spark-cluster](#set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster).

3. De sjabloon wordt toegevoegd een voorbeeldcode (**LogQuery**) onder de **src** map die u lokaal op uw computer uitvoeren kunt.
   
   ![Locatie van LogQuery](./media/apache-spark-eclipse-tool-plugin/local-app.png)
   
4. Met de rechtermuisknop op de **LogQuery** toepassing, wijs **uitvoeren als**, en selecteer vervolgens **1 Scala toepassing**. Uitvoer zoals deze wordt weergegeven op de **Console** tabblad:
   
   ![Spark toepassing lokale resultaat van uitgevoerde](./media/apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="known-problems"></a>Bekende problemen
Als u een toepassing met Azure Data Lake Store, selecteer **interactief** modus tijdens het proces voor Azure aanmelden. Als u selecteert **automatisch** modus, krijgt u mogelijk een fout opgetreden.

![Interactief aanmelden](./media/apache-spark-eclipse-tool-plugin/interactive-authentication.png)

U kunt een Azure Data Lake-cluster verzenden van uw toepassing met een methode voor aanmelden.

Op dit moment wordt bekijken Spark uitvoer rechtstreeks niet ondersteund.

## <a name="feedback"></a>Feedback
Als u feedback hebt, of als u andere problemen die optreden bij het gebruik van dit hulpprogramma, stuur ons een e-mailbericht op hdivstool@microsoft.com.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](../hdinsight-apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure-Toolkit voor IntelliJ gebruiken voor het maken en verzenden van Spark Scala-toepassingen](apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](../hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark-toepassingen op afstand via SSH](../hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

