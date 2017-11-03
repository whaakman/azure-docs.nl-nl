---
title: Testen en foutopsporing van U-SQL-taken met behulp van lokaal uitvoeren en de Azure Data Lake U-SQL-SDK | Microsoft Docs
description: Informatie over het Azure Data Lake Tools voor Visual Studio en de Azure Data Lake U-SQL-SDK gebruiken om te testen en foutopsporing van U-SQL-taken op uw lokale werkstation.
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: yanacai
ms.openlocfilehash: 771a96df5cc66bac46e7144785be8cc072b57b31
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="test-and-debug-u-sql-jobs-by-using-local-run-and-the-azure-data-lake-u-sql-sdk"></a>Testen en foutopsporing van U-SQL-taken met behulp van lokaal uitvoeren en de Azure Data Lake U-SQL-SDK

Met Azure Data Lake-tools voor Visio Studio en de Azure Data Lake U-SQL-SDK kunt u U-SQL-taken op uw werkstation uitvoeren, net zoals u in de Azure Data Lake-service. Deze twee lokaal uitgevoerde functies besparen tijd op het gebied van het testen en de foutopsporing van uw U-SQL-taken.

## <a name="understand-the-data-root-folder-and-the-file-path"></a>Inzicht in de hoofdmap voor de gegevens en het bestandspad

Zowel lokale uitvoering en de U-SQL-SDK moet een gegevens-hoofdmap. De gegevens-hoofdmap is 'lokale store' voor de lokale compute-account. Dit is gelijk aan het Azure Data Lake Store-account van een Data Lake Analytics-account. Overschakelen naar een andere hoofdmap van de gegevens is net als overschakelen naar een andere store-account. Als u toegang wilt tot vaak gedeelde gegevens met verschillende gegevenshoofdmap mappen, moet u de absolute paden gebruiken in uw scripts. Of het bestand system symbolische koppelingen maken (bijvoorbeeld **mklink** van NTFS) onder de hoofdmap voor gegevens om te verwijzen naar de gedeelde gegevens.

De hoofdmap voor de gegevens wordt gebruikt voor:

- Metagegevens, zoals databases, tabellen, tabelfuncties (tvf's) en assembly's opgeslagen.
- Zoek de invoer- en exportpaden die zijn gedefinieerd als relatieve paden in de U-SQL. U vergemakkelijkt het implementeren van uw U-SQL-projecten in Azure met relatieve paden.

U kunt een relatief pad en een lokaal absoluut pad in U-SQL-scripts gebruiken. Het relatieve pad is ten opzichte van het pad voor de opgegeven gegevens-hoofdmap. Het is raadzaam dat u '/' als het padscheidingsteken uw scripts om compatibel te maken met de serverzijde. Hier volgen enkele voorbeelden van relatieve paden en hun equivalente absolute paden. In deze voorbeelden is C:\LocalRunDataRoot het gegevens-hoofdmap.

|Relatief pad|Het absolute pad|
|-------------|-------------|
|/ABC/DEF/Input.csv |C:\LocalRunDataRoot\abc\def\input.csv|
|ABC/DEF/Input.csv  |C:\LocalRunDataRoot\abc\def\input.csv|
|D:/ABC/DEF/Input.csv |D:\abc\def\input.csv|

## <a name="use-local-run-from-visual-studio"></a>Gebruik lokale uitvoeren vanuit Visual Studio

Data Lake Tools voor Visual Studio biedt een U-SQL lokaal uitvoeren ervaring in Visual Studio. Met behulp van deze functie kunt u het volgende doen:

- Een U-SQL-script lokaal uitvoeren samen met C#-assembly's.
- Fouten opsporen in een C#-assembly lokaal.
- Maken, weergeven en verwijderen van U-SQL-catalogussen (lokale databases, assembly's, schema's en tabellen) in Server Explorer. U kunt ook de lokale catalogus ook vinden in Server Explorer.

    ![Data Lake Tools voor Visual Studio lokaal uitvoeren lokale catalogus](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-local-catalog.png)

Het Data Lake Tools-installatieprogramma maakt een map C:\LocalRunRoot moet worden gebruikt als de standaardmap voor de hoofdmap van de gegevens. De standaard lokale uitvoeren parallelle uitvoering is 1.

### <a name="to-configure-local-run-in-visual-studio"></a>Lokale uitvoering in Visual Studio configureren

1. Open Visual Studio.
2. Open **Server Explorer**.
3. Vouw **Azure** > **Data Lake Analytics**.
4. Klik op de **Data Lake** menu en klik vervolgens op **opties en instellingen**.
5. Vouw in het linkerdeelvenster, **Azure Data Lake**, en vouw vervolgens **algemene**.

    ![Data Lake Tools voor Visual Studio lokaal uitvoeren configureren instellingen](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-configure.png)

Een Visual Studio U-SQL-project is vereist voor het uitvoeren van lokale uitvoeren. Dit onderdeel wijkt af van het uitvoeren van U-SQL-scripts uit Azure.

### <a name="to-run-a-u-sql-script-locally"></a>Een U-SQL-script lokaal uitvoeren
1. Open uw U-SQL-project in Visual Studio.   
2. Met de rechtermuisknop op een U-SQL-script in Solution Explorer en klik vervolgens op **Submit Script**.
3. Selecteer **(lokaal)** als het Analytics-account aan uw script lokaal uitvoeren.
U kunt ook klikken op de **(lokaal)** account boven aan de script-venster en klik vervolgens op **indienen** (of gebruik Ctrl + F5 sneltoets).

    ![Data Lake Tools voor Visual Studio lokaal uitvoeren indienen taken](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-for-visual-studio-local-run-submit-job.png)

### <a name="debug-scripts-and-c-assemblies-locally"></a>Lokaal fouten opsporen in scripts en C#-assembly's

U kunt fouten opsporen C#-assembly's zonder verzenden en registreren voor Azure Data Lake Analytics-Service. U kunt onderbrekingspunten instellen in zowel het onderliggende-codebestand als een C#-project waarnaar wordt verwezen.

#### <a name="to-debug-local-code-in-code-behind-file"></a>Foutopsporing in lokale code in onderliggende-codebestand

1. Onderbrekingspunten instellen in het onderliggende-codebestand.
2. Druk op F5 drukken om op te sporen in het script lokaal.

> [!NOTE]
   > De volgende procedure werkt alleen in Visual Studio 2015. In oudere Visual Studio-versies moet u mogelijk de PDB-bestanden handmatig toevoegen.  
   >
   >

#### <a name="to-debug-local-code-in-a-referenced-c-project"></a>Fouten opsporen in lokale code in een C#-project waarnaar wordt verwezen

1. Maak een C#-assemblyproject en bouw het zo dat het de DLL-uitvoer genereert.
2. Registreer het DLL-bestand met een U-SQL-instructie:

        CREATE ASSEMBLY assemblyname FROM @"..\..\path\to\output\.dll";
        
3. Stel onderbrekingspunten in in de C#-code.
4. Druk op F5 om het script dat verwijst naar de C#-dll lokaal fouten opsporen.

## <a name="use-local-run-from-the-data-lake-u-sql-sdk"></a>Gebruik-lokaal uitvoeren van de Data Lake U-SQL-SDK

Naast het uitvoeren van U-SQL-scripts lokaal met behulp van Visual Studio, kunt u de SDK van Azure Data Lake U-SQL lokaal uitvoeren van U-SQL-scripts met de opdrachtregel en programming interfaces. Via deze, kunt u uw lokale test U-SQL te schalen.

Meer informatie over [SDK van Azure Data Lake U-SQL](data-lake-analytics-u-sql-sdk.md).


## <a name="next-steps"></a>Volgende stappen

* Zie voor een complexere query [websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Taakdetails Zie [gebruik taak Browser en weergave van de taak voor Azure Data Lake Analytics-taken](data-lake-analytics-data-lake-tools-view-jobs.md).
* Zie voor het gebruik van de weergave van de uitvoering hoekpunt [gebruik van de Vertex Execution View in Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md).
