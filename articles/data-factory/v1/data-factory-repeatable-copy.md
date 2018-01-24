---
title: Herhaalbare kopie in Azure Data Factory | Microsoft Docs
description: "Informatie over het voorkomen van duplicaten, ondanks dat is meer dan één keer worden uitgevoerd in een segment dat gegevens worden gekopieerd."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 7a9023253b87d97d874dbcf1ade97bf32e1f0639
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Herhaalbare kopie in Azure Data Factory

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare leesbewerking van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, moet u herhaalbaarheid Houd in gedachten om te voorkomen dat ongewenste resultaten. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook een beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment opnieuw in beide gevallen wordt uitgevoerd, moet u ervoor zorgen dat dezelfde gegevens is gelezen ongeacht hoe vaak een segment wordt uitgevoerd.  
 
> [!NOTE]
> De volgende voorbeelden zijn voor Azure SQL, maar zijn van toepassing op een gegevensopslag die ondersteuning biedt voor rechthoekige gegevenssets. Wellicht hebt u om aan te passen de **type** van bron- en de **query** eigenschap (bijvoorbeeld: query in plaats van sqlReaderQuery) voor de gegevens opslaan.   

Normaal gesproken bij het lezen van relationele winkels, wilt u lezen alleen de gegevens die overeenkomt met dat segment. Een manier om dit te doen zou met behulp van de WindowStart en WindowEnd systeemvariabelen beschikbaar in Azure Data Factory zijn. Meer informatie over de variabelen en functies in Azure Data Factory hier in de [Azure Data Factory - functies en systeemvariabelen](data-factory-functions-variables.md) artikel. Voorbeeld: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Deze query leest de gegevens die in het segment duur bereik (WindowStart -> WindowEnd) uit de tabel MijnTabel valt. Opnieuw uitvoeren van dit segment zou ook altijd voor zorgen dat dezelfde gegevens gelezen is. 

In andere gevallen wilt lezen van de gehele tabel en de sqlReaderQuery kan als volgt gedefinieerd:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-to-sqlsink"></a>Herhaalbare schrijfbewerking naar SqlSink
Bij het kopiëren van gegevens naar **Azure SQL/SQL-Server** uit andere gegevensarchieven, moet u rekening moet houden herhaalbaarheid om te voorkomen dat ongewenste resultaten. 

Bij het kopiëren van gegevens naar Azure SQL/SQL Server-Database, de kopieeractiviteit worden gegevens toegevoegd aan de tabel sink standaard. Stel, u gegevens wilt kopiëren uit een CSV (door komma's gescheiden waarden)-bestand met twee records aan de volgende tabel in een Azure SQL/SQL Server-Database. Wanneer een segment wordt uitgevoerd, worden de twee records gekopieerd naar de SQL-tabel. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Stel dat u fouten gevonden in de bron-bestand en de hoeveelheid omlaag buis uit 2 tot 4 bijgewerkt. Als u het gegevenssegment handmatig opnieuw voor deze periode, vindt u twee nieuwe records die zijn toegevoegd aan Azure SQL/SQL Server-Database. In dit voorbeeld wordt ervan uitgegaan dat geen van de kolommen in de tabel de primary key-beperking heeft.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Om te voorkomen dat dit probleem, moet u UPSERT semantiek opgeven met behulp van een van de volgende twee mechanismen:

### <a name="mechanism-1-using-sqlwritercleanupscript"></a>Methode 1: met behulp van sqlWriterCleanupScript
U kunt de **sqlWriterCleanupScript** eigenschap opschonen van gegevens uit de tabel sink voordat de gegevens worden ingevoegd wanneer een segment wordt uitgevoerd. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Wanneer een segment wordt uitgevoerd, wordt het script voor opschoning eerst uitvoeren om gegevens die overeenkomt met het segment uit de SQL-tabel te verwijderen. De kopieeractiviteit voegt vervolgens de gegevens in de SQL-tabel. Als het segment opnieuw, de hoeveelheid wordt bijgewerkt naar wens.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Stel dat de platte was is verwijderd uit de oorspronkelijke csv. Vervolgens het segment opnieuw uit te voeren, zou het volgende resultaat opleveren: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

Het script voor opschoning van voor het verwijderen van de bijbehorende gegevens voor dat segment hebt uitgevoerd met de kopieerbewerking. Vervolgens wordt de invoer uit de csv lezen (die vervolgens bevat slechts één record) en het ingevoegd in de tabel. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>Methode 2: met behulp van sliceIdentifierColumnName
> [!IMPORTANT]
> SliceIdentifierColumnName wordt momenteel niet ondersteund voor Azure SQL Data Warehouse. 

De tweede methode om het bereiken van herhaalbaarheid is door een specifieke kolom (sliceIdentifierColumnName) in de doel-tabel. In deze kolom zou worden gebruikt door Azure Data Factory om te controleren of dat de bron- en doelserver gesynchroniseerd blijven. Deze methode werkt wanneer er flexibiliteit bij het definiëren van het schema van de SQL-tabel doel of wijzigen. 

In deze kolom wordt gebruikt door Azure Data Factory voor herhaalbaarheid doeleinden en in het proces Azure Data Factory maakt geen eventuele wijzigingen in het schema met de tabel. Manier voor het gebruik van deze benadering:

1. Een kolom van het type definieert **binair (32)** in de doel-SQL-tabel. Er mag geen beperkingen voor deze kolom. Laten we in deze kolom als AdfSliceIdentifier naam voor dit voorbeeld.


    Brontabel:

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL
    )
    ```

    Doeltabel: 

    ```sql
    CREATE TABLE [dbo].[Student](
       [Id] [varchar](32) NOT NULL,
       [Name] [nvarchar](256) NOT NULL,
       [AdfSliceIdentifier] [binary](32) NULL
    )
    ```

2. Gebruik deze als volgt in de kopieeractiviteit:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Azure Data Factory vult deze kolom aan de hand van de noodzaak om te controleren of dat de bron- en doelserver gesynchroniseerd blijven. De waarden van deze kolom mag niet worden gebruikt buiten deze context. 

Net als bij mechanisme 1, Kopieeractiviteit ruimt automatisch de gegevens voor het opgegeven segment van de doel-SQL-tabel. Vervolgens worden ingevoegd gegevens uit de gegevensbron in de doeltabel. 

## <a name="next-steps"></a>Volgende stappen
Bekijk de volgende connector artikelen die voor volledige JSON-voorbeelden: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)