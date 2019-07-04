---
title: 'Quickstart: Een R-script uitvoeren op een cluster ML-Services in Azure HDInsight met behulp van R-console'
description: In de Quick Start, kunt u een R-script uitvoeren op een ML-Services-cluster in Azure HDInsight met behulp van R-console.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 06/19/2019
ms.author: hrasheed
ms.custom: mvc
ms.openlocfilehash: 682ee4f44dcdd2619668645fa7a8aa22cb645273
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450924"
---
# <a name="quickstart-execute-an-r-script-on-an-ml-services-cluster-in-azure-hdinsight-using-r-console"></a>Quickstart: Een R-script uitvoeren op een cluster ML-Services in Azure HDInsight met behulp van R-console

ML-Services op Azure HDInsight kunnen R-scripts met Apache Spark en Apache Hadoop MapReduce gedistribueerde berekeningen kunt uitvoeren. ML-Services wordt bepaald hoe aanroepen zijn uitgevoerd door de instelling van de compute-context. Het edge-knooppunt van een cluster biedt een handige locatie verbinding maken met het cluster en uw R-scripts uit te voeren. Met een edge-knooppunt hebt u de mogelijkheid van het uitvoeren van de geparallelliseerde gedistribueerde functies van RevoScaleR over de kernen van het edge-knooppunt-server. U kunt ook ze uitvoeren op de knooppunten van het cluster met behulp van RevoScaleR Hadoop-Mapreduce of Apache Spark compute-context.

In deze snelstartgids leert u hoe u een R-script uitvoeren met R-console waarin wordt gedemonstreerd hoe Spark voor gedistribueerde R-berekeningen. U definieert een compute-context voor het uitvoeren van berekeningen lokaal op een edge-knooppunt en opnieuw worden verdeeld over de knooppunten in het HDInsight-cluster.

## <a name="prerequisites"></a>Vereisten

* Een ML-Services-cluster in HDInsight. Zie [Apache Hadoop-clusters maken met behulp van de Azure-portal](../hdinsight-hadoop-create-linux-clusters-portal.md) en selecteer **ML-Services** voor **clustertype**.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).


## <a name="connect-to-r-console"></a>Verbinding maken met R-console

1. Verbinding maken met het edge-knooppunt van een ML-Services HDInsight-cluster via SSH. De onderstaande opdracht bewerken door te vervangen `CLUSTERNAME` met de naam van het cluster, en voer vervolgens de opdracht:

    ```cmd
    ssh sshuser@CLUSTERNAME-ed-ssh.azurehdinsight.net
    ```

1. Gebruik tijdens de SSH-sessie de volgende opdracht om de R-console te starten:

    ```
    R
    ```

    U moet uitvoer zien die naast andere informatie de versie van ML Server bevat.


## <a name="use-a-compute-context"></a>Een compute-context gebruiken

1. U kunt de R-code invoeren vanuit de `>`-prompt. Gebruik de volgende code om voorbeeldgegevens te laden in de standaardopslag voor HDInsight:

    ```R
    # Set the HDFS (WASB) location of example data
     bigDataDirRoot <- "/example/data"
    
     # create a local folder for storing data temporarily
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
    ```

    Deze stap kan ongeveer 10 minuten duren om uit te voeren.

1. Maak enkele gegevens en definieer twee gegevensbronnen. Voer de volgende code in de R-console:

    ```R
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
    ```

1. Voer logistic regression via de via de **lokale** compute-context. Voer de volgende code in de R-console:

    ```R
    # Set a local compute context
     rxSetComputeContext("local")
    
     # Run a logistic regression
     system.time(
        modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
     )
    
     # Display a summary
     summary(modelLocal)
    ```

    De berekeningen moeten in ongeveer 7 minuten voltooid. Hier ziet u uitvoer die met regels die vergelijkbaar is met het volgende codefragment eindigt:

    ```output
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
    ```

1. Uitvoeren van de dezelfde logistic regression met behulp van de **Spark** context. De Spark-context distribueert de verwerking over alle werkknooppunten in het HDInsight-cluster. Voer de volgende code in de R-console:

    ```R
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
    ```

    De berekeningen moeten in circa 5 minuten voltooid.

1. Gebruik de volgende opdracht om de R-console af te sluiten:

    ```R
    quit()
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de Quick Start hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Als u wilt verwijderen van een cluster, Zie [verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u een R-script uitvoeren met R-console die gedemonstreerd met behulp van Spark voor gedistribueerde R-berekeningen.  Ga naar het volgende artikel voor meer informatie over de opties die beschikbaar zijn om op te geven of en hoe de uitvoering is geparallelliseerd over kernen van het edge-knooppunt of de HDInsight-cluster.

> [!div class="nextstepaction"]
>[Opties voor COMPUTE context voor ML-Services op HDInsight](./r-server-compute-contexts.md)