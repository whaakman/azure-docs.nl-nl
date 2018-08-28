---
title: Uitbreiden met Python in Azure Data Lake Analytics U-SQL-scripts
description: Meer informatie over het Python-code uitvoeren in U-SQL-scripts met Azure Data Lake Analytics
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.topic: conceptual
ms.date: 06/20/2017
ms.openlocfilehash: b2179f9d81a2dad877c8ae58471f7440eb9edbe7
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43050988"
---
# <a name="extend-u-sql-scripts-with-python-code-in-azure-data-lake-analytics"></a>Uitbreiden met Python-code in Azure Data Lake Analytics U-SQL-scripts

## <a name="prerequisites"></a>Vereisten

Voordat u begint, controleert u of dat de Python-extensies zijn geïnstalleerd in uw Azure Data Lake Analytics-account.

* Navigeer naar u Data Lake Analytics-Account in Azure portal
* In het menu links onder **aan de slag** klikt u op **voorbeeldscripts**
* Klik op **U-SQL Extensions installeren** vervolgens **OK**

## <a name="overview"></a>Overzicht 

Python-extensies voor U-SQL kunnen ontwikkelaars enorme parallelle uitvoering van de Python-code uitvoeren. Het volgende voorbeeld wordt de eenvoudige stappen:

* Gebruik de `REFERENCE ASSEMBLY` instructie voor het inschakelen van Python-extensies voor de U-SQL-Script
* Met behulp van de `REDUCE` bewerking voor het partitioneren van de ingevoerde gegevens op een sleutel
* De Python-uitbreidingen voor U-SQL bevatten een ingebouwde reducer (`Extension.Python.Reducer`) die op elk hoekpunt dat is toegewezen aan de reducer Python-code wordt uitgevoerd
* De U-SQL-script bevat de ingesloten Python-code die een functie met de naam bevat `usqlml_main` die een pandas DataFrame als invoer accepteert en retourneert een pandas DataFrame als uitvoer.

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

## <a name="how-python-integrates-with-u-sql"></a>Hoe Python kan worden geïntegreerd met U-SQL

### <a name="datatypes"></a>Gegevenstypen

* Zo worden geconverteerd als tekenreeks en numerieke kolommen in U-SQL-tussen Pandas en U-SQL
* U-SQL-null-waarden worden geconverteerd naar en van Pandas `NA` waarden

### <a name="schemas"></a>Schema 's

* Index aanvalsvectoren in Pandas worden niet ondersteund in U-SQL. Alle invoergegevens frames in de Python-functie hebben altijd een 64-bits numerieke index tussen 0 en het aantal rijen min 1. 
* U-SQL-gegevenssets geen dubbele kolomnamen
* U-SQL gegevenssets kolomnamen die geen tekenreeksen. 

### <a name="python-versions"></a>Python-versies
Alleen Python 3.5.1 (gecompileerd voor Windows) wordt ondersteund. 

### <a name="standard-python-modules"></a>Standaard Python-modules
Alle standaard Python-modules zijn opgenomen.

### <a name="additional-python-modules"></a>Extra Python-modules
Naast de standaard Python-bibliotheken, zijn enkele veelgebruikte python-bibliotheken zijn opgenomen:

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a>Uitzondering berichten
Op dit moment wordt een uitzondering in Python-code weergegeven als algemene hoekpunt mislukt. In de toekomst weergegeven de foutberichten van U-SQL-taak bericht met de Python-uitzondering.

### <a name="input-and-output-size-limitations"></a>Invoer en uitvoer beperkingen
Elke hoekpunt heeft een beperkte hoeveelheid van het geheugen toegewezen. Deze limiet is momenteel 6 GB voor een AU. Omdat de invoer- en DataFrames moet bestaan in het geheugen van de Python-code, kan de totale grootte voor de invoer en uitvoer niet langer zijn dan 6 GB.

## <a name="see-also"></a>Zie ook
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [U-SQL-scripts ontwikkelen met Data Lake-hulpmiddelen voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [U-SQL-vensterfuncties gebruiken voor Azure Data Lake Analytics-taken](data-lake-analytics-use-window-functions.md)
* [Azure Data Lake Tools voor Visual Studio Code gebruiken](data-lake-analytics-data-lake-tools-for-vscode.md)
