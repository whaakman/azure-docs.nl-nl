---
title: Het testen van uw Azure Data Lake Analytics-code | Microsoft Docs
description: Informatie over het toevoegen van testscenario's voor uw U-SQL en uitgebreide C#-code voor Azure Data Lake Analytics.
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
ms.openlocfilehash: bfb314348caf1d5bf83c940c0bce79e87d6d2593
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889539"
---
# <a name="how-to-test-your-azure-data-lake-analytics-code"></a>Het testen van uw Azure Data Lake Analytics-code

Azure Data Lake biedt U-SQL als een taal die verklarende SQL met imperatieve C# om gegevens te verwerken op elke schaal combineert. In dit document leert u over het maken van testscenario's voor uw U-SQL en uitgebreide C# UDO (door de gebruiker gedefinieerde Operator)-code.

## <a name="test-u-sql-scripts"></a>Testen van U-SQL-scripts

De U-SQL-script is gecompileerd en geoptimaliseerd voor uitvoerbare code en uitgevoerd op computers in de cloud of uw lokale computer. De compilatie en optimalisatie van het proces wordt het gehele U-SQL-script behandeld als geheel uit te voeren. U kunt niet traditionele 'eenheidstest' voor elke instructie. Echter met behulp van U-SQL-test SDK en lokaal uitvoeren van de SDK, kunt u doen script niveau tests.

### <a name="create-test-cases-for-u-sql-script"></a>Testcases voor U-SQL-script maken

Azure Data Lake Tools voor Visual Studio biedt goede ervaring voor het maken van U-SQL-script Testscenario's.

1.  Met de rechtermuisknop op een U-SQL-script in Solution Explorer en kies **eenheidstest maken**.
2.  Configureren voor het maken van een nieuw project te testen of invoegen van de test-aanvraag aan een bestaand testproject.

    ![Data Lake Tools voor Visual Studio maken u-sql-test-project](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project.png) 

    ![Data Lake Tools voor Visual Studio maakt de Testconfiguratie project u-sql](./media/data-lake-analytics-cicd-test/data-lake-tools-create-usql-test-project-configure.png) 

### <a name="manage-test-data-source"></a>Test-gegevensbron beheren

Wanneer U-SQL-scripts te testen, test-invoerbestanden nodig zijn. U kunt deze beheren testgegevens door het configureren van **gegevensbron testen** in U-SQL-projecteigenschap. Wanneer u aanroepen de `Initialize()` interface in U-SQL-test SDK, een tijdelijke lokale gegevens hoofdmap wordt gemaakt onder de werkmap van het testproject en alle bestanden en submappen (en bestanden in submappen) in de bronmap voor test-gegevens worden gekopieerd naar de tijdelijke lokale gegevens hoofdmap voordat testcases van U-SQL-script wordt uitgevoerd. U kunt meer test data source-mappen toevoegen door gespuwd pad naar map van test-gegevens met door puntkomma's.

![Data Lake Tools voor Visual Studio configureren project test-gegevensbron](./media/data-lake-analytics-cicd-test/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-database-environment-for-test"></a>Databaseomgeving voor testen kunnen beheren

Als uw U-SQL-scripts gebruiken of een query met U-SQL database-objecten, bijvoorbeeld, aanroepen van opgeslagen procedures, moet u de databaseomgeving initialiseren voordat U-SQL-testcases wordt uitgevoerd. De `Initialize()` interface in U-SQL-test SDK helpt u bij het implementeren van alle databases waarnaar wordt verwezen door de U-SQL-project naar de tijdelijke lokale gegevens hoofdmap in de werkmap van het testproject. 

Meer informatie over [projecten hoe u U-SQL-database kunt beheren met verwijzingen voor U-SQL project](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).

### <a name="verify-test-results"></a>Controleer of de resultaten

De `Run()` interface het resultaat van taak kan worden uitgevoerd, 0 betekent dat slagen en 1 betekent dat is mislukt. U kunt ook een C# assert functies gebruiken om te controleren of de uitvoer. 

### <a name="execute-test-cases-in-visual-studio"></a>Testcases uitvoeren in Visual Studio

U-SQL-script-testproject is gebaseerd op testframework van C#-eenheid. Na de build het project, u kunt alle testcases uitvoeren via **testen Explorer > afspeellijst**, of met de rechtermuisknop op het bestand .cs en kies **Tests uitvoeren**.

## <a name="test-c-udos"></a>C# UDO's testen

### <a name="create-test-cases-for-c-udos"></a>Testcases voor C# UDO's maken

U kunt C#-eenheid testframework gebruiken voor het testen van uw C#-UDOs(User-Defined Operator). Wanneer u test UDO's, moet u voorbereiden overeenkomt **IRowset** object als invoer.

Er zijn twee manieren om te maken van IRowset:

1.  Gegevens laden uit een bestand IRowset maken

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

2.  Gegevens uit het verzamelen van gegevens gebruiken om te maken van IRowset

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

Nadat u UDO functies aanroept, kunt u het resultaat via schema en de rijenset waarde verificatie met behulp van functions C# Assert controleren. Voorbeeld van code, kan zich in U-SQL C# UDO eenheid Test voorbeeldproject via **bestand > Nieuw > Project** in Visual Studio.

### <a name="execute-test-cases-in-visual-studio"></a>Testcases uitvoeren in Visual Studio

Na build van het testproject, kunt u uitvoeren alle testcases echter **testen Explorer > afspeellijst**, of met de rechtermuisknop op het bestand .cs en kies **Tests uitvoeren**.

## <a name="run-test-cases-in-visual-studio-team-service"></a>Testcases uitvoeren in Visual Studio Team Service

Beide **U-SQL-script-testproject** en **C# UDO testproject** overnemen testproject van C#-eenheid. [Visual Studio Test taak](https://docs.microsoft.com/vsts/pipelines/test/getting-started-with-continuous-testing?view=vsts) in Visual Studio Team Service deze testcases kunt uitvoeren. 

### <a name="run-u-sql-test-cases-in-visual-studio-team-service"></a>U-SQL-testcases uitvoeren in Visual Studio Team Service

Voor testen van U-SQL, zorg ervoor dat u hebt geladen `CPPSDK` op uw machine build en geeft u de `CPPSDK` pad naar USqlScriptTestRunner (cppSdkFolderFullPath: @"").

**Wat is CPPSDK?**

CPPSDK is dat een pakket bevat Microsoft Visual C++-14 en Windows-SDK 10.0.10240.0, die het nodig is voor U-SQL-runtime-omgeving is. U kunt dit pakket onder Azure Data Lake Tools voor Visual Studio-installatiemap krijgen:

- Voor Visual Studio 2015 is onder `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Microsoft\Microsoft Azure Data Lake Tools for Visual Studio 2015\X.X.XXXX.X\CppSDK`
- Voor Visual Studio 2017 is onder `C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\SDK\ScopeCppSDK`

**Hoe bereid ik CPPSDK in Visual Studio Team Service-bouwagent**

De gebruikelijke manier voor het voorbereiden van deze afhankelijkheid CPPSDK in Visual Studio Team Service is:

1.  De map ZIP omvat CPPSDK-bibliotheken.
2.  Controleer in het zipbestand in uw bronbeheersysteem. (Zip-bestand kunt Zorg ervoor dat u in alle bibliotheken onder CPPSDK map controleren of bepaalde bestanden worden genegeerd door '.gitignore'.)
3.  Pak het zip-bestand in de Build-pijplijn.
4.  Punt `USqlScriptTestRunner` naar deze uitgepakte map op de build-machine.

### <a name="run-c-udo-test-cases-in-visual-studio-team-service"></a>C# UDO testcases uitvoeren in Visual Studio Team Service

Zorg dat u verwijzen naar hieronder assembly's die nodig zijn voor UDO's voor C# UDO-test. Als u verwijst naar via [het Nuget-pakket Microsoft.Azure.DataLake.USQL.Interfaces](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.Interfaces/), zorg ervoor dat u een taak voor het herstellen van NuGet toevoegen in de build-pijplijn.

* Microsoft.Analytics.Interfaces
* Microsoft.Analytics.Types
* Microsoft.Analytics.UnitTest

## <a name="next-steps"></a>Volgende stappen

- [CI/CD-pijplijn instellen voor Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md)
- [Gebruik U-SQL database-project voor het ontwikkelen van U-SQL-database](data-lake-analytics-data-lake-tools-develop-usql-database.md)

