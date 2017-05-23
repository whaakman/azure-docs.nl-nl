---
title: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio | Microsoft Docs
description: 'Informatie over het installeren van Data Lake Tools voor Visual Studio en het ontwikkelen en testen van U-SQL-scripts. '
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/06/2017
ms.author: edmaca, yanacai
ms.translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: c26ac89bd7ef494331ba309aacf87de03506ac4c
ms.contentlocale: nl-nl
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-develop-u-sql-scripts-using-data-lake-tools-for-visual-studio"></a>Zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Schrijf en test U-SQL-scripts met Data Lake-tools voor Visual Studio.

U-SQL is een uitermate schaalbare en uitbreidbare taal voor het voorbereiden, transformeren en analyseren van alle gegevens in de data lake en daarbuiten. Zie [Naslaginformatie over U-SQL](http://go.microsoft.com/fwlink/p/?LinkId=691348) voor meer informatie.

## <a name="prerequisites"></a>Vereisten
* **Visual Studio 2017 (onder gegevensopslag en verwerkingsworkload), Visual Studio 2015 update 3, Visual Studio 2013 update 4 of Visual Studio 2012. Enterprise- (Ultimate/Premium), Professional- en Community-edities worden ondersteund; de Express-editie wordt niet ondersteund.**
* **Microsoft Azure SDK voor .NET versie 2.7.1 of hoger**.  U kunt dit installeren met het [Webplatforminstallatieprogramma](http://www.microsoft.com/web/downloads/platform.aspx).
* **[Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs)**.

    Wanneer Data Lake Tools voor Visual Studio is geïnstalleerd, ziet u een knooppunt 'Data Lake Analytics' in Server Explorer, onder het 'Azure'-knooppunt (u opent Server Explorer met Ctrl + Alt + S).

* **Data Lake Analytics-account en voorbeeldgegevens** Het maken van Data Lake Analytics-accounts wordt niet ondersteund door de Data Lake-tools. Maak een account met behulp van Azure Portal, Azure PowerShell, .NET-SDK of Azure-CLI.
Een PowerShell-script voor het maken van een Data Lake Analytics-service en het uploaden van een brongegevensbestand vindt u in [Appx-A PowerShell-voorbeeld ter voorbereiding op de zelfstudie](data-lake-analytics-data-lake-tools-get-started.md#appx-a-powershell-sample-for-preparing-the-tutorial).

    Desgewenst kunt u de volgende twee secties doornemen in [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) om uw account te maken en gegevens handmatig te uploaden:

    1. [Een Azure Data Lake Analytics-account maken](data-lake-analytics-get-started-portal.md#create-data-lake-analytics-account).
    2. [SearchLog.tsv uploaden naar het Data Lake Storage-standaardaccount](data-lake-analytics-get-started-portal.md#prepare-source-data).

## <a name="connect-to-azure"></a>Verbinding maken met Azure
**Verbinding maken met Data Lake Analytics**

1. Open Visual Studio.
2. Klik in het menu **View** op **Server Explorer** om Server Explorer te openen. U kunt ook op **[Ctrl] + [Alt] + S** drukken.
3. Klik met de rechtermuisknop op **Azure**, klik op ‘Connect to Microsoft Azure Subscription’ en volg de instructies.
4. Vouw in **Server Explorer** achtereenvolgens **Azure** en **Data Lake Analytics** uit. U ziet een lijst met uw Data Lake Analytics-accounts, als u die hebt. U kunt vanuit Visual Studio geen Data Lake Analytics-accounts maken. Zie [Aan de slag met Azure Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md) of [Aan de slag met Azure Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md) voor meer informatie over het maken van een account.

## <a name="upload-source-data-files"></a>Uploaden van brongegevensbestanden
Eerder in de zelfstudie, in de sectie **Vereisten**, hebt u enkele gegevens geüpload.  

Als u uw eigen gegevens wilt gebruiken, volgt u deze stappen voor het uploaden van gegevens vanuit de Data Lake-tools.

**Bestanden uploaden naar het afhankelijke Azure Data Lake-account**

1. Ga naar **Server Explorer**, vouw **Azure** uit, vouw **Data Lake Analytics** uit, vouw uw Data Lake Analytics-account uit en vouw **Storage Accounts** uit. U ziet de Data Lake Storage-standaardaccounts, de gekoppelde Data Lake Storage-accounts en de gekoppelde Azure Storage-accounts. Het Data Lake-standaardaccount heeft het label ‘Default Storage Account’.
2. Klik met de rechtermuisknop op het Data Lake Storage-standaardaccount en klik vervolgens op **Explorer**.  Hiermee opent u het deelvenster Data Lake Tools voor Visual Studio Explorer.  Aan de linkerkant wordt een structuurweergave en aan de rechterkant de inhoudsweergave weergegeven.
3. Blader naar de map waarnaar u bestanden wilt uploaden,
4. klik met de rechtermuisknop op een lege ruimte en klik vervolgens op **Upload**.

    ![U-SQL Visual Studio-project U-SQL](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-upload-files.png)

**Bestanden uploaden naar een gekoppeld Azure Blob-opslagaccount**

1. Ga naar **Server Explorer**, vouw **Azure** uit, vouw **Data Lake Analytics** uit, vouw uw Data Lake Analytics-account uit en vouw **Storage Accounts** uit. U ziet de Data Lake Storage-standaardaccounts, de gekoppelde Data Lake Storage-accounts en de gekoppelde Azure Storage-accounts.
2. Vouw de Azure Storage-account uit.
3. Klik met de rechtermuisknop op de container waarnaar u de bestanden wilt uploaden en klik vervolgens op **Explorer**. Als u geen container hebt, moet u er eerst een maken via Azure Portal, met Azure PowerShell of met een ander hulpprogramma.
4. Blader naar de map waarnaar u bestanden wilt uploaden,
5. klik met de rechtermuisknop op een lege ruimte en klik vervolgens op **Upload**.

## <a name="develop-u-sql-scripts"></a>U-SQL-scripts ontwikkelen
Data Lake Analytics-taken worden geschreven in de U-SQL-taal. Zie [Aan de slag met de U-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [Naslaginformatie voor de U-SQL-taal](http://go.microsoft.com/fwlink/?LinkId=691348) voor meer informatie over U-SQL.

**Een Data Lake Analytics-taak maken en verzenden**

1. Klik in het menu **File** op **New** en klik vervolgens op **Project**.
2. Selecteer het type **U-SQL Project**.

    ![nieuw U-SQL Visual Studio-project](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klik op **OK**. Visual maakt een oplossing met een **Script.usql**-bestand.
4. Geef het volgende script op in **Script.usql**:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();

        @res =
            SELECT *
            FROM @searchlog;        

        OUTPUT @res   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Dit U-SQL-script leest het brongegevensbestand met **Extractors.Tsv()** en maakt vervolgens een CSV-bestand met **Outputters.Csv()**.

    Wijzig de twee paden niet, tenzij u het bronbestand naar een andere locatie hebt gekopieerd.  Data Lake Analytics maakt de uitvoermap als deze nog niet bestaat.

    Het is eenvoudiger om relatieve paden te gebruiken voor bestanden die zijn opgeslagen in Data Lake-standaardaccounts. Maar u kunt ook absolute paden gebruiken.  Bijvoorbeeld

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    U dient absolute paden te gebruiken om toegang te krijgen tot bestanden in gekoppelde Storage-accounts.  De syntaxis voor bestanden die zijn opgeslagen in het gekoppelde Azure Storage-account is:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

   > [!NOTE]
   > Azure Blob-containers met openbare blobs of machtigingen voor openbare containers worden momenteel niet ondersteund.  
   >
   >

    U ziet de volgende functies:

   * **IntelliSense**

       Namen worden automatisch aangevuld en de leden worden weergegeven voor rijenset, klassen, databases, schema’s en door de gebruiker gedefinieerde objecten (User Defined Objects, UDO’s).

       IntelliSense voor catalogusentiteiten (databases, schema's, tabellen, UDO’s enz.) is gerelateerd aan uw Compute-account. U kunt het huidige actieve Compute-account, de database en het schema controleren in de bovenste werkbalk en ze overschakelen via de vervolgkeuzelijsten.
   * **kolommen * uitvouwen**

       Als u rechts van de * klikt, ziet u een blauwe onderstreping onder de *. Houd de muisaanwijzer op de blauwe onderstreping en klik vervolgens op de pijl-omlaag.
       ![Data Lake-hulpprogramma's voor Visual Studio uitvouwen *](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-expand-asterisk.png)

       Klik op **Expand Columns** en het hulpprogramma vervangt de * door de kolomnamen.
   * **Auto-indeling**

       U kunt de inspringing van het U-SQL-script wijzigen op basis van de codestructuur onder Edit->Advanced:

     * Format Document (Ctrl + E, D): het hele document wordt opgemaakt.   
     * Format Selection (Ctrl + K, Ctrl + F): de selectie wordt opgemaakt. Als u geen selectie hebt gemaakt, wordt met deze sneltoets de regel opgemaakt waarin de cursor staat.  

       Alle opmaakregels kunnen worden geconfigureerd onder Tools->Options->Text Editor->SIP->Formatting.  
   * **Slim inspringen**

       Data Lake Tools voor Visual Studio kan expressies automatisch laten inspringen tijdens het schrijven van scripts. Deze functie is standaard uitgeschakeld. Gebruikers kunnen deze inschakelen via U-SQL->Options and Settings ->Switches->Enable Smart Indent.
   * **Ga naar definitie en Alle verwijzingen zoeken**

       Klik met de rechtermuisknop op de naam van een rijenset/parameter/kolom/UDO enzovoort, en klik vervolgens op Go To Definition (F12) om naar de definitie te gaan. Klik op Find All References (Shift+F12) om alle verwijzingen weer te geven.
   * **Azure-pad invoegen**

       In plaats van dat u moet onthouden wat het Azure-bestandspad is en het handmatig moet typen bij het schrijven van scripts, biedt Data Lake Tools voor Visual Studio een eenvoudigere manier: klik met de rechtermuisknop in de editor en klik op Insert Azure Path. Ga naar het bestand in het dialoogvenster Azure Blob Browser. Klik op **OK**. het bestandspad wordt ingevoegd in de code.
5. Geef het Data Lake Analytics-account, de database en het schema op. U kunt **(local)** selecteren om het script lokaal uit te voeren voor testdoeleinden. Zie [U-SQL lokaal uitvoeren](#run-u-sql-locally) voor meer informatie.

    ![U-SQL Visual Studio-project verzenden](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

    Zie [U-SQL-catalogus gebruiken](data-lake-analytics-use-u-sql-catalog.md) voor meer informatie.
6. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Build Script**. Controleer het resultaat in het deelvenster Output.
7. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Submit Script**. U kunt ook klikken op **Submit** in het deelvenster Script.usql.  Zie de vorige schermafbeelding.  Klik op de pijl-omlaag naast de knop Submit om te verzenden met de geavanceerde opties:
8. Geef de **Job name** op, controleer het **Analytics Account** en klik vervolgens op **Submit**. Het resultaat van het verzenden en de koppeling naar de taak zijn beschikbaar in het resultaatvenster van Data Lake Tools voor Visual Studio wanneer het verzenden is voltooid.

    ![U-SQL Visual Studio-project verzenden](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job-advanced.png)
9. U moet op de knop Refresh klikken om de status van de meest recente taak weer te geven en het scherm te vernieuwen. Wanneer de taak is voltooid, worden de **Job Graph**, **Meta Data Operations**, **State History** en **Diagnostics** weergegeven:

    ![Prestatiegrafiek U-SQL Visual Studio Data Lake Analytics-taak](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * Job Summary. De samenvattende informatie over de huidige taak wordt weergeven: State, Progress, Execution Time, Runtime Name, Submitter, enzovoort.   
   * Job Details. Gedetailleerde informatie over deze taak wordt weergegeven, waaronder Script, Resource, Vertex Execution View.
   * Job Graph. Er worden vier grafieken weergegeven die de informatie over de taak visualiseren: Progress, Data Read, Data Written, Execution Time, Average Execution Time Per Node, Input Throughput, Output Throughput.
   * Bewerkingen voor metagegevens. Alle bewerkingen voor metagegevens worden weergegeven.
   * State History.
   * Diagnostics. Data Lake Tools voor Visual Studio onderzoekt automatisch de taakuitvoering. U ontvangt waarschuwingen wanneer er fouten of prestatieproblemen in taken optreden. Zie het gedeelte Job Diagnostics (koppeling nog te bepalen) voor meer informatie.

**De taakstatus controleren**

1. Ga naar Server Explorer, vouw **Azure** uit, vouw **Data Lake Analytics** uit en vouw uw Data Lake Analytics-account uit.
2. Dubbelklik op **Jobs** om de taken weer te geven.
3. Klik op een taak om de status weer te geven.

**De taakuitvoer weergeven**

1. Ga naar **Server Explorer**, vouw **Azure** uit, vouw **Data Lake Analytics** uit, vouw uw Data Lake Analytics-account uit, vouw **Storage Accounts** uit, klik met de rechtermuisknop op het Data Lake Storage-standaardaccount en klik vervolgens op **Explorer**.
2. Dubbelklik op **output** om de map te openen.
3. Dubbelklik op **SearchLog-From-adltools.csv**.

### <a name="job-playback"></a>Taak afspelen
Met Taak afspelen kunt u de uitvoering van de taak bekijken en afwijkingen en knelpunten visueel vaststellen. Deze functie kunt u gebruiken voordat de taakuitvoering is voltooid (dus tijdens de periode waarin de taak actief wordt uitgevoerd) en nadat de taakuitvoering is voltooid. Als u de taak afspeelt tijdens het uitvoeren ervan, wordt de voortgang afgespeeld tot het huidige tijdstip.

**Voortgang van de taak weergeven**  

1. Klik op **Load Profile** rechtsboven. Zie de vorige schermafbeelding.
2. Klik op de knop Play linksonder om de voortgang van de taak te controleren.
3. Klik tijdens het afspelen op **Pause** om het afspelen te stoppen of de voortgangsbalk direct naar een specifieke positie te slepen.

### <a name="heat-map"></a>Heat Map
Data Lake Tools voor Visual Studio biedt instelbare kleurenoverlays voor de taakweergave, voor het aanduiden van de voortgang, gegevens-I/O, uitvoeringstijd en I/O-doorvoer in elke fase. Hiermee kunt u potentiële problemen identificeren en taakeigenschappen direct en op intuïtieve wijze distribueren. Kies de gegevensbron die u wilt weergeven in de vervolgkeuzelijst.  

## <a name="run-u-sql-locally"></a>U-SQL lokaal uitvoeren

Met Azure Data Lake-tools voor Visio Studio en de Azure Data Lake U-SQL-SDK kunt u U-SQL-taken op uw werkstation uitvoeren, net zoals u in de Azure Data Lake-service. Deze twee lokaal uitgevoerde functies besparen tijd op het gebied van het testen en de foutopsporing van uw U-SQL-taken. 

* [U-SQL-taken testen en controleren op fouten met behulp van lokale uitvoering en de Azure Data Lake U-SQL-SDK](data-lake-analytics-data-lake-tools-local-run.md)


## <a name="see-also"></a>Zie ook
Om aan de slag te gaan met Data Lake Analytics met verschillende hulpprogramma's, zie:

* [Aan de slag met Data Lake Analytics met Azure Portal](data-lake-analytics-get-started-portal.md)
* [Aan de slag met Data Lake Analytics met Azure PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met .NET SDK](data-lake-analytics-get-started-net-sdk.md)
* [Problemen met C#-code oplossen in U-SQL-taken](data-lake-analytics-debug-u-sql-jobs.md)

Zie [De hulpprogramma's voor Visual Studio van Azure Data Lake gebruiken](data-lake-analytics-data-lake-tools-for-vscode.md) voor meer informatie over Data Lake-hulpprogramma’s voor Visual Studio-code.

Overige onderwerpen over ontwikkelen:

* [Weblogboeken analyseren met Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
* [U-SQL-scripts ontwikkelen met Data Lake-hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Aan de slag met Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md)
* [Door de gebruiker gedefinieerde U-SQL-operators ontwikkelen voor Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md)

## <a name="appx-a-powershell-sample-for-preparing-the-tutorial"></a>Appx-A PowerShell-voorbeeld ter voorbereiding op de zelfstudie
Het volgende PowerShell-script bereidt een Azure Data Lake Analytics-account en de brongegevens voor, zodat u verder kunt gaan met [U-SQL-scripts ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md#develop-u-sql-scripts).

    #region - used for creating Azure service names
    $nameToken = "<Enter an alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - service names
    $resourceGroupName = $namePrefix + "rg"
    $dataLakeStoreName = $namePrefix + "adas"
    $dataLakeAnalyticsName = $namePrefix + "adla"
    $location = "East US 2"
    #endregion


    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an Azure Data Lake Analytics service account
    Write-Host "Create a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
    New-AzureRmDataLakeStoreAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeStoreName `
        -Location $location

    Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
    New-AzureRmDataLakeAnalyticsAccount `
        -Name $dataLakeAnalyticsName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName

    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsName  
    #endregion

    #region - prepare the source data
    Write-Host "Import the source data ..."  -ForegroundColor Green
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file.
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

    Write-Host "List the source data ..."  -ForegroundColor Green
    Get-AzureRmDataLakeStoreChildItem -Account $dataLakeStoreName -Path  "/Samples/Data/"
    #endregion

