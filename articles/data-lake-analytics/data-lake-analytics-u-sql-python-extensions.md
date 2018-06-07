---
title: Uitbreiden met behulp van Python in Azure Data Lake Analytics U-SQL-scripts
description: Meer informatie over het Python-code in met behulp van Azure Data Lake Analytics U-SQL-scripts uitvoeren
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: 3e895a6ea9bccc0d210f43748edb3eea80ddc6ad
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625077"
---
# <a name="extend-u-sql-scripts-with-python-code-in-azure-data-lake-analytics"></a>Uitbreiden met Python-code in Azure Data Lake Analytics U-SQL-scripts

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat de Python-serverextensies zijn geïnstalleerd in uw Azure Data Lake Analytics-account.

* Navigeer naar u Data Lake Analytics-Account in de Azure portal
* In het menu links onder **aan de slag** klikt u op **voorbeeldscripts**
* Klik op **U-SQL-uitbreidingen installeren** vervolgens **OK**

## <a name="overview"></a>Overzicht 

Python-uitbreidingen voor de U-SQL kunnen ontwikkelaars massively parallelle uitvoering van Python-code uitvoeren. Het volgende voorbeeld ziet u de eenvoudige stappen:

* Gebruik de `REFERENCE ASSEMBLY` instructie Python-uitbreidingen voor de U-SQL-Script inschakelen
* Met behulp van de `REDUCE` bewerking voor het partitioneren van de invoergegevens voor een sleutel
* De Python-uitbreidingen voor de U-SQL bevatten een ingebouwde reducer (`Extension.Python.Reducer`) die op elk hoekpunt toegewezen aan de reducer Python-code wordt uitgevoerd
* De U-SQL-script bevat de ingesloten Python-code die een functie die wordt aangeroepen `usqlml_main` die een pandas DataFrame als invoer accepteert en retourneert een pandas DataFrame als uitvoer.

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

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
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a>Hoe Python integreert met U-SQL

### <a name="datatypes"></a>Gegevenstypen

* Zo worden geconverteerd als tekenreeks en numerieke kolommen uit de U-SQL-tussen Pandas en U-SQL
* U-SQL null-waarden worden geconverteerd naar en van Pandas `NA` waarden

### <a name="schemas"></a>Schema 's

* Index aanvalsvectoren in Pandas worden niet ondersteund in de U-SQL. Alle ingevoerde gegevensframes in de functie Python hebben altijd een 64-bits numerieke index 0 tot en met het aantal rijen min 1. 
* U-SQL-gegevenssets kan geen dubbele kolomnamen hebben
* U-SQL gegevenssets kolomnamen die geen tekenreeksen. 

### <a name="python-versions"></a>Python-versies
Alleen Python 3.5.1 (gecompileerd voor Windows) wordt ondersteund. 

### <a name="standard-python-modules"></a>Standaard Python-modules
Alle standard Python-modules zijn opgenomen.

### <a name="additional-python-modules"></a>Extra Python-modules
Naast de standaard Python-bibliotheken zijn enkele veelgebruikte python-bibliotheken opgenomen:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Uitzondering berichten
Op dit moment is een uitzondering in Python-code wordt weergegeven als algemene hoekpunt mislukt. De U-SQL-taak foutberichten verschijnt in de toekomst, het uitzonderingsbericht Python.

### <a name="input-and-output-size-limitations"></a>Invoer en uitvoer groottebeperkingen
Elke hoekpunt heeft een beperkte hoeveelheid geheugen die zijn toegewezen. Deze limiet is momenteel 6 GB voor een AU. Omdat de invoer en uitvoer DataFrames moet bestaan in het geheugen van de Python-code, kan de totale grootte voor de invoer en uitvoer kan niet groter zijn dan 6 GB.

## <a name="see-also"></a>Zie ook
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [U-SQL-scripts ontwikkelen met Data Lake-hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [U-SQL-vensterfuncties gebruiken voor Azure Data Lake Analytics-taken](data-lake-analytics-use-window-functions.md)
* [Azure Data Lake Tools voor Visual Studio Code gebruiken](data-lake-analytics-data-lake-tools-for-vscode.md)
