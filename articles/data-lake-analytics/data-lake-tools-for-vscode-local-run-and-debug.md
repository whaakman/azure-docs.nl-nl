---
title: 'Azure Data Lake Tools: U-SQL lokaal uitvoeren en lokale foutopsporing met Visual Studio Code | Microsoft Docs'
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio Code voor lokale uitvoering en lokale foutopsporing.
Keywords: VScode,Azure Data Lake Tools,Local run,Local debug,Local Debug,preview storage file,upload to storage path
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: d109e4d57f4ad5ab2be73805ba41bf9ed362cccb
ms.sourcegitcommit: 21a58a43ceceaefb4cd46c29180a629429bfcf76
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="u-sql-local-run-and-local-debug-for-windows-with-visual-studio-code"></a>U-SQL lokaal uitvoeren en lokale foutopsporing voor Windows met Visual Studio Code
In dit document leert u hoe U-SQL-taken uitvoeren op een lokale ontwikkelcomputer vroege codering fasen versnellen of voor foutopsporing van code lokaal in Visual Studio Code. Zie voor instructies voor Azure Data Lake Tool voor Visual Studio Code, [gebruik Azure Data Lake Tools voor Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md). 


## <a name="set-up-the-u-sql-local-run-environment"></a>Instellen van de omgeving U-SQL lokaal uitvoeren

1. Selecteer Ctrl + Shift + P om het palet opdracht openen en voer vervolgens **ADL: lokale uitvoeren afhankelijkheid downloaden** voor het downloaden van de pakketten.  

   ![De afhankelijkheid van ADL LocalRun pakketten downloaden](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Zoek de pakketten afhankelijkheid van het pad wordt weergegeven in de **uitvoer** deelvenster en installeer vervolgens BuildTools en Win10SDK 10240. Hier volgt een voorbeeldpad:  
`C:\Users\xxx\AppData\Roaming\LocalRunDependency` 

   ![Zoek de pakketten afhankelijkheid](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   2.1 voor het installeren van **BuildTools**, klikt u op visualcppbuildtools_full.exe in de map LocalRunDependency en volg de instructies van de wizard.   

    ![BuildTools installeren](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   2.2 voor het installeren van **Win10SDK 10240**, klikt u op sdksetup.exe in de map LocalRunDependency/Win10SDK_10.0.10240_2 en volg de instructies van de wizard.  

    ![Installeer Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Instellen van de omgevingsvariabele. Stel de **SCOPE_CPP_SDK** omgevingsvariabele:  
`C:\Users\XXX\AppData\Roaming\LocalRunDependency\CppSDK_3rdparty`  


## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a>De lokale uitvoering service starten en het verzenden van de U-SQL-taak op een lokaal account 
Gebruik voor de gebruiker eerst **ADL: lokale uitvoeren afhankelijkheid downloaden** voor het downloaden van lokale uitvoering pakketten, hebt u geen [U-SQL lokaal uitvoeren omgeving instellen](#set-up-the-u-sql-local-run-environment).

1. Selecteer Ctrl + Shift + P om het palet opdracht openen en voer vervolgens **ADL: lokale Run Service Start**.   
2. Selecteer **accepteren** te accepteren van de licentievoorwaarden voor Microsoft-Software voor de eerste keer. 

   ![Accepteer de licentievoorwaarden voor Microsoft-Software](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. De cmd-console wordt geopend. Eerste keer dat gebruikers, moet u opgeven **3**, en ga vervolgens naar het lokale pad naar uw invoer en uitvoer. U kunt de standaardwaarden gebruiken voor andere opties. 

   ![Data Lake Tools voor Visual Studio Code lokaal uitvoeren cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Selecteer Ctrl + Shift + P als het palet opdracht openen, voert u **ADL: Submit Job**, en selecteer vervolgens **lokale** voor het verzenden van de taak op uw lokale account.

   ![Data Lake Tools voor Visual Studio Code selecteren lokale](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Nadat u de taak hebt ingediend, kunt u de details van de verzending van weergeven. Selecteer om te bekijken van de verzending van **jobUrl** in de **uitvoer** venster. U kunt ook de status van de verzending van de cmd-console weergeven. Voer **7** in als u wilt weten meer taakdetails de cmd-console.

   ![Data Lake Tools voor Visual Studio Code lokaal uitvoeren uitvoer](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools voor Visual Studio Code lokaal uitvoeren cmd-status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-the-u-sql-job"></a>Start een lokale foutopsporing voor de U-SQL-taak  
Voor de gebruiker eerst:

1. Gebruik **ADL: lokale uitvoeren afhankelijkheid downloaden** voor het downloaden van lokale uitvoering pakketten, hebt u geen [U-SQL lokaal uitvoeren omgeving instellen](#set-up-the-u-sql-local-run-environment).
2. Installeer .NET Core SDK 2.0 zoals voorgesteld in het berichtvak als niet geïnstalleerd.
 
  ![herinnering installeert Dotnet](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/remind-install-dotnet.png)
3. C# installatie voor Visual Studio Code als voorgesteld in het bericht als niet geïnstalleerd. Klik op **installeren** blijven en VSCode start opnieuw op.

    ![Herinnering voor het installeren van C#](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/install-csharp.png)

Volg onderstaande stappen voor het lokale foutopsporing uitvoeren:
  
1. Selecteer Ctrl + Shift + P om het palet opdracht openen en voer vervolgens **ADL: lokale Run Service Start**. De cmd-console wordt geopend. Zorg ervoor dat de **DataRoot** is ingesteld.
2. Stel een onderbrekingspunt in uw C# code-behind.
3. Terug naar scripteditor, klik met de rechtermuisknop en selecteer **ADL: fouten opsporen in lokale**.
    
   ![Data Lake Tools voor Visual Studio Code lokale foutopsporing resultaat](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Volgende stappen
* [De Azure Data Lake-tools gebruiken voor Visual Studio-code](data-lake-analytics-data-lake-tools-for-vscode.md)
* [U-SQL met Python, R en CSharp voor Azure Data Lake Analytics in VSCode ontwikkelen](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
* [Ontwikkelen van U-SQL-assembly's voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-assemblies.md)
* [Aan de slag met Data Lake Analytics met PowerShell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met Azure portal](data-lake-analytics-get-started-portal.md)
* [Data Lake Tools voor Visual Studio gebruiken voor het ontwikkelen van U-SQL-toepassingen](data-lake-analytics-data-lake-tools-get-started.md)
* [Gebruik Data Lake Analytics(U-SQL) catalogus](data-lake-analytics-use-u-sql-catalog.md)
