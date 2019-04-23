---
title: Eenvoudige R-scripts maken en uitvoeren
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Eenvoudige R-scripts uitvoeren in Azure SQL Database Machine Learning-Services (preview).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ms.openlocfilehash: ada09959391c551a9eff4d96b186be29c1e3b7a8
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60013258"
---
# <a name="create-and-run-simple-r-scripts-in-azure-sql-database-machine-learning-services-preview"></a>Maken en eenvoudige R-scripts uitvoeren in Azure SQL Database Machine Learning-Services (preview)

In deze Quick Start, moet u maken en uitvoeren van een set eenvoudige R-scripts met behulp van de openbare preview van [Machine Learning-Services (met R) in Azure SQL Database](sql-database-machine-learning-services-overview.md). Leert u hoe u een goed ingedeelde R-script in de opgeslagen procedure inpakt [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) en voer het script uit in een SQL-database.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

- Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/) voordat u begint.

- Als u wilt uitvoeren met de voorbeeldcode in deze oefeningen, moet u eerst een Azure SQL database met Machine Learning-Services (met R) ingeschakeld hebben. Na de onboarding voor de openbare preview wordt Machine Learning voor u ingeschakeld voor uw bestaande of nieuwe database. Volg de stappen in [Meld u aan voor de preview-versie](sql-database-machine-learning-services-overview.md#signup).

- Zorg ervoor dat u hebt de meest recente geïnstalleerd [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS). U kunt R-scripts met behulp van het beheer van andere databases of hulpmiddelen voor query's uitvoeren, maar in deze snelstartgids gebruikt u SSMS.

- Deze snelstart moet u een firewallregel op serverniveau configureren. Zie voor informatie over hoe u dit doet, [maken-firewallregel op serverniveau](sql-database-server-level-firewall-rule.md).

## <a name="run-a-simple-script"></a>Een eenvoudig script uitvoeren

Als u wilt een R-script uitvoeren, u zult geven deze als een argument naar de procedure in het systeem opgeslagen [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

In de volgende stappen, zult u dit R-voorbeeldscript uitvoeren in uw SQL-database:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Open **SQL Server Management Studio** en maak verbinding met de SQL-database.

   Als u hulp bij het maken van een verbinding nodig hebt, raadpleegt u [Quick Start: SQL Server Management Studio gebruiken om verbinding te maken en query's uit te voeren op een Azure SQL database](sql-database-connect-query-ssms.md).

1. Het volledige R-script uit te geven de [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) opgeslagen procedure.

   Het script wordt doorgegeven via de `@script` argument. Alles in de `@script` argument moet een geldige R-code.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

   Als er fouten optreden, kan dit zijn omdat de openbare preview van Machine Learning Services (met R) niet is ingeschakeld voor uw SQL-database. Zie [vereisten](#prerequisites) hierboven.

   > [!NOTE]
   > Als u een beheerder bent, kunt u automatisch externe code uitvoeren. U kunt de machtiging verlenen aan andere gebruikers met de opdracht:
   <br>**TOEKENNEN uitvoeren van een externe SCRIPT aan**  *\<gebruikersnaam\>*.

2. Het juiste resultaat wordt berekend en de R- `print` functie retourneert het resultaat dat de **berichten** venster.

   Dit ziet er ongeveer als volgt.

    **Results**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Een Hello World-script uitvoeren

Er is een typisch voorbeeld-script die alleen de tekenreeks "Hallo wereld levert". Voer de volgende opdracht uit.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Invoer voor deze opgeslagen procedure zijn onder andere:

| | |
|-|-|
|*@language* | definieert de uitbreiding van de taal om aan te roepen, in dit geval R |
|*@script* | Hiermee definieert u de opdrachten die is doorgegeven aan de R-runtime. Dit argument moet u uw hele R-script tussen als Unicode-tekst. U kunt ook de tekst toevoegen aan een variabele van het type **nvarchar** en roep vervolgens de variabele |
|*@input_data_1* | gegevens die zijn geretourneerd door de query doorgegeven aan de R-runtime, die als resultaat de gegevens naar SQL Server als een gegevensframe |
|MET RESULTAATSETS | component definieert het schema van de geretourneerde tabel voor SQL Server, "Hallo wereld" Als de naam van de kolom toe te voegen **int** voor het gegevenstype |

De opdracht levert de volgende tekst:

| Hallo wereld |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Gebruik invoer en uitvoer

Standaard [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) accepteert van een enkele gegevensset als invoer, die doorgaans die u in de vorm van een geldige SQL-query opgeven. Vervolgens wordt een enkele R-gegevensframe als uitvoer.

Er kan maar één invoergegevensset worden doorgegeven als parameter, en u kunt slechts één gegevensset retourneren. U kunt vanuit de R-code wel andere gegevenssets aanroepen en u kunt, naast de gegevensset, ook andere typen uitvoer retourneren. U kunt ook het uitvoertrefwoord toevoegen aan elke willekeurige parameter om deze te laten retourneren met de resultaten.

Nu gaan we gebruiken de standaard-invoer en uitvoer variabelen van [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql): **InputDataSet** en **OutputDataSet**.

1. Maak een kleine tabel met testgegevens.

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

1. Gebruik de `SELECT` instructie query uitvoeren op de tabel.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Results**

    ![Inhoud van de RTestData-tabel](./media/sql-database-connect-query-r/select-rtestdata.png)

1. Voer het volgende R-script. Het ophalen van de gegevens uit de tabel met de `SELECT` -instructie wordt doorgegeven via de R-runtime en de gegevens retourneert als een gegevensframe. De `WITH RESULT SETS` component definieert het schema van de geretourneerde tabel voor SQL Database, de naam van de kolom toegevoegd *NewColName*.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Results**

    ![Uitvoer van R-script waarmee gegevens uit een tabel worden geretourneerd](./media/sql-database-connect-query-r/r-output-rtestdata.png)

1. Nu gaan we wijzigen de namen van de variabelen voor de invoer en uitvoer. De standaard invoer en uitvoernamen zijn **InputDataSet** en **OutputDataSet**, dit script wijzigt de namen **SQL_in** en **SQL_out**:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Houd er rekening mee dat R hoofdlettergevoelig is. De invoer- en variabelen die worden gebruikt in de R-script (**SQL_out**, **SQL_in**) moeten overeenkomen met de waarden die zijn gedefinieerd met `@input_data_1_name` en `@output_data_1_name`, met inbegrip van de aanvraag.

   > [!TIP]
   > Er kan maar één invoergegevensset worden doorgegeven als parameter, en u kunt slechts één gegevensset retourneren. U kunt vanuit de R-code wel andere gegevenssets aanroepen en u kunt, naast de gegevensset, ook andere typen uitvoer retourneren. U kunt ook het uitvoertrefwoord toevoegen aan elke willekeurige parameter om deze te laten retourneren met de resultaten.

1. Kunt u ook waarden alleen met behulp van het R-script zonder invoer gegevens genereren (`@input_data_1` is ingesteld op leeg).

   Het volgende script voert de tekst "Hallo" en "wereld".

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Results**

    ![Resultaten doorzoeken met @script als invoer](./media/sql-database-connect-query-r/r-data-generated-output.png)

## <a name="check-r-version"></a>R-versie controleren

Als u wilt zien welke versie van R is geïnstalleerd in uw SQL-database, moet u het volgende script uitvoeren.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

Met de R-functie `print` wordt de versie geretourneerd in het **Berichtvenster**. In de onderstaande voorbeelduitvoer ziet u dat SQL-Database in dit geval R versie 3.4.4 geïnstalleerd heeft.

**Results**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>R-pakketten vermelden

Microsoft biedt een aantal R-pakketten waarin Machine Learning Services vooraf zijn geïnstalleerd in de SQL-database.

Een overzicht van welke R pakketten zijn geïnstalleerd, met inbegrip van versie, afhankelijkheden, licentie en informatie over het pad van de bibliotheek, moet u het volgende script uitvoeren.

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

De uitvoer is van `installed.packages()` in R en wordt geretourneerd als gevolg hiervan ingesteld.

**Results**

![Geïnstalleerde pakketten in R](./media/sql-database-connect-query-r/r-installed-packages.png)

## <a name="next-steps"></a>Volgende stappen

Voor het maken van een model voor machine learning met R in SQL-Database, moet u deze Quick Start volgen:

> [!div class="nextstepaction"]
> [Maken en train een Voorspellend model in R met Azure SQL Database Machine Learning Services (preview)](sql-database-quickstart-r-train-score-model.md)

Zie de onderstaande artikelen over Machine Learning Services voor meer informatie. Sommige van deze artikelen zijn bestemd voor SQL Server, maar de meeste informatie is ook van toepassing op Machine Learning Services (met R) in Azure SQL Database.

- [Azure SQL Database Machine Learning Services (met R)](sql-database-machine-learning-services-overview.md)
- [SQL Server Machine Learning-services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning)
- [Zelfstudie: Informatie over in-database analyse met behulp van R in SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-in-database-r-for-sql-developers)
- [End-to-end data science walkthrough for R and SQL Server (Overzicht van end-to-end-informatiewetenschap voor R en SQL Server)](https://docs.microsoft.com/sql/advanced-analytics/tutorials/walkthrough-data-science-end-to-end-walkthrough)
- [Zelfstudie: RevoScaleR R-functies gebruiken met SQL Server-gegevens](https://docs.microsoft.com/sql/advanced-analytics/tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages)
