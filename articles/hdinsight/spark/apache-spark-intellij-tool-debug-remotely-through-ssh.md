---
title: 'Azure Toolkit voor IntelliJ: fouten opsporen in Spark-toepassingen op afstand via SSH '
description: Stapsgewijze instructies over het gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ bij foutopsporing in toepassingen op afstand op HDInsight-clusters via SSH
keywords: foutopsporing op afstand intellij, externe foutopsporing intellij, ssh, intellij, hdinsight, fouten opsporen in intellij, foutopsporing
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/25/2017
ms.openlocfilehash: ae22319c54335695a52e803c0c030858e2782c38
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582465"
---
# <a name="debug-spark-applications-locally-or-remotely-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Fouten opsporen in Spark toepassingen lokaal of extern op een HDInsight-cluster met de Azure Toolkit voor IntelliJ via SSH

In dit artikel biedt stapsgewijze instructies over het gebruik van HDInsight-hulpprogramma's in [Azure Toolkit voor IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij?view=azure-java-stable) bij foutopsporing in toepassingen op afstand op een HDInsight-cluster. Als u wilt fouten opsporen in uw project, kunt u ook bekijken de [fouten opsporen in HDInsight Spark-toepassingen met Azure Toolkit voor IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) video.

**Vereisten**
* **HDInsight-hulpprogramma's in Azure Toolkit voor IntelliJ**. Dit hulpprogramma maakt deel uit van de Azure Toolkit voor IntelliJ. Zie voor meer informatie, [Installeer Azure Toolkit voor IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation). En **Azure Toolkit voor IntelliJ**. Gebruik deze toolkit om Apache Spark-toepassingen voor een HDInsight-cluster te maken. Voor meer informatie, volg de instructies in [Azure-Toolkit voor IntelliJ om een Apache Spark-toepassingen voor een HDInsight-cluster maken](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).

* **HDInsight SSH service met de gebruikersnaam en wachtwoord management**. Zie voor meer informatie, [verbinding maken met HDInsight (Apache Hadoop) met behulp van SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) en [SSH gebruiken voor toegang tot de Ambari-tunneling web-UI, JobHistory, NameNode, Oozie en andere web-UI](https://docs.microsoft.com/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 
## <a name="learn-how-to-perform-local-run-and-debugging"></a>Leer hoe u uitvoert lokaal uitvoeren en fouten opsporen
### <a name="scenario-1-create-a-spark-scala-application"></a>Scenario 1: Een Spark Scala-toepassing maken 

1. Start IntelliJ IDEA en maak een project. Voer in het dialoogvenster **Nieuw project** de volgende handelingen uit:

   a. Selecteer **HDInsight**. 

   b. Selecteer een Java- of Scala-sjabloon op basis van uw voorkeur. Selecteer een van de volgende opties:

      - **Spark in HDInsight (Scala)**

      - **Spark in HDInsight (Java)**

      - **Spark in HDInsight-voorbeeld (Scala)**

      In dit voorbeeld wordt een **Spark op HDInsight-voorbeeld (Scala)** sjabloon.

   c. Selecteer in de lijst **Build-programma** een van de volgende opties, afhankelijk van uw behoeften:

      - **Maven**, voor de ondersteuning van de wizard Scala-project maken

      -  **SBT**, voor het beheren van de afhankelijkheden en het maken van het Scala-project 

      ![Maak een project foutopsporing](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. Selecteer **Volgende**.     
 
1. In de volgende **nieuw Project** venster de volgende handelingen uit:

   ![Selecteer de Spark-SDK](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Voer een naam van het project en de projectlocatie.

   b. In de **Project SDK** vervolgkeuzelijst, selecteer **Java 1.8** voor **Spark 2.x** cluster of selecteer **Java 1.7** voor **Spark 1.x**  cluster.

   c. In de **Spark-versie** vervolgkeuzelijst, de wizard Scala-project maken voor Spark-SDK en Scala-SDK de juiste versie kan worden geïntegreerd. Als de spark-cluster-versie lager dan 2.0 is, selecteert u **Spark 1.x**. Selecteer anders **2.x Spark.** In dit voorbeeld wordt **Spark 2.0.2 (Scala 2.11.8)** gebruikt.

   d. Selecteer **Voltooien**.

1. Selecteer **src** > **belangrijkste** > **scala** aan uw code in het project openen. In dit voorbeeld wordt de **SparkCore_wasbloTest** script.

### <a name="prerequisite-for-windows"></a>Vereiste voor windows
Terwijl u de lokale Spark Scala-toepassing op een Windows-computer uitvoert, krijgt u mogelijk een uitzondering, zoals uitgelegd in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). De uitzondering treedt op omdat WinUtils.exe in Windows ontbreekt. 

Deze fout op te lossen [downloaden van het uitvoerbare bestand](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) naar een locatie zoals **C:\WinUtils\bin**. Vervolgens voegt u de omgevingsvariabele **HADOOP_HOME**, en stel de waarde van de variabele **C:\WinUtils**.

### <a name="scenario-2-perform-local-run"></a>Scenario 2: Voer lokaal uitvoeren
1. Open de **SparkCore_wasbloTest** script, met de rechtermuisknop op de script-editor en selecteer vervolgens de optie **uitvoeren '[Spark-taak] XXX'** om uit te voeren lokaal uitvoeren.
1. Eenmaal lokale uitvoering is voltooid, ziet u het uitvoerbestand opslaan in uw huidige Projectverkenner **gegevens** > **__standaard__**.

    ![Lokaal uitvoeren van resultaat](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-result.png)
1. Onze programma's hebben standaard lokale uitvoeringsconfiguratie automatisch bij het uitvoeren van de lokale uitvoeren en lokale foutopsporing ingesteld. Open de configuratie van de **[Spark-taak] XXX** in de rechterbovenhoek ziet u de **[Spark-taak] XXX** al zijn gemaakt onder **Azure HDInsight Spark-taak**. Schakel over naar **lokaal uitvoeren** tabblad.

    ![Lokale uitvoering configuratie](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/local-run-configuration.png)
    - [Omgevingsvariabelen](#prerequisite-for-windows): als u de systeemomgevingsvariabele al ingesteld **HADOOP_HOME** naar **C:\WinUtils**, automatisch kunnen detecteren dat hoeft niet handmatig toe te voegen.
    - [WinUtils.exe locatie](#prerequisite-for-windows): als u de omgevingsvariabele van het systeem niet hebt ingesteld, kunt u de locatie vinden door te klikken op de knop.
    - Kies een van twee opties en ze niet nodig zijn in MacOS en Linux.
1. U kunt ook instellen op de configuratie handmatig voordat u lokaal fouten opsporen en lokaal worden uitgevoerd. In de vorige schermafbeelding, selecteer het plusteken (**+**). Selecteer vervolgens de **Azure HDInsight Spark-taak** optie. Informatie invoeren voor **naam**, **de naam van de Main-klasse** om op te slaan, klik vervolgens op de knop voor lokale uitvoeren.

### <a name="scenario-3-perform-local-debugging"></a>Scenario 3: Voer lokale foutopsporing
1. Open de **SparkCore_wasbloTest** script, onderbrekingspunten instellen.
1. Met de rechtermuisknop op de script-editor, en selecteer vervolgens de optie **fouten opsporen in '[Spark-taak] XXX'** om uit te voeren lokale foutopsporing.   



## <a name="learn-how-to-perform-remote-run-and-debugging"></a>Leer hoe u extern uitvoeren en fouten opsporen
### <a name="scenario-1-perform-remote-run"></a>Scenario 1: Extern uitvoeren uitvoeren

1. Toegang krijgen tot de **configuraties bewerken** menu, selecteer het pictogram in de rechterbovenhoek. In dit menu kunt u maken of bewerken van de configuraties voor foutopsporing op afstand.

   ![Configuraties bewerken](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png) 

1. In de **uitvoeren/Debug configuraties** dialoogvenster vak, selecteer het plusteken (**+**). Selecteer vervolgens de **Azure HDInsight Spark-taak** optie.

   ![Nieuwe configuratie toevoegen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)
1. Schakel over naar **op afstand uitvoeren in Cluster** tabblad. Informatie invoeren voor **naam**, **Spark-cluster**, en **de naam van de Main-klasse**. Selecteer vervolgens **geavanceerde configuratie**. Onze hulpprogramma's voor ondersteuning van foutopsporing met **Executor**. De **numExectors**, de standaardwaarde is 5. U zou beter niet instellen hoger is dan 3.

   ![Foutopsporing configuraties uitvoeren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

1. In de **geavanceerde configuratie van Spark inzending** in het dialoogvenster, selecteer **externe foutopsporing voor Spark inschakelen**. De SSH-gebruikersnaam invoeren en een wachtwoord invoeren of een bestand met persoonlijke sleutel gebruiken. Als u wilt de configuratie op te slaan, selecteer **OK**. Als u foutopsporing op afstand uitvoeren wilt, moet u deze in te stellen. Er is niet nodig in te stellen als u alleen wilt gebruiken extern worden uitgevoerd.

   ![Externe foutopsporing voor Spark inschakelen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

1. De configuratie is nu opgeslagen met de naam die u hebt opgegeven. Als u wilt weergeven van informatie over de configuratie, door de configuratienaam te selecteren. Als u wilt wijzigen, selecteert u **configuraties bewerken**. 

1. Nadat u de configuratie-instellingen hebt voltooid, kunt u het project te voeren op het externe cluster of afstand fouten kunt opsporen.
   
   ![Knop voor extern uitvoeren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/perform-remote-run.png)

1. Klik op de **verbinding verbreken** knop waarmee de verzending van Logboeken niet in het linkerdeelvenster weergegeven. Het wordt echter nog steeds uitgevoerd op de back-end.

   ![Knop voor extern uitvoeren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/remote-run-result.png)



### <a name="scenario-2-perform-remote-debugging"></a>Scenario 2: Voer foutopsporing op afstand
1. Instellen van belangrijke punten en klik vervolgens op de **foutopsporing op afstand** pictogram. Het verschil met het indienen van externe is die SSH-gebruikersnaam en wachtwoord moeten worden geconfigureerd.

   ![Selecteer het pictogram voor foutopsporing](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

1. Wanneer de programma-uitvoering is het laatste punt bereikt, ziet u een **stuurprogramma** tabblad en twee **Executor** tabbladen de **foutopsporingsprogramma** deelvenster. Selecteer de **hervatten programma** pictogram om door te gaan met het uitvoeren van de code, die vervolgens het volgende onderbrekingspunt bereikt. U moet overschakelen naar de juiste **Executor** tabblad voor de doel-uitvoerder om op te sporen. U kunt de logboeken van taakuitvoeringen weergeven op de bijbehorende **Console** tabblad.

   ![Tabblad foutopsporing](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-3-perform-remote-debugging-and-bug-fixing"></a>Scenario 3: Fouten opsporen op afstand en het verhelpen van bug uitvoeren
1. Instellen van twee belangrijke punten en selecteer vervolgens de **Debug** pictogram om de externe foutopsporing proces te starten.

1. De code stopt op de eerste belangrijke punt en de parameter en de variabele informatie worden weergegeven in de **variabelen** deelvenster. 

1. Selecteer de **hervatten programma** pictogram om door te gaan. De code stopt op de tweede plaats. De uitzondering is opgetreden, zoals verwacht.

   ![Fout bij genereren](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png) 

1. Selecteer de **hervatten programma** pictogram opnieuw. De **HDInsight Spark-inzending** venster een fout "job uitvoeren is mislukt" weergegeven.

   ![Fout bij verzenden](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png) 

1. Als u wilt de variabele waarde dynamisch bijwerken met behulp van de mogelijkheid foutopsporing IntelliJ, selecteer **Debug** opnieuw. De **variabelen** deelvenster wordt opnieuw weergegeven. 

1. Het doel met de rechtermuisknop op de **Debug** tabblad, en selecteer vervolgens **waarde instellen**. Voer vervolgens een nieuwe waarde voor de variabele. Selecteer vervolgens **Enter** om op te slaan de waarde. 

   ![Waarde instellen](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value.png) 

1. Selecteer de **hervatten programma** pictogram om door te gaan het programma uit te voeren. Dit moment is geen uitzondering opgetreden. U kunt zien dat het project met succes wordt uitgevoerd zonder eventuele uitzonderingen.

   ![Fouten opsporen zonder uitzondering](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Volgende stappen
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demo
* Create Scala-project (video): [Apache Spark Scala-toepassingen maken](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Foutopsporing op afstand (video): [Azure-Toolkit voor IntelliJ voor foutopsporing van Apache Spark-toepassingen op afstand op een HDInsight-cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-hulpprogramma's](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens gebruiken](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight op de resultaten van voedingsinspectie voorspellen gebruiken](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Apache Spark-cluster met behulp van Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Azure Toolkit voor IntelliJ gebruiken voor het maken van Apache Spark-toepassingen voor een HDInsight-cluster](apache-spark-intellij-tool-plugin.md)
* [Azure Toolkit voor IntelliJ voor foutopsporing van Apache Spark-toepassingen op afstand via VPN-verbinding gebruiken](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het maken van Apache Spark-toepassingen](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in het Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
