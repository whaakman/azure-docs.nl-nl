---
title: Uitvoeren van U-SQL lokaal en fouten opsporen in Azure Data Lake Tools voor Visual Studio Code
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio Code uitvoeren en fouten opsporen in U-SQL-taken lokaal.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.topic: conceptual
ms.date: 07/14/2017
ms.openlocfilehash: 42982e3fa0a854109a6b887640604498ac68847c
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632256"
---
# <a name="run-u-sql-and-debug-locally-in-visual-studio-code"></a>Uitvoeren van U-SQL lokaal en fouten opsporen in Visual Studio Code
In dit artikel wordt beschreven hoe U-SQL-taken worden uitgevoerd op een computer lokale ontwikkeling te versnellen eerste fasen van de code of fouten opsporen in code lokaal in Visual Studio Code. Zie voor instructies voor Azure Data Lake-hulpprogramma voor Visual Studio Code, [gebruik Azure Data Lake Tools voor Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

Alleen Windows-installaties van de Azure Data Lake Tools voor Visual Studio ondersteuning voor de actie voor het U-SQL lokaal uitvoeren en lokaal fouten opsporen in U-SQL. Installaties in macOS en Linux-besturingssystemen bieden geen ondersteuning voor deze functie.

## <a name="set-up-the-u-sql-local-run-environment"></a>Stel de omgeving U-SQL lokaal uitvoeren

1. Selecteer Ctrl + Shift + P om het opdrachtenpalet te openen en voer vervolgens **ADL: lokaal uitvoeren pakket downloaden** voor het downloaden van de pakketten.  

   ![De ADL LocalRun afhankelijke pakketten downloaden](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/downloadtheadllocalrunpackage.png)

2. Zoek de afhankelijkheidspakketten van het pad dat wordt weergegeven in de **uitvoer** in het deelvenster en installeer vervolgens BuildTools en Win10SDK 10240. Hier volgt een voorbeeld-pad:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Zoek de afhankelijkheidspakketten](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 voor het installeren van **BuildTools**, klikt u op visualcppbuildtools_full.exe in de map LocalRunDependency en volg de instructies van de wizard.   

    ![BuildTools installeren](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 voor het installeren van **Win10SDK 10240**, klikt u op sdksetup.exe in de map LocalRunDependency/Win10SDK_10.0.10240_2 en volg de instructies van de wizard.  

    ![Installeer Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Stel de omgevingsvariabele. Stel de **SCOPE_CPP_SDK** omgevingsvariabele:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>Start de service voor lokale uitvoering en verzenden van de U-SQL-taak op een lokaal account 
Gebruik voor de gebruiker eerst **ADL: lokaal uitvoeren pakket downloaden** voor het downloaden van lokale uitvoering pakketten, als u niet hebt gedaan [instellen van U-SQL lokaal uitvoeren omgeving](#set-up-the-u-sql-local-run-environment).

1. Selecteer Ctrl + Shift + P om het opdrachtenpalet te openen en voer vervolgens **ADL: lokale uitvoeren Service Start**.   
2. Selecteer **accepteren** te accepteren van de licentievoorwaarden voor Microsoft-Software voor de eerste keer. 

   ![Accepteer de licentievoorwaarden voor Microsoft-Software](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. De cmd-console wordt geopend. Voor de eerste keer dat gebruikers, moet u invoeren **3**, en ga vervolgens naar het lokale pad naar uw invoer en uitvoer. U kunt de standaardwaarden gebruiken voor andere opties. 

   ![Data Lake Tools voor Visual Studio Code lokaal uitvoeren cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Selecteer Ctrl + Shift + P, open het opdrachtenpalet, geeft u **ADL: taak verzenden**, en selecteer vervolgens **lokale** om de taak op uw lokale account te verzenden.

   ![Data Lake Tools voor Visual Studio Code selecteren lokale](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Nadat u de taak hebt verzonden, kunt u de details van de verzending van weergeven. De inzending om details te bekijken, selecteert u **jobUrl** in de **uitvoer** venster. U kunt ook de status van de taak indienen vanuit de console cmd weergeven. Voer **7** in de cmd-console als u wilt meer taakdetails weten.

   ![Data Lake Tools voor Visual Studio Code-lokaal uitvoeren van uitvoer](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools voor Visual Studio Code lokaal cmd uitvoeringsstatus](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Een lokale foutopsporing voor de U-SQL-taak starten  
Voor de gebruiker eerst:

1. Gebruik **ADL: lokaal uitvoeren pakket downloaden** voor het downloaden van lokale uitvoering pakketten, als u niet hebt gedaan [instellen van U-SQL lokaal uitvoeren omgeving](#set-up-the-u-sql-local-run-environment).
2. Installeer .NET Core SDK 2.0 zoals voorgesteld in het berichtenvak als niet is geïnstalleerd.
 
  ![herinnering installeert Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. C# installeren voor Visual Studio Code als voorgesteld in het bericht als dit niet is gebeurd. Klik op **installeren** blijven en VSCode start opnieuw op.

    ![Herinnering voor het installeren van C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Volg de stappen hieronder om het lokale foutopsporing uitvoeren:
  
1. Selecteer Ctrl + Shift + P om het opdrachtenpalet te openen en voer vervolgens **ADL: lokale uitvoeren Service Start**. De cmd-console wordt geopend. Zorg ervoor dat de **DataRoot** is ingesteld.
2. Stel een onderbrekingspunt in uw C#-code-behind.
3. Terug naar de script-editor, klik met de rechtermuisknop en selecteert u **ADL: lokale foutopsporing**.
    
   ![Data Lake Tools voor Visual Studio Code lokaal fouten opsporen resultaat](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Volgende stappen
* [De Azure Data Lake-tools gebruiken voor Visual Studio-code](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL met Python, R en CSharp voor Azure Data Lake Analytics vscode ontwikkelen](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [Aan de slag met Data Lake Analytics met behulp van PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met Azure portal](data-lake-analytics-get-started-portal.md)
* [Data Lake Tools voor Visual Studio gebruiken voor het ontwikkelen van U-SQL-toepassingen](data-lake-analytics-data-lake-tools-get-started.md)
* [Data Lake gebruiken Analytics(U-SQL) catalogus](data-lake-analytics-use-u-sql-catalog.md)
