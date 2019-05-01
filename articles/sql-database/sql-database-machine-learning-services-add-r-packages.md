---
title: Een R-pakket toevoegen aan Azure SQL Database Machine Learning Services (preview)
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In dit artikel wordt uitgelegd hoe u een R-pakket dat niet is al geïnstalleerd in Azure SQL Database Machine Learning-Services (preview) installeren.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: tutorial
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/29/2019
ms.openlocfilehash: 4e7145570cbc906ea540c9d8f95f6c3cbde1c610
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927098"
---
# <a name="add-an-r-package-to-azure-sql-database-machine-learning-services-preview"></a>Een R-pakket toevoegen aan Azure SQL Database Machine Learning Services (preview)

In dit artikel wordt uitgelegd hoe u een R-pakket toevoegen aan Azure SQL Database Machine Learning Services (preview).

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Vereisten

- Installeer [R](https://www.r-project.org) en [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) op uw lokale computer. R is beschikbaar voor Windows, MacOS en Linux. In dit artikel wordt ervan uitgegaan dat u gebruikmaakt van Windows.

- Dit artikel bevat een voorbeeld van het gebruik van [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) om uit te voeren van een R-script in Azure SQL Database. U kunt R-scripts met behulp van het beheer van andere databases of hulpmiddelen voor query's uitvoeren, maar in dit voorbeeld wordt ervan uitgegaan dat Azure Data Studio of SSMS.
   
> [!NOTE]
> U kunt een pakket niet installeren door het uitvoeren van een R-script met **sp_execute_external_script** in Azure Data Studio of in SSMS. U kunt alleen installeren en verwijderen van pakketten met behulp van de opdrachtregel voor R en RStudio, zoals beschreven in dit artikel. Wanneer het pakket is geïnstalleerd, kunt u toegang tot de pakket-functies in een R-script met **sp_execute_external_script**.

## <a name="list-r-packages"></a>R-pakketten vermelden

Microsoft biedt een aantal R-pakketten vooraf geïnstalleerd met Machine Learning-Services in uw Azure SQL database.
U ziet een lijst met geïnstalleerde R-pakketten door het uitvoeren van de volgende opdracht uit in Azure Data Studio of in SSMS.

1. Open Azure Data Studio of SSMS en maak verbinding met uw Azure SQL Database.

1. Voer de volgende opdracht uit:

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License")]);'
WITH RESULT SETS((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            ));
```

De uitvoer moet eruitzien als de volgende.

**Results**

![Geïnstalleerde pakketten in R](./media/sql-database-machine-learning-services-add-r-packages/r-installed-packages.png)

## <a name="add-a-package-with-sqlmlutils"></a>Een pakket met sqlmlutils toevoegen

Als u een pakket gebruiken dat al in uw Azure SQL Database is niet geïnstalleerd wilt, kunt u installeren met behulp van [sqlmlutils](https://github.com/Microsoft/sqlmlutils). **sqlmlutils** is een pakket dat is ontworpen om u te helpen gebruikers communiceren met de SQL-databases (SQL Server en Azure SQL Database) en uitvoeren van R of Python-code in SQL van een R of Python-client. Op dit moment alleen de R-versie van **sqlmlutils** wordt ondersteund in Azure SQL Database.

In het volgende voorbeeld, installeert u de **[lijm](https://cran.r-project.org/web/packages/glue/)** pakket kunt opmaken en interpoleren tekenreeksen. Installeren van deze stappen **sqlmlutils** en **RODBCext** (een vereiste voor **sqlmlutils**), en voeg de **lijm** pakket.

### <a name="install-sqlmlutils"></a>Installeer **sqlmlutils**

1. Download de meest recente **sqlmlutils** zip-bestand van https://github.com/Microsoft/sqlmlutils/tree/master/R/dist naar uw lokale computer. U hoeft niet te Decomprimeer het bestand.

1. Open een **opdrachtprompt** en voer de volgende opdrachten voor het installeren van **RODBCext** en **sqlmlutils** op uw lokale computer. Vervang het volledige pad naar de **sqlmlutils** zip-bestand u gedownloade (in het voorbeeld wordt ervan uitgegaan dat het bestand is in de map documenten).
    
    ```console
    R -e "install.packages('RODBCext', repos='https://cran.microsoft.com')"
    R CMD INSTALL %UserProfile%\Documents\sqlmlutils_0.5.1.zip
    ```

    De uitvoer u ziet moet er ongeveer als volgt.

    ```text
    In R CMD INSTALL
    * installing to library 'C:/Users/<username>/Documents/R/win-library/3.5'
    package sqlmlutils successfully unpacked and MD5 sums checked
    ```

    > [!TIP]
    > Als u de fout optreedt, '' R' wordt niet herkend als een interne of externe opdracht, programma of batch-bestand', betekent dit waarschijnlijk dat het pad naar R.exe niet is opgenomen uw **pad** omgevingsvariabele op Windows. U kunt het pad toevoegen aan de omgevingsvariabele of gaat u naar de map in de opdrachtprompt (bijvoorbeeld `cd C:\Program Files\R\R-3.5.3\bin`) en voer de opdracht vervolgens opnieuw uit.

### <a name="add-the-package"></a>Het pakket toevoegen

1. RStudio openen en maak een nieuwe **R-Script** bestand. 

1. Gebruik de volgende R-code voor het installeren van de **lijm** met behulp van het pakket **sqlmlutils**. Vervangen door uw eigen Azure SQL Database-verbindingsgegevens.

    ```R
    library(sqlmlutils)
    connection <- connectionInfo(
    server= "yourserver.database.windows.net",
    database = "yourdatabase",
    uid = "yoursqluser",
    pwd = "yoursqlpassword")
    
    sql_install.packages(connectionString = connection, pkgs = "glue", verbose = TRUE, scope = "PUBLIC")
    ```

    > [!TIP]
    > De **bereik** kan **openbare** of **persoonlijke**. Een openbaar bereik is handig voor de databasebeheerder om pakketten te installeren die alle gebruikers kunnen gebruiken. Persoonlijke scope maakt het pakket beschikbaar alleen voor de gebruiker die deze installeert. Als u het bereik niet opgeeft, is het standaardbereik **PRIVATE**.

### <a name="verify-the-package"></a>Controleer of het pakket

Controleer de **lijm** pakket is geïnstalleerd door het uitvoeren van de volgende R-script in RStudio. Gebruik dezelfde **verbinding** u hebt gedefinieerd in de vorige stap.

```R
r<-sql_installed.packages(connectionString = connection, fields=c("Package", "Version", "Depends", "License"))
View(r)
```

**Results**

![Inhoud van de RTestData-tabel](./media/sql-database-machine-learning-services-add-r-packages/r-verify-package-install.png)

### <a name="use-the-package"></a>Het pakket wordt gebruikt

Wanneer het pakket is geïnstalleerd, kunt u deze in een R-script via **sp_execute_external_script**.

1. Open Azure Data Studio of SSMS en maak verbinding met uw Azure SQL Database.

1. Voer de volgende opdracht uit:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    library(glue)
    
    name <- "Fred"
    age <- 50
    anniversary <- as.Date("2020-06-14")
    text <- glue(''My name is {name}, '',
    ''my age next year is {age + 1}, '',
    ''my anniversary is {format(anniversary, "%A, %B %d, %Y")}.'')
    
    print(text)
    ';
    ```

    U ziet het volgende resultaat in de **berichten** tabblad.

    **Results**

    ```text
    My name is Fred, my age next year is 51, my anniversary is Sunday, June 14, 2020.
    ```

### <a name="remove-the-package"></a>Verwijder het pakket

Als u wilt het pakket te verwijderen, moet u de volgende R-script uitvoeren in RStudio. Gebruik dezelfde **verbinding** u eerder hebt gedefinieerd.

```R
sql_remove.packages(connectionString = connection, pkgs = "glue", scope = "PUBLIC")
```

> [!TIP]
> Een andere manier voor het installeren van een R-pakket aan uw Azure SQL database is voor het uploaden van het R-pakket vanuit een byte stream met de **CREATE EXTERNAL LIBRARY** T-SQL-instructie. Zie [maken van een bibliotheek van een bytestream](/sql/t-sql/statements/create-external-library-transact-sql#c-create-a-library-from-a-byte-stream) in de [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) verwijzen naar documentatie.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over Azure SQL Database Machine Learning-Services met R (preview).

- [Azure SQL Database Machine Learning-Services met R (preview)](sql-database-machine-learning-services-overview.md)
- [Schrijven van geavanceerde R-functies in Azure SQL Database met behulp van Machine Learning-Services (preview)](sql-database-machine-learning-services-functions.md)
- [Werken met R- en SQL-gegevens in Azure SQL Database Machine Learning Services (preview)](sql-database-machine-learning-services-data-issues.md)