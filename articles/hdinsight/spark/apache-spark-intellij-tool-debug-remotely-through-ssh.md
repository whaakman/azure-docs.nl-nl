---
title: 'Azure Toolkit voor IntelliJ: fouten opsporen in Spark scala-toepassingen op afstand via SSH | Microsoft Docs'
description: Stapsgewijze instructies voor het gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ opsporen toepassingen op afstand op HDInsight-clusters via SSH
keywords: foutopsporing op afstand intellij, externe foutopsporing intellij, ssh, intellij, hdinsight, fouten opsporen in intellij, foutopsporing
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 11/25/2017
ms.author: jejiang
ms.openlocfilehash: 6ea6d94453583c6b5ed680f96ebc43d33d0262a0
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Fouten opsporen in Spark scala-toepassingen lokaal of extern op een HDInsight-cluster in Azure werkset voor IntelliJ via SSH

Dit artikel bevat stapsgewijze instructies over het gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ opsporen toepassingen op afstand op een HDInsight-cluster. Als u wilt opsporen in uw project, u kunt ook bekijken de [fouten opsporen in HDInsight Spark-toepassingen met Azure Toolkit voor IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.

**Vereisten**
* **HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ**. Dit hulpprogramma maakt deel uit van Azure Toolkit voor IntelliJ. Zie voor meer informatie [Azure Toolkit installeren voor IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation). En **Azure Toolkit voor IntelliJ**. Gebruik deze toolkit Spark om toepassingen te maken voor een HDInsight-cluster. Voor meer informatie, volg de instructies in [gebruik Azure Toolkit voor IntelliJ Spark om toepassingen te maken voor een HDInsight-cluster](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).

* **HDInsight SSH-service met gebruikersnaam en wachtwoord management**. Zie voor meer informatie [verbinding maken met HDInsight (Hadoop) met behulp van SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) en [SSH gebruiken voor toegang tot de Ambari-tunneling web-UI, JobHistory, NameNode, Oozie en andere web-UI](https://docs.microsoft.com/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 
## <a name="learn-how-to-perform-local-run-and-debugging"></a>Meer informatie over het uitvoeren van lokale uitvoeren en foutopsporing
### <a name="scenario-1-create-a-spark-scala-application"></a>Scenario 1: Een Spark Scala-toepassing maken 

1. Start IntelliJ IDEA en maak vervolgens een project. In de **nieuw Project** dialoogvenster de volgende handelingen uit:

   a. Selecteer **HDInsight**. 

   b. Selecteer een Java of Scala sjabloon op basis van uw voorkeur. Selecteer een van de volgende opties:

      - **Spark in HDInsight (Scala)**

      - **Spark in HDInsight (Java)**

      - **Spark in HDInsight voorbeeld (Scala)**

      In dit voorbeeld wordt een **Spark in HDInsight voorbeeld (Scala)** sjabloon.

   c. In de **Build hulpprogramma** , selecteert u een van de volgende, afhankelijk van uw behoeften:

      - **Maven**, voor ondersteuning van de wizard Scala project maken

      -  **SBT**, voor het beheren van de afhankelijkheden en bouwen voor het project Scala 

      ![Een debug-project maken](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. Selecteer **volgende**.     
 
2. In de volgende **nieuw Project** venster de volgende handelingen uit:

   ![Selecteer de Spark SDK](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Voer een naam van het project en de projectlocatie.

   b. In de **Project SDK** vervolgkeuzelijst, selecteer **Java 1.8** voor **2.x Spark** cluster of selecteer **Java 1.7** voor **Spark 1.x** cluster.

   c. In de **Spark versie** vervolgkeuzelijst, de wizard Scala project maken voor Spark SDK en Scala SDK de juiste versie kan worden geïntegreerd. Als de spark-cluster versie ouder dan 2.0 is, selecteert u **Spark 1.x**. Selecteer anders **2.x Spark.** In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)**.

   d. Selecteer **Voltooien**.

3. Selecteer **src** > **belangrijkste** > **scala** uw code in het project te openen. In dit voorbeeld wordt de **SparkCore_wasbloTest** script.

### <a name="prerequisite-for-windows"></a>Vereiste voor windows
Terwijl u de lokale Spark Scala-toepassingen op een Windows-computer uitvoert, krijgt u mogelijk een uitzondering, zoals wordt beschreven in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). De uitzondering is omdat WinUtils.exe in Windows ontbreekt. 

Deze fout op te lossen [downloaden van het uitvoerbare bestand](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**. Voeg de omgevingsvariabele **HADOOP_HOME**, en stel de waarde van de variabele **C:\WinUtils**.

### <a name="scenario-2-perform-local-run"></a>Scenario 2: Lokale uitvoering uitvoeren
1. Open de **SparkCore_wasbloTest** script, met de rechtermuisknop op de scripteditor en selecteer vervolgens de optie **uitvoeren '[Spark taak] XXX'** om uit te voeren, lokaal uitvoeren.
2. Eenmaal lokale uitvoering is voltooid, ziet u het uitvoerbestand opslaan in uw huidige Projectverkenner **gegevens** > **__standaard__**.

    ![Lokaal uitvoeren resultaat](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-result.png)
3. Onze's hebt ingesteld de standaard lokale uitvoering configuratie automatisch wanneer u de lokale uitvoering en lokaal uitvoeren voor foutopsporing. Openen van de configuratie van de **[Spark taak] XXX** op de rechterbovenhoek ziet u de **[Spark taak] XXX** al gemaakt onder **Azure HDInsight Spark-taak**. Overschakelen naar **lokaal uitvoeren** tabblad.

    ![Lokale uitvoering configuratie](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-configuration.png)
    - [Omgevingsvariabelen](#prerequisite-for-windows): als u al de systeemomgevingsvariabele ingesteld **HADOOP_HOME** naar **C:\WinUtils**, kunt het automatisch detecteren die niet nodig handmatig toe te voegen.
    - [WinUtils.exe locatie](#prerequisite-for-windows): als u de systeemomgevingsvariabele niet hebt ingesteld, kunt u de locatie vinden door te klikken op de knop.
    - Kies een van twee opties en zijn ze niet nodig op Mac OS- en Linux.
4. U kunt ook instellen op de configuratie handmatig alvorens de lokale uitgevoerd en lokale foutopsporing uit te voeren. Selecteer in de vorige schermafbeelding het plusteken (**+**). Selecteer vervolgens de **Azure HDInsight Spark-taak** optie. Geef gegevens voor **naam**, **Main klassenaam** wilt opslaan, klikt u op lokaal uitvoeren.

### <a name="scenario-3-perform-local-debugging"></a>Scenario 3: Voer lokale foutopsporing
1. Open de **SparkCore_wasbloTest** script, onderbrekingspunten instellen.
2. Met de rechtermuisknop op de scripteditor en selecteer vervolgens de optie **Debug '[Spark taak] XXX'** lokale foutopsporing uitvoeren.   



## <a name="learn-how-to-perform-remote-run-and-debugging"></a>Meer informatie over het uitvoeren van externe uitvoeren en foutopsporing
### <a name="scenario-1-perform-remote-run"></a>Scenario 1: Extern uitvoeren uitvoeren

1. Voor toegang tot de **configuraties bewerken** menu, selecteer het pictogram in de rechterbovenhoek. U kunt vanuit dit menu maken of bewerken van de configuraties voor foutopsporing op afstand.

   ![Configuraties bewerken](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png) 

2. In de **uitvoeren/Debug configuraties** dialoogvenster Selecteer het plusteken (**+**). Selecteer vervolgens de **Azure HDInsight Spark-taak** optie.

   ![Nieuwe configuratie toevoegen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)
3. Overschakelen naar **op afstand uitvoeren in het Cluster** tabblad. Geef gegevens voor **naam**, **Spark-cluster**, en **Main klassenaam**. Selecteer vervolgens **geavanceerde configuratie**. Onze extra ondersteuning voor foutopsporing met **Executor**. De **numExectors**, de standaardwaarde is 5. U zou beter niet instellen hoger is dan 3.

   ![Configuraties voor foutopsporing uitvoeren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

4. In de **Spark verzending van geavanceerde configuratie** dialoogvenster, **foutopsporing op afstand inschakelen Spark**. Geef de SSH-gebruikersnaam en een wachtwoord invoeren of een bestand met een persoonlijke sleutel gebruiken. Selecteer om de configuratie op te slaan, **OK**. Als u uitvoeren, foutopsporing op afstand wilt, moet u deze in te stellen. Hoeft niet in te stellen als u alleen wilt gebruiken, extern uitvoeren.

   ![Spark foutopsporing op afstand inschakelen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

5. De configuratie wordt nu opgeslagen met de naam die u hebt opgegeven. Selecteer de om configuratiedetails te bekijken, de naam van de configuratie. Als u wilt wijzigen, selecteert u **configuraties bewerken**. 

6. Nadat u de configuratie-instellingen hebt voltooid, kunt u het project uitvoeren op de externe cluster of foutopsporing op afstand uitvoeren.
   
   ![Knop voor extern uitvoeren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/perform-remote-run.png)

7. Klik op de **Disconnect** knop waarmee de logboeken van de verzending niet wordt weergegeven in het linkerdeelvenster. Het is echter nog steeds uitgevoerd op de back-end.

   ![Knop voor extern uitvoeren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/remote-run-result.png)



### <a name="scenario-2-perform-remote-debugging"></a>Scenario 2: Voer foutopsporing op afstand
1. Dingen instellen en klik vervolgens op de **foutopsporing op afstand** pictogram. Het verschil met het indienen van externe is die SSH-gebruikersnaam en wachtwoord moeten worden geconfigureerd.

   ![Selecteer het pictogram voor foutopsporing](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

2. Wanneer de programma-uitvoering is het laatste punt bereikt, ziet u een **stuurprogramma** tabblad en de twee **Executor** tabbladen de **foutopsporingsprogramma** deelvenster. Selecteer de **hervatten programma** pictogram om door te gaan met de code, die vervolgens het volgende onderbrekingspunt bereikt. U moet overschakelen naar de juiste **Executor** tabblad voor de doel-executor om op te sporen. U kunt de uitvoeringslogboeken weergeven voor de overeenkomende **Console** tabblad.

   ![Tabblad foutopsporing](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-3-perform-remote-debugging-and-bug-fixing"></a>Scenario 3: Voer foutopsporing op afstand en het verhelpen van problemen
1. Twee dingen instellen en selecteer vervolgens de **Debug** pictogram om de externe foutopsporing proces te starten.

2. Hiermee stopt u de code op het eerste belangrijk punt en de parameter en de variabele gegevens worden weergegeven in de **variabelen** deelvenster. 

3. Selecteer de **hervatten programma** pictogram om door te gaan. De code reageert op het tweede punt. De uitzondering is opgetreden, zoals verwacht.

   ![Genereert fout](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png) 

4. Selecteer de **hervatten programma** pictogram opnieuw. De **HDInsight Spark verzending** venster een fout met 'job run is mislukt' weergegeven.

   ![Verzending van de fout](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png) 

5. Schakel wilt de variabele waarde dynamisch bijwerken met behulp van de mogelijkheid foutopsporing IntelliJ **Debug** opnieuw. De **variabelen** deelvenster opnieuw wordt weergegeven. 

6. Het doel met de rechtermuisknop op de **Debug** tabblad en selecteer vervolgens **waarde instellen**. Voer vervolgens een nieuwe waarde voor de variabele. Selecteer vervolgens **Enter** om op te slaan van de waarde. 

   ![Waarde instellen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value.png) 

7. Selecteer de **hervatten programma** pictogram om door te gaan het programma uit te voeren. Dit moment is geen uitzondering opgetreden. U kunt zien dat het project met succes wordt uitgevoerd zonder eventuele uitzonderingen.

   ![Fouten opsporen in zonder uitzondering](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Volgende stappen
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demo
* Maak Scala project (video): [Spark Scala-toepassingen maken](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Foutopsporing op afstand (video): [gebruik Azure Toolkit voor IntelliJ fouten opsporen in Spark scala-toepassingen op afstand op een HDInsight-cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight analyseren gebouwtemperatuur met behulp van HVAC-gegevens gebruiken](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Spark in HDInsight voor het bouwen van realtime streamingtoepassingen gebruiken](apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Gebruik van Azure Toolkit voor IntelliJ Spark-toepassingen voor een HDInsight-cluster maken](apache-spark-intellij-tool-plugin.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
