---
title: Uitbreiden met R in Azure Data Lake Analytics U-SQL-scripts | Microsoft Docs
description: Meer informatie over het R-code in de U-SQL-Scripts uitvoeren
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: sukvg
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: d479af515566f497d9611e75426f6acb8f8276d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-r"></a>Zelfstudie: Aan de slag met de U-SQL met R uitbreiden

Het volgende voorbeeld wordt de basisstappen voor het implementeren van R-code:
* Gebruik de `REFERENCE ASSEMBLY` instructie R uitbreidingen inschakelen voor de U-SQL-Script.
* Gebruik de` REDUCE` bewerking voor het partitioneren van de invoergegevens voor een sleutel.
* De R-uitbreidingen voor de U-SQL bevatten een ingebouwde reducer (`Extension.R.Reducer`) die op elk hoekpunt toegewezen aan de reducer R code wordt uitgevoerd. 
* Informatie over het gebruik van speciale met de naam gegevensframes aangeroepen `inputFromUSQL` en `outputToUSQL `respectievelijk naar het doorgeven van gegevens tussen de U-SQL en R. invoer en uitvoer DataFrame id-namen zijn opgelost (dat wil zeggen, gebruikers niet wijzigen van deze vooraf gedefinieerde namen van de invoer en uitvoer DataFrame-id's).

## <a name="embedding-r-code-in-the-u-sql-script"></a>R code insluiten in de U-SQL-script

U kunt de R code de U-SQL-script met behulp van de opdrachtparameter inline de `Extension.R.Reducer`. U kunt bijvoorbeeld het R-script als een string-variabele te declareren en als een parameter doorgeven aan de Reducer.


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

## <a name="keep-the-r-code-in-a-separate-file-and-reference-it--the-u-sql-script"></a>De R-code in een afzonderlijk bestand opslaan en verwijzen naar de U-SQL-script

Het volgende voorbeeld ziet u een complexere gebruik. In dit geval wordt de R-code geïmplementeerd als een RESOURCE die de U-SQL-script.

Sla deze R-code als een afzonderlijk bestand.

    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence")) 

Gebruik een U-SQL-script wilt implementeren die R-script met de instructie RESOURCE implementeren.

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

## <a name="how-r-integrates-with-u-sql"></a>Hoe R integreert met U-SQL

### <a name="datatypes"></a>Gegevenstypen
* Zo worden geconverteerd als tekenreeks en numerieke kolommen uit de U-SQL-tussen R DataFrame en U-SQL [ondersteunde typen: `double`, `string`, `bool`, `integer`, `byte`].
* De `Factor` gegevenstype wordt niet ondersteund in de U-SQL.
* `byte[]`Als een base64-codering moet worden geserialiseerd `string`.
* U-SQL-tekenreeksen kunnen worden geconverteerd naar factoren in R code, wanneer U-SQL maakt R invoer dataframe of door de parameter reducer `stringsAsFactors: true`.

### <a name="schemas"></a>Schema 's
* U-SQL-gegevenssets kunnen geen dubbele kolomnamen hebben.
* U-SQL gegevenssets kolomnamen moeten tekenreeksen zijn.
* Kolomnamen moet hetzelfde zijn in U-SQL en R-scripts.
* ReadOnly-kolom kan geen deel uitmaken van de dataframe uitvoer. Omdat readonly-kolommen worden automatisch terug in de U-SQL-tabel ingevoegd als het een deel van het uitvoerschema van UDO.

### <a name="functional-limitations"></a>Functionele beperkingen
* De R-Engine kan niet tweemaal in hetzelfde proces worden gemaakt. 
* U-SQL biedt momenteel geen ondersteuning voor Combiner UDO's voor de prognose gepartitioneerde modellen die worden gegenereerd met Reducer UDO's met. Gebruikers kunnen de gepartitioneerde modellen declareren als bron en deze gebruiken in hun R-Script (Zie voorbeeldcode `ExtR_PredictUsingLMRawStringReducer.usql`)

### <a name="r-versions"></a>R-versies
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

### <a name="input-and-output-size-limitations"></a>Invoer en uitvoer groottebeperkingen
Elke hoekpunt heeft een beperkte hoeveelheid geheugen die zijn toegewezen. Omdat de invoer en uitvoer DataFrames moet bestaan in het geheugen van de R-code, kan de totale grootte voor de invoer en uitvoer kan niet groter zijn dan 500 MB.

### <a name="sample-code"></a>Voorbeeldcode
Meer voorbeelden van code is beschikbaar in uw Data Lake Store-account nadat u de U-SQL Advanced Analytics extensions installeren. Het pad voor meer voorbeelden van code is: `<your_account_address>/usqlext/samples/R`. 

## <a name="deploying-custom-r-modules-with-u-sql"></a>Aangepaste R-modules met U-SQL implementeren

Maak eerst een aangepaste R-module en het zip- en vervolgens het gecomprimeerde R aangepaste module-bestand uploaden naar uw winkel ADL. In het voorbeeld gaat we magittr_1.5.zip uploaden naar de hoofdmap van het standaard ADLS-account voor het ADLA-account dat wordt gebruikt. Zodra u de module naar ADL store uploaden, declareert u deze als RESOURCE implementeren zodat deze beschikbaar in de U-SQL-scripts en aanroep gebruik `install.packages` om deze te installeren.

    REFERENCE ASSEMBLY [ExtR];
    DEPLOY RESOURCE @"/magrittr_1.5.zip";

    DECLARE @IrisData string =  @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFileModelSummary string = @"/R/Output/CustomePackages.txt";

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
