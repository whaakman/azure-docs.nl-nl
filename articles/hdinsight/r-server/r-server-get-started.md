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
ms.openlocfilehash: e688068efb41cdccbeb23de3c8ad7a09021e5b3f
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="get-started-with-r-server-on-hdinsight"></a>Aan de slag met R Server in HDInsight

Azure HDInsight bevat een R Server-optie die kan worden geïntegreerd in uw HDInsight-cluster. Met deze optie kunnen R-scripts gebruikmaken van Spark en MapReduce om gedistribueerde berekeningen uit te voeren. In dit artikel leest u hoe u R Server op een HDInsight-cluster kunt maken. Vervolgens leert u hoe u een R-script uitvoert waarmee het gebruik van Spark voor gedistribueerde R-berekeningen wordt gedemonstreerd.


## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**: voordat u aan deze zelfstudie begint, moet u beschikken over een Azure-abonnement. Zie [Get Microsoft Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Gratis proefversie van Microsoft Azure downloaden) voor meer informatie.
* **Een SSH-client (Secure Shell)**: er wordt een SSH-client gebruikt om extern verbinding te maken met het HDInsight-cluster en om opdrachten rechtstreeks uit te voeren op het cluster. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.
* **SSH-sleutels (optioneel)**: u kunt het SSH-account dat wordt gebruikt om verbinding te maken met het cluster, beveiligen met behulp van een wachtwoord of een openbare sleutel. Het is eenvoudiger om een wachtwoord te gebruiken. Bovendien kunt u dan aan de slag gaan zonder eerst een openbaar/persoonlijk sleutelpaar te hoeven maken. Het gebruik van een sleutel is echter veiliger.

  > [!NOTE]
  > Bij de stappen in dit artikel wordt ervan uitgegaan dat u een wachtwoord gebruikt.


## <a name="automate-cluster-creation"></a>Automatisch een cluster maken

U kunt het maken van HDInsight R Server-exemplaren automatiseren met Azure Resource Manager-sjablonen, de SDK of PowerShell.

* Zie [Deploy an R-server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Een HDInsight-cluster op basis van R Server implementeren) voor het maken van een R Server-exemplaar met een Azure Resource Manager-sjabloon.
* Zie [Create Linux-based clusters in HDInsight using the .NET SDK](../hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) (In HDInsight op Linux gebaseerde clusters maken met de .NET-SDK) als u een R Server-exemplaar wilt maken met behulp van de .NET-SDK.
* Zie [Linux-clusters in HDInsight maken met Azure PowerShell](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md) als u R Server wil implementeren met behulp van PowerShell.


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-a-cluster-by-using-the-azure-portal"></a>Een cluster maken met Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Nieuw** > **Intelligence en analyse** > **HDInsight**.

    ![Afbeelding van het maken van een nieuw cluster](./media/r-server-get-started/newcluster.png)

3. Voer onder **Snel maken** in het vak **Clusternaam** een naam in voor het cluster. Als u meerdere Azure-abonnementen hebt, gebruikt u het vak **Abonnement** om het abonnement te selecteren dat u wilt gebruiken.

    ![Clusternaam en geselecteerde abonnementen](./media/r-server-get-started/clustername.png)

4. Selecteer **Clustertype** om het deelvenster **Clusterconfiguratie** te openen. Selecteer in het deelvenster **Clusterconfiguratie** de volgende opties:

    * **Clustertype**: selecteer **R Server**.
    * **Versie**: selecteer de versie van R Server die u op het cluster wilt installeren. De momenteel beschikbare versie is **R Server 9.1 (HDI 3.6)**. Releaseopmerkingen voor de beschikbare versies van R Server vindt u op [MSDN](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **R Studio Community-editie voor R Server**: deze IDE op basis van een browser wordt standaard geïnstalleerd op het edge-knooppunt. Als u de IDE niet wilt installeren, schakelt u het selectievakje uit. Als u ervoor kiest deze wel te laten installeren, is de URL naar de aanmeldingspagina voor RStudio Server aanwezig in een portaltoepassingsvenster voor het cluster nadat het is gemaakt.
    * Laat bij de andere opties de standaardwaarden staan en gebruik de knop **Selecteren** om clustertype op te slaan.

        ![Schermopname van het deelvenster Clustertype](./media/r-server-get-started/clustertypeconfig.png)

5. In het deelvenster **Basisinformatie** voert u in de vakken **Gebruikersnaam voor clusteraanmeldgegevens** en **Wachtwoord voor clusteraanmeldgegevens** een gebruikersnaam respectievelijk een wachtwoord voor het cluster in.

6. Geef in het vak **SSH-gebruikersnaam (Secure Shell)** de SSH-gebruikersnaam op. SSH wordt gebruikt om extern verbinding te maken met het cluster met behulp van een SSH-client. U kunt de SSH-gebruiker in dit vak opgeven of nadat het cluster is gemaakt (op het tabblad **Configuratie** voor het cluster).
   
   > [!NOTE] 
   > R Server is zo geconfigureerd dat de SSH-gebruikersnaam ´remoteuser´ wordt verwacht. Als u een andere gebruikersnaam gebruikt, moet u een extra stap uitvoeren nadat het cluster is gemaakt.

7. Als u **WACHTWOORD** wilt gebruiken als verificatietype, laat u het vakje **Hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** ingeschakeld, tenzij u liever een openbare sleutel gebruikt. U hebt een openbaar/persoonlijk sleutelpaar nodig voor toegang tot R Server op het cluster via een externe client, bijvoorbeeld R Tools voor Visual Studio, RStudio of een andere bureaublad-IDE. Als u de RStudio Server Community-editie installeert, moet u een SSH-wachtwoord opgeven.     

   Als u een openbaar/persoonlijk sleutelpaar wilt maken en gebruiken, schakelt u het selectievakje **Hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** uit. Selecteer vervolgens **OPENBARE SLEUTEL** en ga als volgt door. Bij deze instructies wordt ervan uitgegaan dat u Cygwin SSH-keygen of iets gelijkwaardigs hebt geïnstalleerd.

   a. Een openbaar/persoonlijk sleutelpaar genereren via de opdrachtprompt op uw laptop:

        ssh-keygen -t rsa -b 2048

   b. Volg de prompt om een sleutelbestand een naam te geven en voer vervolgens een wachtwoordzin in voor extra beveiliging. Het scherm moet er ongeveer als volgt uitzien:

      ![SSH-opdrachtregel in Windows](./media/r-server-get-started/sshcmdline.png)

      Met deze opdracht maakt u een persoonlijk sleutelbestand en een openbaar sleutelbestand onder de naam <persoonlijke-sleutel-bestandsnaam>.pub. In dit voorbeeld worden de bestanden furiosa en furiosa.pub gebruikt:

      ![Voorbeeld van resultaat van dir-opdracht](./media/r-server-get-started/dir.png)

   c. Geef het openbare-sleutelbestand (&#42;.pub) op bij het toewijzen van HDI-clusterreferenties. Bevestig de resourcegroep en -regio en selecteer **Volgende**.

      ![Deelvenster Referenties](./media/r-server-get-started/publickeyfile.png)  

   d. Machtigingen voor het persoonlijke-sleutelbestand op uw laptop wijzigen:

        chmod 600 <private-key-filename>

   e. Het persoonlijke-sleutelbestand met SSH gebruiken voor externe aanmelding:

        ssh –i <private-key-filename> remoteuser@<hostname public ip>

      Of gebruik het persoonlijke-sleutelbestand als onderdeel van de definitie van uw Hadoop Spark Compute-context voor R Server op de client. Zie [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started) (Een Compute-context voor Spark maken) voor meer informatie.

8. Met Snel maken gaat u over naar het deelvenster **Storage**. Daar selecteert u de instellingen voor het opslagaccount dat moet worden gebruikt voor de primaire locatie van het HDFS-bestandssysteem dat door het cluster wordt gebruikt. Selecteer een nieuw of bestaand Azure-opslagaccount of selecteer een bestaand Azure Data Lake Store-account.

    - Als u een Azure-opslagaccount selecteert, kunt u een bestaand account kiezen door **Een opslagaccount selecteren** te selecteren en vervolgens het bijbehorende account. Maak een nieuw account via de koppeling **Nieuw** in de sectie **Een opslagaccount selecteren**.

      > [!NOTE]
      > Als u **Nieuw** selecteert, moet u een naam invoeren voor het nieuwe opslagaccount. Er verschijnt een groen vinkje als de naam wordt geaccepteerd.

      **Standaardcontainer** krijgt dezelfde naam als het cluster. Laat deze standaardwaarde staan.

      Als u een nieuw opslagaccount hebt geselecteerd, gebruikt u **Locatie** in de prompt om er een regio voor te selecteren.  

         ![Deelvenster Gegevensbron](./media/r-server-get-started/datastore.png)  

      > [!IMPORTANT]
      > Als u de locatie van de standaardgegevensbron selecteert, wordt ook de locatie van het HDInsight-cluster ingesteld. Het cluster en de standaardgegevensbron moeten zich in dezelfde regio bevinden.

    - Als u een bestaand Data Lake Store-account wilt gebruiken, selecteert u het gewenste account. Voeg vervolgens de clusteridentiteit *ADD* aan het cluster toe voor toegang tot de store. Raadpleeg [Create HDInsight cluster with Data Lake Store by using the Azure portal](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) (HDInsight-cluster met Data Lake Store maken met behulp van Azure Portal) voor meer informatie over dit proces.

    Gebruik de knop **Selecteren** om de configuratie van de gegevensbron op te slaan.


9. Het deelvenster **Samenvatting** verschijnt, zodat u alle instellingen kunt valideren. Hier kunt u de clustergrootte aanpassen om het aantal servers in het cluster te wijzigen. U kunt er ook scriptacties opgeven die u wilt uitvoeren. Tenzij u zeker weet dat u een groter cluster nodig hebt, laat u het aantal worker-knooppunten ingesteld op het standaardaantal van **4**. In het deelvenster ziet u tevens de geraamde kosten voor het cluster.

    ![Clusteroverzicht](./media/r-server-get-started/clustersummary.png)

   > [!NOTE]
   > Indien nodig, kunt u de grootte van het cluster later wijzigen via de portal (**Cluster** > **Instellingen** > **Cluster schalen**) om het aantal worker-knooppunten te verlagen of te verhogen. De grootte aanpassen kan nuttig zijn als u het cluster minder actief wilt maken wanneer het niet wordt gebruikt of om capaciteit toe te voegen wanneer dit nodig is voor grotere taken.
   >
   >

   Factoren waarmee u rekening moet houden wanneer u de grootte van het cluster, de gegevensknooppunten en het edge-knooppunt wijzigt zijn:  

   * De prestaties van gedistribueerde R Server-analyses op Spark zijn gekoppeld aan het aantal worker-knooppunten wanneer er sprake is van grote hoeveelheden gegevens.  

   * De prestaties van R Server-analyses zijn lineair in de grootte van de gegevens die worden geanalyseerd. Bijvoorbeeld:  

     * Bij kleine tot gemiddelde hoeveelheden gegevens zijn de prestaties het beste wanneer de gegevens worden geanalyseerd in een lokale Compute-context op het edge-knooppunt. Zie [Opties voor Compute-context voor R Server op HDInsight](r-server-compute-contexts.md) voor meer informatie over de scenario's waarin de lokale Compute-context en de Spark Compute-context het beste werken.<br>
     * Als u zich aanmeldt bij het edge-knooppunt en uw R-script vervolgens uitvoert, worden alle functies behalve de ScaleR rx-functie *lokaal* uitgevoerd op het edge-knooppunt. Dus moeten het geheugen en het aantal kernen op het edge-knooppunt dienovereenkomstig worden aangepast. Hetzelfde geldt dat als u R Server op HDI gebruikt als een externe compute-context vanaf uw laptop.

   Gebruik de knop **Selecteren** om de prijsconfiguratie voor knooppunten op te slaan.

   ![Deelvenster voor prijscategorieën van knooppunten](./media/r-server-get-started/pricingtier.png)

   Er is ook een koppeling **Sjabloon en parameters downloaden**. Selecteer deze koppeling om scripts weer te geven die kunnen worden gebruikt om automatisch een cluster te maken met de geselecteerde configuratie. Deze scripts zijn ook toegankelijk in de Azure Portal-vermelding voor uw cluster nadat het is gemaakt.

   > [!NOTE]
   > Meestal duurt het genereren van het cluster ongeveer 20 minuten. Gebruik de tegel op het Startboard of de vermelding **Meldingen** aan de linkerkant van de pagina om dit proces te controleren.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>Verbinding maken met RStudio Server

Als u de RStudio Server Community-editie wilt opnemen in de installatie, hebt u op twee manieren toegang tot de aanmeldingspagina van RStudio:

- Ga naar de volgende URL (waarbij *CLUSTERNAAM* de naam is van het cluster dat u hebt gemaakt):

    https://*CLUSTERNAAM*.azurehdinsight.net/rstudio/

- Of open de vermelding voor het cluster in Azure Portal. Selecteer de snelkoppeling naar **R Server-dashboards** en selecteer vervolgens **R Studio-dashboard**:

  ![Het R Studio-dashboard openen](./media/r-server-get-started/rstudiodashboard1.png)

  ![Het R Studio-dashboard openen](./media/r-server-get-started/rstudiodashboard2.png)

> [!IMPORTANT]
> Ongeacht welke methode u kiest, de eerste keer dat u zich aanmeldt moet u zich twee keer verifiëren. Bij de eerste verificatie geeft u de gebruikers-id en het wachtwoord voor de beheerder op voor het cluster. Bij de tweede prompt geeft u de gebruikers-id en het wachtwoord voor SSH op. Voor opvolgende aanmeldingen zijn alleen de gebruikers-id en het wachtwoord voor SSH vereist.

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a>Verbinding maken met het R Server-edge-knooppunt

Maak verbinding met het R Server-edge-knooppunt van het HDInsight-cluster door gebruik te maken van deze opdracht:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> U vindt het `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`-adres in Azure Portal. Selecteer uw cluster en vervolgens **Alle instellingen** > **Apps** > **RServer**. U ziet dan de SSH-eindpuntgegevens van het edge-knooppunt.
>
> ![Afbeelding van het SSH-eindpunt voor het edge-knooppunt](./media/r-server-get-started/sshendpoint.png)
>
>

Als u een wachtwoord hebt gebruikt om uw SSH gebruikersaccount te beveiligen, wordt u gevraagd het wachtwoord in te voeren. Als u een openbare sleutel hebt gebruikt, moet u mogelijk de `-i`-parameter gebruiken om de overeenkomende persoonlijke sleutel op te geven. Bijvoorbeeld:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Zie voor meer informatie [Verbinding maken met HDInsight (Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

Nadat er verbinding is gemaakt, wordt er een prompt weergegeven die er ongeveer als volgt uitziet:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Meerdere gelijktijdige gebruikers inschakelen

U kunt meerdere gelijktijdige gebruikers inschakelen door meer gebruikers voor het Edge-knooppunt toe te voegen waarop de RStudio-community-versie wordt uitgevoerd.

Wanneer u een HDInsight-cluster maakt, moet u twee gebruikers opgeven: een HTTP-gebruiker en een SSH-gebruiker.

![Referenties invoeren voor het cluster en de SSH-gebruiker](./media/r-server-get-started/concurrent-users-1.png)

- **Gebruikersnaam voor aanmelding cluster**: een HTTP-gebruiker voor verificatie via de HDInsight-gateway die wordt gebruikt voor het beveiligen van de HDInsight-clusters die u hebt gemaakt. Deze HTTP-gebruiker wordt gebruikt voor toegang tot de Ambari-gebruikersinterface, de YARN-gebruikersinterface en andere gebruikersinterfaceonderdelen.
- **Secure Shell (SSH)-gebruikersnaam**: een SSH-gebruiker voor toegang tot het cluster via Secure Shell. Deze gebruiker is een gebruiker in het Linux-systeem voor alle hoofdknooppunten, werkknooppunten en Edge-knooppunten. U kunt SSH gebruiken voor toegang tot alle knooppunten in een extern cluster.

De R Studio Server-communityversie die wordt gebruikt in het clustertype Microsoft R Server op HDInsight accepteert alleen de Linux-gebruikersnaam en -wachtwoord als aanmeldingsmechanisme. Doorgegeven tokens worden niet ondersteund. Dus als u een nieuw cluster hebt gemaakt en u wilt RStudio gebruiken om dat te openen, moet u zich twee keer aanmelden.

1. Meld u aan met de referenties van de HTTP-gebruiker via de gateway van HDInsight: 

    ![Referenties van de HTTP-gebruiker invoeren](./media/r-server-get-started/concurrent-users-2a.png)

2. Gebruik de referenties van de SSH-gebruiker om u aan te melden bij RStudio:
  
    ![Referenties van de SSH-gebruiker invoeren](./media/r-server-get-started/concurrent-users-2b.png)

Op dit moment kunt u slechts één SSH gebruikersaccount maken bij het inrichten van een HDInsight-cluster. Om meerdere gebruikers toegang tot Microsoft R Server op HDInsight-clusters te geven, dient u extra gebruikers in het Linux-systeem te maken.

Omdat de RStudio Server-communityversie op het edge-knooppunt van het cluster wordt uitgevoerd, zijn er drie stappen:

1. Gebruik de SSH-gebruiker om u aan te melden bij het edge-knooppunt.
2. Voeg meer Linux-gebruikers toe aan het edge-knooppunt.
3. Gebruik de RStudio-communityversie voor de gemaakte gebruiker.

### <a name="use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Gebruik de SSH-gebruiker om u aan te melden bij het Edge-knooppunt

Download een SSH-hulpprogramma (zoals PuTTY) en gebruik de bestaande SSH-gebruiker om u aan te melden. Volg de instructies in [Verbinding maken met HDInsight (Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) voor toegang tot het edge-knooppunt. Het adres van het edge-knooppunt voor R Server op HDInsight-cluster is: **clusternaam-ed-ssh.azurehdinsight.net**


### <a name="add-more-linux-users-to-the-edge-node"></a>Meer Linux-gebruikers toevoegen aan het edge-knooppunt

Gebruik de volgende opdrachten om een gebruiker aan het edge-knooppunt toe te voegen:

    sudo useradd yournewusername -m
    sudo passwd yourusername

De volgende items moeten worden geretourneerd: 

![Uitvoer van sudo-opdrachten](./media/r-server-get-started/concurrent-users-3.png)

Als u wordt gevraagd om het huidige Kerberos-wachtwoord, drukt u op de Enter-toets om de vraag te negeren. De `-m`-optie in de `useradd`-opdracht geeft aan dat er een basismap voor de gebruiker wordt gemaakt. Deze map is vereist voor de RStudio-communityversie.


### <a name="use-the-rstudio-community-version-with-the-created-user"></a>De RStudio-communityversie gebruiken voor de gemaakte gebruiker

Gebruik de gemaakte gebruiker om u aan te melden bij RStudio:

![Aanmeldingspagina van RStudio](./media/r-server-get-started/concurrent-users-4.png)

U ziet dat RStudio aangeeft dat u van de nieuwe gebruiker gebruikmaakt (hier bijvoorbeeld **sshuser6**) om u aan te melden bij het cluster: 

![Locatie van de nieuwe gebruiker op de RStudio-opdrachtbalk](./media/r-server-get-started/concurrent-users-5.png)

U kunt u ook tegelijkertijd vanuit een ander browservenster aanmelden met de oorspronkelijke referenties (dit is standaard **sshuser**).

U kunt een taak met ScaleR-functies verzenden. Hier is een voorbeeld van de opdrachten voor het uitvoeren van een taak:

    # Set the HDFS (Azure Blob storage) location of example data
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storing data temporarily
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

    # Set the directory in bigDataDirRoot to load the data
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (Blob storage) file system
    hdfsFS <- RxHdfsFileSystem()

    # Create an info list for the airline data
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in the local system
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

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


U ziet dat de verzonden taken in de gebruikersinterface van YARN onder andere gebruikersnamen worden weergegeven:

![Lijst met gebruikers in de gebruikersinterface van YARN](./media/r-server-get-started/concurrent-users-6.png)

U ziet dat de nieuw toegevoegde gebruikers geen basisbevoegdheden in het Linux-systeem hebben. Ze hebben echter wel dezelfde toegang tot alle bestanden in het externe HDFS-bestandssysteem en Blob Storage.


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

3. U kunt de R-code invoeren vanuit de `>`-prompt. R-server bevat pakketten waarmee u eenvoudig kunt werken met Hadoop en gedistribueerde berekeningen kunt uitvoeren. Gebruik bijvoorbeeld de volgende opdracht om de hoofdmap te bekijken van het standaardbestandssysteem voor het HDInsight-cluster:

        rxHadoopListFiles("/")

4. U kunt ook wijze van adresseren van Blob Storage gebruiken:

        rxHadoopListFiles("wasb:///")


## <a name="use-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>R Server op HDI gebruiken vanaf een extern exemplaar van Microsoft R Server of Microsoft R Client

Het is mogelijk toegang in te stellen voor de HDI Hadoop Spark Compute-context van een extern exemplaar van Microsoft R Server of Microsoft R Client dat wordt uitgevoerd op een desktop of laptop. Zie de sectie 'Using Microsoft R Server as a Hadoop Client' (Microsoft R Server als Hadoop Client gebruiken) in [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md) (een Compute-context voor Spark maken) voor meer informatie. Hiervoor geeft u de volgende opties op wanneer u de RxSpark Compute-context definieert op uw laptop: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches en sshProfileScript. Hier volgt een voorbeeld:


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

U kunt een Compute-context gebruiken om te bepalen of een berekening lokaal op het edge-knooppunt wordt uitgevoerd of wordt gedistribueerd naar de verschillende knooppunten in het HDInsight-cluster.

1. Gebruik vanuit RStudio Server of vanuit de R-console (tijdens een SSH-sessie) de volgende code om voorbeeldgegevens te laden in de standaardopslag voor HDInsight:

        # Set the HDFS (Blob storage) location of example data
        bigDataDirRoot <- "/example/data"

        # Create a local folder for storing data temporarily
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

        # Set the directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

        # Make the directory
        rxHadoopMakeDir(inputDir)

        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Maak enkele gegevens en definieer twee gegevensbronnen zodat u met de gegevens kunnen werken:

        # Define the HDFS (Blob storage) file system
        hdfsFS <- RxHdfsFileSystem()

        # Create an info list for the airline data
        airlineColInfo <- list(
             DAY_OF_WEEK = list(type = "factor"),
             ORIGIN = list(type = "factor"),
             DEST = list(type = "factor"),
             DEP_TIME = list(type = "integer"),
             ARR_DEL15 = list(type = "logical"))

        # Get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in HDFS
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

        # Define the text data source in the local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # Formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Voer Logistic Regression voor de gegevens uit met behulp van de lokale Compute-context:

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

4. Voer dezelfde Logistic Regression uit met behulp van de Spark-context. De Spark-context distribueert de verwerking over alle werkknooppunten in het HDInsight-cluster.

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
   > U kunt ook MapReduce gebruiken om de berekening te distribueren naar verschillende clusterknooppunten. Zie [Opties voor compute-context voor R Server op HDInsight](r-server-compute-contexts.md) voor meer informatie over compute-context.


## <a name="distribute-r-code-to-multiple-nodes"></a>R-code distribueren naar meerdere knooppunten

Met R Server kunt u gemakkelijk bestaande R-code uitvoeren op verschillende knooppunten in het cluster door gebruik te maken van `rxExec`. Deze functie is handig bij het uitvoeren van een parameteropschoning of van simulaties. Hier volgt een voorbeeldcode van het gebruik van `rxExec`:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Als u nog steeds de Spark- of MapReduce-context gebruikt, wordt door deze opdracht de waarde `nodename` geretourneerd voor het worker-knooppunt waarop de code `(Sys.info()["nodename"])` wordt uitgevoerd. Bijvoorbeeld: bij een cluster van vier knooppunten verwacht u uitvoer die er ongeveer als volgt uitziet:

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


## <a name="access-data-in-hive-and-parquet"></a>Gegevens openen in Hive en Parquet

Dankzij een functie die beschikbaar is in R Server 9.1, hebt u nu direct toegang tot de gegevens in Hive en Parquet, zodat deze kunnen worden gebruikt voor ScaleR-functies in de Spark-compute-context. Deze mogelijkheden zijn beschikbaar via nieuwe ScaleR-gegevensbronfuncties, RxHiveData en RxParquetData genaamd. Deze functies werken met behulp van Spark SQL voor het rechtstreeks laden van gegevens in een Spark DataFrame voor analyse door ScaleR.  

De volgende code toont een paar voorbeelden van het gebruik van de nieuwe functies:

    #Create a Spark compute context
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model
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

    #Check on Spark data objects, clean up, and close the Spark session
    lsObj <- rxSparkListData() #Two data objects are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() #It should show an empty list
    rxSparkDisconnect(myHadoopCluster)


Zie de online-Help in R Server voor meer informatie met behulp van de opdrachten `?RxHivedata` en `?RxParquetData`.  


## <a name="install-additional-r-packages-on-the-edge-node"></a>Extra R-pakketten op het edge-knooppunt installeren

Als u extra R-pakketten wilt installeren op het edge-knooppunt, kunt u `install.packages()` rechtstreeks vanuit de R-console gebruiken wanneer u via SSH bent verbonden met het edge-knooppunt. Als u echter R-pakketten wilt installeren op de worker-knooppunten van het cluster, moet u een scriptactie gebruiken.

Scriptacties zijn Bash-scripts die worden gebruikt om configuratiewijzigingen aan te brengen in het HDInsight-cluster of om extra software te installeren, zoals extra R-pakketten. Gebruik de volgende stappen om extra pakketten te installeren met behulp van een scriptactie.

> [!IMPORTANT]
> Pas nadat het cluster is gemaakt, kunt u scriptacties gebruiken om extra R-pakketten te installeren. Gebruik deze procedure niet tijdens het maken van het cluster, omdat voor een script is vereist dat R Server volledig is geïnstalleerd en geconfigureerd.
>
>

1. Selecteer in [Azure Portal](https://portal.azure.com) het R Server op HDInsight-cluster.

2. Selecteer in het deelvenster **Instellingen** de opties **Scriptacties** > **Nieuwe verzenden**.

   ![Afbeelding van het deelvenster Scriptacties](./media/r-server-get-started/scriptaction.png)

3. Geef in het deelvenster **Scriptactie verzenden** de volgende informatie op:

   * **Naam**: een beschrijvende naam om dit script te identificeren.

   * **Bash-script-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Head**: dit item moet zijn uitgeschakeld.

   * **Worker**: dit item moet zijn uitgeschakeld.

   * **Zookeeper**: dit item moet zijn uitgeschakeld.

   * **Edge-knooppunten**: dit item moet zijn ingeschakeld.

   * **Parameters**: de R-pakketten die moeten worden geïnstalleerd, bijvoorbeeld `bitops stringr arules`.

   * **Dit script opnieuw laten uitvoeren**: dit item moet zijn ingeschakeld.  

   > [!NOTE]
   > Standaard worden alle R-pakketten geïnstalleerd vanuit een momentopname van de Microsoft R Application Network-opslagplaats, die consistent is met de geïnstalleerde R Server-versie. Als u nieuwere versies van pakketten wilt installeren, is er kans op incompatibiliteit. Dit soort installatie is echter mogelijk door `useCRAN` op te geven als het eerste element van de pakketlijst, bijvoorbeeld `useCRAN bitops, stringr, arules`.  
   > 
   > Voor sommige R-pakketten zijn aanvullende Linux-systeembibliotheken vereist. Voor het gemak hebben we de afhankelijkheden die nodig zijn voor de honderd meest populaire R-pakketten vooraf geïnstalleerd. Als voor de R-pakketten die u wilt installeren, echter meer bibliotheken zijn vereist, downloadt u het standaardscript dat hier wordt gebruikt, en voegt u stappen toe om de systeembibliotheken te installeren. Vervolgens moet u het gewijzigde script uploaden naar een openbare blobcontainer in Azure Storage en het gewijzigde script gebruiken om de pakketten te installeren.
   >
   > Zie [Ontwikkeling van scriptacties](../hdinsight-hadoop-script-actions-linux.md) voor meer informatie over het ontwikkelen van scriptacties.  
   >
   >

   ![Een scriptactie toevoegen](./media/r-server-get-started/submitscriptaction.png)

4. Selecteer **Maken** om het script uit te voeren. Nadat het script is voltooid, zijn de R-pakketten beschikbaar op alle worker-knooppunten.


## <a name="configure-microsoft-r-server-operationalization"></a>Microsoft R Server-uitoefening configureren

Wanneer de gegevensmodellering is voltooid, kunt u het model uitvoeren om voorspellingen te maken. Voer de stappen hieronder uit om Microsoft R Server-uitoefening te configureren:

1. Gebruik de opdracht `ssh` voor het edge-knooppunt, bijvoorbeeld: 

       ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

2. Wijzig de map voor de bijbehorende versie en gebruik de opdracht `sudo dotnet` voor het DLL-bestand. 

   Voor Microsoft R Server 9.1:

       cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
       sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

   Voor Microsoft R Server 9.0:

       cd /usr/lib64/microsoft-deployr/9.0.1
       sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Ga als volgt te werk om Microsoft R Server-uitoefening te configureren met een alles-in-één configuratie:

   a. Selecteer `Configure R Server for Operationalization`.

   b. Selecteer `A. One-box (web + compute nodes)`.

   c. Voer een wachtwoord in voor gebruiker `admin`.

   ![Alles-in-één uitoefening](./media/r-server-get-started/admin-util-one-box-.png)

4. Optioneel kunt u als volgt een diagnostische test uitvoeren:

   a. Selecteer `6. Run diagnostic tests`.

   b. Selecteer `A. Test configuration`.

   c. Voer `admin` in voor de gebruikersnaam en voer het wachtwoord van de vorige configuratiestap in.

   d. Bevestig `Overall Health = pass`.

   e. Sluit het beheerprogramma.

   f. Sluit SSH.

   ![Diagnostische test voor uitoefening](./media/r-server-get-started/admin-util-diagnostics.png)


>[!NOTE]
>Als er lange vertragingen optreden bij het gebruik van een webservice die is gemaakt met mrsdeploy-functies in een Spark Compute-context, moet u mogelijk een aantal ontbrekende mappen toevoegen. De Spark-toepassing is van een gebruiker met de naam *rserve2* wanneer de toepassing wordt aangeroepen vanuit een webservice via mrsdeploy-functies. Dit probleem omzeilen:

    #Create these required folders for user rserve2 in local and HDFS
    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    #Create a new Spark compute context 
    rxSparkConnect(reset = TRUE)


In dit stadium is de configuratie voor uitoefening voltooid. U kunt nu het mrsdeploy-pakket op R Client gebruiken om verbinding te maken met de uitoefening op het edge-knooppunt. Vervolgens kunt u de functies ervan gebruiken, bijvoorbeeld [externe uitvoering](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) en [webservices](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette). Afhankelijk van of het cluster is ingesteld in een virtueel netwerk of niet, moet u mogelijk forward tunneling via een SSH-aanmelding instellen voor de poort.

### <a name="r-server-cluster-on-a-virtual-network"></a>R Server-cluster in een virtueel netwerk

Zorg ervoor dat u verkeer via poort 12800 naar het edge-knooppunt toestaat. Op deze manier kunt u het edge-knooppunt gebruiken om verbinding te maken met de uitoefeningsfunctie.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Als `remoteLogin()` geen verbinding kan maken met het edge-knooppunt maar u hiervoor wel SSH kunt gebruiken, dient u te controleren of de regel voor het toestaan van verkeer op poort 12800 goed is ingesteld. Als dit probleem zich blijft voordoen, kunt u een tijdelijke oplossing gebruiken door forward tunneling via SSH in te stellen voor de poort. Raadpleeg de volgende sectie voor meer informatie.

### <a name="r-server-cluster-not-set-up-on-a-virtual-network"></a>R Server-cluster is niet ingesteld in een virtueel netwerk

Als het cluster niet is ingesteld op een virtueel netwerk of als u problemen hebt met connectiviteit via een virtueel netwerk, kunt u forward tunneling via SSH gebruiken:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

U kunt dit ook instellen op PuTTY:

![PuTTy SSH-verbinding](./media/r-server-get-started/putty.png)

Zodra de SSH-sessie actief is, wordt het verkeer bij poort 12800 op uw computer via de SSH-sessie doorgestuurd naar poort 12800 van het edge-knooppunt. Zorg ervoor dat u `127.0.0.1:12800` gebruikt voor de `remoteLogin()`-methode. Met deze methode wordt u aangemeld bij de uitoefening van het edge-knooppunt via doorsturen naar de poort.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Schalen van rekenknooppunten in Microsoft R Server-uitoefening op worker-knooppunten in HDInsight

### <a name="decommission-the-worker-nodes"></a>De worker-knooppunten uit bedrijf nemen

Microsoft R Server wordt momenteel niet beheerd via Yarn. Als de worker-knooppunten niet uit bedrijf worden genomen, werkt resourcemanager Yarn niet zoals verwacht, omdat in dit geval niet wordt gedetecteerd welke resources de server gebruikt. Om deze situatie te voorkomen, raden we u aan de worker-knooppunten uit bedrijf te nemen voordat u de rekenknooppunten uitschaalt.

Worker-knooppunten uit bedrijf nemen:

1. Meld u aan bij de Ambari-console van het HDI-cluster en selecteer het tabblad **Hosts**.
2. Selecteer worker-knooppunten die uit bedrijf moeten worden genomen en selecteer vervolgens **Acties** > **Geselecteerde hosts** > **Hosts** > **Onderhoudsmodus inschakelen**. In de volgende afbeelding zijn bijvoorbeeld wk3 en wk4 geselecteerd om uit bedrijf te worden genomen.  

   ![Schermafbeelding van de opdrachten voor het inschakelen van de onderhoudsmodus](./media/r-server-get-started/get-started-operationalization.png)  

3. Selecteer **Acties** > **Geselecteerde hosts** > **DataNodes** > **Uit bedrijf nemen**.
4. Selecteer **Acties** > **Geselecteerde hosts** > **NodeManagers** > **Uit bedrijf nemen**.
5. Selecteer **Acties** > **Geselecteerde hosts** > **DataNodes**  > **Stoppen**.
6. Selecteer **Acties** > **Geselecteerde hosts** > **NodeManagers**  > **Stoppen**.
7. Selecteer **Acties** > **Geselecteerde hosts** > **Hosts** > **Alle onderdelen stoppen**.
8. Hef de selectie van de worker-knooppunten op en selecteer de hoofdknooppunten.
9. Selecteer **Acties** > **Geselecteerde hosts** > **Hosts** > **Alle onderdelen opnieuw starten**.

### <a name="configure-compute-nodes-on-each-decommissioned-worker-node"></a>Rekenknooppunten configureren op elk uit bedrijf genomen worker-knooppunt

1. Gebruik SSH om verbinding te maken met elk uit bedrijf genomen worker-knooppunt.
2. Voer een beheerprogramma uit door gebruik te maken van `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll`.
3. Voer `1` in om optie `Configure R Server for Operationalization` te selecteren.
4. Voer `c` in om optie `C. Compute node` te selecteren. In deze stap configureert u het rekenknooppunt op het worker-knooppunt.
5. Sluit het beheerprogramma.

### <a name="add-compute-nodes-details-on-the-web-node"></a>Details van rekenknooppunten toevoegen op het webknooppunt

Zodra alle uit bedrijf genomen werkknooppunten zijn geconfigureerd om op het rekenknooppunt te worden uitgevoerd, gaat u terug naar het edge-knooppunt en voegt u de IP-adressen van de uit bedrijf genomen worker-knooppunten toe in de configuratie van het Microsoft R Server-webknooppunt:

1. Gebruik SSH om verbinding te maken met het edge-knooppunt.
2. Voer `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` uit.
3. Ga naar de sectie `URIs` en voeg het IP-adres en de poortgegevens van het worker-knooppunt toe.

    ![Opdrachtregel voor het edge-knooppunt](./media/r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](../hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen hebt met het maken van HDInsight-clusters.


## <a name="next-steps"></a>Volgende stappen

U hebt nu voldoende kennis om een HDInsight-cluster te maken waarin R Server is opgenomen. Ook hebt u voldoende kennis van de basisbeginselen voor het gebruik van de R-console vanuit een SSH-sessie. In de volgende onderwerpen worden andere manieren beschreven om R Server op HDInsight te beheren en ermee te werken:

* [Opties voor compute-context voor R Server op HDInsight](r-server-compute-contexts.md)
* [Opties voor Azure-opslag voor R Server op HDInsight](r-server-storage.md)
