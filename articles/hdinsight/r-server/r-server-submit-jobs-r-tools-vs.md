---
title: Verzenden van taken van R-hulpprogramma's voor Visual Studio - Azure HDInsight | Microsoft Docs
description: R-taken van uw lokale computer in de Visual Studio om een HDInsight-cluster te verzenden.
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: maxluk
ms.openlocfilehash: 1a82ba7790f739768156a8bee33a74d7130e24e1
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="submit-jobs-from-r-tools-for-visual-studio"></a>Verzenden van taken van R-hulpprogramma's voor Visual Studio

[R-Tools voor Visual Studio](https://www.visualstudio.com/vs/rtvs/) (RTVS) is een gratis, open source-uitbreiding voor de Community (gratis), Professional en Enterprise-edities van beide [Visual Studio 2017](https://www.visualstudio.com/downloads/), en [Visual Studio 2015 Update 3](http://go.microsoft.com/fwlink/?LinkId=691129) of hoger.

RTVS uw R-werkstroom verbetert door het aanbieden van hulpprogramma's zoals de [R interactieve venster](https://docs.microsoft.com/visualstudio/rtvs/interactive-repl) (REPL) intellisense (codevoltooiing), [tekenen visualisatie](https://docs.microsoft.com/visualstudio/rtvs/visualizing-data) via R bibliotheken zoals ggplot2 en ggviz, [R code foutopsporing](https://docs.microsoft.com/visualstudio/rtvs/debugging), en meer.

## <a name="set-up-your-environment"></a>Uw omgeving instellen

1. Installeer [R-Tools voor Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation).

    ![Installing RTVS in Visual Studio 2017](./media/r-server-submit-jobs-r-tools-vs/install-r-tools-for-vs.png)

2. Selecteer de *wetenschappelijke gegevens en analytische toepassingen* werkbelasting, selecteer vervolgens de **R taalondersteuning**, **Runtime-ondersteuning voor het ontwikkelen van R**, en  **Microsoft R Client** opties.

3. U moet de openbare en persoonlijke sleutels voor SSH-verificatie.
<!-- {TODO tbd, no such file yet}[use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) -->

4. Installeer [R Server](https://msdn.microsoft.com/microsoft-r/rserver-install-windows) op uw computer. R Server biedt de [ `RevoScaleR` ](https://msdn.microsoft.com/microsoft-r/scaler/scaler) en `RxSpark` functies.

5. Installeer [PuTTY](http://www.putty.org/) leveren een compute-context om uit te voeren `RevoScaleR` functies van de lokale client met uw HDInsight-cluster.

6. U hebt de optie voor het toepassen van de instellingen van de wetenschappelijke gegevens aan uw Visual Studio-omgeving, wat zorgt voor een nieuwe indeling voor uw werkruimte voor de R-hulpprogramma's.
    1. Voor het opslaan van uw huidige instellingen voor Visual Studio gebruiken de **Extra > importeren en exporteren van instellingen** opdracht en selecteer vervolgens **geselecteerde omgevingsinstellingen exporteren** en geef een bestandsnaam op. Gebruik dezelfde opdracht die om instellingen te herstellen, en selecteer **importeren geselecteerd omgevingsinstellingen**.

    2. Ga naar de **R extra** menu artikel, vervolgens selecteert u **gegevens wetenschappelijke instellingen...** .

        ![Instellingen voor het wetenschappelijke gegevens...](./media/r-server-submit-jobs-r-tools-vs/data-science-settings.png)

    > [!NOTE]
    > Met de benadering in stap 1 kunt u ook opslaan en herstellen van uw persoonlijke gegevens wetenschappelijk-indeling in plaats van herhalende de **instellingen van wetenschappelijke gegevens** opdracht.

## <a name="execute-local-r-methods"></a>Lokale R methoden uitvoeren

1. Maak uw [R Server HDInsight-cluster](r-server-get-started.md).
2. Installeer de [RTVS extensie](https://docs.microsoft.com/visualstudio/rtvs/installation).
3. Download de [voorbeelden zip-bestand](https://github.com/Microsoft/RTVS-docs/archive/master.zip).
4. Open `examples/Examples.sln` starten van de oplossing in Visual Studio.
5. Open de `1-Getting Started with R.R` bestand de `A first look at R` oplossingenmap.
6. Vanaf de bovenkant van het bestand, drukt u op Ctrl + Enter per regel één voor één verzenden naar het interactieve R-venster. Sommige regels kunnen even duren als ze pakketten installeren.
    * U kunt ook kunt u selecteren alle regels in het R-bestand (Ctrl + A), vervolgens ofwel alle (Ctrl + Enter) uitvoeren, of de interactieve uitvoeren pictogram op de werkbalk.
        ![Interactieve uitvoeren](./media/r-server-submit-jobs-r-tools-vs/execute-interactive.png)

7. Nadat alle regels in het script is uitgevoerd, ziet u uitvoer ongeveer als volgt:

    ![Instellingen voor het wetenschappelijke gegevens...](./media/r-server-submit-jobs-r-tools-vs/workspace.png)

## <a name="submit-jobs-to-an-hdinsight-r-cluster"></a>Verzenden van taken naar een cluster HDInsight R

Met behulp van een Microsoft R Server of Microsoft R-Client van een Windows-computer uitgerust met PuTTY kunt u een compute-context die wordt uitgevoerd gedistribueerde `RevoScaleR` functies van de lokale client met uw HDInsight-cluster. Gebruik `RxSpark` voor het maken van de compute-context, geven de gebruikersnaam, het Hadoop-cluster edge-knooppunt, SSH-switches, enzovoort.

1. Aan de hostnaam van de edge-knooppunt vinden, opent u het deelvenster HDInsight R cluster in Azure en selecteer **Secure Shell (SSH)** in het bovenste menu van het overzichtsvenster van.

    ![Secure Shell (SSH)](./media/r-server-submit-jobs-r-tools-vs/ssh.png)

2. Kopieer de **hostnaam Edge-knooppunt** waarde.

    ![Hostnaam van Edge-knooppunt](./media/r-server-submit-jobs-r-tools-vs/edge-node.png)

3. Plak de volgende code in de interactieve R venster in Visual Studio, wijzigen de waarden van de setup-variabelen moeten overeenkomen met uw omgeving.

    ```R
    # Setup variables that connect the compute context to your HDInsight cluster
    mySshHostname <- 'r-cluster-ed-ssh.azurehdinsight.net ' # HDI secure shell hostname
    mySshUsername <- 'sshuser' # HDI SSH username
    mySshClientDir <- "C:\\Program Files (x86)\\PuTTY"
    mySshSwitches <- '-i C:\\Users\\azureuser\\r.ppk' # Path to your private ssh key
    myHdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep = "/")
    myShareDir <- paste("/var/RevoShare", mySshUsername, sep = "/")
    mySshProfileScript <- "/usr/lib64/microsoft-r/3.3/hadoop/RevoHadoopEnvVars.site"

    # Create the Spark Cluster compute context
    mySparkCluster <- RxSpark(
          sshUsername = mySshUsername,
      sshHostname = mySshHostname,
      sshSwitches = mySshSwitches,
      sshProfileScript = mySshProfileScript,
      consoleOutput = TRUE,
      hdfsShareDir = myHdfsShareDir,
      shareDir = myShareDir,
      sshClientDir = mySshClientDir
    )
    
    # Set the current compute context as the Spark compute context defined above
    rxSetComputeContext(mySparkCluster)
    ```
    
    ![De context voor Spark instellen](./media/r-server-submit-jobs-r-tools-vs/spark-context.png)

4. Voer de volgende opdrachten in het venster R interactieve:

    ```R
    rxHadoopCommand("version") # should return version information
    rxHadoopMakeDir("/user/RevoShare/newUser") # creates a new folder in your storage account
    rxHadoopCopy("/example/data/people.json", "/user/RevoShare/newUser") # copies file to new folder
    ```

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    ![Rx-opdracht is uitgevoerd](./media/r-server-submit-jobs-r-tools-vs/rx-commands.png)

5. Controleer de `rxHadoopCopy` is gekopieerde de `people.json` bestand van de voorbeeld-gegevensmap naar het zojuist gemaakte `/user/RevoShare/newUser` map:

    1. Selecteer in het deelvenster HDInsight R cluster in Azure **opslagaccounts** uit in het menu links.

        ![Opslagaccounts](./media/r-server-submit-jobs-r-tools-vs/storage-accounts.png)

    2. Selecteer het standaardopslagaccount voor uw cluster, waardoor de naam van de container en de map.

    3. Selecteer **Containers** uit in het menu links op uw storage-account deelvenster.

        ![Containers](./media/r-server-submit-jobs-r-tools-vs/containers.png)

    4. Uw cluster containernaam selecteert, blader naar de **gebruiker** map (mogelijk moet u op *laden meer* onder aan de lijst), selecteert u vervolgens *RevoShare*, en vervolgens **nieuwegebruiker**. De `people.json` bestand moet worden weergegeven de `newUser` map.

        ![Gekopieerde bestand](./media/r-server-submit-jobs-r-tools-vs/copied-file.png)

6. Nadat u klaar met de huidige context voor Spark bent, moet u het stoppen. U kunt meerdere contexten niet tegelijk uitvoeren.

    ```R
    rxStopEngine(mySparkCluster)
    ```

## <a name="next-steps"></a>Volgende stappen

* [Opties voor compute-context voor R Server op HDInsight](r-server-compute-contexts.md)
* [ScaleR en SparkR combineren](../hdinsight-hadoop-r-scaler-sparkr.md) bevat een voorbeeld van luchtvaartmaatschappij vlucht vertraging voorspellingen.
<!-- * You can also submit R jobs with the [R Studio Server](hdinsight-submit-jobs-from-r-studio-server.md) -->
