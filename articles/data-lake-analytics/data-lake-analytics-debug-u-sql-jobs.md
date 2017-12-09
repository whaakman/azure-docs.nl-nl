---
title: Foutopsporing van de gebruiker gedefinieerde C#-code voor mislukte Azure Data Lake U-SQL-taken | Microsoft Docs
description: Informatie over fouten opsporen in een mislukte hoekpunt van U-SQL met Azure Data Lake Tools voor Visual Studio.
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: jhubbard
editor: cgronlun
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/31/2017
ms.author: yanacai
ms.openlocfilehash: 739d46753729b70a24dbd3d6e2d78f8513e143e6
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Foutopsporing van de gebruiker gedefinieerde C#-code voor mislukte U-SQL-taken

U-SQL biedt een model van uitbreidbaarheid met C#. In de U-SQL-scripts is het eenvoudig C#-functies aanroepen en analytische functies uitvoeren die biedt geen ondersteuning voor SQL-achtige declaratieve taal. Zie voor meer informatie voor de U-SQL-uitbreidbaarheid, [U-SQL programmeerbaarheid handleiding](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). 

In de praktijk code wellicht foutopsporing, maar het is moeilijk fouten opsporen in een gedistribueerde taak met aangepaste code in de cloud met beperkte logboekbestanden. [Azure Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs) biedt een functie **hoekpunt foutopsporing kan niet**, waarmee u gemakkelijker fouten opsporen in de fouten die optreden in uw aangepaste code. Wanneer U-SQL-taak is mislukt, de service blijft de foutstatus en het hulpprogramma helpt u bij het downloaden van de cloudomgeving fout naar de lokale computer voor foutopsporing. Het downloaden van de lokale bevat de volledige in de cloud-omgeving, inclusief eventuele invoergegevens en gebruikerscode.

De volgende video toont mislukt hoekpunt fouten opsporen in Azure Data Lake Tools voor Visual Studio.

> [!VIDEO https://www.youtube.com/embed/3enkNvprfm4]
>

> [!IMPORTANT]
> Visual Studio de volgende twee updates vereist voor het gebruik van deze functie: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) en de [universeel C Runtime voor Windows](https://www.microsoft.com/download/details.aspx?id=50410).
>

## <a name="download-failed-vertex-to-local-machine"></a>Hoekpunt naar de lokale computer downloaden is mislukt

Als u een taak die is mislukt in Azure Data Lake Tools voor Visual Studio opent, ziet u een gele waarschuwing balk met gedetailleerde foutberichten op het tabblad fout.

1. Klik op **downloaden** voor het downloaden van de vereiste resources en invoer stromen. Als het downloaden niet voltooid, klikt u op **probeer**.

2. Klik op **Open** nadat het downloaden is voltooid voor het genereren van een lokale foutopsporingsomgeving. Een nieuw exemplaar van de Visual Studio met een oplossing voor foutopsporing is automatisch gemaakt en geopend.

![Azure Data Lake Analytics U-SQL foutopsporing visual studio downloaden hoekpunt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

## <a name="configure-the-debugging-environment"></a>De foutopsporingsomgeving configureren

> [!NOTE]
> Voordat u foutopsporing, moet u controleren **Common Language Runtime uitzonderingen** in het venster Instellingen voor uitzonderingen (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL foutopsporing visual studio-instelling](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

In het nieuwe gestarte Visual Studio-exemplaar mogelijk of kan de gebruiker gedefinieerde C#-broncode niet vinden:

1. [Ik kan mijn broncode vinden in de oplossing](#source-code-is-included-in-debugging-solution)

2. [Ik kan mijn broncode niet vinden in de oplossing](#source-code-is-not-included-in-debugging-solution)

### <a name="source-code-is-included-in-debugging-solution"></a>Broncode is opgenomen in de oplossing voor foutopsporing

Er zijn twee cases dat de C#-broncode wordt vastgelegd:

1. De gebruikerscode is gedefinieerd in de code-behind-bestand (meestal onder de naam `Script.usql.cs` in een project U-SQL).

2. De gebruikerscode is gedefinieerd in C# class library-project voor de U-SQL-toepassing en geregistreerd als een assembly met **info debug**.

Als de broncode wordt geïmporteerd naar de oplossing, kunt u de foutopsporingsprogramma's van Visual Studio (controle, variabelen, enz.) het probleem oplossen:

1. Druk op **F5** foutopsporing te starten. De code wordt uitgevoerd totdat deze is gestopt vanwege een uitzondering.

2. Open het bronbestand van de code en Stel onderbrekingspunten, drukt u vervolgens op **F5** fouten opsporen in de code stap voor stap.

    ![Azure Data Lake Analytics U-SQL-uitzondering voor foutopsporing](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

### <a name="source-code-is-not-included-in-debugging-solution"></a>Broncode is niet opgenomen in de oplossing voor foutopsporing

Als de gebruikerscode niet in de code-behind-bestand opgenomen is registreren of u hebt niet de assembly met **info debug**, en vervolgens de broncode niet automatisch in de oplossing voor foutopsporing opgenomen wordt. In dit geval moet u extra stappen voor het toevoegen van uw broncode:

1. Met de rechtermuisknop op **oplossing 'VertexDebug' > toevoegen > bestaand Project...**  vinden van de assembly broncode en het project toevoegen aan de oplossing voor foutopsporing.

    ![Azure Data Lake Analytics U-SQL-foutopsporing project toevoegen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Ophalen van het pad van de map project voor **FailedVertexDebugHost** project. 

3. Met de rechtermuisknop op **het CodeProject toegevoegde assembly-bron > eigenschappen**, selecteer de **bouwen** tabblad aan de linkerkant en plak de gekopieerde pad eindigt met \bin\debug als **uitvoer > uitvoerpad**. Het pad van de uiteindelijke uitvoer is vergelijkbaar met '<DataLakeTemp path>\fd91dd21-776e-4729-a78b-81ad85a4fba6\loiu0t1y.mfo\FailedVertexDebug\FailedVertexDebugHost\bin\Debug\".

    ![Azure Data Lake Analytics U-SQL-foutopsporing instellen pdb pad](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

Start na deze instellingen foutopsporing met **F5** en onderbrekingspunten. U kunt ook de Visual Studio tools (controle, variabelen, enz.) het probleem oplossen foutopsporing gebruiken.

> [!NOTE]
> Bouw het CodeProject voor assembly-bron opnieuw telkens nadat u de code voor het genereren van bijgewerkte .pdb bestanden aanpassen.

## <a name="resubmit-the-job"></a>De taak opnieuw indienen

Als het project voltooid is ziet het uitvoervenster na foutopsporing, het volgende bericht:

    The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).

![Azure Data Lake Analytics U-SQL-foutopsporing is mislukt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

Aan de mislukte taak opnieuw:

1. Voor taken met oplossingen voor code-behind, door de C#-code te kopiëren naar het code-behind bronbestand (meestal `Script.usql.cs`).

2. Voor taken met assembly's met de rechtermuisknop op het CodeProject assembly-bron in een oplossing voor foutopsporing en registreer de bijgewerkte .dll-assembly's in uw Azure Data Lake-catalogus.

3. De U-SQL-taak opnieuw indienen.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor het programmeren van U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
- [Ontwikkelen van U-SQL-gebruiker gedefinieerde operators voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [U-SQL-taken testen en controleren op fouten met behulp van lokale uitvoering en de Azure Data Lake U-SQL-SDK](data-lake-analytics-data-lake-tools-local-run.md)
- [Problemen met een abnormale terugkerende taak oplossen](data-lake-analytics-data-lake-tools-debug-recurring-job.md)