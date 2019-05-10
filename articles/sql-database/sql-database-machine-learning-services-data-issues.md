---
title: Werken met R- en SQL-gegevenstypen en objecten
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Informatie over het werken met de gegevenstypen en objecten in R met Azure SQL Database met behulp van Machine Learning Services (preview), met inbegrip van veelvoorkomende problemen die kunnen optreden.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: 01d3af14963e92393d34a952bddc8097b7b08f18
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232611"
---
# <a name="work-with-r-and-sql-data-in-azure-sql-database-machine-learning-services-preview"></a>Werken met R- en SQL-gegevens in Azure SQL Database Machine Learning Services (preview)

In dit artikel komen enkele van de problemen die optreden kunnen tijdens het verplaatsen van gegevens tussen de R- en SQL-Database in [Machine Learning-Services (met R) in Azure SQL Database](sql-database-machine-learning-services-overview.md). De ervaring u via deze oefening krijgt bevat essentiële achtergrond bij het werken met gegevens in uw eigen script.

Veelvoorkomende problemen die kunnen optreden zijn onder andere:

- Gegevenstypen soms komen niet overeen
- Impliciete conversies kunnen plaatsvinden
- Cast en converteren naar bewerkingen zijn soms vereist
- R- en SQL gebruikmaken van verschillende objecten

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

- Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/) voordat u begint.

- Als u wilt uitvoeren met de voorbeeldcode in deze oefeningen, moet u eerst een Azure SQL database met Machine Learning-Services (met R) ingeschakeld hebben. Na de onboarding voor de openbare preview wordt Machine Learning voor u ingeschakeld voor uw bestaande of nieuwe database. Volg de stappen in [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

- Zorg ervoor dat u hebt de meest recente geïnstalleerd [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). U kunt R-scripts met behulp van het beheer van andere databases of hulpmiddelen voor query's uitvoeren, maar in deze snelstartgids gebruikt u SSMS.

## <a name="working-with-a-data-frame"></a>Werken met een gegevensframe

Wanneer het script resultaten van R met SQL retourneert, moet deze de gegevens op als retourneren een **data.frame**. Een ander type object dat u in de script - of die een lijst, van meerdere factoren, vector of binaire gegevens - moet worden geconverteerd naar een gegevensframe als u wilt uitvoeren als onderdeel van de resultaten van de opgeslagen procedure. Gelukkig zijn er meerdere R-functies voor de ondersteuning van andere objecten naar een gegevensframe wijzigen. U kunt zelfs een binaire model serialiseren en terug in een gegevensframe, u verderop in dit artikel doet.

Eerst gaan we experimenteren met enkele R basisobjecten - vectoren, matrices en lijsten - en zien hoe de conversie naar een gegevensframe verandert de uitvoer die wordt doorgegeven aan SQL.

Deze twee "Hallo wereld"-scripts in R. vergelijken De scripts zien er bijna identiek, maar de eerste retourneert één kolom van drie waarden, terwijl de tweede retourneert drie kolommen met één waarde elk.

**Voorbeeld 1**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
'
    , @input_data_1 = N'';
```

**Voorbeeld 2**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
    , @input_data_1 = N'';
```

Waarom zijn de resultaten waardoor er verschillende?

Het antwoord kan doorgaans worden gevonden met behulp van de R `str()` opdracht. Voeg de functie `str(object_name)` overal in uw R-script om de gegevens schema van de opgegeven R-object geretourneerd als een informatief bericht. U vindt de berichten in de **berichten** tabblad in SSMS.

Als u wilt weten waarom voorbeeld 1 en 2 van voorbeeld die verschillende resultaten hebt, voeg de regel `str(OutputDataSet)` aan het einde van de `@script` variabeledefinitie in elke instructie als volgt:

**Voorbeeld 1 met str functie toegevoegd**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

**Voorbeeld 2 met str functie toegevoegd**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(c("hello"), " ", c("world"));
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

Bekijk nu de tekst in **berichten** om te zien waarom de uitvoer is anders.

**Resultaten - voorbeeld 1**

```text
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Resultaten - voorbeeld 2**

```text
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Zoals u ziet, heeft een kleine wijziging in de R-syntaxis een grote invloed op het schema van de resultaten. Voor meer informatie de verschillen in de R-gegevenstypen worden beschreven in de details in de *gegevensstructuren* in sectie ['R geavanceerde' door Hadley Wickham](http://adv-r.had.co.nz).

Op dit moment alleen worden Let op: u moet de verwachte resultaten controleren wanneer bij R-objecten in gegevensframes toewijzen.

> [!TIP]
> Ook kunt u R-functies voor identiteit, zoals `is.matrix`, `is.vector`, om informatie over de structuur van de interne gegevens te retourneren.

## <a name="implicit-conversion-of-data-objects"></a>De impliciete conversie van objecten

Elke R-gegevens-object heeft eigen regels voor hoe waarden worden verwerkt als de twee objecten hebben die hetzelfde aantal dimensies in combinatie met andere objecten, of als een object voor heterogene gegevenstypen bevat.

Bijvoorbeeld, wordt ervan uitgegaan dat u wilt uitvoeren van de matrix vermenigvuldigen met R. U wilt een matrix met één kolom met de drie waarden vermenigvuldigen met een matrix met vier waarden en een matrix met 4 x 3 als gevolg hiervan wordt verwacht.

Maak eerst een kleine tabel met testgegevens.

```sql
CREATE TABLE RTestData (col1 INT NOT NULL)

INSERT INTO RTestData
VALUES (1);

INSERT INTO RTestData
VALUES (10);

INSERT INTO RTestData
VALUES (100);
GO
```

Voer nu het volgende script.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:15);
OutputDataSet <- as.data.frame(x %*% y);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col1] INT
            , [Col2] INT
            , [Col3] INT
            , Col4 INT
            ));
```

De kolom van drie waarden wordt op de achtergrond geconverteerd naar een matrix met één kolom. Omdat een matrix slechts een speciaal geval van een matrix in R, de matrix is `y` is impliciet gedwongen naar een matrix met één kolom te maken van de twee argumenten voldoen.

**Results**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1500|

Bedenk wat er gebeurt wanneer u de grootte van de matrix `y`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:14);
OutputDataSet <- as.data.frame(y %*% x);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS(([Col1] INT));
```

R wordt nu een enkelvoudige waarde geretourneerd als het resultaat.

**Results**
    
|Col1|
|---|
|1542|

Hoe komt dat? In dit geval, omdat de twee argumenten kunnen worden verwerkt als vectoren met dezelfde lengte, retourneert R de binnenste als een matrix.  Dit is het verwachte gedrag volgens de regels van lineair wiskundige. Het kan echter problemen veroorzaken als uw downstream-toepassing wordt verwacht dat het uitvoerschema nooit wijzigen.

## <a name="merge-or-multiply-columns-of-different-length"></a>Samenvoegen of kolommen van verschillende lengtes vermenigvuldigen

R biedt hoge mate van flexibiliteit voor het werken met vectoren van verschillende grootte en voor het combineren van deze structuren voor kolom-achtige in gegevensframes. Lijsten van vectoren kunnen er uitzien als een tabel, maar ze niet alle regels die databasetabellen bepalen volgen.

Bijvoorbeeld het volgende script definieert een numerieke matrix van de lengte van 6 en slaat ze op in de R-variabele `df1`. De numerieke matrix vervolgens wordt gecombineerd met de gehele getallen van de RTestData-tabel (hierboven gemaakte) die bevat drie (3) waarden, zodat een nieuwe gegevensframe `df2`.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
df1 <- as.data.frame( array(1:6) );
df2 <- as.data.frame( c( InputDataSet , df1 ));
OutputDataSet <- df2;
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col2] INT NOT NULL
            , [Col3] INT NOT NULL
            ));
```

Als u wilt de gegevensframe invullen, R wordt herhaald de elementen die is opgehaald uit RTestData zo vaak als nodig is zodat deze overeenkomt met het aantal elementen in de matrix `df1`.

**Results**
    
|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Houd er rekening mee dat een gegevensframe alleen ziet als een tabel eruit, maar een lijst van vectoren is.

## <a name="cast-or-convert-sql-data"></a>CAST of convert SQL-gegevens

R- en SQL gebruik niet de dezelfde gegevenstypen, zodat wanneer u een query in SQL uitvoert voor het ophalen van gegevens en die vervolgens doorgeven aan de R-runtime, sommige typen van de impliciete conversie meestal vindt plaats. Een andere set conversies vindt plaats als u gegevens van R naar SQL terugkeert.

- SQL duwt de gegevens van de query naar de R-proces en geconverteerd naar een interne weergave voor grotere efficiëntie.
- De R-runtime de gegevens worden geladen in een variabele data.frame en voert een eigen bewerkingen op de gegevens.
- De database-engine retourneert de gegevens naar SQL met behulp van een beveiligde verbinding met interne en geeft de gegevens in termen van SQL-gegevenstypen.
- U de gegevens door verbinding te maken met SQL met behulp van een client of het netwerk bibliotheek die kan SQL-query's uitgeven en in tabelvorm gegevenssets worden verwerkt. Deze clienttoepassing kan mogelijk de gegevens op andere manieren beïnvloeden.

Als u wilt zien hoe dit werkt, kunt u een query zoals deze uitvoeren op de [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) datawarehouse. In deze weergave retourneert verkoopgegevens gebruikt bij het maken van prognoses.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
    , CAST(ModelRegion AS VARCHAR(50)) AS ProductSeries
    , Amount
FROM [AdventureWorksDW].[dbo].[vTimeSeries]
WHERE [ModelRegion] = 'M200 Europe'
ORDER BY ReportingDate ASC
```

> [!NOTE]
> U kunt een willekeurige versie van AdventureWorks gebruiken of maken van een andere query met behulp van een database van uw eigen. Het is om te proberen voor het afhandelen van sommige gegevens die tekst, datum/tijd en numerieke waarden bevat.

Probeer nu met behulp van deze query als invoer voor de opgeslagen procedure.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
str(InputDataSet);
OutputDataSet <- InputDataSet;
'
    , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Als u een fout optreedt, moet u mogelijk enkele wijzigingen aanbrengen in de tekst van de query. Bijvoorbeeld, het predicaat tekenreeks in de WHERE-component moet worden omsloten door twee sets met enkele aanhalingstekens.

Nadat u de query werkt, bekijk de resultaten van de `str` functie om te zien hoe R omgaat met de ingevoerde gegevens.

**Results**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- De datum/tijd-kolom is verwerkt met behulp van het gegevenstype R **POSIXct**.
- De kolom "ProductSeries" is geïdentificeerd als een **factor**, wat betekent dat een categorische variabele. Tekenreekswaarden worden behandeld als factoren standaard. Als u een tekenreeks aan R doorgeeft, wordt deze geconverteerd naar een geheel getal voor intern gebruik en vervolgens terug naar de tekenreeks in de uitvoer is toegewezen.

## <a name="summary"></a>Samenvatting

In ook deze korte voorbeelden ziet u de noodzaak om te controleren of de gevolgen van de gegevensconversie bij het doorgeven van SQL query's als invoer. Omdat sommige SQL-gegevenstypen worden niet ondersteund door R, u kunt de volgende manieren om fouten te voorkomen:

- Uw gegevens vooraf te testen en controleer of kolommen of waarden in uw schema die mogelijk een probleem bij het doorgegeven aan R-code.
- Kolommen in de bron van de invoergegevens afzonderlijk opgeven in plaats van dat `SELECT *`, en weet hoe elke kolom worden verwerkt.
- Voer expliciete webcasts zo nodig bij het voorbereiden van uw invoergegevens, om te voorkomen verrassingen.
- Vermijd het doorgeven van kolommen met gegevens (zoals GUID's of ROWGUID) die fouten veroorzaken en die niet zijn handig voor het maken van modellering.

Zie voor meer informatie over ondersteunde en niet-ondersteunde R-gegevenstypen [R-bibliotheken en gegevenstypen](/sql/advanced-analytics/r/r-libraries-and-data-types).
