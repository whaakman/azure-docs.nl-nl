---
title: Aan de slag met R Server op HDInsight | Microsoft Docs
description: Informatie over hoe u een Apache Spark op HDInsight-cluster maakt dat R-Server omvat, en vervolgens een R-script op het cluster verzendt.
services: HDInsight
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/13/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: bf245d8885e6d6628f182e36e0a9c99a4854873a
ms.lasthandoff: 04/15/2017


---
# <a name="get-started-using-r-server-on-hdinsight"></a>Aan de slag met R Server op HDInsight

HDInsight bevat een R Server-optie die kan worden geïntegreerd in uw HDInsight-cluster. Hierdoor kunnen R-scripts gebruikmaken van Spark en MapReduce om gedistribueerde berekeningen uit te voeren. In dit document leert u hoe u een R Server op HDInsight-cluster maakt. Vervolgens voert u een R-script uit waarin wordt gedemonstreerd hoe Spark wordt gebruikt voor gedistribueerde R-berekeningen.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**: voordat u aan deze zelfstudie begint, moet u beschikken over een Azure-abonnement. Raadpleeg het artikel [Een gratis proefversie van Azure krijgen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) voor meer informatie.
* **Een SSH-client (Secure Shell)**: er wordt een SSH-client gebruikt om extern verbinding te maken met het HDInsight-cluster en om opdrachten rechtstreeks uit te voeren op het cluster. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
* **SSH-sleutels (optioneel)**: u kunt het SSH-account dat wordt gebruikt om verbinding te maken met het cluster, beveiligen met behulp van een wachtwoord of een openbare sleutel. Het is eenvoudiger om een wachtwoord te gebruiken. Bovendien kunt u dan aan de slag gaan zonder eerst een openbaar/persoonlijk sleutelpaar te hoeven maken. Het gebruik van een sleutel is echter veiliger.

> [!NOTE]
> Bij de stappen in dit document wordt ervan uitgegaan dat u een wachtwoord gebruikt.


### <a name="access-control-requirements"></a>Vereisten voor toegangsbeheer

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="automated-cluster-creation"></a>Automatisch een cluster maken

U kunt het maken van HDInsight R Servers automatiseren met ARM-sjablonen, de SDK of PowerShell.

* Zie [Deploy an R-server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Een HDInsight-cluster op basis van R Server implementeren) voor het maken van een R Server met een Azure Resource Manager-sjabloon.
* Zie [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) (In HDInsight op Linux gebaseerde clusters maken met de .NET-SDK) als u een R Server wilt maken met behulp van de .NET-SDK.
* Als u R Server wilt implementeren met PowerShell, raadpleegt u het artikel [Creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) (Een R Server in HDInsight maken met PowerShell).


## <a name="create-the-cluster-using-the-azure-portal"></a>Het cluster maken met Azure Portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

2. Selecteer **NIEUW**, **Intelligence + Analytics**, en vervolgens **HDInsight**.

    ![Afbeelding van het maken van een nieuw cluster](./media/hdinsight-getting-started-with-r/newcluster.png)

3. Voer onder **Snel maken** in het veld **Clusternaam** een naam in voor het cluster. Als u meerdere Azure-abonnementen hebt, gebruikt u de vermelding **Abonnement** om het abonnement te selecteren dat u wilt gebruiken.

    ![Clusternaam en geselecteerde abonnementen](./media/hdinsight-getting-started-with-r/clustername.png)

4. Selecteer **Clustertype** om de blade **Clusterconfiguratie** te openen. Selecteer op de blade **Clusterconfiguratie** de volgende opties:

   * **Clustertype**: R Server
   * **Versie**: selecteer de versie van R Server om te installeren op het cluster. Selecteer de nieuwste versie voor de nieuwste mogelijkheden. Andere versies zijn beschikbaar indien dit nodig is voor de compatibiliteit. Releaseopmerkingen voor alle beschikbare versies vindt u [hier](https://msdn.microsoft.com/en-us/microsoft-r/notes/r-server-notes).
   * **R Studio Community-editie voor R Server**: deze IDE op basis van een browser wordt standaard geïnstalleerd op het Edge-knooppunt.  Schakel het selectievakje uit als u deze liever niet wilt laten installeren. Als u ervoor kiest deze wel te laten installeren, kunt u de URL naar de aanmeldingspagina voor RStudio Server vinden op een portaltoepassingsblade voor het cluster nadat het is gemaakt.

   Laat bij de andere opties de standaardwaarden staan en gebruik de knop **Selecteren** om clustertype op te slaan.

   ![Schermafbeelding van de blade Clustertype](./media/hdinsight-getting-started-with-r/clustertypeconfig.png)

5. Voer een **gebruikersnaam** en **wachtwoord** voor aanmelden bij het cluster in.

   Geef een **SSH-gebruikersnaam** op.  SSH wordt gebruikt om extern verbinding te maken met het cluster met behulp van een **SSH-client (Secure Shell)**. U kunt de SSH-gebruiker in dit dialoogvenster opgeven of nadat het cluster is gemaakt (tabblad Configuratie voor het cluster). R Server is zo geconfigureerd dat een **SSH-gebruikersnaam** ´remoteuser´ wordt verwacht.  **Als u een andere gebruikersnaam gebruikt, moet u een extra stap uitvoeren nadat het cluster is gemaakt.**

   Als u een **WACHTWOORD** wilt gebruiken als verificatietype, laat u het vakje **Hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** ingeschakeld, tenzij u liever een openbare sleutel gebruikt.  U hebt een openbaar/persoonlijk sleutelpaar nodig voor toegang tot R Server op het cluster via een externe client, bijvoorbeeld RTVS, RStudio of een andere bureaublad-IDE. U moet een SSH-wachtwoord opgeven als u de RStudio Server Community-editie installeert.     

   Schakel **Hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** uit en selecteer vervolgens **OPENBARE SLEUTEL** om een openbaar/persoonlijk sleutelpaar te maken en te gebruiken. Volg de aangegeven stappen.  Bij deze instructies wordt ervan uitgegaan dat u Cygwin SSH-keygen of iets gelijkwaardigs hebt geïnstalleerd.

   * Een openbaar/persoonlijk sleutelpaar genereren via de opdrachtprompt op uw laptop:

   `ssh-keygen -t rsa -b 2048`

   * Volg de prompt om een sleutelbestand een naam te geven en voer vervolgens een wachtwoordzin in voor extra beveiliging. Het scherm moet er ongeveer als volgt uitzien:

   ![SSH-opdrachtregel in Windows](./media/hdinsight-getting-started-with-r/sshcmdline.png)

   * Hiermee maakt u een persoonlijk sleutelbestand en een openbaar sleutelbestand onder de naam <persoonlijke-sleutel-bestandsnaam.pub, bijvoorbeeld furiosa en furiosa.pub.

   ![SSH-dir](./media/hdinsight-getting-started-with-r/dir.png)

   * Geef vervolgens het openbare sleutelbestand (*.pub) op bij het toewijzen van de referenties voor het HDI-cluster. Ten slotte bevestigt u de resourcegroep en regio, en selecteert u **Volgende**

   ![Blade Referenties](./media/hdinsight-getting-started-with-r/publickeyfile.png)  

   * Machtigingen voor het persoonlijke sleutelbestand op uw laptop wijzigen

   `chmod 600 <private-key-filename>`

   * Het persoonlijke sleutelbestand met SSH gebruiken voor externe aanmelding

   `ssh –i <private-key-filename> remoteuser@<hostname public ip>`

   of als onderdeel van de definitie van de Hadoop Spark-compute-context voor R Server op de client. (Zie Microsoft R Server gebruiken als een Hadoop-client in de sectie [Een compute-context maken voor Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) in het onlinedocument [Aan de slag met ScaleR op Apache Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started).)

6. Via Snel maken wordt u naar de blade **Opslag** geleid om de instellingen voor het opslagaccount voor de primaire locatie van het HDFS-bestandssysteem te selecteren dat wordt gebruikt voor het cluster. Selecteer een nieuw of een bestaand Azure Storage-account of een bestaand Data Lake Storage-account.

   1. Als u een Azure Storage-account selecteert, kunt u een bestaand opslagaccount selecteren door eerst **Een opslagaccount selecteren** te selecteren en vervolgens het account te selecteren. Of maak een nieuw account via de koppeling **Nieuw** in de sectie **Een opslagaccount selecteren**.

      > [!NOTE]
      > Als u **Nieuw** selecteert, moet u een naam invoeren voor het nieuwe opslagaccount. Er verschijnt een groen vinkje als de naam is geaccepteerd.

      De **Standaardcontainer** krijgt dezelfde naam als het cluster. Laat deze waarde staan.

      Als er een nieuwe optie voor het opslagaccount is geselecteerd, ziet u een prompt om de **Locatie** te selecteren, waar u selecteert in welke regio het opslagaccount moet worden gemaakt.  

         ![Blade Gegevensbron](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > Als u de locatie van de standaardgegevensbron selecteert, wordt ook de locatie van het HDInsight-cluster ingesteld. Het cluster en de standaardgegevensbron moeten zich in dezelfde regio bevinden.

   2. Als u ervoor kiest om een bestaand Data Lake Store-account te gebruiken, selecteert u het gewenste ADLS-opslagaccount en voegt u de ADD-clusteridentiteit toe aan uw cluster om toegang tot Data Lake Store toe te staan. Raadpleeg [HDInsight-cluster met Data Lake Store maken met behulp van Azure Portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) voor meer informatie over dit proces.

   Gebruik de knop **Selecteren** om de configuratie van de gegevensbron op te slaan.


7. De blade **Samenvatting** wordt vervolgens weergegeven om al uw instellingen te valideren. Hier kunt u de **Clustergrootte** wijzigen om het aantal servers in het cluster te wijzigen, en ook eventuele **Scriptacties** opgeven die u wilt uitvoeren. Tenzij u zeker weet dat u een groter cluster nodig hebt, laat u het aantal worker-knooppunten ingesteld op het standaardaantal van `4`. De geschatte kosten voor het cluster worden in de blade weergegeven.

   ![clusteroverzicht](./media/hdinsight-getting-started-with-r/clustersummary.png)

   > [!NOTE]
   > Indien nodig, kunt u de grootte van het cluster later wijzigen via de portal (Cluster -> Instellingen -> Cluster schalen) om het aantal worker-knooppunten te verlagen of te verhogen.  Dit kan nuttig zijn als u het cluster minder actief wilt maken wanneer het niet wordt gebruikt, of om capaciteit toe te voegen wanneer dit nodig is voor grotere taken.
   >
   >

    Een aantal factoren waarmee u rekening moet houden wanneer u de grootte van het cluster, de gegevensknooppunten en het Edge-knooppunt wijzigt zijn:  

   * De prestaties van gedistribueerde R Server-analyses op Spark zijn gekoppeld aan het aantal worker-knooppunten wanneer er sprake is van grote hoeveelheden gegevens.  

   * De prestaties van R Server-analyses zijn lineair in de grootte van de gegevens die worden geanalyseerd. Bijvoorbeeld:  

     * Bij kleine tot gemiddelde hoeveelheden gegevens zijn de prestaties het beste wanneer de gegevens worden geanalyseerd in een lokale compute-context op het Edge-knooppunt.  Zie Opties voor compute-context voor R Server op HDInsight voor meer informatie over de scenario's waarin de lokale compute-context en de Spark-compute-context het beste werken.<br>
     * Als u zich aanmeldt bij het Edge-knooppunt en het R-script uitvoert, worden alle functies, met uitzondering van de ScaleR rx-functies, <strong>lokaal</strong> uitgevoerd op het Edge-knooppunt. De grootte van het geheugen en het aantal kernen van het Edge-knooppunt worden op dezelfde manier gewijzigd. Hetzelfde geldt dat als u R Server op HDI gebruikt als een externe compute-context vanaf uw laptop.

     ![Blade Prijscategorieën voor knooppunten](./media/hdinsight-getting-started-with-r/pricingtier.png)

     Gebruik de knop **Selecteren** om de prijsconfiguratie voor knooppunten op te slaan.

   U ziet hier ook de koppeling **Sjabloon en parameters downloaden**. Als u op deze koppeling klikt, worden er scripts weergegeven die kunnen worden gebruikt om automatisch een cluster te maken met de geselecteerde configuratie. Deze scripts zijn ook toegankelijk in Azure Portal-vermelding voor uw cluster zodra het is gemaakt.

   > [!NOTE]
   > Meestal duurt het ongeveer 20 minuten om het cluster te maken. Gebruik de tegel op het Startboard of de vermelding **Meldingen** aan de linkerkant van de pagina om dit proces te controleren.
   >
   >

## <a name="connect-to-rstudio-server"></a>Verbinding maken met RStudio Server

Als u ervoor hebt gekozen om de RStudio Server Community-editie op te nemen in de installatie, hebt u op twee manieren toegang tot de aanmeldingspagina van RStudio.

1. Door naar de volgende URL te gaan (waarbij **CLUSTERNAAM** de naam is van het cluster dat u hebt gemaakt):

    https://**CLUSTERNAAM**.azurehdinsight.net/rstudio/

2. Of door de vermelding voor het cluster te openen in Azure Portal. Selecteer de snelkoppeling naar het R Server-dashboard en selecteer vervolgens het R Server-dashboard:

     ![Het R Studio-dashboard openen](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Het R Studio-dashboard openen](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Ongeacht welke methode u kiest, de eerste keer dat u zich aanmeldt moet u zich twee keer verifiëren.  Bij de eerste verificatie geeft u de gebruikers-id en het wachtwoord voor de beheerder op voor het cluster. Bij de tweede prompt geeft u de gebruikers-id en het wachtwoord voor SSH op. Bij volgende aanmeldingen zijn alleen de gebruikers-id en het wachtwoord voor SSH vereist.

## <a name="connect-to-the-r-server-edge-node"></a>Verbinding maken met het R Server Edge-knooppunt

Verbinding maken met het R Server Edge-knooppunt van het HDInsight-cluster met behulp van SSH:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> U kunt het `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`-adres ook vinden in Azure Portal door het cluster te selecteren en vervolgens **Alle instellingen**, **Apps** en **RServer**. U ziet dan de SSH-eindpuntgegevens van het Edge-knooppunt.
>
> ![Afbeelding van het SSH-eindpunt voor het Edge-knooppunt](./media/hdinsight-getting-started-with-r/sshendpoint.png)
>
>

Als u een wachtwoord hebt gebruikt om het SSH-gebruikersaccount te beveiligen, wordt u gevraagd dit in te voeren. Als u een openbare sleutel hebt gebruikt, moet u mogelijk de `-i`-parameter gebruiken om de overeenkomende persoonlijke sleutel op te geven. Bijvoorbeeld `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`.

Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

Zodra er verbinding is gemaakt, wordt er een prompt weergegeven die er ongeveer als volgt uitziet.

`username@ed00-myrser:~$`

## <a name="use-the-r-console"></a>De R-console gebruiken

1. Gebruik tijdens de SSH-sessie de volgende opdracht om de R-console te starten.  

   ```
   R

   You will see output similar to the following.
   R version 3.2.2 (2015-08-14) -- "Fire Safety"
   Copyright (C) 2015 The R Foundation for Statistical Computing
   Platform: x86_64-pc-linux-gnu (64-bit)

   R is free software and comes with ABSOLUTELY NO WARRANTY.
   You are welcome to redistribute it under certain conditions.
   Type 'license()' or 'licence()' for distribution details.

   Natural language support but running in an English locale

   R is a collaborative project with many contributors.
   Type 'contributors()' for more information and
   'citation()' on how to cite R or R packages in publications.

   Type 'demo()' for some demos, 'help()' for on-line help, or
   'help.start()' for an HTML browser interface to help.
   Type 'q()' to quit R.

   Microsoft R Server version 8.0: an enhanced distribution of R
   Microsoft packages Copyright (C) 2016 Microsoft Corporation

   Type 'readme()' for release notes.
   >
   ```

2. U kunt de R-code invoeren vanuit de `>`-prompt. R-server bevat pakketten waarmee u eenvoudig kunt werken met Hadoop en gedistribueerde berekeningen kunt uitvoeren. Gebruik bijvoorbeeld de volgende opdracht om de hoofdmap te bekijken van het standaardbestandssysteem voor het HDInsight-cluster.

`rxHadoopListFiles("/")`

U kunt ook de adressering in WASB-stijl gebruiken.

`rxHadoopListFiles("wasbs:///")`

## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>R Server op HDI gebruiken vanaf een extern exemplaar van Microsoft R Server of Microsoft R Client

In de sectie hierboven over het gebruik van openbare/persoonlijke sleutelparen voor toegang tot het cluster ziet u dat het mogelijk is om toegang tot de HDI Hadoop Spark-context voor berekenen in te stellen vanaf een extern exemplaar van Microsoft R Server of Microsoft R Client, uitgevoerd op een desktop of laptop. (Zie Microsoft R Server gebruiken als een Hadoop-client in de sectie [Een context voor berekenen maken voor Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) in de onlinehandleiding [Aan de slag met RevoScaleR Hadoop Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started).)  Hiervoor moet u de volgende opties opgeven wanneer u de RxSpark-compute-context definieert op uw laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches en sshProfileScript. Bijvoorbeeld:

```
    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
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
```


## <a name="use-a-compute-context"></a>Een compute-context gebruiken

Met een compute-context kunt u bepalen of een berekening lokaal op het Edge-knooppunt wordt uitgevoerd of dat de berekening wordt gedistribueerd naar de verschillende knooppunten in het HDInsight-cluster.

1. Gebruik vanuit RStudio Server of vanuit de R-console (tijdens een SSH-sessie) het volgende om voorbeeldgegevens te laden in de standaardopslag voor HDInsight.

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"

        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)

        # Download data to the tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
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

2. Hierna gaan we enkele gegevens maken en twee gegevensbronnen definiëren zodat we met de gegevens kunnen werken.

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

3. We gaan eerst een logistic regression voor de gegevens uitvoeren met behulp van de lokale compute-context.

        # Set a local compute context
        rxSetComputeContext("local")

        # Run a logistic regression
        system.time(
           modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )

        # Display a summary
        summary(modelLocal)

    De uitvoer die u nu ziet, eindigt met regels die er ongeveer als volgt uitzien.

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

4. Hierna voeren we dezelfde logistic regression uit met behulp van Spark-context. Via de Spark-context wordt de verwerking gedistribueerd over alle worker-knooppunten in het HDInsight-cluster.

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
   > U kunt ook MapReduce gebruiken om de berekening te distribueren naar verschillende clusterknooppunten. Zie [Opties voor compute-context voor R Server op HDInsight](hdinsight-hadoop-r-server-compute-contexts.md) voor meer informatie over compute-context.


## <a name="distribute-r-code-to-multiple-nodes"></a>R-code distribueren naar meerdere knooppunten

Met R Server kunt u gemakkelijk bestaande R-code uitvoeren op verschillende knooppunten in het cluster door gebruik te maken van `rxExec`. Dit is handig bij het uitvoeren van een parameteropschoning of van simulaties. Hier volgt een voorbeeld van het gebruik van `rxExec`.

`rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )`

Als u nog steeds de Spark- of MapReduce-context gebruikt, wordt de knooppuntnaam geretourneerd voor het worker-knooppunt waarop de code `(Sys.info()["nodename"])` is uitgevoerd. Bijvoorbeeld: bij een cluster van vier knooppunten ontvangt u mogelijk uitvoer die er ongeveer als volgt uitziet.


    ```
    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"
    ```

## <a name="accessing-data-in-hive-and-parquet"></a>Toegang tot gegevens in Hive en Parquet

Dankzij een nieuwe functie die beschikbaar is in R Server 9.0 en hoger, hebt u nu direct toegang tot de gegevens in Hive en Parquet, zodat deze kunnen worden gebruikt voor ScaleR-functies in de Spark-compute-context. Deze mogelijkheden zijn beschikbaar via nieuwe functies voor ScaleR-gegevensbronnen (genaamd RxHiveData en RxParquetData) die gebruikmaken van Spark SQL om gegevens rechtstreeks in Spark DataFrame te laden voor analyse met ScaleR.  

Hieronder ziet u een voorbeeld van code bij het gebruik van de nieuwe functies:



    ```
    #..create a Spark compute context

    myHadoopCluster <- rxSparkConnect(reset = TRUE)
    ```


    ```
    #..retrieve some sample data from Hive and run a model

    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)
    ```

    ```
    #..retrieve some sample data from Parquet and run a model

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
    ```


    ```
    #..check on Spark data objects, cleanup, and close the Spark session

    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)
    ```

Raadpleeg de online-Help in R Server via de opdrachten ?RxHivedata en ?RxParquetdata voor aanvullende informatie over het gebruik van deze nieuwe functies.  


## <a name="install-r-packages"></a>R-pakketten installeren

Als u extra R-pakketten wilt installeren op het Edge-knooppunt, kunt u `install.packages()` rechtstreeks vanuit de R-console gebruiken wanneer u via SSH bent verbonden met het Edge-knooppunt. Als u echter R-pakketten wilt installeren op de worker-knooppunten van het cluster, moet u een scriptactie gebruiken.

Scriptacties zijn Bash-scripts die worden gebruikt om configuratiewijzigingen aan te brengen in het HDInsight-cluster, of om extra software te installeren. In dit geval worden ze gebruiken om extra R-pakketten te installeren. Gebruik de volgende stappen om extra pakketten te installeren met behulp van een scriptactie.

> [!IMPORTANT]
> U kunt scriptacties alleen gebruiken om extra R-pakketten te installeren nadat het cluster is gemaakt. U kunt geen scriptacties gebruiken tijdens het maken van het cluster, omdat voor een script is vereist dat R Server volledig is geïnstalleerd en geconfigureerd.
>
>

1. Selecteer in [Azure Portal](https://portal.azure.com) het R Server op HDInsight-cluster.

2. Selecteer op de blade **Instellingen** de optie **Scriptacties** en vervolgens **Nieuwe verzenden** om een nieuwe scriptactie te verzenden.

   ![Afbeelding van de blade Scriptacties](./media/hdinsight-getting-started-with-r/scriptaction.png)

3. Geef op de blade **Scriptactie verzenden** de volgende informatie op.

   * **Naam**: een beschrijvende naam om dit script te identificeren

   * **Bash-script-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Kop**: dit moet zijn **uitgeschakeld**

   * **Worker**: dit moet zijn **ingeschakeld**

   * **Edge-knooppunten**: dit moet zijn **uitgeschakeld**.

   * **Zookeeper**: dit moet zijn **uitgeschakeld**

   * **Parameters**: de R-pakketten die moeten worden geïnstalleerd. Bijvoorbeeld: `bitops stringr arules`

   * **Deze scriptactie opnieuw laten uitvoeren...**: dit moet zijn **ingeschakeld**  

   > [!NOTE]
   > 1. Standaard worden alle R-pakketten geïnstalleerd vanuit een momentopname van de Microsoft MRAN-opslagplaats, consistent met de R Server-versie die is geïnstalleerd.  Als u nieuwere versies van pakketten wilt installeren, kan het zijn dat deze niet compatibel zijn. In dit geval kunt u echter `useCRAN` opgeven als het eerste element van de lijst met pakketten, bijvoorbeeld `useCRAN bitops, stringr, arules`.  
   > 2. Voor sommige R-pakketten zijn aanvullende Linux-systeembibliotheken vereist. Voor het gemak hebben we de afhankelijkheden die nodig zijn voor de 100 meest populaire R-pakketten vooraf geïnstalleerd. Als voor de R-pakketten die u wilt installeren, meer bibliotheken zijn vereist, downloadt u het standaardscript dat hier wordt gebruikt, en voegt u stappen toe om de systeembibliotheken te installeren. Vervolgens moet u het gewijzigde script uploaden naar een openbare blobcontainer in Azure-opslag en het gewijzigde script gebruiken om de pakketten te installeren.
   >    Zie [Ontwikkeling van scriptacties](hdinsight-hadoop-script-actions-linux.md) voor meer informatie over het ontwikkelen van scriptacties.  
   >
   >

   ![Een scriptactie toevoegen](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. Selecteer **Maken** om het script uit te voeren. Nadat het script is voltooid, zijn de R-pakketten beschikbaar zijn op alle worker-knooppunten.

## <a name="using-microsoft-r-server-operationalization"></a>Microsoft R Server-uitoefening gebruiken

Wanneer de gegevensmodellering is voltooid, kunt u het model uitvoeren om voorspellingen te maken. Voer de stappen hieronder uit om Microsoft R Server-uitoefening te configureren.

Ten eerste, SSH op het Edge-knooppunt. Bijvoorbeeld ```ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net```.

Nadat u SSH hebt gebruikt, wijzigt u de map in de volgende map en sudo in dll zoals hieronder wordt weergegeven.

```
   cd /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil
   sudo dotnet Microsoft.DeployR.Utils.AdminUtil.dll
```

Ga als volgt te werk om Microsoft R Server-uitoefening te configureren met een alles-in-één configuratie:

* Selecteer 1. R-Server voor uitoefening configureren
* Selecteer A. Alles-in-één (web- + rekenknooppunten)
* Voer een wachtwoord in voor de gebruiker **beheerder**

![De optie Alles-in-één](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Als optionele stap kunt u Diagnostische controles uitvoeren door een diagnostische test uit te voeren zoals hieronder wordt weergegeven.

* Selecteer 6. Diagnostische tests uitvoeren
* Selecteer A. Configuratie testen
* Voer als gebruikersnaam Beheerder in en geef het wachtwoord uit de bovenstaande configuratiestap op
* Algehele status bevestigen = geslaagd
* Sluit het beheerprogramma
* Sluit SSH

![De optie Diagnose voor](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)

In dit stadium is de configuratie voor uitoefening voltooid. U kunt nu het pakket mrsdeploy op de RClient gebruiken om verbinding te maken met het Uitoefening op Edge-knooppunt en de bijbehorende functies gaan gebruiken, zoals [externe uitvoering](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) en [webservices](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). Afhankelijk van of het cluster is ingesteld in een virtueel netwerk of niet, moet u mogelijk forward tunneling via SSH-aanmelding instellen voor de poort. Hieronder wordt uitgelegd hoe dit werkt:

### <a name="rserver-cluster-on-virtual-network"></a>RServer-cluster in een virtueel netwerk

Zorg ervoor dat u verkeer via poort 12800 naar het Edge-knooppunt toestaat. Op deze manier kunt u het Edge-knooppunt gebruiken om verbinding te maken met de functie Uitoefening.

```
library(mrsdeploy)

remoteLogin(
    deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
    username = "admin",
    password = "xxxxxxx"
)
```

Als de remoteLogin() geen verbinding kan maken met het Edge-knooppunt maar als u wel verbinding hebt via SSH, moet u controleren of de regel op basis waarvan verkeer via poort 12800 is toegestaan, juist is ingesteld of niet. Als dit probleem zich blijft voordoen, kunt u een tijdelijke oplossing gebruiken door forward tunneling via SSH in te stellen voor de poort.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>RServer-cluster is niet ingesteld in het virtuele netwerk

Als het cluster niet is ingesteld in het virtuele netwerk vnet OF als u problemen ondervindt met de connectiviteit via dit netwerk, kunt u forward tunneling via SSH instellen voor de poort. Dit ziet er als volgt uit:

```
ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net
```

U kunt dit ook instellen op Putty.

![Putty SSH-verbinding](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Zodra de SSH-sessie actief is, wordt het verkeer bij poort 12800 op de computer via de SSH-sessie doorgestuurd naar poort 12800 van het Edge-knooppunt. Zorg ervoor dat u `127.0.0.1:12800` gebruikt in de remoteLogin()-methode. Hierdoor wordt u aangemeld bij de uitoefening van het Edge-knooppunt via doorsturen naar de poort.

```
library(mrsdeploy)

remoteLogin(
    deployr_endpoint = "http://127.0.0.1:12800",
    username = "admin",
    password = "xxxxxxx"
)
```

## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Hoe werkt het schalen van rekenknooppunten in Microsoft R Server-uitoefening op worker-knooppunten in HDInsight?


### <a name="decommission-the-worker-nodes"></a>De worker-knooppunten uit bedrijf nemen

Microsoft R Server wordt momenteel niet beheerd via Yarn. Als de worker-knooppunten niet uit bedrijf worden genomen, werkt de resourcemanager Yarn niet zoals verwacht, omdat in dit geval niet wordt gedetecteerd welke resources de server bevat. Om dit te voorkomen raden we u aan de worker-knooppunten uit bedrijf te nemen daar waar u de rekenknooppunten naartoe wilt schalen.

Stappen voor het uit bedrijf nemen van worker-knooppunten:

* Meld u bij de Ambari-console van het HDI-cluster en klik op het tabblad Hosts
* Selecteer de worker-knooppunten (die uit bedrijf moeten worden genomen), klik op Acties > Geselecteerd hosts > Hosts, en klik op Onderhoudsmodus inschakelen. In de onderstaande afbeelding zijn bijvoorbeeld wk3 en wk4 geselecteerd om uit bedrijf te worden genomen.  

   ![worker-knooppunten uit bedrijf nemen](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* Selecteer Acties > Geselecteerde hosts > Datanodes, en klik op Uit bedrijf nemen
* Selecteer Acties > Geselecteerde hosts > NodeManagers, en klik op Uit bedrijf nemen
* Selecteer Acties > Geselecteerde hosts > Datanodes, en klik op Stoppen
* Selecteer Acties > Geselecteerde hosts > NodeManagers, en klik op Stoppen
* Selecteer Acties > Geselecteerde hosts > Hosts, en klik op Alle onderdelen stoppen
* Hef de selectie van de worker-knooppunten op en selecteer de hoofdknooppunten
* Selecteer Acties > Geselecteerde hosts > Hosts > Alle onderdelen opnieuw starten


###    <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Rekenknooppunten configureren op elk uit bedrijf genomen worker-knooppunt

* SSH op elk uit bedrijf genomen worker-knooppunt
* Voer het beheerprogramma uit met behulp van `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`
* Voer 1 in om optie 1. R-Server voor uitoefening configureren te selecteren
* Voer C in om optie C. Rekenknooppunt te selecteren. Hiermee configureert u het rekenknooppunt op het worker-knooppunt.
* Sluit het beheerprogramma

### <a name="add-compute-nodes-details-on-web-node"></a>Details van rekenknooppunten toevoegen op het webknooppunt

Zodra alle uit bedrijf genomen worker-knooppunten zijn geconfigureerd om het rekenknooppunt uit te voeren, keert u terug naar het Edge-knooppunt en voegt u de IP-adressen van de uit bedrijf genomen worker-knooppunten toe in de configuratie van het Microsoft R Server-webknooppunt:

* SSH op het Edge-knooppunt
* Voer `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` uit.
* Ga naar de sectie URI's en voeg het IP-adres en de poortgegevens van het worker-knooppunt toe.

![opdrachtregel worker-knooppunten uit bedrijf nemen](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)

## <a name="next-steps"></a>Volgende stappen

Nu u weet hoe u een nieuw HDInsight-cluster maakt dat R Server bevat, en u de basistechnieken kent van het gebruik van de R-console vanuit een SSH-sessie, kunt u de volgende artikelen raadplegen voor manieren waarop u kunt werken met R Server op HDInsight.

* [RStudio Server toevoegen aan HDInsight (indien niet geïnstalleerd tijdens het maken van het cluster)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Opties voor compute-context voor R Server op HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Opties voor Azure-opslag voor R Server op HDInsight](hdinsight-hadoop-r-server-storage.md)

