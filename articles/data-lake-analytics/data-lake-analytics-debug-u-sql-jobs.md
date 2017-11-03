---
title: U-SQL-taken voor foutopsporing | Microsoft Docs
description: Informatie over fouten opsporen in een mislukte hoekpunt van U-SQL met Visual Studio.
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/02/2016
ms.author: saveenr
ms.openlocfilehash: 2a77c72d3062272305208934d6406d040266c753
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Foutopsporing van de gebruiker gedefinieerde C#-code voor mislukte U-SQL-taken

U-SQL biedt een model van uitbreidbaarheid met C#, zodat u kunt uw code om toe te voegen zoals een aangepaste zelfstandig uitpakken of reducer schrijven. Zie voor meer informatie, [U-SQL programmeerbaarheid handleiding](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). In de praktijk code mogelijk moet foutopsporing en big-datasystemen leveren alleen beperkt runtime foutopsporingsinformatie zoals logboekbestanden.

Azure Data Lake Tools voor Visual Studio biedt een functie **hoekpunt foutopsporing kan niet**, waarmee u een mislukte taak vanuit de cloud op uw lokale computer voor het opsporen van klonen. De lokale kloon bevat de volledige in de cloud-omgeving, inclusief eventuele invoergegevens en gebruikerscode.

De volgende video toont mislukt hoekpunt fouten opsporen in Azure Data Lake Tools voor Visual Studio.

> [!VIDEO https://e0d1.wpc.azureedge.net/80E0D1/OfficeMixProdMediaBlobStorage/asset-d3aeab42-6149-4ecc-b044-aa624901ab32/b0fc0373c8f94f1bb8cd39da1310adb8.mp4?sv=2012-02-12&sr=c&si=a91fad76-cfdd-4513-9668-483de39e739c&sig=K%2FR%2FdnIi9S6P%2FBlB3iLAEV5pYu6OJFBDlQy%2FQtZ7E7M%3D&se=2116-07-19T09:27:30Z&rscd=attachment%3B%20filename%3DDebugyourcustomcodeinUSQLADLA.mp4]
>

> [!NOTE]
> Visual Studio de volgende twee updates is vereist als ze nog niet zijn geïnstalleerd: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) en de [universeel C Runtime voor Windows](https://www.microsoft.com/download/details.aspx?id=50410).

## <a name="download-failed-vertex-to-local-machine"></a>Hoekpunt naar de lokale computer downloaden is mislukt

Als u een taak die is mislukt in Azure Data Lake Tools voor Visual Studio opent, ziet u een gele waarschuwing balk met gedetailleerde foutberichten op het tabblad fout.

1. Klik op **downloaden** voor het downloaden van de vereiste resources en invoer stromen. Als het downloaden niet voltooid, klikt u op **probeer**.

2. Klik op **Open** nadat het downloaden is voltooid voor het genereren van een lokale foutopsporingsomgeving. Een nieuw exemplaar van de Visual Studio met een oplossing voor foutopsporing is automatisch gemaakt en geopend.

![Azure Data Lake Analytics U-SQL foutopsporing visual studio downloaden hoekpunt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

Taken kunnen de bronbestanden van de code-behind of geregistreerde assembly's bevatten, en deze twee typen hebben verschillende scenario's voor foutopsporing.

- [Fouten opsporen in een taak die is mislukt met code-behind](#debug-job-failed-with-code-behind)
- [Fouten opsporen in een mislukte taak met assembly 's](#debug-job-failed-with-assemblies)


## <a name="debug-job-failed-with-code-behind"></a>Taak is mislukt met de code-behind voor foutopsporing

Als een U-SQL-taak is mislukt en de taak bevat een gebruikerscode (meestal onder de naam `Script.usql.cs` in een project U-SQL), dat de broncode wordt geïmporteerd in de oplossing voor foutopsporing.  Daar kunt u de Visual Studio foutopsporingsprogramma's (controle, variabelen, enz.) het probleem op te lossen.

> [!NOTE]
> Voordat u foutopsporing, moet u controleren **Common Language Runtime uitzonderingen** in het venster Instellingen voor uitzonderingen (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL foutopsporing visual studio-instelling](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

1. Druk op **F5** het code-behind code uit te voeren. Deze wordt uitgevoerd totdat deze is gestopt vanwege een uitzondering.

2. Open de `ADLTool_Codebehind.usql.cs` bestands- en Stel onderbrekingspunten, drukt u vervolgens op **F5** fouten opsporen in de code stap voor stap.

    ![Azure Data Lake Analytics U-SQL-uitzondering voor foutopsporing](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

## <a name="debug-job-failed-with-assemblies"></a>Taak is mislukt met de assembly's voor foutopsporing

Als u geregistreerde assembly's in uw U-SQL-script gebruikt, kan geen het systeem automatisch de broncode ophalen. Handmatig in dit geval is de assembly's broncodebestanden toevoegen aan de oplossing.

### <a name="configure-the-solution"></a>De oplossing configureren

1. Met de rechtermuisknop op **oplossing 'VertexDebug' > toevoegen > bestaand Project...**  broncode de assembly's vinden en het project toevoegen aan de oplossing voor foutopsporing.

    ![Azure Data Lake Analytics U-SQL-foutopsporing project toevoegen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Met de rechtermuisknop op **LocalVertexHost > eigenschappen** in de oplossing en kopieer de **werkmap** pad.

3. Met de rechtermuisknop op **CodeProject voor assembly-bron > eigenschappen**, selecteer de **bouwen** tabblad aan de linkerkant en plak de gekopieerde pad als **uitvoer > uitvoerpad**.

    ![Azure Data Lake Analytics U-SQL-foutopsporing instellen pdb pad](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

4. Druk op **Ctrl + Alt + E**, Controleer **Common Language Runtime uitzonderingen** in het venster Instellingen voor uitzondering.

### <a name="start-debug"></a>Foutopsporing starten

1. Met de rechtermuisknop op **CodeProject voor assembly-bron > opnieuw samenstellen** uitvoer .pdb bestanden voor de `LocalVertexHost` werkmap.

2. Druk op **F5** en het project wordt uitgevoerd totdat deze is gestopt vanwege een uitzondering. Mogelijk ziet u de volgende waarschuwing die u kunt negeren. Het kan tot een minuut duren ophalen naar het scherm voor foutopsporing.

    ![Azure Data Lake Analytics U-SQL foutopsporing visual studio-waarschuwing](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

3. Open de broncode en Stel onderbrekingspunten, drukt u vervolgens op **F5** fouten opsporen in de code stap voor stap.

U kunt ook de Visual Studio tools (controle, variabelen, enz.) het probleem oplossen foutopsporing gebruiken.

> [!NOTE]
> Bouw het CodeProject voor assembly-bron opnieuw telkens nadat u de code voor het genereren van bijgewerkte .pdb bestanden aanpassen.

Als het project voltooid is ziet het uitvoervenster na foutopsporing, het volgende bericht:

```
The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).
```

![Azure Data Lake Analytics U-SQL-foutopsporing is mislukt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

## <a name="resubmit-the-job"></a>De taak opnieuw indienen

Als u klaar bent met het opsporen van fouten opnieuw in als de mislukte taak.

1. Voor taken met oplossingen voor code-behind door uw C#-code te kopiëren naar het code-behind bronbestand (meestal `Script.usql.cs`).
2. Registreer de bijgewerkte .dll-assembly's in uw database ADLA voor taken met assembly's:
    1. Vouw in Server Explorer of Cloud Explorer, de **ADLA account > Databases** knooppunt.
    2. Met de rechtermuisknop op **assembly's** en registreren van uw nieuwe .dll-assembly's met de database ADLA: ![Azure Data Lake Analytics U-SQL-foutopsporing assembly registreren](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-register-assembly.png)
3. De taak opnieuw.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor het programmeren van U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
- [Ontwikkelen van U-SQL-gebruiker gedefinieerde operators voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Zelfstudie: U-SQL-scripts ontwikkelen met Data Lake Tools voor Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
