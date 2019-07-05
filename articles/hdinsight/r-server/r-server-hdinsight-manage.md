---
title: Cluster op HDInsight - Azure ML-Services beheren
description: Informatie over het beheren van een ML-Services-cluster in Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: fa838f371607f3c0b0f76f81d6755c842a5901f7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448966"
---
# <a name="manage-ml-services-cluster-on-azure-hdinsight"></a>Cluster op HDInsight van Azure ML-Services beheren

In dit artikel leert u hoe u voor het beheren van een bestaand cluster ML-Services op Azure HDInsight om uit te voeren taken, zoals het toevoegen van meerdere gelijktijdige gebruikers, op afstand verbinding maken met een ML-Services-cluster wijzigen compute-context, enzovoort.

## <a name="prerequisites"></a>Vereisten

* Een ML-Services-cluster in HDInsight. Zie [Apache Hadoop-clusters maken met behulp van de Azure-portal](../hdinsight-hadoop-create-linux-clusters-portal.md) en selecteer **ML-Services** voor **clustertype**.


* Een Secure Shell (SSH)-client: Een SSH-client wordt gebruikt om op afstand verbinding maken met het HDInsight-cluster en opdrachten rechtstreeks op het cluster uitvoeren. Zie voor meer informatie, [SSH gebruiken met HDInsight.](../hdinsight-hadoop-linux-use-ssh-unix.md).


## <a name="enable-multiple-concurrent-users"></a>Meerdere gelijktijdige gebruikers inschakelen

U kunt meerdere gelijktijdige gebruikers voor ML-Services-cluster in HDInsight inschakelen door het toevoegen van meer gebruikers voor het edge-knooppunt waarop de RStudio community-versie wordt uitgevoerd. Wanneer u een HDInsight-cluster maakt, moet u twee gebruikers opgeven, te weten een HTTP-gebruiker en een SSH-gebruiker:

![Gelijktijdige gebruiker 1](./media/r-server-hdinsight-manage/concurrent-users-1.png)

- **Gebruikersnaam voor aanmelding cluster**: een HTTP-gebruiker voor verificatie via de HDInsight-gateway die wordt gebruikt voor het beveiligen van de HDInsight-clusters die u hebt gemaakt. Deze HTTP-gebruiker wordt gebruikt voor toegang tot de Apache Ambari-Webgebruikersinterface, Apache Hadoop YARN-gebruikersinterface, evenals andere UI-onderdelen.
- **Secure Shell (SSH)-gebruikersnaam**: een SSH-gebruiker voor toegang tot het cluster via Secure Shell. Deze gebruiker is een gebruiker in het Linux-systeem voor alle hoofdknooppunten, werkknooppunten en Edge-knooppunten. U kunt Secure Shell gebruiken voor toegang tot alle knooppunten in een extern cluster.

De R Studio Server Community-versie die wordt gebruikt in het ML-Services-cluster op HDInsight accepteert alleen Linux-gebruikersnaam en wachtwoord als een teken in mechanisme. Doorgegeven tokens worden niet ondersteund. Wanneer u probeert toegang tot R Studio voor het eerst op een cluster ML-Services, moet u dus twee keer aanmelden.

- Eerste melden zich aan met de referenties van de HTTP-gebruiker via de HDInsight-Gateway. 

- Gebruik vervolgens de referenties van de SSH-gebruiker aanmelden bij RStudio.
  
Op dit moment kan slechts één SSH gebruikersaccount worden gemaakt tijdens het inrichten van een HDInsight-cluster. Dus als u wilt dat meerdere gebruikers toegang hebben tot ML-Services-cluster op HDInsight, moet u extra gebruikers in het Linux-systeem maken.

Omdat RStudio wordt uitgevoerd op het edge-knooppunt van het cluster, zijn er verschillende stappen:

1. Gebruik de bestaande SSH-gebruiker zich aanmeldt bij het edge-knooppunt
2. Meer Linux-gebruikers toevoegen in Edge-knooppunt
3. RStudio Community-versie gebruiken met de gemaakte gebruiker

### <a name="step-1-use-the-created-ssh-user-to-sign-in-to-the-edge-node"></a>Stap 1: Gebruik de SSH-gebruiker zich aanmeldt bij het edge-knooppunt

Volg de instructies op [verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) voor toegang tot het edge-knooppunt. Het adres van het edge-knooppunt voor ML-Services-cluster in HDInsight wordt `CLUSTERNAME-ed-ssh.azurehdinsight.net`.

### <a name="step-2-add-more-linux-users-in-edge-node"></a>Stap 2: Meer Linux-gebruikers toevoegen in Edge-knooppunt

Om een gebruiker toe te voegen aan het Edge-knooppunt, voert u de volgende opdrachten uit:

    # Add a user 
    sudo useradd <yournewusername> -m

    # Set password for the new user
    sudo passwd <yournewusername>

De volgende schermafbeelding ziet u de uitvoer.

![Gelijktijdige gebruiker 3](./media/r-server-hdinsight-manage/concurrent-users-2.png)

Wanneer u wordt gevraagd voor ' Huidig Kerberos-wachtwoord: ', drukt u **Enter** om dit te negeren. De `-m`-optie in de opdracht `useradd` geeft aan dat het systeem een basismap voor de gebruiker maakt die vereist is voor RStudio Community-versie.

### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>Stap 3: RStudio Community-versie gebruiken met de gemaakte gebruiker

Toegang tot RStudio van https://CLUSTERNAME.azurehdinsight.net/rstudio/. Als u zijn aangemeld voor het eerst na het maken van het cluster, voert u de beheerdersreferenties van het cluster gevolgd door de SSH-gebruikersreferenties die u hebt gemaakt. Als dit niet de eerste keer aanmelden, moet u alleen de referenties invoeren voor de SSH-gebruiker die u hebt gemaakt.

U kunt ook aanmelden met de oorspronkelijke referenties (dit is standaard *sshuser*) tegelijkertijd vanuit een ander browservenster.

U ziet ook dat de zojuist toegevoegde gebruikers geen hoofdmapbevoegdheden in het Linux-systeem hebben, maar wel dezelfde toegang tot alle bestanden in de externe HDFS- en WASB-opslag hebben.

## <a name="connect-remotely-to-microsoft-ml-services"></a>Extern verbinding maken met Microsoft ML-Services

U kunt toegang instellen voor de HDInsight Spark-compute-context van een extern exemplaar van ML-Client die wordt uitgevoerd op het bureaublad. Om dit te doen, moet u de opties (hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches en sshProfileScript) wanneer het definiëren van de RxSpark compute-context op het bureaublad: Bijvoorbeeld:

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

Zie voor meer informatie de sectie 'Met behulp van Microsoft Machine Learning Server als een Apache Hadoop Client' in [RevoScaleR gebruiken in een Apache Spark-compute-context](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark#more-spark-scenarios)

## <a name="use-a-compute-context"></a>Een compute-context gebruiken

Met een compute-context kunt u bepalen of een berekening lokaal op het Edge-knooppunt wordt uitgevoerd of wordt gedistribueerd naar de verschillende knooppunten in het HDInsight-cluster.  Zie voor een voorbeeld van het instellen van een compute-context met RStudio Server [een R-script uitvoeren op een cluster ML-Services in Azure HDInsight met behulp van RStudio Server](machine-learning-services-quickstart-job-rstudio.md).

## <a name="distribute-r-code-to-multiple-nodes"></a>R-code distribueren naar meerdere knooppunten

Met ML-Services op HDInsight, kunt u bestaande R-code en uitvoeren op meerdere knooppunten in het cluster met behulp van `rxExec`. Deze functie is handig bij het uitvoeren van een parameteropschoning of van simulaties. Hier volgt een voorbeeldcode van het gebruik van `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Als u nog steeds de Spark-context gebruikt, met deze opdracht de knooppuntnaam geretourneerd voor de worker-knooppunten die de code `(Sys.info()["nodename"])` op wordt uitgevoerd. Bij een cluster van vier knooppunten verwacht u bijvoorbeeld voor het ontvangen van uitvoer is vergelijkbaar met het volgende codefragment:

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

## <a name="access-data-in-apache-hive-and-parquet"></a>Toegang tot gegevens in Apache Hive en Parquet

HDInsight ML-Services kunt direct toegang tot gegevens in Hive en Parquet gebruikt voor ScaleR-functies in de Spark compute-context. Deze mogelijkheden zijn beschikbaar via nieuwe functies voor ScaleR-gegevensbronnen (genaamd RxHiveData en RxParquetData) die gebruikmaken van Spark SQL om gegevens rechtstreeks in Spark DataFrame te laden voor analyse met ScaleR.

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


Zie voor meer informatie over het gebruik van deze nieuwe functies de online-help in ML-Services door gebruik te maken van de `?RxHivedata` en `?RxParquetData` opdrachten.  

## <a name="install-additional-r-packages-on-the-cluster"></a>Extra R-pakketten installeren op het cluster

### <a name="to-install-r-packages-on-the-edge-node"></a>R-pakketten installeren op het edge-knooppunt

Als u extra R-pakketten installeren op het edge-knooppunt wilt, kunt u `install.packages()` rechtstreeks vanuit de R-console, eenmaal verbonden met het edge-knooppunt via SSH. 

### <a name="to-install-r-packages-on-the-worker-node"></a>R-pakketten installeren op de worker-knooppunt

Voor het installeren van R-pakketten op de worker-knooppunten van het cluster, moet u een scriptactie gebruiken. Scriptacties zijn Bash-scripts die worden gebruikt om configuratiewijzigingen aan te brengen in het HDInsight-cluster of om extra software te installeren, zoals extra R-pakketten. 

> [!IMPORTANT]  
> U kunt scriptacties alleen gebruiken om extra R-pakketten te installeren nadat het cluster is gemaakt. Gebruik deze procedure niet tijdens het maken van het cluster, zoals het script maakt gebruik van ML-Services die zijn volledig wordt geconfigureerd.

1. Volg de stappen in [clusters aanpassen met Script Action](../hdinsight-hadoop-customize-cluster-linux.md).

3. Voor **scriptactie verzenden**, geef de volgende informatie:

   * Voor **scripttype**, selecteer **aangepaste**.

   * Voor **naam**, Geef een naam op voor de scriptactie.

     * Voor **Bash-script-URI**, voer `https://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`. Dit is het script die extra R-pakketten op de worker-knooppunt installeert

   * Schakel het selectievakje in als u alleen voor **Worker**.

   * **Parameters**: De R-pakketten worden geïnstalleerd. Bijvoorbeeld: `bitops stringr arules`

   * Schakel het selectievakje in om te **deze scriptactie**.  

   > [!NOTE]
   > 1. Standaard worden alle R-pakketten geïnstalleerd vanuit een momentopname van de Microsoft MRAN-opslagplaats consistent zijn met de versie van ML-Server die is geïnstalleerd. Als u nieuwere versies van pakketten wilt installeren, is er kans op incompatibiliteit. Dit soort installatie is echter mogelijk door `useCRAN` op te geven als het eerste element van de pakketlijst, bijvoorbeeld `useCRAN bitops, stringr, arules`.  
   > 2. Voor sommige R-pakketten zijn aanvullende Linux-systeembibliotheken vereist. Voor het gemak de ML-Services voor HDInsight wordt geleverd vooraf worden geïnstalleerd met de afhankelijkheden die nodig zijn voor de top 100 meest populaire R-pakketten. Als voor de R-pakketten die u wilt installeren, meer bibliotheken zijn vereist, downloadt u het standaardscript dat hier wordt gebruikt, en voegt u stappen toe om de systeembibliotheken te installeren. Vervolgens moet u het gewijzigde script uploaden naar een openbare blobcontainer in Azure-opslag en het gewijzigde script gebruiken om de pakketten te installeren.
   >    Zie [Ontwikkeling van scriptacties](../hdinsight-hadoop-script-actions-linux.md) voor meer informatie over het ontwikkelen van scriptacties.  
   >
   >

   ![Een scriptactie toevoegen](./media/r-server-hdinsight-manage/submitscriptaction.png)

4. Selecteer **Maken** om het script uit te voeren. Nadat het script is voltooid, zijn de R-pakketten beschikbaar op alle werkknooppunten.

## <a name="next-steps"></a>Volgende stappen

* [ML Services-cluster operationeel maken in HDInsight](r-server-operationalize.md)
* [Opties voor COMPUTE context voor ML-Service-cluster in HDInsight](r-server-compute-contexts.md)
* [Opties voor Azure Storage voor ML Services-cluster in HDInsight](r-server-storage.md)
