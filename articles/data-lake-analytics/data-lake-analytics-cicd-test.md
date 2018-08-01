---
title: Het testen van uw Azure Data Lake Analytics-code | Microsoft Docs
description: Informatie over het toevoegen van testcases voor U-SQL en uitgebreide C#-code voor Azure Data Lake Analytics.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: fa5c113541452a93c25adc7c14bdaa6994434c71
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365857"
---
# <a name="test-your-azure-data-lake-analytics-code"></a>Testen van uw Azure Data Lake Analytics-code

Azure Data Lake biedt de U-SQL-taal, combineert een verklarende SQL met imperatieve C# om gegevens te verwerken op elke schaal. In dit document leert u hoe u testcases voor U-SQL en uitgebreide C# UDO (door de gebruiker gedefinieerde operator)-code maken.

## <a name="test-u-sql-scripts"></a>Testen van U-SQL-scripts

De U-SQL-script is gecompileerd en geoptimaliseerd voor uitvoerbare code om uit te voeren op computers in de cloud of op uw lokale computer. De compilatie en optimalisatie van het proces wordt het gehele U-SQL-script behandeld als geheel. U kunt niet een traditionele 'eenheidstest' voor elke instructie. Echter met behulp van de U-SQL testen SDK en de lokale SDK worden uitgevoerd, kunt u doen scriptniveau tests.

### <a name="create-test-cases-for-u-sql-script"></a>Testcases voor U-SQL-script maken

Azure Data Lake Tools voor Visual Studio kunt u U-SQL-script-testcases maken.

1.  Met de rechtermuisknop op een U-SQL-script in Solution Explorer en selecteer vervolgens **eenheidstest maken**.
2.  Maak een nieuw project voor het testen of de Testscenario invoegen in een bestaand testproject.

    ![Data Lake Tools voor Visual Studio, maak een project voor U-SQL-testen](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project.png) 

    ![Data Lake Tools voor Visual Studio, maken een U-SQL-Testconfiguratie project](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project-configure.png) 

### <a name="manage-the-test-data-source"></a>Beheren van de gegevensbron testen

Wanneer u het testen van U-SQL-scripts, moet u de invoerbestanden testen. U kunt de testgegevens beheren door het configureren van **gegevensbron testen** in de U-SQL project eigenschappen. 

Wanneer u aanroepen de `Initialize()` interface in de U-SQL-test SDK, de hoofdmap van een tijdelijke lokale gegevens wordt gemaakt onder de werkmap van de testproject en alle bestanden en submappen (en bestanden in submappen) in de bronmap voor test-gegevens worden gekopieerd naar de tijdelijke lokale gegevens hoofdmap voordat u de testcases van de U-SQL-script uitvoeren. U kunt meer test data source-mappen toevoegen door op te splitsen test gegevens pad naar de map met een puntkomma.

![Data Lake Tools voor Visual Studio - project test-gegevensbron configureren](./media/data-lake-analytics-cicd-test/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-the-database-environment-for-testing"></a>De database-omgeving voor het testen van beheren

Als uw U-SQL-scripts gebruiken of query's met uitvoeren U-SQL-database-objecten (bijvoorbeeld bij het aanroepen van opgeslagen procedures) moet u de databaseomgeving initialiseren voordat U-SQL-testcases wordt uitgevoerd. De `Initialize()` interface in de U-SQL-test SDK helpt u bij het implementeren van alle databases waarnaar wordt verwezen door de U-SQL-project naar de hoofdmap van de tijdelijke lokale gegevens in de werkmap van de testproject. 

Meer informatie over [over het beheren van U-SQL-database projectverwijzingen voor een U-SQL project](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).

### <a name="verify-test-results"></a>Controleer of de resultaten

De `Run()` interface retourneert een resultaat van de uitvoering van taak. 0 wordt aangegeven dat geslaagd, en 1 betekent mislukt. U kunt ook een C# assert functies gebruiken om te controleren of de uitvoer. 

### <a name="run-test-cases-in-visual-studio"></a>Testcases uitvoeren in Visual Studio

Een project U-SQL-script te testen, is gebaseerd op de testframework van een C#-eenheid. Nadat u het project bouwt, kunt u alle testcases uitvoeren via **testen Explorer > afspeellijst**. U kunt ook met de rechtermuisknop op het bestand .cs en selecteer vervolgens **Tests uitvoeren**.

## <a name="test-c-udos"></a>C# UDO's testen

### <a name="create-test-cases-for-c-udos"></a>Testcases voor C# UDO's maken

U kunt een C#-eenheid testframework gebruiken voor het testen van uw C# UDO's (door de gebruiker gedefinieerde operators). Wanneer u test UDO's, moet u voorbereiden overeenkomt **IRowset** objecten als invoer.

Er zijn twee manieren om te maken van een object IRowset:

- Gegevens laden uit een bestand IRowset maken:

    ```csharp
    //Schema: "a:int, b:int"
    USqlColumn<int> col1 = new USqlColumn<int>("a");
    USqlColumn<int> col2 = new USqlColumn<int>("b");
    List<IColumn> columns = new List<IColumn> { col1, col2 };
    USqlSchema schema = new USqlSchema(columns);

    //Generate one row with default values
    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Get data from file
    IRowset rowset = UnitTestHelper.GetRowsetFromFile(@"processor.txt", schema, output.AsReadOnly(), discardAdditionalColumns: true, rowDelimiter: null, columnSeparator: '\t');
    ```

- Gebruik gegevens uit een gegevensverzameling IRowset maken:

    ```csharp
    //Schema: "a:int, b:int"
    USqlSchema schema = new USqlSchema(
        new USqlColumn<int>("a"),
        new USqlColumn<int>("b")
    );

    IUpdatableRow output = new USqlRow(schema, null).AsUpdatable();

    //Generate Rowset with specified values
    List<object[]> values = new List<object[]>{
        new object[2] { 2, 3 },
        new object[2] { 10, 20 }
    };

    IEnumerable<IRow> rows = UnitTestHelper.CreateRowsFromValues(schema, values);
    IRowset rowset = UnitTestHelper.GetRowsetFromCollection(rows, output.AsReadOnly());
    ```

### <a name="verify-test-results"></a>Controleer of de resultaten

Nadat u UDO functies aanroept, kunt u de resultaten in het schema en de rijenset waarde verificatie controleren met behulp van C# assert functies. U kunt de voorbeeldcode gebruiken in een U-SQL C# UDO eenheid test-voorbeeldproject via **bestand > Nieuw > Project** in Visual Studio.

### <a name="run-test-cases-in-visual-studio"></a>Testcases uitvoeren in Visual Studio

Nadat u de testproject maakt, kunt u echter alle testcases uitvoeren **testen Explorer > afspeellijst**, of met de rechtermuisknop op het bestand .cs en kies **Tests uitvoeren**.

## <a name="run-test-cases-in-visual-studio-team-service"></a>Testcases uitvoeren in Visual Studio Team Service

Beide **U-SQL-script-en Testprojecten** en **C# UDO en Testprojecten** C#-eenheid en Testprojecten overnemen. De [Visual Studio test taak](https://docs.microsoft.com/vsts/pipelines/test/getting-started-with-continuous-testing?view=vsts) in Visual Studio Team Services deze testcases kunt uitvoeren. 

### <a name="run-u-sql-test-cases-in-visual-studio-team-service"></a>U-SQL-testcases uitvoeren in Visual Studio Team Service

Voor het testen van een U-SQL, zorg ervoor dat u hebt geladen `CPPSDK` op uw computer van de build, en vervolgens pas de `CPPSDK` pad naar USqlScriptTestRunner (cppSdkFolderFullPath: @"").

**Wat is CPPSDK?**

CPPSDK is een pakket met Microsoft Visual C++-14 en Windows SDK 10.0.10240.0. Dit is de omgeving die nodig is door de runtime van U-SQL. U kunt dit pakket onder het Azure Data Lake Tools voor Visual Studio-installatiemap krijgen:

- Voor Visual Studio 2015 is onder `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK`
- Voor Visual Studio 2017 is onder `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\SDK\ScopeCppSDK`

**CPPSDK voorbereiden in de Visual Studio Team Services build-agent**

De meest voorkomende manier om voor te bereiden van de afhankelijkheid CPPSDK in Visual Studio Team Service is als volgt:

1.  ZIP-de map die de CPPSDK-bibliotheken omvat.
2.  Controleer in het ZIP-bestand in uw bronbeheersysteem. (Het ZIP-bestand zorgt ervoor dat u in alle bibliotheken onder de map CPPSDK controleren zodat bepaalde bestanden niet worden genegeerd door '.gitignore'.)   
3.  Pak het ZIP-bestand in de build-pijplijn.
4.  Punt `USqlScriptTestRunner` naar deze uitgepakte map op de build-machine.

### <a name="run-c-udo-test-cases-in-visual-studio-team-services"></a>C# UDO testcases uitvoeren in Visual Studio Team Services

Zorg dat u verwijst naar de volgende assembly's, die nodig zijn voor UDO's voor een C# UDO-test. Als u verwijst naar via [het Nuget-pakket Microsoft.Azure.DataLake.USQL.Interfaces](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.Interfaces/), zorg ervoor dat u een taak voor het herstellen van NuGet toevoegen in de build-pijplijn.

* Microsoft.Analytics.Interfaces
* Microsoft.Analytics.Types
* Microsoft.Analytics.UnitTest

## <a name="next-steps"></a>Volgende stappen

- [CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md)
- [Gebruik U-SQL database-project voor het ontwikkelen van U-SQL-database](data-lake-analytics-data-lake-tools-develop-usql-database.md)

