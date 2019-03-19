---
title: Herhaalbare kopiëren in Azure Data Factory | Microsoft Docs
description: Leer hoe u om te voorkomen dat dubbele waarden, zelfs als een segment dat gegevens worden gekopieerd meer dan één keer wordt uitgevoerd.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: b15dcd9ae5f2f1668db3925b659625497eca491f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58079755"
---
# <a name="repeatable-copy-in-azure-data-factory"></a>Herhaalbare kopiëren in Azure Data Factory

## <a name="repeatable-read-from-relational-sources"></a>Herhaalbare lezen van relationele bronnen
Bij het kopiëren van gegevens van relationele gegevens worden opgeslagen, houd er rekening mee om te voorkomen dat ongewenste resultaten herhaalbaarheid. In Azure Data Factory, kunt u een segment handmatig opnieuw. U kunt ook beleid voor opnieuw proberen voor een gegevensset configureren zodat een segment wordt opnieuw uitgevoerd wanneer een fout optreedt. Wanneer een segment wordt opnieuw uitgevoerd in beide gevallen, moet u om ervoor te zorgen dat de dezelfde gegevens worden gelezen ongeacht hoe vaak een segment wordt uitgevoerd.  
 
> [!NOTE]
> De volgende voorbeelden voor Azure SQL maar zijn van toepassing op elk gegevensarchief die ondersteuning biedt voor rechthoekige gegevenssets. Mogelijk moet u pas de **type** van de bron en de **query** eigenschap (bijvoorbeeld: query in plaats van sqlReaderQuery) voor de gegevens opslaat.   

Normaal gesproken bij het lezen van relationele winkels, wilt u lezen alleen de gegevens voor dat segment. Een manier om dit te doen is met behulp van de WindowStart en WindowEnd systeemvariabelen beschikbaar in Azure Data Factory. Meer informatie over de variabelen en functies in Azure Data Factory hier in de [Azure Data Factory - functies en systeemvariabelen](data-factory-functions-variables.md) artikel. Voorbeeld: 

```json
"source": {
    "type": "SqlSource",
    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
},
```
Deze query leest de gegevens die in het segment duur bereik (WindowStart -> WindowEnd) uit de tabel MyTable valt. Opnieuw uitvoeren van dit segment zou ook altijd voor zorgen dat de dezelfde gegevens worden gelezen. 

In andere gevallen mogelijk wilt lezen van de hele tabel en de sqlReaderQuery kan als volgt gedefinieerd:

```json
"source": 
{            
    "type": "SqlSource",
    "sqlReaderQuery": "select * from MyTable"
},
```

## <a name="repeatable-write-to-sqlsink"></a>Herhaalbare schrijven naar SqlSink
Bij het kopiëren van gegevens naar **Azure SQL/SQL Server** uit andere gegevensarchieven, moet u herhaalbaarheid Houd er rekening mee om te voorkomen dat ongewenste resultaten. 

Het kopiëren van gegevens met Azure SQL/SQL Server-Database, de kopieeractiviteit worden gegevens toegevoegd aan de sink-tabel standaard. Stel dat u bij het kopiëren van gegevens uit een CSV (door komma's gescheiden waarden)-bestand met twee records aan de volgende tabel in een Azure SQL/SQL Server-Database. Wanneer een segment wordt uitgevoerd, worden de twee records worden gekopieerd naar de SQL-tabel. 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    2            2015-05-01 00:00:00
```

Stel dat u in het bronbestand zijn fouten gevonden en de hoeveelheid omlaag buis uit 2 tot en met 4 bijgewerkt. Als u het gegevenssegment handmatig opnieuw voor deze periode, vindt u twee nieuwe records die zijn toegevoegd aan Azure SQL/SQL Server-Database. In dit voorbeeld wordt ervan uitgegaan dat geen van de kolommen in de tabel de primary key-beperking heeft.

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
U kunt de **sqlWriterCleanupScript** eigenschap voor het opschonen van gegevens uit de sink-tabel voordat u de gegevens invoegen wanneer een segment wordt uitgevoerd. 

```json
"sink":  
{ 
  "type": "SqlSink", 
  "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM table WHERE ModifiedDate >= \\'{0:yyyy-MM-dd HH:mm}\\' AND ModifiedDate < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
}
```

Wanneer een segment wordt uitgevoerd, wordt het script voor opschoning eerst uitvoeren om gegevens die overeenkomt met het segment de status van de SQL-tabel te verwijderen. De kopieeractiviteit voegt vervolgens de gegevens in de SQL-tabel. Als het segment opnieuw wordt uitgevoerd, de hoeveelheid wordt bijgewerkt naar wens.

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
6    Flat Washer    3            2015-05-01 00:00:00
7     Down Tube    4            2015-05-01 00:00:00
```

Stel dat de platte was-record wordt verwijderd uit de oorspronkelijke CSV-bestand. Vervolgens opnieuw uit te voeren van het segment de status geeft als resultaat het volgende resultaat: 

```
ID    Product        Quantity    ModifiedDate
...    ...            ...            ...
7     Down Tube    4            2015-05-01 00:00:00
```

De kopieeractiviteit is het script voor opschoning als u wilt verwijderen van de bijbehorende gegevens voor dat segment. Vervolgens wordt de invoer uit het CSV-bestand lezen (die vervolgens bevat slechts één record) en in de tabel ingevoegd. 

### <a name="mechanism-2-using-sliceidentifiercolumnname"></a>Methode 2: met behulp van sliceIdentifierColumnName
> [!IMPORTANT]
> SliceIdentifierColumnName wordt momenteel niet ondersteund voor Azure SQL Data Warehouse. 

De tweede mechanisme voor het bereiken van herhaalbaarheid is door een specifieke kolom (sliceIdentifierColumnName) in de tabel van het doel. Deze kolom kan worden gebruikt door Azure Data Factory om te controleren of dat de bron- en gesynchroniseerd blijven. Deze methode werkt wanneer er flexibiliteit bij het wijzigen van of het doelschema van de SQL-tabel te definiëren. 

Deze kolom wordt gebruikt door Azure Data Factory voor herhaalbaarheid en in de Azure Data Factory maakt geen eventuele wijzigingen in het schema met de tabel. Manier voor het gebruik van deze benadering:

1. Een kolom van het type definieert **binair (32)** in de doel-SQL-tabel. Er mag geen beperkingen voor deze kolom. We noemen deze kolom als AdfSliceIdentifier voor dit voorbeeld.


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

1. Gebruik deze als volgt in de kopieeractiviteit:
   
    ```json
    "sink":  
    { 
   
        "type": "SqlSink", 
        "sliceIdentifierColumnName": "AdfSliceIdentifier"
    }
    ```

Azure Data Factory vult deze kolom aan de hand van de noodzaak om te controleren of dat de bron- en gesynchroniseerd blijven. De waarden van deze kolom mag niet worden gebruikt buiten deze context. 

Net als bij mechanisme 1, Copy Activity automatisch opschonen van de gegevens voor het opgegeven segment van de doel-SQL-tabel. Vervolgens voegt naar de doeltabel gegevens uit de gegevensbron in. 

## <a name="next-steps"></a>Volgende stappen
Lees de volgende connector artikelen die voor volledige JSON-voorbeelden: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
