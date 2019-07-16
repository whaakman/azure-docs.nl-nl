---
title: 'Zelfstudie: R gebruiken in een Spark-compute-context in Azure HDInsight'
description: 'Zelfstudie: aan de slag met R en Spark op een cluster van Azure HDInsight, Machine Learning-services.'
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/21/2019
ms.openlocfilehash: f072b6905881da7b7854b0e51d690dbbd40dffb5
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68227442"
---
# <a name="tutorial-use-r-in-a-spark-compute-context-in-azure-hdinsight"></a>Zelfstudie: R gebruiken in een Spark-compute-context in Azure HDInsight

Deze zelfstudie bevat een stapsgewijze inleiding tot het gebruik van de R-functies in Apache Spark die worden uitgevoerd op een cluster van Azure HDInsight, Machine Learning-services.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De voorbeeldgegevens downloaden naar de lokale opslag
> * De gegevens naar de standaardopslag voor kopiëren
> * Instellen van een gegevensset
> * Gegevensbronnen maken
> * Een compute-context voor Spark maken
> * Een lineair model passen
> * Samengestelde XDF-bestanden gebruiken
> * XDF converteren naar CSV

## <a name="prerequisites"></a>Vereisten

* Een cluster van Azure HDInsight, Machine Learning-services. Ga naar [Apache Hadoop-clusters maken met behulp van de Azure-portal](../hdinsight-hadoop-create-linux-clusters-portal.md) en, voor **clustertype**, selecteer **ML-Services**.

## <a name="connect-to-rstudio-server"></a>Verbinding maken met RStudio Server

RStudio Server wordt uitgevoerd op het edge-knooppunt van het cluster. Ga naar de volgende site (waarbij *CLUSTERNAME* in de URL is de naam van het HDInsight Machine Learning-services-cluster die u hebt gemaakt):

```
https://CLUSTERNAME.azurehdinsight.net/rstudio/
```

De eerste keer dat u zich aanmeldt, u zich twee keer verifiëren. Bij de eerste verificatieprompt geeft u de gebruikersnaam van de cluster-beheerder en het wachtwoord (de standaardwaarde is *admin*). Bij de tweede verificatieprompt geeft u de SSH-gebruikersnaam en het wachtwoord (de standaardwaarde is *sshuser*). Volgende aanmeldingen zijn alleen SSH-referenties vereist.

## <a name="download-the-sample-data-to-local-storage"></a>De voorbeeldgegevens downloaden naar de lokale opslag

De *luchtvaartmaatschappij 2012 op tijd gegevensset* bestaat uit 12 door komma's gescheiden bestanden met vlucht aankomst en vertrek details voor alle commerciële vluchten binnen de VS voor het jaar 2012. Deze gegevensset is groot, met meer dan 6 miljoen metingen.

1. Een aantal omgevingsvariabelen worden geïnitialiseerd. Voer de volgende code in de console van de RStudio Server:

    ```R
    bigDataDirRoot <- "/tutorial/data" # root directory on cluster default storage
    localDir <- "/tmp/AirOnTimeCSV2012" # directory on edge node
    remoteDir <- "https://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012" # location of data
    ```

1. Selecteer in het rechterdeelvenster de **omgeving** tabblad. De variabelen worden weergegeven onder **waarden**.

    ![RStudio](./media/ml-services-tutorial-spark-compute/rstudio.png)

1.  Maak een lokale map en de voorbeeldgegevens downloaden. In RStudio, voer de volgende code:

    ```R
    # Create local directory
    dir.create(localDir)
    
    # Download data to the tmp folder(local)
    download.file(file.path(remoteDir, "airOT201201.csv"), file.path(localDir, "airOT201201.csv"))
    download.file(file.path(remoteDir, "airOT201202.csv"), file.path(localDir, "airOT201202.csv"))
    download.file(file.path(remoteDir, "airOT201203.csv"), file.path(localDir, "airOT201203.csv"))
    download.file(file.path(remoteDir, "airOT201204.csv"), file.path(localDir, "airOT201204.csv"))
    download.file(file.path(remoteDir, "airOT201205.csv"), file.path(localDir, "airOT201205.csv"))
    download.file(file.path(remoteDir, "airOT201206.csv"), file.path(localDir, "airOT201206.csv"))
    download.file(file.path(remoteDir, "airOT201207.csv"), file.path(localDir, "airOT201207.csv"))
    download.file(file.path(remoteDir, "airOT201208.csv"), file.path(localDir, "airOT201208.csv"))
    download.file(file.path(remoteDir, "airOT201209.csv"), file.path(localDir, "airOT201209.csv"))
    download.file(file.path(remoteDir, "airOT201210.csv"), file.path(localDir, "airOT201210.csv"))
    download.file(file.path(remoteDir, "airOT201211.csv"), file.path(localDir, "airOT201211.csv"))
    download.file(file.path(remoteDir, "airOT201212.csv"), file.path(localDir, "airOT201212.csv"))
    ```

    Het downloaden van het moet worden voltooid in ongeveer 9.5 minuten zijn.

## <a name="copy-the-data-to-default-storage"></a>De gegevens naar de standaardopslag voor kopiëren

De locatie van het Hadoop Distributed File System (HDFS) wordt opgegeven met de `airDataDir` variabele. In RStudio, voer de volgende code:

```R
# Set directory in bigDataDirRoot to load the data into
airDataDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

# Create directory (default storage)
rxHadoopMakeDir(airDataDir)

# Copy data from local storage to default storage
rxHadoopCopyFromLocal(localDir, bigDataDirRoot)
    
# Optional. Verify files
rxHadoopListFiles(airDataDir)
```

De stap moet worden voltooid in ongeveer 10 seconden.

## <a name="set-up-a-dataset"></a>Instellen van een gegevensset

1. Maak een bestandssysteemobject die gebruikmaakt van de standaardwaarden. In RStudio, voer de volgende code:

    ```R
    # Define the HDFS (WASB) file system
    hdfsFS <- RxHdfsFileSystem()
    ```

1. Omdat de oorspronkelijke CSV-bestanden in plaats daarvan onhandig variabelenamen hebben, Geef een *colInfo* lijst om ze gemakkelijker te maken. In RStudio, voer de volgende code:

    ```R
    airlineColInfo <- list(
         MONTH = list(newName = "Month", type = "integer"),
        DAY_OF_WEEK = list(newName = "DayOfWeek", type = "factor",
            levels = as.character(1:7),
            newLevels = c("Mon", "Tues", "Wed", "Thur", "Fri", "Sat",
                          "Sun")),
        UNIQUE_CARRIER = list(newName = "UniqueCarrier", type =
                                "factor"),
        ORIGIN = list(newName = "Origin", type = "factor"),
        DEST = list(newName = "Dest", type = "factor"),
        CRS_DEP_TIME = list(newName = "CRSDepTime", type = "integer"),
        DEP_TIME = list(newName = "DepTime", type = "integer"),
        DEP_DELAY = list(newName = "DepDelay", type = "integer"),
        DEP_DELAY_NEW = list(newName = "DepDelayMinutes", type =
                             "integer"),
        DEP_DEL15 = list(newName = "DepDel15", type = "logical"),
        DEP_DELAY_GROUP = list(newName = "DepDelayGroups", type =
                               "factor",
           levels = as.character(-2:12),
           newLevels = c("< -15", "-15 to -1","0 to 14", "15 to 29",
                         "30 to 44", "45 to 59", "60 to 74",
                         "75 to 89", "90 to 104", "105 to 119",
                         "120 to 134", "135 to 149", "150 to 164",
                         "165 to 179", ">= 180")),
        ARR_DELAY = list(newName = "ArrDelay", type = "integer"),
        ARR_DELAY_NEW = list(newName = "ArrDelayMinutes", type =
                             "integer"),  
        ARR_DEL15 = list(newName = "ArrDel15", type = "logical"),
        AIR_TIME = list(newName = "AirTime", type =  "integer"),
        DISTANCE = list(newName = "Distance", type = "integer"),
        DISTANCE_GROUP = list(newName = "DistanceGroup", type =
                             "factor",
         levels = as.character(1:11),
         newLevels = c("< 250", "250-499", "500-749", "750-999",
             "1000-1249", "1250-1499", "1500-1749", "1750-1999",
             "2000-2249", "2250-2499", ">= 2500")))
    
    varNames <- names(airlineColInfo)
    ```

## <a name="create-data-sources"></a>Gegevensbronnen maken

In een Spark-compute-context, kunt u gegevensbronnen kunt maken met behulp van de volgende functies:

|Function | Description |
|---------|-------------|
|`RxTextData` | Een gegevensbron met door komma's gescheiden tekst. |
|`RxXdfData` | Gegevens in de indeling van de XDF-bestand. In RevoScaleR, wordt de XDF-indeling voor Hadoop voor het opslaan van gegevens in een samengestelde set van bestanden in plaats van een enkel bestand gewijzigd. |
|`RxHiveData` | Genereert een gegevensbron Hive-object.|
|`RxParquetData` | Genereert een gegevensbron Parquet-object.|
|`RxOrcData` | Genereert een gegevensbron Orc-object.|

Maak een [RxTextData](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxtextdata) object met behulp van de bestanden die u hebt gekopieerd naar HDFS. In RStudio, voer de volgende code:

```R
airDS <- RxTextData( airDataDir,
                        colInfo = airlineColInfo,
                        varsToKeep = varNames,
                        fileSystem = hdfsFS ) 
```

## <a name="create-a-compute-context-for-spark"></a>Een compute-context voor Spark maken

Als u wilt laden van gegevens en analyses uitvoeren op de worker-knooppunten, u de compute-context instellen in uw script [RxSpark](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxspark). In deze context, R-functies automatisch de workload gedistribueerd over alle worker-knooppunten, geen ingebouwde vereisten voor het beheren van taken of de wachtrij. De compute-context voor Spark tot stand is gebracht via `RxSpark` of `rxSparkConnect()` maken de Spark compute-context en maakt gebruik van `rxSparkDisconnect()` om terug te keren naar een lokale compute-context. In RStudio, voer de volgende code:

```R
# Define the Spark compute context
mySparkCluster <- RxSpark()

# Set the compute context
rxSetComputeContext(mySparkCluster)
```

## <a name="fit-a-linear-model"></a>Een lineair model passen

1. Gebruik de [rxLinMod](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxlinmod) werken aan een lineair model met uw `airDS` gegevensbron. In RStudio, voer de volgende code:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDS,
              cube = TRUE)
    )
    ```
    
    Deze stap moet worden voltooid in 2-3 minuten.

1. Bekijk de resultaten. In RStudio, voer de volgende code:

    ```R
    summary(delayArr)
    ```

    U ziet de volgende resultaten:

    ```output
    Call:
    rxLinMod(formula = ArrDelay ~ DayOfWeek, data = airDS, cube = TRUE)
    
    Cube Linear Regression Results for: ArrDelay ~ DayOfWeek
    Data: airDataXdf (RxXdfData Data Source)
    File name: /tutorial/data/AirOnTimeCSV2012
    Dependent variable(s): ArrDelay
    Total independent variables: 7 
    Number of valid observations: 6005381
    Number of missing observations: 91381 
     
    Coefficients:
                   Estimate Std. Error t value Pr(>|t|)     | Counts
    DayOfWeek=Mon   3.54210    0.03736   94.80 2.22e-16 *** | 901592
    DayOfWeek=Tues  1.80696    0.03835   47.12 2.22e-16 *** | 855805
    DayOfWeek=Wed   2.19424    0.03807   57.64 2.22e-16 *** | 868505
    DayOfWeek=Thur  4.65502    0.03757  123.90 2.22e-16 *** | 891674
    DayOfWeek=Fri   5.64402    0.03747  150.62 2.22e-16 *** | 896495
    DayOfWeek=Sat   0.91008    0.04144   21.96 2.22e-16 *** | 732944
    DayOfWeek=Sun   2.82780    0.03829   73.84 2.22e-16 *** | 858366
    ---
    Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
    
    Residual standard error: 35.48 on 6005374 degrees of freedom
    Multiple R-squared: 0.001827 (as if intercept included)
    Adjusted R-squared: 0.001826 
    F-statistic:  1832 on 6 and 6005374 DF,  p-value: < 2.2e-16 
    Condition number: 1 
    ```

    De resultaten blijkt dat u alle gegevens, hebt verwerkt 6 miljoen metingen, met behulp van de CSV-bestanden in de opgegeven map. Omdat u hebt opgegeven `cube = TRUE`, hebt u een geschatte coëfficiënt voor elke dag van de week (en niet de intercept).

## <a name="use-composite-xdf-files"></a>Samengestelde XDF-bestanden gebruiken

Als u hebt gezien, kunt u de CSV-bestanden rechtstreeks met R op Hadoop analyseren. Maar u kunt de analyse sneller doen als u de gegevens opslaan in een efficiëntere indeling. De R-XDF-indeling is efficiënt, maar deze heeft gewijzigd enigszins voor HDFS zodat afzonderlijke bestanden binnen één HDFS-blok blijven. (De grootte van het HDFS-blok is afhankelijk van de installatie installatie maar is doorgaans 64 MB of 128 MB.) 

Bij het gebruik [rxImport](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rximport) op Hadoop om een set van samengestelde XDF-bestanden te maken, geeft u een `RxTextData` gegevensbron zoals `AirDS` als de inData en een `RxXdfData` gegevensbron met bestandssysteem ingesteld op een HDFS-bestandssysteem als de Uitvoerbestand argument. Vervolgens kunt u de `RxXdfData` object als het argument van de gegevens in de volgende R-analyses.

1. Definieer een `RxXdfData` object. In RStudio, voer de volgende code:

    ```R
    airDataXdfDir <- file.path(bigDataDirRoot,"AirOnTimeXDF2012")
    
    airDataXdf <- RxXdfData( airDataXdfDir,
                            fileSystem = hdfsFS )
    ```

1. Stel een blokgrootte van 250000 rijen en opgeven dat we de gegevens lezen. In RStudio, voer de volgende code:

    ```R
    blockSize <- 250000
    numRowsToRead = -1
    ```

1. Importeer de gegevens met `rxImport`. In RStudio, voer de volgende code:

    ```R
    rxImport(inData = airDS,
             outFile = airDataXdf,
             rowsPerRead = blockSize,
             overwrite = TRUE,
             numRows = numRowsToRead )
    ```
    
    Deze stap moet worden voltooid in een paar minuten.

1. Schatting maken van de dezelfde lineair model met behulp van de nieuwe en snellere gegevensbron opnieuw. In RStudio, voer de volgende code:

    ```R
    system.time(
         delayArr <- rxLinMod(ArrDelay ~ DayOfWeek, data = airDataXdf,
              cube = TRUE)
    )
    ```
    
    De stap moet worden voltooid in minder dan een minuut.

1. Bekijk de resultaten. De resultaten moeten hetzelfde zijn op de CSV-bestanden. In RStudio, voer de volgende code:

    ```R
    summary(delayArr)
    ```

## <a name="convert-xdf-to-csv"></a>XDF converteren naar CSV

### <a name="in-a-spark-context"></a>In een Spark-context

Als u uw CSV-bestanden naar XDF-bestandsindeling voor grotere efficiëntie omgezet tijdens het uitvoeren van de analyses, maar nu wilt converteren van uw gegevens terug naar CSV, u kunt doen met behulp van [rxDataStep](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxdatastep).

Voor het maken van een map met CSV-bestanden, maakt u eerst een `RxTextData` object met behulp van de naam van een map als het argument. Dit object vertegenwoordigt de map waarin u wilt maken van de CSV-bestanden. Deze map wordt gemaakt tijdens het uitvoeren van de `rxDataStep`. Wijs vervolgens aan deze `RxTextData` van het object in de `outFile` argument van de `rxDataStep`. Elke CSV die gemaakt met de naam op basis van de naam van de map en gevolgd door een getal.

Stel dat u wilt uitschrijven van een map met CSV-bestanden in HDFS uit uw `airDataXdf` samengestelde XDF na het uitvoeren van de logistieke regressie en de voorspelling, zodat de nieuwe CSV-bestanden de voorspelde waarden en dit bevatten. In RStudio, voer de volgende code:

```R
airDataCsvDir <- file.path(bigDataDirRoot,"AirDataCSV2012")
airDataCsvDS <- RxTextData(airDataCsvDir,fileSystem=hdfsFS)
rxDataStep(inData=airDataXdf, outFile=airDataCsvDS)
```

Deze stap moet worden voltooid in ongeveer 2,5 minuten zijn.

De `rxDataStep` uit een CSV-bestand voor elk bestand XDFD in het invoerbestand samengestelde XDF is geschreven. Dit is de standaardinstelling voor het CSV-bestanden schrijven van samengestelde XDF-bestanden naar HDFS, wanneer de compute-context is ingesteld op `RxSpark`.

### <a name="in-a-local-context"></a>In een lokale context

U kunt ook wanneer u klaar bent uw analyses uitvoeren, kunt u uw compute-context overschakelen terug naar `local` om te profiteren van twee argumenten binnen `RxTextData` die hebt u iets meer controle bij het schrijven van CSV-bestanden naar HDFS: `createFileSet` en `rowsPerOutFile`. Als u instelt `createFileSet` naar `TRUE`, een map met CSV-bestanden wordt geschreven naar de map die u opgeeft. Als u instelt `createFileSet` naar `FALSE`, één CSV-bestand wordt geschreven. U kunt instellen dat het tweede argument `rowsPerOutFile`, naar een geheel getal om aan te geven hoeveel rijen u wilt het schrijven naar elk CSV-bestand wanneer `createFileSet` is `TRUE`.

In RStudio, voer de volgende code:

```R
rxSetComputeContext("local")
airDataCsvRowsDir <- file.path(bigDataDirRoot,"AirDataCSVRows2012")
airDataCsvRowsDS <- RxTextData(airDataCsvRowsDir, fileSystem=hdfsFS, createFileSet=TRUE, rowsPerOutFile=1000000)
rxDataStep(inData=airDataXdf, outFile=airDataCsvRowsDS)
```

Deze stap moet worden voltooid in ongeveer 10 minuten.

Wanneer u gebruikt een `RxSpark` compute-context, `createFileSet` standaard ingesteld op `TRUE` en `rowsPerOutFile` heeft geen effect. Dus als u wilt maken van een enkele CSV of het aantal rijen per bestand aanpassen, voeren `rxDataStep` in een `local` compute-context (de gegevens kan nog steeds in HDFS).

## <a name="final-steps"></a>Laatste stappen

1. De gegevens opgeschoond. In RStudio, voer de volgende code:

    ```R
    rxHadoopRemoveDir(airDataDir)
    rxHadoopRemoveDir(airDataXdfDir)
    rxHadoopRemoveDir(airDataCsvDir)
    rxHadoopRemoveDir(airDataCsvRowsDir)
    rxHadoopRemoveDir(bigDataDirRoot)
    ```

1. Stop de externe Spark-toepassing. In RStudio, voer de volgende code:

    ```R
    rxStopEngine(mySparkCluster)
    ```

1. De R-sessie sluit. In RStudio, voer de volgende code:

    ```R
    quit()
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de zelfstudie hebt voltooid, is het raadzaam om het cluster te verwijderen. Uw gegevens worden opgeslagen in Azure Storage met HDInsight, zodat u een cluster veilig verwijderen kunt wanneer deze zich niet in gebruik. U bent ook kosten in rekening gebracht voor een HDInsight-cluster, zelfs wanneer deze zich niet in gebruik. Omdat de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, logischer het financieel gezien clusters te verwijderen wanneer ze zich niet in gebruik.

Als u wilt verwijderen van een cluster, Zie [verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u R-functies gebruiken in Apache Spark die worden uitgevoerd op een cluster HDInsight Machine Learning-services. Raadpleeg voor meer informatie de volgende artikelen:

* [Opties voor COMPUTE context voor een cluster van Azure HDInsight, Machine Learning-services](r-server-compute-contexts.md)
* [R-functies voor Spark op Hadoop.](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler-hadoop-functions)
