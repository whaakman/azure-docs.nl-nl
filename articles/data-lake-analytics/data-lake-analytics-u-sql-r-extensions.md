---
title: Uitbreiden met R in Azure Data Lake Analytics U-SQL-scripts
description: Meer informatie over het uitvoeren van R-code in U-SQL-scripts met Azure Data Lake Analytics
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: 5d10d4d603312b3c75760a5d7c17a9028ddecea8
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401212"
---
# <a name="extend-u-sql-scripts-with-r-code-in-azure-data-lake-analytics"></a>Uitbreiden met R-code in Azure Data Lake Analytics U-SQL-scripts

Het volgende voorbeeld wordt de basisstappen voor het implementeren van R-code:
* Gebruik de `REFERENCE ASSEMBLY` instructie voor het inschakelen van R-extensies voor de U-SQL-Script.
* Gebruik de` REDUCE` bewerking voor het partitioneren van de ingevoerde gegevens op een sleutel.
* De R-uitbreidingen voor U-SQL bevatten een ingebouwde reducer (`Extension.R.Reducer`) die op elk hoekpunt dat is toegewezen aan de reducer R-code wordt uitgevoerd. 
* Gebruik van toegewezen met de naam met de naam gegevensframes `inputFromUSQL` en `outputToUSQL `respectievelijk voor doorgeven van gegevens tussen de U-SQL en R. invoer en uitvoer DataFrame id-namen zijn opgelost (dat wil zeggen, gebruikers niet wijzigen van deze vooraf gedefinieerde namen van de invoer en uitvoer DataFrame id's).

## <a name="embedding-r-code-in-the-u-sql-script"></a>Het insluiten van R-code in de U-SQL-script

U kunt de R-code de U-SQL-script met behulp van de opdrachtparameter van inline de `Extension.R.Reducer`. U kunt bijvoorbeeld het R-script als een string-variabele declareren en geven deze als een parameter aan de Reducer.


    REFERENCE ASSEMBLY [ExtR];
    
    DECLARE @myRScript = @"
    inputFromUSQL$Species = as.factor(inputFromUSQL$Species)
    lm.fit=lm(unclass(Species)~.-Par, data=inputFromUSQL)
    #do not return readonly columns and make sure that the column names are the same in usql and r scripts,
    outputToUSQL=data.frame(summary(lm.fit)$coefficients)
    colnames(outputToUSQL) <- c(""Estimate"", ""StdError"", ""tValue"", ""Pr"")
    outputToUSQL
    ";
    
    @RScriptOutput = REDUCE … USING new Extension.R.Reducer(command:@myRScript, rReturnType:"dataframe");

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a>Houd de R-code in een afzonderlijk bestand en verwijs ernaar de U-SQL-script

Het volgende voorbeeld wordt een complexere gebruik. In dit geval wordt de R-code geïmplementeerd als een RESOURCE die de U-SQL-script.

Sla deze R-code als een afzonderlijk bestand.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

Gebruik een U-SQL-script te implementeren dat R-script met de implementatie van RESOURCE-instructie.

    REFERENCE ASSEMBLY [ExtR];

    DEPLOY RESOURCE @"/usqlext/samples/R/RinUSQL_PredictUsingLinearModelasDF.R";
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT 
            SepalLength double,
            SepalWidth double,
            PetalLength double,
            PetalWidth double,
            Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT 
            Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput = REDUCE @ExtendedData ON Par
        PRODUCE Par, fit double, lwr double, upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile:"RinUSQL_PredictUsingLinearModelasDF.R", rReturnType:"dataframe", stringsAsFactors:false);
        OUTPUT @RScriptOutput TO @OutputFilePredictions USING Outputters.Tsv();

## <a name="how-r-integrates-with-u-sql"></a>Hoe R kan worden geïntegreerd met U-SQL

### <a name="datatypes"></a>Gegevenstypen
* Zo worden geconverteerd als tekenreeks en numerieke kolommen in U-SQL-tussen R DataFrame en U-SQL [ondersteunde typen: `double`, `string`, `bool`, `integer`, `byte`].
* De `Factor` gegevenstype wordt niet ondersteund in U-SQL.
* `byte[]` moet het worden geserialiseerd als een base64-gecodeerd `string`.
* U-SQL-tekenreeksen kunnen worden geconverteerd naar factoren in R-code, wanneer U-SQL R invoer gegevensframe maken of door de parameter reducer `stringsAsFactors: true`.

### <a name="schemas"></a>Schema 's
* U-SQL-gegevenssets geen dubbele kolomnamen.
* U-SQL gegevenssets-kolomnamen moeten tekenreeksen zijn.
* Kolomnamen moeten hetzelfde als in U-SQL- en R-scripts.
* Alleen-lezen-kolom mag niet deel uit van het gegevensframe uitvoer. Omdat alleen-lezen-kolommen worden automatisch terug in de U-SQL-tabel toegevoegd als het is een onderdeel van het uitvoerschema van UDO.

### <a name="functional-limitations"></a>Functionele beperkingen
* De R-Engine kan niet twee keer in hetzelfde proces worden gemaakt. 
* U-SQL biedt op dit moment geen ondersteuning voor Combiner UDO's voor voorspelling met gepartitioneerde modellen die zijn gegenereerd met Reducer UDO's. Gebruikers kunnen de gepartitioneerde modellen declareren als bron en deze gebruiken in hun R-Script (Bekijk voorbeeldcode `ExtR_PredictUsingLMRawStringReducer.usql`)

### <a name="r-versions"></a>Versies van R
Alleen R 3.2.2 wordt ondersteund.

### <a name="standard-r-modules"></a>Standaard-R-modules

    base
    boot
    Class
    Cluster
    codetools
    compiler
    datasets
    doParallel
    doRSR
    foreach
    foreign
    Graphics
    grDevices
    grid
    iterators
    KernSmooth
    lattice
    MASS
    Matrix
    Methods
    mgcv
    nlme
    Nnet
    Parallel
    pkgXMLBuilder
    RevoIOQ
    revoIpe
    RevoMods
    RevoPemaR
    RevoRpeConnector
    RevoRsrConnector
    RevoScaleR
    RevoTreeView
    RevoUtils
    RevoUtilsMath
    Rpart
    RUnit
    spatial
    splines
    Stats
    stats4
    survival
    Tcltk
    Tools
    translations
    utils
    XML

### <a name="input-and-output-size-limitations"></a>Invoer en uitvoer beperkingen
Elke hoekpunt heeft een beperkte hoeveelheid van het geheugen toegewezen. Omdat de invoer- en DataFrames moet bestaan in het geheugen van de R-code, kan de totale grootte voor de invoer en uitvoer kan niet groter zijn dan 500 MB.

### <a name="sample-code"></a>Voorbeeldcode
Meer voorbeelden van code is beschikbaar in uw Data Lake Store-account nadat u de U-SQL Advanced Analytics extensions hebt geïnstalleerd. Het pad voor meer voorbeelden van code is: `<your_account_address>/usqlext/samples/R`. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>Implementeren van aangepaste R-modules met U-SQL

Eerst maakt u een aangepaste R-module en het zip- en vervolgens de ingepakte R aangepaste module-bestand uploaden naar uw ADL-store. In het voorbeeld wordt magittr_1.5.zip geüpload naar de hoofdmap van het standaard ADLS-account voor de ADLA-account die wordt gebruikt. Nadat u de module naar ADL-store uploaden, Declareer hem als RESOURCE implementeren zodat deze beschikbaar in uw U-SQL-script en de aanroep `install.packages` om deze te installeren.

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomPackages.txt";

    // R script to run
    DECLARE @myRScript = @"
    # install the magrittr package,
    install.packages('magrittr_1.5.zip', repos = NULL),
    # load the magrittr package,
    require(magrittr),
    # demonstrate use of the magrittr package,
    2 %>% sqrt
    ";

    @InputData =
    EXTRACT SepalLength double,
    SepalWidth double,
    PetalLength double,
    PetalWidth double,
    Species string
    FROM @IrisData
    USING Extractors.Csv();

    @ExtendedData =
    SELECT 0 AS Par,
    *
    FROM @InputData;

    @RScriptOutput = REDUCE @ExtendedData ON Par
    PRODUCE Par, RowId int, ROutput string
    READONLY Par
    USING new Extension.R.Reducer(command:@myRScript, rReturnType:"charactermatrix");

    OUTPUT @RScriptOutput TO @OutputFileModelSummary USING Outputters.Tsv();

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [U-SQL-scripts ontwikkelen met Data Lake-hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [U-SQL-vensterfuncties gebruiken voor Azure Data Lake Analytics-taken](data-lake-analytics-use-window-functions.md)
