---
title: Beheer van clusters op HDInsight - Azure ML-Services | Microsoft Docs
description: Informatie over het beheren van een cluster ML-Services in Azure HDInsight.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: bb3af3b1614c8afc98d2dcf12ecb53fb80b6037a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049741"
---
# <a name="manage-ml-services-cluster-on-azure-hdinsight"></a>ML-Services-cluster in Azure HDInsight beheren

In dit artikel leert u hoe voor het beheren van een bestaand cluster ML-Services in Azure HDInsight om uit te voeren taken, zoals het toevoegen van meerdere gelijktijdige gebruikers, extern verbinding maakt met een cluster ML-Services, het wijzigen van compute-context, enzovoort.

## <a name="prerequisites"></a>Vereisten

* **Een cluster ML-Services op HDInsight**: Zie voor instructies [aan de slag met ML-Services op HDInsight](r-server-get-started.md).

* **Een SSH-client (Secure Shell)**: er wordt een SSH-client gebruikt om extern verbinding te maken met het HDInsight-cluster en om opdrachten rechtstreeks uit te voeren op het cluster. Zie voor meer informatie [SSH gebruiken met HDInsight.](../hdinsight-hadoop-linux-use-ssh-unix.md).


## <a name="enable-multiple-concurrent-users"></a>Meerdere gelijktijdige gebruikers inschakelen

U kunt meerdere gelijktijdige gebruikers voor cluster in HDInsight ML Services inschakelen door het toevoegen van meer gebruikers voor het edge-knooppunt waarop de RStudio community-versie wordt uitgevoerd. Wanneer u een HDInsight-cluster maakt, moet u twee gebruikers opgeven, te weten een HTTP-gebruiker en een SSH-gebruiker:

![Gelijktijdige gebruiker 1](./media/r-server-hdinsight-manage/concurrent-users-1.png)

- **Gebruikersnaam voor aanmelding cluster**: een HTTP-gebruiker voor verificatie via de HDInsight-gateway die wordt gebruikt voor het beveiligen van de HDInsight-clusters die u hebt gemaakt. Deze HTTP-gebruiker wordt gebruikt voor toegang tot de Ambari-gebruikersinterface, de YARN-gebruikersinterface en andere gebruikersinterfaceonderdelen.
- **Secure Shell (SSH)-gebruikersnaam**: een SSH-gebruiker voor toegang tot het cluster via Secure Shell. Deze gebruiker is een gebruiker in het Linux-systeem voor alle hoofdknooppunten, werkknooppunten en Edge-knooppunten. U kunt Secure Shell gebruiken voor toegang tot alle knooppunten in een extern cluster.

De R Studio Server Community-versie die wordt gebruikt in het cluster ML-Services op HDInsight accepteert alleen een Linux-gebruikersnaam en wachtwoord als een mechanisme voor aanmelding. Doorgegeven tokens worden niet ondersteund. Wanneer u probeert te krijgen van R Studio voor het eerst op een cluster ML-Services, moet u dus tweemaal aanmelden.

- Zich eerst aanmelden met de referenties van de HTTP-gebruiker via de Gateway van HDInsight. 

- Gebruik vervolgens de SSH-referenties aan te melden bij RStudio.
  
Op dit moment kan slechts één SSH gebruikersaccount worden gemaakt tijdens het inrichten van een HDInsight-cluster. Dus om meerdere gebruikers toegang tot cluster in HDInsight ML-Services, moet u extra gebruikers in het Linux-systeem.

Omdat RStudio op de edge-knooppunt van het cluster wordt uitgevoerd, zijn er verschillende volgende stappen uit:

1. Gebruik de bestaande SSH-gebruiker zich aanmeldt bij het edge-knooppunt
2. Meer Linux-gebruikers toevoegen in Edge-knooppunt
3. RStudio Community-versie gebruiken met de gemaakte gebruiker

### <a name="step-1-use-the-created-ssh-user-to-sign-in-to-the-edge-node"></a>Stap 1: De SSH-gebruiker zich aanmeldt bij het edge-knooppunt gebruiken

Volg de instructies voor [verbinding maken met HDInsight (Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) voor toegang tot het edge-knooppunt. Het adres van de edge-knooppunt voor ML-Services-cluster in HDInsight is `CLUSTERNAME-ed-ssh.azurehdinsight.net`.

### <a name="step-2-add-more-linux-users-in-edge-node"></a>Stap 2: meer Linux-gebruikers toevoegen in Edge-knooppunt

Om een gebruiker toe te voegen aan het Edge-knooppunt, voert u de volgende opdrachten uit:

    # Add a user 
    sudo useradd <yournewusername> -m

    # Set password for the new user
    sudo passwd <yournewusername>

De volgende schermafbeelding ziet de uitvoer.

![Gelijktijdige gebruiker 3](./media/r-server-hdinsight-manage/concurrent-users-2.png)

Als u wordt gevraagd naar 'Huidig Kerberos-wachtwoord:', drukt u op **Enter** om dit te negeren. De `-m`-optie in de opdracht `useradd` geeft aan dat het systeem een basismap voor de gebruiker maakt die vereist is voor RStudio Community-versie.

### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>Stap 3: RStudio Community-versie gebruiken met de gemaakte gebruiker

Toegang tot RStudio van https://CLUSTERNAME.azurehdinsight.net/rstudio/. Als u aan te melden voor de eerste keer na het maken van het cluster, voert u de beheerdersreferenties van het cluster gevolgd door de SSH-gebruikersreferenties die u hebt gemaakt. Als dit niet uw eerste aanmelding, Voer alleen de referenties voor de SSH-gebruiker die u hebt gemaakt.

U kunt ook aanmelden met de oorspronkelijke referenties (standaard is *sshuser*) gelijktijdig vanaf een ander browservenster.

U ziet ook dat de zojuist toegevoegde gebruikers geen hoofdmapbevoegdheden in het Linux-systeem hebben, maar wel dezelfde toegang tot alle bestanden in de externe HDFS- en WASB-opslag hebben.

## <a name="connect-remotely-to-microsoft-ml-services"></a>Extern verbinding maken met Microsoft ML-Services

U kunt instellen toegang aan de HDInsight Hadoop, Spark compute-context van een extern exemplaar van de Client ML uitgevoerd op uw bureaublad. Om dit te doen, moet u de opties (hdfsShareDir, shareDir sshUsername, sshHostname, sshSwitches en sshProfileScript) wanneer de RxSpark definiëren compute context op het bureaublad: bijvoorbeeld:

    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- '<clustername>-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- '<sshuser>'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )

Zie voor meer informatie de sectie 'Met behulp van Microsoft Machine Learning-Server als een Hadoop Client' in [RevoScaleR gebruiken in een Spark compute-context](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark#more-spark-scenarios)

## <a name="use-a-compute-context"></a>Een compute-context gebruiken

Met een compute-context kunt u bepalen of een berekening lokaal op het Edge-knooppunt wordt uitgevoerd of wordt gedistribueerd naar de verschillende knooppunten in het HDInsight-cluster.

1. Gebruik vanuit RStudio Server of vanuit de R-console (tijdens een SSH-sessie) de volgende code om voorbeeldgegevens te laden in de standaardopslag voor HDInsight:

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
        remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))

        # Set directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Vervolgens maken van sommige gegevens info en twee gegevensbronnen definiëren.

        # Define the HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Voer een logistic regression via de gegevens met behulp van de lokale compute-context.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    Hier ziet u uitvoer die met regels die vergelijkbaar zijn met het volgende fragment eindigt:

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                         Estimate Std. Error z value Pr(>|z|)
         (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
         ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
         ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
         ......
         DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
         DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
         DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
         DEST=BPT         Dropped    Dropped Dropped  Dropped

         ---

         Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

         Condition number of final variance-covariance matrix: 11904202
         Number of iterations: 7

4. Voer de dezelfde logistic regression met behulp van de Spark-context. De Spark-context distribueert de verwerking over alle werkknooppunten in het HDInsight-cluster.

        # Define the Spark compute context
        mySparkCluster <- RxSpark()

        # Set the compute context
        rxSetComputeContext(mySparkCluster)

        # Run a logistic regression
        system.time(  
           modelSpark <- rxLogit(formula, data = airOnTimeData)
        )

        # Display a summary
        summary(modelSpark)


   > [!NOTE]
   > U kunt ook MapReduce gebruiken om de berekening te distribueren naar verschillende clusterknooppunten. Zie voor meer informatie over compute context [Compute context opties voor het ML-Services op HDInsight-cluster](r-server-compute-contexts.md).

## <a name="distribute-r-code-to-multiple-nodes"></a>R-code distribueren naar meerdere knooppunten

Met ML-Services op HDInsight, u kunt bestaande R-code en uitvoeren op meerdere knooppunten in het cluster met behulp van `rxExec`. Deze functie is handig bij het uitvoeren van een parameteropschoning of van simulaties. Hier volgt een voorbeeldcode van het gebruik van `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Als u nog steeds de Spark- of MapReduce-context gebruikt, wordt door deze opdracht de knooppuntnaam geretourneerd voor het werkknooppunt waarop de code `(Sys.info()["nodename"])` wordt uitgevoerd. Bijvoorbeeld: op een cluster met vier knooppunten u verwacht te ontvangen uitvoer die vergelijkbaar is met het volgende fragment:

    $rxElem1
        nodename
    "wn3-mymlser"

    $rxElem2
        nodename
    "wn0-mymlser"

    $rxElem3
        nodename
    "wn3-mymlser"

    $rxElem4
        nodename
    "wn3-mymlser"

## <a name="access-data-in-hive-and-parquet"></a>Gegevens openen in Hive en Parquet

HDInsight ML Services kunnen rechtstreekse toegang tot gegevens in de component en parketvloeren voor gebruik door ScaleR functies in de context van de compute Spark. Deze mogelijkheden zijn beschikbaar via nieuwe functies voor ScaleR-gegevensbronnen (genaamd RxHiveData en RxParquetData) die gebruikmaken van Spark SQL om gegevens rechtstreeks in Spark DataFrame te laden voor analyse met ScaleR.

Hieronder ziet u code met voorbeeldcode bij het gebruik van de nieuwe functies:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Zie voor meer informatie over het gebruik van deze nieuwe functies de online-help in ML-Services door het gebruik van de `?RxHivedata` en `?RxParquetData` opdrachten.  

## <a name="install-additional-r-packages-on-the-cluster"></a>Extra R-pakketten installeren op het cluster

### <a name="to-install-r-packages-on-the-edge-node"></a>R-pakketten installeren op de edge-knooppunt

Als u aanvullende R-pakketten installeren op de edge-knooppunt wilt, kunt u `install.packages()` rechtstreeks vanuit de console R eenmaal verbonden met het edge-knooppunt via SSH. 

### <a name="to-install-r-packages-on-the-worker-node"></a>R-pakketten installeren op het werkrolknooppunt

Voor het installeren van R-pakketten op de worker-knooppunten van het cluster, moet u een Script in te grijpen. Scriptacties zijn Bash-scripts die worden gebruikt om configuratiewijzigingen aan te brengen in het HDInsight-cluster of om extra software te installeren, zoals extra R-pakketten. 

> [!IMPORTANT]
> U kunt scriptacties alleen gebruiken om extra R-pakketten te installeren nadat het cluster is gemaakt. Gebruik deze procedure niet tijdens het maken van het cluster, zoals het script is afhankelijk van ML-Services die volledig worden geconfigureerd.
>
>

1. Volg de stappen in [clusters met behulp van de scriptactie aanpassen](../hdinsight-hadoop-customize-cluster-linux.md).

3. Voor **indienen scriptactie**, geef de volgende informatie:

   * Voor **type Script**, selecteer **aangepaste**.

   * Voor **naam**, Geef een naam voor de scriptactie.

    * Voor **Bash script URI**, voer `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`. Dit is het script waarmee extra R-pakketten worden geïnstalleerd op het werkrolknooppunt

   * Schakel het selectievakje alleen voor **Worker**.

   * **Parameters**: de R-pakketten die moeten worden geïnstalleerd. Bijvoorbeeld: `bitops stringr arules`

   * Schakel het selectievakje in om te **deze scriptactie**.  

   > [!NOTE]
   > 1. Standaard worden alle R-pakketten geïnstalleerd vanuit een momentopname van de Microsoft MRAN-opslagplaats consistent zijn met de versie van ML-Server die is geïnstalleerd. Als u nieuwere versies van pakketten wilt installeren, is er kans op incompatibiliteit. Dit soort installatie is echter mogelijk door `useCRAN` op te geven als het eerste element van de pakketlijst, bijvoorbeeld `useCRAN bitops, stringr, arules`.  
   > 2. Voor sommige R-pakketten zijn aanvullende Linux-systeembibliotheken vereist. Voor het gemak het ML-Services van HDInsight wordt geleverd vooraf worden geïnstalleerd met de afhankelijkheden die nodig is voor de top 100 meest populaire R-pakketten. Als voor de R-pakketten die u wilt installeren, meer bibliotheken zijn vereist, downloadt u het standaardscript dat hier wordt gebruikt, en voegt u stappen toe om de systeembibliotheken te installeren. Vervolgens moet u het gewijzigde script uploaden naar een openbare blobcontainer in Azure-opslag en het gewijzigde script gebruiken om de pakketten te installeren.
   >    Zie [Ontwikkeling van scriptacties](../hdinsight-hadoop-script-actions-linux.md) voor meer informatie over het ontwikkelen van scriptacties.  
   >
   >

   ![Een scriptactie toevoegen](./media/r-server-hdinsight-manage/submitscriptaction.png)

4. Selecteer **Maken** om het script uit te voeren. Nadat het script is voltooid, zijn de R-pakketten beschikbaar op alle werkknooppunten.

## <a name="next-steps"></a>Volgende stappen

* [Operationeel ML-Services-cluster in HDInsight](r-server-operationalize.md)
* [Opties voor ML Service-cluster in HDInsight context voor berekenen](r-server-compute-contexts.md)
* [Azure ML-Services-cluster in HDInsight opslagopties](r-server-storage.md)
