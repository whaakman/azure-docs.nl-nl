---
title: Taken verzenden vanuit R-hulpprogramma's voor Visual Studio - Azure HDInsight
description: R-taken verzenden vanuit uw lokale computer in de Visual Studio naar een HDInsight-cluster.
services: hdinsight
ms.service: hdinsight
author: maxluk
ms.author: maxluk
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: 400cb16e4f4440283a783116c4ee843bc0a7344c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248568"
---
# <a name="submit-jobs-from-r-tools-for-visual-studio"></a>Taken verzenden vanuit R-hulpprogramma's voor Visual Studio

[R-hulpprogramma's voor Visual Studio](https://www.visualstudio.com/vs/rtvs/) (RTVS) is een gratis, open-source-uitbreiding voor de Community (gratis), Professional en Enterprise-edities van beide [Visual Studio 2017](https://www.visualstudio.com/downloads/), en [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)of hoger.

RTVS verbetert uw R-werkstroom door het aanbieden van hulpprogramma's zoals de [R interactieve venster](https://docs.microsoft.com/visualstudio/rtvs/interactive-repl) (REPL), intellisense (code is voltooid), [tekenen visualisatie](https://docs.microsoft.com/visualstudio/rtvs/visualizing-data) via R-bibliotheken zoals ggplot2 en ggviz, [Foutopsporing van R-code](https://docs.microsoft.com/visualstudio/rtvs/debugging), en nog veel meer.

## <a name="set-up-your-environment"></a>Uw omgeving instellen

1. Installeer [R Tools voor Visual Studio](https://docs.microsoft.com/visualstudio/rtvs/installation).

    ![RTVS in Visual Studio 2017 installeren](./media/r-server-submit-jobs-r-tools-vs/install-r-tools-for-vs.png)

2. Selecteer de *Data science en analytische toepassingen* werkbelasting, selecteer vervolgens de **R taalondersteuning**, **Runtime-ondersteuning voor de ontwikkeling van R**, en  **Microsoft R Client** opties.

3. U moet openbare en persoonlijke sleutels voor SSH-verificatie.
<!-- {TODO tbd, no such file yet}[use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) -->

4. Installeer [ML Server](https://msdn.microsoft.com/microsoft-r/rserver-install-windows) op uw computer. ML-Server biedt de [ `RevoScaleR` ](https://msdn.microsoft.com/microsoft-r/scaler/scaler) en `RxSpark` functies.

5. Installeer [PuTTY](http://www.putty.org/) voor een compute-context om uit te voeren `RevoScaleR` functies van de lokale client met uw HDInsight-cluster.

6. U hebt de mogelijkheid om toe te passen van de Data Science-instellingen aan uw Visual Studio-omgeving, waarmee u een nieuwe indeling voor uw werkruimte voor de R-hulpprogramma's.
    1. Om op te slaan in de huidige instellingen voor Visual Studio, gebruikt u de **Extra > instellingen exporteren en importeren** opdracht, en selecteer vervolgens **geselecteerde omgevingsinstellingen exporteren** en geef een bestandsnaam op. Gebruik dezelfde opdracht als u deze instellingen herstellen, en selecteer **importeren geselecteerd omgevingsinstellingen**.

    2. Ga naar de **R-hulpprogramma's** menu item, selecteer vervolgens **Data Science-instellingen...** .

        ![Data Science-instellingen...](./media/r-server-submit-jobs-r-tools-vs/data-science-settings.png)

    > [!NOTE]
    > Met behulp van de methode in stap 1, u kunt ook opslaan en herstellen van de indeling van de gegevenswetenschappers van elk persoonlijke gegevens in plaats van herhalende de **Data Science instellingen** opdracht.

## <a name="execute-local-r-methods"></a>Lokale R-methoden uitvoeren

1. Maak uw [ML-Services van HDInsight-cluster](r-server-get-started.md).
2. Installeer de [RTVS extensie](https://docs.microsoft.com/visualstudio/rtvs/installation).
3. Download de [voorbeelden zip-bestand](https://github.com/Microsoft/RTVS-docs/archive/master.zip).
4. Open `examples/Examples.sln` om te starten van de oplossing in Visual Studio.
5. Open de `1-Getting Started with R.R` -bestand in de `A first look at R` oplossingenmap.
6. Vanaf de bovenkant van het bestand, drukt u op Ctrl + Enter per regel, één voor één verzenden naar het R interactieve venster. Sommige regels kunnen even duren tijdens de installatie van pakketten.
    * U kunt ook kunt u selecteert u alle regels in het R-bestand (Ctrl + A), klikt u vervolgens alle (Ctrl + Enter) worden uitgevoerd, of Selecteer het uitvoeren van interactieve pictogram op de werkbalk.
        ![Uitvoeren van interactieve](./media/r-server-submit-jobs-r-tools-vs/execute-interactive.png)

7. Nadat alle regels in het script is uitgevoerd, ziet u uitvoer die vergelijkbaar is met dit:

    ![Data Science-instellingen...](./media/r-server-submit-jobs-r-tools-vs/workspace.png)

## <a name="submit-jobs-to-an-hdinsight-ml-services-cluster"></a>Verzenden van taken naar een ML-Services van HDInsight-cluster

Met behulp van een Microsoft ML Server/Microsoft R Client van een Windows-computer die zijn uitgerust met PuTTY, kunt u een compute-context die wordt uitgevoerd gedistribueerde `RevoScaleR` functies van de lokale client met uw HDInsight-cluster. Gebruik `RxSpark` te maken van de compute-context, op te geven uw gebruikersnaam, de Hadoop-cluster edge-knooppunt, SSH-switches, enzovoort.

1. Als u de hostnaam van uw edge-knooppunt zoekt, opent u het deelvenster ML-Services van HDInsight-cluster op Azure, en selecteer vervolgens **Secure Shell (SSH)** in het bovenste menu van het overzichtsvenster van.

    ![Secure Shell (SSH)](./media/r-server-submit-jobs-r-tools-vs/ssh.png)

2. Kopieer de **hostnaam van Edge-knooppunt** waarde.

    ![Hostnaam van Edge-knooppunt](./media/r-server-submit-jobs-r-tools-vs/edge-node.png)

3. Plak de volgende code in het R interactieve venster in Visual Studio, het wijzigen van de waarden van de setup-variabelen moeten overeenkomen met uw omgeving.

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
    
    ![Instellen van de Spark-context](./media/r-server-submit-jobs-r-tools-vs/spark-context.png)

4. Voer de volgende opdrachten uit in het R interactieve venster:

    ```R
    rxHadoopCommand("version") # should return version information
    rxHadoopMakeDir("/user/RevoShare/newUser") # creates a new folder in your storage account
    rxHadoopCopy("/example/data/people.json", "/user/RevoShare/newUser") # copies file to new folder
    ```

    Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    ![Uitvoering van de opdracht geslaagd rx](./media/r-server-submit-jobs-r-tools-vs/rx-commands.png)

5. Controleer de `rxHadoopCopy` met succes gekopieerde de `people.json` bestand van het voorbeeld van gegevens de map naar de zojuist gemaakte `/user/RevoShare/newUser` map:

    1. Selecteer in het deelvenster van de cluster HDInsight ML-Services in Azure **opslagaccounts** in het menu links.

        ![Opslagaccounts](./media/r-server-submit-jobs-r-tools-vs/storage-accounts.png)

    2. Selecteer het standaardopslagaccount voor uw cluster, zodat de naam van de container en de map.

    3. Selecteer **Containers** in het menu links in het deelvenster storage-account.

        ![Containers](./media/r-server-submit-jobs-r-tools-vs/containers.png)

    4. De containernaam van uw cluster selecteert, blader naar de **gebruiker** map (mogelijk moet u op *meer laden* aan de onderkant van de lijst) en selecteer vervolgens *RevoShare*, vervolgens **nieuwegebruiker**. De `people.json` bestand moet worden weergegeven de `newUser` map.

        ![Gekopieerde bestand](./media/r-server-submit-jobs-r-tools-vs/copied-file.png)

6. Nadat u klaar met de huidige Spark-context bent, moet u voorkomen dat deze. U kunt verschillende contexten niet tegelijk uitvoeren.

    ```R
    rxStopEngine(mySparkCluster)
    ```

## <a name="next-steps"></a>Volgende stappen

* [Opties voor COMPUTE context voor ML-Services op HDInsight](r-server-compute-contexts.md)
* [ScaleR en SparkR combineren](../hdinsight-hadoop-r-scaler-sparkr.md) geeft een voorbeeld van luchtvaartmaatschappij vertragingen van vluchten.
<!-- * You can also submit R jobs with the [R Studio Server](hdinsight-submit-jobs-from-r-studio-server.md) -->
