---
title: Aan de slag met R Server op HDInsight - Azure | Microsoft Docs
description: Informatie over hoe u een Apache Spark op HDInsight-cluster maakt dat R-Server omvat en over het verzenden van een R-script op het cluster.
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/14/2017
ms.author: bradsev
ms.openlocfilehash: 89fa80b3e3409b7cd2f600776fffdeb3a5271b5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-using-r-server-on-hdinsight"></a>Aan de slag met R Server op HDInsight

HDInsight bevat een R Server-optie die kan worden geïntegreerd in uw HDInsight-cluster. Met deze optie kunnen R-scripts gebruikmaken van Spark en MapReduce om gedistribueerde berekeningen uit te voeren. In dit document leert u hoe u een R Server op HDInsight-cluster maakt. Vervolgens voert u een R-script uit waarin wordt gedemonstreerd hoe Spark wordt gebruikt voor gedistribueerde R-berekeningen.


## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**: voordat u aan deze zelfstudie begint, moet u beschikken over een Azure-abonnement. Raadpleeg het artikel [Een gratis proefversie van Microsoft Azure krijgen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) voor meer informatie.
* **Een SSH-client (Secure Shell)**: er wordt een SSH-client gebruikt om extern verbinding te maken met het HDInsight-cluster en om opdrachten rechtstreeks uit te voeren op het cluster. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
* **SSH-sleutels (optioneel)**: u kunt het SSH-account dat wordt gebruikt om verbinding te maken met het cluster, beveiligen met behulp van een wachtwoord of een openbare sleutel. Het is eenvoudiger om een wachtwoord te gebruiken. Bovendien kunt u dan aan de slag gaan zonder eerst een openbaar/persoonlijk sleutelpaar te hoeven maken. Het gebruik van een sleutel is echter veiliger.

> [!NOTE]
> Bij de stappen in dit document wordt ervan uitgegaan dat u een wachtwoord gebruikt.


## <a name="automated-cluster-creation"></a>Automatisch een cluster maken

U kunt het maken van HDInsight R Servers automatiseren met Azure Resource Manager-sjablonen, de SDK of PowerShell.

* Zie [Deploy an R-server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Een HDInsight-cluster op basis van R Server implementeren) voor het maken van een R Server met een Azure Resource Manager-sjabloon.
* Zie [Create Linux-based clusters in HDInsight using the .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) (In HDInsight op Linux gebaseerde clusters maken met de .NET-SDK) als u een R Server wilt maken met behulp van de .NET-SDK.
* Als u R Server wilt implementeren met PowerShell, raadpleegt u het artikel [Creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) (Een R Server in HDInsight maken met PowerShell).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a>Het cluster maken met Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **NIEUW** -> **Intelligence en analyse**, -> **HDInsight**.

    ![Afbeelding van het maken van een nieuw cluster](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. Voer onder **Snel maken** in het veld **Clusternaam** een naam in voor het cluster. Als u meerdere Azure-abonnementen hebt, gebruikt u de vermelding **Abonnement** om het abonnement te selecteren dat u wilt gebruiken.

    ![Clusternaam en geselecteerde abonnementen](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. Selecteer **Clustertype** om de blade **Clusterconfiguratie** te openen. Selecteer op de blade **Clusterconfiguratie** de volgende opties:

    * **Clustertype**: R Server
    * **Versie**: selecteer de versie van R Server om te installeren op het cluster. De momenteel beschikbare versie is ***R Server 9.1 (HDI 3.6)***. Releaseopmerkingen voor de beschikbare versies van R Server vindt u [hier](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **R Studio Community-editie voor R Server**: deze IDE op basis van een browser wordt standaard geïnstalleerd op het Edge-knooppunt. Schakel het selectievakje uit als u deze liever niet wilt laten installeren. Als u ervoor kiest deze wel te laten installeren, bevindt de URL naar de aanmeldingspagina voor RStudio Server zich op een portaltoepassingsblade voor het cluster nadat het is gemaakt.
    * Laat bij de andere opties de standaardwaarden staan en gebruik de knop **Selecteren** om clustertype op te slaan.

        ![Schermafbeelding van de blade Clustertype](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. Voer een **gebruikersnaam** en **wachtwoord** voor aanmelden bij het cluster in.

    Geef een **SSH-gebruikersnaam** op. SSH wordt gebruikt om extern verbinding te maken met het cluster met behulp van een **SSH-client (Secure Shell)**. U kunt de SSH-gebruiker in dit dialoogvenster opgeven of nadat het cluster is gemaakt (in het tabblad Configuratie voor het cluster). R Server is zo geconfigureerd dat een **SSH-gebruikersnaam** ´remoteuser´ wordt verwacht.  **Als u een andere gebruikersnaam gebruikt, moet u een extra stap uitvoeren nadat het cluster is gemaakt.**

    Als u een **WACHTWOORD** wilt gebruiken als verificatietype, laat u het vakje **Hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** ingeschakeld, tenzij u liever een openbare sleutel gebruikt.  U hebt een openbaar/persoonlijk sleutelpaar nodig voor toegang tot R Server op het cluster via een externe client, bijvoorbeeld RTVS, RStudio of een andere bureaublad-IDE. Als u de RStudio Server Community-editie installeert, moet u een SSH-wachtwoord opgeven.     

    Schakel **Hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** uit en selecteer vervolgens **OPENBARE SLEUTEL** en volg de aangegeven stappen. Bij deze instructies wordt ervan uitgegaan dat u Cygwin SSH-keygen of iets gelijkwaardigs hebt geïnstalleerd.

    * Een openbaar/persoonlijk sleutelpaar genereren via de opdrachtprompt op uw laptop:

        ssh-keygen -t rsa -b 2048

    * Volg de prompt om een sleutelbestand een naam te geven en voer vervolgens een wachtwoordzin in voor extra beveiliging. Het scherm moet er ongeveer als volgt uitzien:

        ![SSH-opdrachtregel in Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * Met deze opdracht maakt u een persoonlijk sleutelbestand en een openbaar sleutelbestand onder de naam <persoonlijke-sleutel-bestandsnaam>.pub, bijvoorbeeld furiosa en furiosa.pub.

        ![SSH-dir](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * Geef vervolgens het openbare sleutelbestand (&#42;.pub) op bij het toewijzen van de referenties voor het HDI-cluster. Ten slotte bevestigt u de resourcegroep en regio, en selecteert u **Volgende**.

        ![Blade Referenties](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * Machtigingen voor het persoonlijke sleutelbestand op uw laptop wijzigen:

        chmod 600 <bestandsnaam-persoonlijke-sleutel>

   * Het persoonlijke sleutelbestand met SSH gebruiken voor externe aanmelding:

        ssh – i <bestandsnaam-persoonlijke-sleutel> remoteuser@<hostname public ip>

      Of als onderdeel van de definitie van uw Hadoop Spark-compute-context voor R Server op de client. Zie de subsectie **Using Microsoft R Server as a Hadoop Client** (Microsoft R Server als Hadoop Client gebruiken) [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) (een Compute-Context voor Spark maken).

6. Via Snel maken wordt u naar de blade **Opslag** geleid om de instellingen voor het opslagaccount voor de primaire locatie van het HDFS-bestandssysteem te selecteren dat wordt gebruikt voor het cluster. Selecteer een nieuw of een bestaand Azure Storage-account of een bestaand Data Lake Storage-account.

    - Als u een Azure Storage-account selecteert, wordt een bestaand opslagaccount geselecteerd door eerst **Een opslagaccount selecteren** te kiezen en vervolgens het relevante account te selecteren. Maak een nieuw account via de koppeling **Nieuw** in de sectie **Een opslagaccount selecteren**.

      > [!NOTE]
      > Als u **Nieuw** selecteert, moet u een naam invoeren voor het nieuwe opslagaccount. Er verschijnt een groen vinkje als de naam wordt geaccepteerd.

      De **Standaardcontainer** krijgt dezelfde naam als het cluster. Laat deze standaardwaarde staan.

      Als er een nieuwe optie voor het opslagaccount is geselecteerd, ziet u een prompt om de **Locatie** te selecteren, waar u selecteert in welke regio het opslagaccount moet worden gemaakt.  

         ![Blade Gegevensbron](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > Als u de locatie van de standaardgegevensbron selecteert, wordt ook de locatie van het HDInsight-cluster ingesteld. Het cluster en de standaardgegevensbron moeten zich in dezelfde regio bevinden.

    - Als u een bestaand Data Lake Store-account wilt gebruiken, selecteert u het gewenste ADLS-opslagaccount en voegt u de *ADD*-clusteridentiteit toe aan uw cluster om toegang tot Data Lake Store toe te staan. Raadpleeg [Creating HDInsight cluster with Data Lake Store using Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) (HDInsight-cluster met Data Lake Store maken met behulp van Azure Portal) voor meer informatie over dit proces.

    Gebruik de knop **Selecteren** om de configuratie van de gegevensbron op te slaan.


7. De blade **Samenvatting** wordt vervolgens weergegeven om al uw instellingen te valideren. Hier kunt u de **Clustergrootte** wijzigen om het aantal servers in het cluster te wijzigen, en ook eventuele **Scriptacties** opgeven die u wilt uitvoeren. Tenzij u zeker weet dat u een groter cluster nodig hebt, laat u het aantal worker-knooppunten ingesteld op het standaardaantal van `4`. De geschatte kosten voor het cluster worden in de blade weergegeven.

    ![clusteroverzicht](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > Indien nodig, kunt u de grootte van het cluster later wijzigen via de portal (**Cluster** -> **Instellingen** -> **Cluster schalen**) om het aantal werkknooppunten te verlagen of te verhogen.  De grootte aanpassen kan nuttig zijn als u het cluster minder actief wilt maken wanneer het niet wordt gebruikt of om capaciteit toe te voegen wanneer dit nodig is voor grotere taken.
   >
   >

   Een aantal factoren waarmee u rekening moet houden wanneer u de grootte van het cluster, de gegevensknooppunten en het Edge-knooppunt wijzigt zijn:  

   * De prestaties van gedistribueerde R Server-analyses op Spark zijn gekoppeld aan het aantal worker-knooppunten wanneer er sprake is van grote hoeveelheden gegevens.  

   * De prestaties van R Server-analyses zijn lineair in de grootte van de gegevens die worden geanalyseerd. Bijvoorbeeld:  

     * Bij kleine tot gemiddelde hoeveelheden gegevens zijn de prestaties het beste wanneer de gegevens worden geanalyseerd in een lokale compute-context op het Edge-knooppunt.  Zie Opties voor compute-context voor R Server op HDInsight voor meer informatie over de scenario's waarin de lokale compute-context en de Spark-compute-context het beste werken.<br>
     * Als u zich aanmeldt bij het Edge-knooppunt en uw R-script vervolgens uitvoert, worden alle functies behalve de ScaleR rx-functie <strong>lokaal</strong> uitgevoerd op het Edge-knooppunt. Dus moeten het geheugen en het aantal kernen van het Edge-knooppunt dienovereenkomstig worden aangepast. Hetzelfde geldt dat als u R Server op HDI gebruikt als een externe compute-context vanaf uw laptop.

     ![Blade Prijscategorieën voor knooppunten](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     Gebruik de knop **Selecteren** om de prijsconfiguratie voor knooppunten op te slaan.

   Er is ook een koppeling **Sjabloon en parameters downloaden**. Klik op deze koppeling om scripts weer te geven die kunnen worden gebruikt om automatisch een cluster te maken met de geselecteerde configuratie. Deze scripts zijn ook toegankelijk in Azure Portal-vermelding voor uw cluster zodra het is gemaakt.

   > [!NOTE]
   > Meestal duurt het genereren van het cluster ongeveer 20 minuten. Gebruik de tegel op het Startboard of de vermelding **Meldingen** aan de linkerkant van de pagina om dit proces te controleren.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>Verbinding maken met RStudio Server

Als u ervoor hebt gekozen om de RStudio Server Community-editie op te nemen in de installatie, hebt u op twee manieren toegang tot de aanmeldingspagina van RStudio.

1. Door naar de volgende URL te gaan (waarbij **CLUSTERNAAM** de naam is van het cluster dat u hebt gemaakt):

    https://**CLUSTERNAAM**.azurehdinsight.net/rstudio/

2. Of door de vermelding voor het cluster te openen in Azure Portal. Selecteer de snelkoppeling naar het **R Server-dashboard** en selecteer vervolgens het **R Studio-dashboard**:

     ![Het R Studio-dashboard openen](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Het R Studio-dashboard openen](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Ongeacht welke methode u kiest, de eerste keer dat u zich aanmeldt moet u zich twee keer verifiëren.  Bij de eerste verificatie geeft u de *gebruikers-id* en het *wachtwoord* voor de beheerder op voor het cluster. Bij de tweede prompt geeft u de *gebruikers-id* en het *wachtwoord* voor SSH op. Bij alle volgende aanmeldingen zijn alleen de *gebruikers-id* en het *wachtwoord* voor SSH vereist.

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a>Verbinding maken met het R Server Edge-knooppunt

Verbinding maken met het R Server Edge-knooppunt van het HDInsight-cluster met SSH met de opdracht:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> U kunt het `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`-adres vinden in Azure Portal door uw cluster te selecteren en vervolgens **Alle instellingen** -> **Apps en**  -> **RServer**. U ziet dan de SSH-eindpuntgegevens van het Edge-knooppunt.
>
> ![Afbeelding van het SSH-eindpunt voor het Edge-knooppunt](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

Als u een wachtwoord hebt gebruikt om uw SSH gebruikersaccount te beveiligen, wordt u gevraagd het wachtwoord in te voeren. Als u een openbare sleutel hebt gebruikt, moet u mogelijk de `-i`-parameter gebruiken om de overeenkomende persoonlijke sleutel op te geven. Bijvoorbeeld:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Zie voor meer informatie [Verbinding maken met HDInsight (Hadoop) via SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

Zodra er verbinding is gemaakt, wordt er een prompt weergegeven die er ongeveer als volgt uitziet:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Meerdere gelijktijdige gebruikers inschakelen

U kunt meerdere gelijktijdige gebruikers inschakelen door meer gebruikers voor het Edge-knooppunt toe te voegen waarop de RStudio-community-versie wordt uitgevoerd.

Wanneer u een HDInsight-cluster maakt, moet u twee gebruikers opgeven, te weten een HTTP-gebruiker en een SSH-gebruiker:

![Gelijktijdige gebruiker 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- **Gebruikersnaam voor aanmelding cluster**: een HTTP-gebruiker voor verificatie via de HDInsight-gateway die wordt gebruikt voor het beveiligen van de HDInsight-clusters die u hebt gemaakt. Deze HTTP-gebruiker wordt gebruikt voor toegang tot de Ambari-gebruikersinterface, de YARN-gebruikersinterface en andere gebruikersinterfaceonderdelen.
- **Secure Shell (SSH)-gebruikersnaam**: een SSH-gebruiker voor toegang tot het cluster via Secure Shell. Deze gebruiker is een gebruiker in het Linux-systeem voor alle hoofdknooppunten, werkknooppunten en Edge-knooppunten. U kunt Secure Shell gebruiken voor toegang tot alle knooppunten in een extern cluster.

De R Studio Server Community-versie die wordt gebruikt in het clustertype Microsoft R Server op HDInsight accepteert alleen een Linux-gebruikersnaam en -wachtwoord als aanmeldingsmechanisme. Doorgegeven tokens worden niet ondersteund. Dus als u een nieuw cluster hebt gemaakt en u wilt R Studio gebruiken om dat te openen, moet u zich twee keer aanmelden.

- Meld u eerst aan met de referenties van de HTTP-gebruiker via de Gateway van HDInsight: 

    ![Gelijktijdige gebruiker 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- Gebruik vervolgens de referenties van de SSH-gebruiker om u aan te melden bij RStudio:
  
    ![Gelijktijdige gebruiker 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

Op dit moment kan slechts één SSH gebruikersaccount worden gemaakt tijdens het inrichten van een HDInsight-cluster. Om meerdere gebruikers toegang tot Microsoft R Server op HDInsight-clusters te geven, moeten er extra gebruikers in het Linux-systeem worden gemaakt.

Omdat RStudio Server Community op het Edge-knooppunt van het cluster wordt uitgevoerd, zijn er verschillende stappen:

1. Gebruik de SSH-gebruiker om u aan te melden bij het Edge-knooppunt
2. Meer Linux-gebruikers toevoegen in Edge-knooppunt
3. RStudio Community-versie gebruiken met de gemaakte gebruiker

### <a name="step-1-use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Stap 1: de SSH-gebruiker gebruiken om u aan te melden bij het Edge-knooppunt

Download een SSH-hulpprogramma (zoals Putty) en gebruik de bestaande SSH-gebruiker om u aan te melden. Volg de instructies in [Verbinding maken met HDInsight (Hadoop) via SSH](hdinsight-hadoop-linux-use-ssh-unix.md) voor toegang tot het Edge-knooppunt. Het adres van het Edge-knooppunt voor R Server op HDInsight-cluster is: *clusternaam-ed-ssh.azurehdinsight.net*


### <a name="step-2-add-more-linux-users-in-edge-node"></a>Stap 2: meer Linux-gebruikers toevoegen in Edge-knooppunt

Om een gebruiker toe te voegen aan het Edge-knooppunt, voert u de volgende opdrachten uit:

    sudo useradd yournewusername -m
    sudo passwd yourusername

De volgende items moeten worden geretourneerd: 

![Gelijktijdige gebruiker 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

Als u wordt gevraagd naar 'Huidig Kerberos-wachtwoord:', drukt u op **Enter** om dit te negeren. De `-m`-optie in de opdracht `useradd` geeft aan dat het systeem een basismap voor de gebruiker maakt die vereist is voor RStudio Community-versie.


### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>Stap 3: RStudio Community-versie gebruiken met de gemaakte gebruiker

Gebruik de gebruiker die is gemaakt voor het aanmelden bij RStudio:

![Gelijktijdige gebruiker 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

U ziet dat RStudio aangeeft dat u van de nieuwe gebruiker gebruikmaakt (hier bijvoorbeeld *sshuser6*) om u aan te melden in het cluster: 

![Gelijktijdige gebruiker 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

U kunt u ook tegelijkertijd vanuit een ander browservenster aanmelden met de oorspronkelijke referenties (dit is standaard *sshuser*).

U kunt een taak met scaleR-functies verzenden. Hier volgt een voorbeeld van de opdrachten om een taak uit te voeren:

    # Set the HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder.
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

    # Set directory in bigDataDirRoot to load the data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory.
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for the airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names.
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context.
    mySparkCluster <- RxSpark()

    # Set the compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


U ziet dat de taken die worden verzonden onder andere gebruikersnamen in de gebruikersinterface van YARN worden weergegeven:

![Gelijktijdige gebruiker 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

U ziet ook dat de zojuist toegevoegde gebruikers geen hoofdmapbevoegdheden in het Linux-systeem hebben, maar wel dezelfde toegang tot alle bestanden in de externe HDFS- en WASB-opslag hebben.


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a>De R-console gebruiken

1. Gebruik tijdens de SSH-sessie de volgende opdracht om de R-console te starten:  

        R

2. De uitvoer ziet er als volgt uit:
    
        R version 3.2.2 (2015-08-14) -- "Fire Safety"
        Copyright (C) 2015 The R Foundation for Statistical Computing
        Platform: x86_64-pc-linux-gnu (64-bit)

        R is free software and comes with ABSOLUTELY NO WARRANTY.
        You are welcome to redistribute it under certain conditions.
        Type 'license()' or 'licence()' for distribution details.

    Ondersteuning van natuurlijke taal, maar uitgevoerd in een Engelse landinstelling

        R is a collaborative project with many contributors.
        Type 'contributors()' for more information and
        'citation()' on how to cite R or R packages in publications.

        Type 'demo()' for some demos, 'help()' for on-line help, or
        'help.start()' for an HTML browser interface to help.
        Type 'q()' to quit R.

        Microsoft R Server version 8.0: an enhanced distribution of R
        Microsoft packages Copyright (C) 2016 Microsoft Corporation

    Typ 'readme()' voor de release-opmerkingen.
    >

3. U kunt de R-code invoeren vanuit de `>`-prompt. R-server bevat pakketten waarmee u eenvoudig kunt werken met Hadoop en gedistribueerde berekeningen kunt uitvoeren. Gebruik bijvoorbeeld de volgende opdracht om de hoofdmap te bekijken van het standaardbestandssysteem voor het HDInsight-cluster:

        rxHadoopListFiles("/")

4. U kunt ook de adressering in WASB-stijl gebruiken.

        rxHadoopListFiles("wasb:///")


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>R Server op HDI gebruiken vanaf een extern exemplaar van Microsoft R Server of Microsoft R Client

Het is mogelijk toegang in te stellen voor de HDI Hadoop Spark compute-context van een extern exemplaar van Microsoft R Server of Microsoft R Client dat wordt uitgevoerd op een desktop of laptop. Zie de subsectie **Using Microsoft R Server as a Hadoop Client** (Microsoft R Server als Hadoop-client gebruiken) in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md) (Een compute-context voor Spark maken). Hiervoor moet u de volgende opties opgeven wanneer u de RxSpark-compute-context definieert op uw laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches en sshProfileScript. Bijvoorbeeld:


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


## <a name="use-a-compute-context"></a>Een compute-context gebruiken

Met een compute-context kunt u bepalen of een berekening lokaal op het Edge-knooppunt wordt uitgevoerd of wordt gedistribueerd naar de verschillende knooppunten in het HDInsight-cluster.

1. Gebruik vanuit RStudio Server of vanuit de R-console (tijdens een SSH-sessie) de volgende code om voorbeeldgegevens te laden in de standaardopslag voor HDInsight:

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

    De uitvoer die u nu ziet, eindigt met regels die er ongeveer als volgt uitzien:

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

4. Hierna voeren we dezelfde logistic regression uit met behulp van Spark-context. De Spark-context distribueert de verwerking over alle werkknooppunten in het HDInsight-cluster.

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

Met R Server kunt u gemakkelijk bestaande R-code uitvoeren op verschillende knooppunten in het cluster door gebruik te maken van `rxExec`. Deze functie is handig bij het uitvoeren van een parameteropschoning of van simulaties. Hier volgt een voorbeeldcode van het gebruik van `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Als u nog steeds de Spark- of MapReduce-context gebruikt, wordt door deze opdracht de knooppuntnaam geretourneerd voor het werkknooppunt waarop de code `(Sys.info()["nodename"])` wordt uitgevoerd. Bijvoorbeeld: bij een cluster van vier knooppunten verwacht u uitvoer die er ongeveer als volgt uitziet:

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


## <a name="accessing-data-in-hive-and-parquet"></a>Toegang tot gegevens in Hive en Parquet

Dankzij een functie die beschikbaar is in R Server 9.1, hebt u nu direct toegang tot de gegevens in Hive en Parquet, zodat deze kunnen worden gebruikt voor ScaleR-functies in de Spark-compute-context. Deze mogelijkheden zijn beschikbaar via nieuwe functies voor ScaleR-gegevensbronnen (genaamd RxHiveData en RxParquetData) die gebruikmaken van Spark SQL om gegevens rechtstreeks in Spark DataFrame te laden voor analyse met ScaleR.  

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


Raadpleeg de online-Help in R Server via de opdrachten `?RxHivedata` en `?RxParquetData` voor aanvullende informatie over het gebruik van deze nieuwe functies.  


## <a name="install-additional-r-packages-on-the-edge-node"></a>Extra R-pakketten op het Edge-knooppunt installeren

Als u extra R-pakketten wilt installeren op het Edge-knooppunt, kunt u `install.packages()` rechtstreeks vanuit de R-console gebruiken wanneer u via SSH bent verbonden met het Edge-knooppunt. Als u echter R-pakketten wilt installeren op de worker-knooppunten van het cluster, moet u een scriptactie gebruiken.

Scriptacties zijn Bash-scripts die worden gebruikt om configuratiewijzigingen aan te brengen in het HDInsight-cluster of om extra software te installeren, zoals extra R-pakketten. Gebruik de volgende stappen om extra pakketten te installeren met behulp van een scriptactie:

> [!IMPORTANT]
> U kunt scriptacties alleen gebruiken om extra R-pakketten te installeren nadat het cluster is gemaakt. Gebruik deze procedure niet tijdens het maken van het cluster, omdat voor een script is vereist dat R Server volledig is geïnstalleerd en geconfigureerd.
>
>

1. Selecteer in [Azure Portal](https://portal.azure.com) het R Server op HDInsight-cluster.

2. Selecteer op de blade **Instellingen** de optie **Scriptacties** en vervolgens **Nieuwe verzenden** om een nieuwe scriptactie te verzenden.

   ![Afbeelding van de blade Scriptacties](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. Geef op de blade **Scriptactie verzenden** de volgende informatie op:

   * **Naam**: een beschrijvende naam om dit script te identificeren

   * **Bash-script-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Head**: dit item moet zijn **uitgeschakeld**

   * **Worker**: dit item moet zijn **ingeschakeld**

   * **Edge nodes**: dit item moet zijn **uitgeschakeld**.

   * **Zookeeper**: dit item moet zijn **uitgeschakeld**

   * **Parameters**: de R-pakketten die moeten worden geïnstalleerd. Bijvoorbeeld: `bitops stringr arules`

   * **Deze scriptactie opnieuw laten uitvoeren...** : dit item moet zijn **ingeschakeld**  

   > [!NOTE]
   > 1. Standaard worden alle R-pakketten geïnstalleerd vanuit een momentopname van de Microsoft MRAN-opslagplaats, consistent met de R Server-versie die is geïnstalleerd. Als u nieuwere versies van pakketten wilt installeren, is er kans op incompatibiliteit. Dit soort installatie is echter mogelijk door `useCRAN` op te geven als het eerste element van de pakketlijst, bijvoorbeeld `useCRAN bitops, stringr, arules`.  
   > 2. Voor sommige R-pakketten zijn aanvullende Linux-systeembibliotheken vereist. Voor het gemak hebben we de afhankelijkheden die nodig zijn voor de 100 meest populaire R-pakketten vooraf geïnstalleerd. Als voor de R-pakketten die u wilt installeren, meer bibliotheken zijn vereist, downloadt u het standaardscript dat hier wordt gebruikt, en voegt u stappen toe om de systeembibliotheken te installeren. Vervolgens moet u het gewijzigde script uploaden naar een openbare blobcontainer in Azure-opslag en het gewijzigde script gebruiken om de pakketten te installeren.
   >    Zie [Ontwikkeling van scriptacties](hdinsight-hadoop-script-actions-linux.md) voor meer informatie over het ontwikkelen van scriptacties.  
   >
   >

   ![Een scriptactie toevoegen](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. Selecteer **Maken** om het script uit te voeren. Nadat het script is voltooid, zijn de R-pakketten beschikbaar op alle werkknooppunten.


## <a name="using-microsoft-r-server-operationalization"></a>Microsoft R Server-uitoefening gebruiken

Wanneer de gegevensmodellering is voltooid, kunt u het model uitvoeren om voorspellingen te maken. Voer de stappen hieronder uit om Microsoft R Server-uitoefening te configureren:

Ten eerste, SSH op het Edge-knooppunt. Bijvoorbeeld: 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Nadat u SSH hebt gebruikt, wijzigt u de map voor de relevante versie en sudo in de dotnet-dll: 

- Voor Microsoft R Server 9.1:

    cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

- Voor Microsoft R Server 9.0:

    cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

Ga als volgt te werk om Microsoft R Server-uitoefening te configureren met een alles-in-één configuratie:

1. 'R-Server voor uitoefening configureren' selecteren
2. Selecteer A. Alles-in-één (web- + rekenknooppunten)
3. Voer een wachtwoord in voor de gebruiker **beheerder**

![De optie Alles-in-één](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Als optionele stap kunt u Diagnostische controles uitvoeren door als volgt een diagnostische test uit te voeren:

1. Selecteer 6. Diagnostische tests uitvoeren
2. Selecteer A. Configuratie testen
3. Voer als gebruikersnaam Beheerder in en geef het wachtwoord uit de voorgaande configuratiestap op
4. Algehele status bevestigen = geslaagd
5. Sluit het beheerprogramma
6. Sluit SSH

![De optie Diagnose voor](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>**Lange vertragingen bij het gebruiken van webservice op Spark**
>
>Als er lange vertragingen optreden bij het gebruiken van een webservice die is gemaakt met mrsdeploy-functies in een Spark-compute-context, moet u mogelijk een aantal ontbrekende mappen toevoegen. De Spark-toepassing is van een gebruiker met de naam *rserve2* wanneer de toepassing wordt aangeroepen vanuit een webservice met behulp van mrsdeploy-functies. Dit probleem omzeilen:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


In dit stadium is de configuratie voor uitoefening voltooid. U kunt nu het pakket mrsdeploy op de RClient gebruiken om verbinding te maken met het Uitoefening op Edge-knooppunt en de bijbehorende functies gaan gebruiken, zoals [externe uitvoering](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) en [webservices](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). Afhankelijk van of het cluster is ingesteld in een virtueel netwerk of niet, moet u mogelijk forward tunneling via SSH-aanmelding instellen voor de poort. In de volgende secties wordt uitgelegd hoe u deze tunnel instelt.

### <a name="rserver-cluster-on-virtual-network"></a>RServer-cluster in een virtueel netwerk

Zorg ervoor dat u verkeer via poort 12800 naar het Edge-knooppunt toestaat. Op deze manier kunt u het Edge-knooppunt gebruiken om verbinding te maken met de functie Uitoefening.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Als de `remoteLogin()` geen verbinding kan maken met het Edge-knooppunt maar als u wel verbinding hebt via SSH, moet u controleren of de regel op basis waarvan verkeer via poort 12800 is toegestaan, juist is ingesteld of niet. Als dit probleem zich blijft voordoen, kunt u een tijdelijke oplossing gebruiken door forward tunneling via SSH in te stellen voor de poort. Raadpleeg de volgende sectie voor meer informatie.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>RServer-cluster is niet ingesteld in het virtuele netwerk

Als het cluster niet is ingesteld in het virtuele netwerk vnet of als u problemen ondervindt met de connectiviteit via dit netwerk, kunt u forward tunneling via SSH instellen voor de poort:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

U kunt dit ook instellen op Putty.

![Putty SSH-verbinding](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Zodra de SSH-sessie actief is, wordt het verkeer bij poort 12800 op uw computer via de SSH-sessie doorgestuurd naar poort 12800 van het Edge-knooppunt. Zorg ervoor dat u `127.0.0.1:12800` gebruikt in de `remoteLogin()`-methode. Hierdoor wordt u aangemeld bij de uitoefening van het Edge-knooppunt via doorsturen naar de poort.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Hoe werkt het schalen van rekenknooppunten in Microsoft R Server-uitoefening op worker-knooppunten in HDInsight?

### <a name="decommission-the-worker-nodes"></a>De worker-knooppunten uit bedrijf nemen

Microsoft R Server wordt momenteel niet beheerd via Yarn. Als de werkknooppunten niet uit bedrijf worden genomen, werkt de resourcemanager Yarn niet zoals verwacht, omdat in dit geval niet wordt gedetecteerd welke resources de server bevat. Om deze sitatie te voorkomen raden we u aan de werkknooppunten uit bedrijf te nemen voordat u de rekenknooppunten uitschaalt.

Stappen voor het uit bedrijf nemen van worker-knooppunten:

* Meld u bij de Ambari-console van het HDI-cluster en klik op het tabblad Hosts
* Selecteer de worker-knooppunten (die uit bedrijf moeten worden genomen), klik op Acties > Geselecteerd hosts > Hosts, en klik op Onderhoudsmodus inschakelen. In de volgende afbeelding zijn bijvoorbeeld wk3 en wk4 geselecteerd om uit bedrijf te worden genomen.  

   ![worker-knooppunten uit bedrijf nemen](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* Selecteer **Acties** > **Geselecteerde hosts** > **Datanodes** > klik op **Uit bedrijf nemen**
* Selecteer **Acties** > **Geselecteerde hosts** > **NodeManagers** > klik op **Uit bedrijf nemen**
* Selecteer **Acties** > **Geselecteerde hosts** > **Datanodes** > klik op **Stoppen**
* Selecteer **Acties** > **Geselecteerde hosts** > **NodeManagers** > klik op **Stoppen**
* Selecteer **Acties** > **Geselecteerde hosts** > **Hosts** > klik op **Alle onderdelen stoppen**
* Hef de selectie van de werkknooppunten op en selecteer de hoofdknooppunten
* Selecteer **Acties** > **Geselecteerde hosts** > **Hosts** > **Alle onderdelen opnieuw starten**

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Rekenknooppunten configureren op elk uit bedrijf genomen worker-knooppunt

1. SSH op elk uit bedrijf genomen werkknooppunt.
2. Voer het beheerprogramma uit met behulp van `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
3. Voer '1' in om de optie 'R-Server voor uitoefening configureren' te selecteren.
4. Voer C in om optie C. Rekenknooppunt te selecteren. Hiermee configureert u het rekenknooppunt op het werkknooppunt.
5. Sluit het beheerprogramma.

### <a name="add-compute-nodes-details-on-web-node"></a>Details van rekenknooppunten toevoegen op het webknooppunt

Zodra alle uit bedrijf genomen werkknooppunten zijn geconfigureerd om het rekenknooppunt uit te voeren, keert u terug naar het Edge-knooppunt en voegt u de IP-adressen van de uit bedrijf genomen werkknooppunten toe in de configuratie van het Microsoft R Server-webknooppunt:

* SSH op het Edge-knooppunt.
* Voer `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` uit.
* Ga naar de sectie URI's en voeg het IP-adres en de poortgegevens van het worker-knooppunt toe.

    ![opdrachtregel worker-knooppunten uit bedrijf nemen](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.


## <a name="next-steps"></a>Volgende stappen

Nu zou u moeten weten hoe u een nieuw HDInsight-cluster maakt met de R Server en de basisbeginselen van het gebruik van de R-console van een SSH-sessie. In de volgende onderwerpen worden andere manieren beschreven om R Server op HDInsight te beheren en ermee te werken:

* [RStudio Server toevoegen aan HDInsight (indien niet geïnstalleerd tijdens het maken van het cluster)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Opties voor compute-context voor R Server op HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Opties voor Azure-opslag voor R Server op HDInsight](hdinsight-hadoop-r-server-storage.md)
