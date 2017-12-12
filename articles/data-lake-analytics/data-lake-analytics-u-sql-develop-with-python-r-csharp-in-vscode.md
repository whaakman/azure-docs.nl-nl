---
title: U-SQL met Python, R en C# ontwikkelen voor Azure Data Lake Analytics in Visual Studio Code | Microsoft Docs
description: Informatie over het verzenden van de taak in Azure Data Lake met code achter met Python, R en C#.
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: 
editor: 
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/22/2017
ms.author: jejiang
ms.openlocfilehash: 8fd8decfde1220be6aaa099c3afb24e8c1eecce4
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="develop-u-sql-with-python-r-and-c-for-azure-data-lake-analytics-in-visual-studio-code"></a>U-SQL met Python, R en C# voor Azure Data Lake Analytics in Visual Studio Code ontwikkelen
Informatie over het gebruik van Visual Studio Code (VSCode) om te schrijven, Python, R- en C# code achter met U-SQL en verzenden van taken naar Azure Data Lake-service. Zie voor meer informatie over Azure Data Lake Tools voor VSCode [gebruik van Azure Data Lake Tools voor Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

Voordat het code-behind aangepaste code schrijven, moet u een map of een werkruimte in VSCode openen.


## <a name="prerequisites-for-python-and-r"></a>Vereisten voor Python en R
Python en R extensies-assembly's voor uw account ADL registreren. 
1. Open uw account in de portal.
   - Selecteer **overzicht**. 
   - Klik op **voorbeeldscript**.
2. Klik op **meer**.
3. Selecteer **U-SQL-uitbreidingen installeren**. 
4. Bevestigingsbericht wordt weergegeven nadat de U-SQL-uitbreidingen zijn geïnstalleerd. 

  ![Instellen van de omgeving voor python en R](./media/data-lake-analytics-data-lake-tools-for-vscode/setup-the-enrionment-for-python-and-r.png)

  > [!Note]
  > Installeer VSCode Python en R-extensie voor de beste ervaring op Python en R-taalservice. 

## <a name="develop-python-file"></a>Python-bestand ontwikkelen
1. Klik op de **nieuw bestand** in uw werkruimte.
2. Schrijf uw code in de U-SQL. Hier volgt een voorbeeld van code.
    ```U-SQL
    REFERENCE ASSEMBLY [ExtPython];
    @t  = 
        SELECT * FROM 
        (VALUES
            ("D1","T1","A1","@foo Hello World @bar"),
            ("D2","T2","A2","@baz Hello World @beer")
        ) AS 
            D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer("pythonSample.usql.py", pyVersion : "3.5.1");

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();
    ```
    
3. Met de rechtermuisknop op een scriptbestand en selecteer vervolgens **ADL: genereren Python Code achter bestand**. 
4. De **xxx.usql.py** -bestand is gegenereerd in uw werkmap. Schrijf uw code in Python-bestand. Hier volgt een voorbeeld van code.

    ```Python
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ```
5. Met de rechtermuisknop in **USQL** -bestand, klikt u op **compileren Script** of **Submit Job** voor het uitvoeren van taak.

## <a name="develop-r-file"></a>R bestand ontwikkelen
1. Klik op de **nieuw bestand** in uw werkruimte.
2. Schrijf uw code in U-SQL-bestand. Hier volgt een voorbeeld van code.
    ```U-SQL
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT SepalLength double,
                SepalWidth double,
                PetalLength double,
                PetalWidth double,
                Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput =
        REDUCE @ExtendedData
        ON Par
        PRODUCE Par,
                fit double,
                lwr double,
                upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile : "RClusterRun.usql.R", rReturnType : "dataframe", stringsAsFactors : false);
    OUTPUT @RScriptOutput
    TO @OutputFilePredictions
    USING Outputters.Tsv();
    ```
3. Met de rechtermuisknop in **USQL** bestand en selecteer vervolgens **ADL: genereren R achter codebestand**. 
4. De **xxx.usql.r** -bestand is gegenereerd in uw werkmap. Schrijf uw code in R-bestand. Hier volgt een voorbeeld van code.

    ```R
    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence"))
    ```
5. Met de rechtermuisknop in **USQL** -bestand, klikt u op **compileren Script** of **Submit Job** voor het uitvoeren van taak.

## <a name="develop-c-file"></a>C#-bestand ontwikkelen
Een code-behind-bestand is een C#-bestanden die zijn gekoppeld aan een enkel U-SQL-script. U kunt een speciale script definiëren UDO, UDA, UDT en UDF in het code-behind-bestand. De UDO, UDA, UDT en UDF kunnen rechtstreeks in het script zonder eerst registreren van de assembly worden gebruikt. De code-behind-bestand is in dezelfde map als de peering U-SQL-scriptbestand geplaatst. Als het script de naam xxx.usql, is de code-behind xxx.usql.cs genoemd. Als u de code-behind-bestand voor het handmatig verwijdert, wordt de code-behind-functie is uitgeschakeld voor de bijbehorende U-SQL-script. Zie voor meer informatie over het schrijven van code voor de U-SQL-script klant [schrijven en met behulp van aangepaste Code in de U-SQL: door de gebruiker gedefinieerde functies]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).

1. Klik op de **nieuw bestand** in uw werkruimte.
2. Schrijf uw code in U-SQL-bestand. Hier volgt een voorbeeld van code.
    ```U-SQL
    @a = 
        EXTRACT 
            Iid int,
        Starts DateTime,
        Region string,
        Query string,
        DwellTime int,
        Results string,
        ClickedUrls string 
        FROM @"/Samples/Data/SearchLog.tsv" 
        USING Extractors.Tsv();

    @d =
        SELECT DISTINCT Region 
        FROM @a;

    @d1 = 
        PROCESS @d
        PRODUCE 
            Region string,
        Mkt string
        USING new USQLApplication_codebehind.MyProcessor();

    OUTPUT @d1 
        TO @"/output/SearchLogtest.txt" 
        USING Outputters.Tsv();
    ```
3. Met de rechtermuisknop in **USQL** bestand en selecteer vervolgens **ADL: genereren CS achter codebestand**. 
4. De **xxx.usql.cs** -bestand is gegenereerd in uw werkmap. Schrijf uw code in CS-bestand. Hier volgt een voorbeeld van code.

    ```CS
    namespace USQLApplication_codebehind
    {
        [SqlUserDefinedProcessor]

        public class MyProcessor : IProcessor
        {
            public override IRow Process(IRow input, IUpdatableRow output)
            {
                output.Set(0, input.Get<string>(0));
                output.Set(1, input.Get<string>(0));
                return output.AsReadOnly();
            } 
        }
    }
    ```
5. Met de rechtermuisknop in **USQL** -bestand, klikt u op **compileren Script** of **Submit Job** voor het uitvoeren van taak.

## <a name="next-steps"></a>Volgende stappen
* [De Azure Data Lake-tools gebruiken voor Visual Studio-code](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-assemblies.md)
* [Aan de slag met Data Lake Analytics met PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met Azure portal](data-lake-analytics-get-started-portal.md)
* [Data Lake Tools voor Visual Studio gebruiken voor het ontwikkelen van U-SQL-toepassingen](data-lake-analytics-data-lake-tools-get-started.md)
* [Gebruik Data Lake Analytics(U-SQL) catalogus](data-lake-analytics-use-u-sql-catalog.md)
