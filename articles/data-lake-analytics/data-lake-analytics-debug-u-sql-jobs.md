---
title: Fouten opsporen in de gebruiker gedefinieerde C#-code voor mislukte Azure Data Lake U-SQL-taken
description: In dit artikel wordt beschreven hoe u fouten opsporen in een mislukte hoekpunt van U-SQL met Azure Data Lake Tools voor Visual Studio.
services: data-lake-analytics
ms.service: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.topic: conceptual
ms.date: 11/30/2017
ms.openlocfilehash: 11587d5a0520d42d554c13a525c3b57db82326aa
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229472"
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Fouten opsporen in de gebruiker gedefinieerde C#-code voor mislukte U-SQL-taken

U-SQL biedt een uitbreidbaarheidsmodel met behulp van C#. In U-SQL-scripts is het eenvoudig C#-functies aanroepen en analytische functies die de declaratieve taal SQL-achtige biedt geen ondersteuning voor uitvoeren. Zie voor meer informatie voor U-SQL-uitbreidbaarheid, [handleiding voor het programmeren van U-SQL](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf). 

In de praktijk code mogelijk moet opsporen van fouten, maar het is moeilijk fouten opsporen in een gedistribueerde taak met aangepaste code in de cloud met beperkte logboekbestanden. [Azure Data Lake Tools voor Visual Studio](https://aka.ms/adltoolsvs) bevat een functie met de naam **kan hoekpunt Debug**, waarmee u meer spoor eenvoudig fouten voor de fouten die optreden in uw aangepaste code. Wanneer U-SQL-taak is mislukt, de service blijft de status mislukt en het hulpprogramma kunt u de fout cloudomgeving downloaden naar de lokale computer voor foutopsporing. Het downloaden van het lokale bevat de volledige in de cloud-omgeving, met inbegrip van een invoer- en gebruikerscode.

De volgende video ziet u hoekpunt foutopsporing is mislukt in Azure Data Lake Tools voor Visual Studio.

> [!VIDEO https://www.youtube.com/embed/3enkNvprfm4]
>

> [!IMPORTANT]
> Visual Studio de volgende twee updates vereist voor het gebruik van deze functie: [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) en de [universeel C Runtime voor Windows](https://www.microsoft.com/download/details.aspx?id=50410).
>

## <a name="download-failed-vertex-to-local-machine"></a>Hoekpunt naar de lokale computer downloaden is mislukt

Wanneer u een mislukte taak in Azure Data Lake Tools voor Visual Studio opent, ziet u een gele waarschuwingsbalk met gedetailleerde foutberichten worden weergegeven in het tabblad fout.

1. Klik op **downloaden** om alle vereiste resources en invoerstromen te downloaden. Als de download wordt niet voltooid, klikt u op **opnieuw**.

2. Klik op **Open** nadat het downloaden is voltooid voor het genereren van een lokale foutopsporingsomgeving. Een nieuwe oplossing voor het opsporen van fouten wordt geopend en hebt u bestaande oplossing in Visual Studio Zorg ervoor dat u opslaan en sluiten voordat u foutopsporing geopend.

![Azure Data Lake Analytics U-SQL foutopsporing visual studio downloaden hoekpunt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

## <a name="configure-the-debugging-environment"></a>De foutopsporing-omgeving configureren

> [!NOTE]
> Voordat u fouten opspoort, moet u controleren **Common Language Runtime-uitzonderingen** in het venster Instellingen voor uitzondering (**Ctrl + Alt + E**).

![Azure Data Lake Analytics U-SQL foutopsporing visual studio-instelling](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

In het nieuwe gestarte Visual Studio-exemplaar, kan of kunnen niet vinden voor de gebruiker gedefinieerde C#-broncode:

1. [Ik kan mijn broncode vinden in de oplossing](#source-code-is-included-in-debugging-solution)

2. [Ik kan mijn broncode niet vinden in de oplossing](#source-code-is-not-included-in-debugging-solution)

### <a name="source-code-is-included-in-debugging-solution"></a>Broncode is opgenomen in de oplossing voor foutopsporing

Er zijn twee mogelijke situaties die de C#-broncode wordt vastgelegd:

1. De gebruikerscode is gedefinieerd in de code-behind-bestand (doorgaans met de naam `Script.usql.cs` in een project U-SQL).

2. De gebruikerscode is gedefinieerd in C#-klassebibliotheekproject voor U-SQL-toepassing en geregistreerd als assembly met **fouten opsporen in info**.

Als de broncode wordt geïmporteerd in de oplossing, kunt u de foutopsporingsprogramma's van Visual Studio (controle, variabelen, enzovoort) op het probleem op te lossen:

1. Druk op **F5** om de foutopsporing te starten. De code wordt uitgevoerd totdat deze is gestopt door een uitzondering.

2. Open het bronbestand van de code en Stel onderbrekingspunten, drukt u vervolgens op **F5** fouten opsporen in de code stap voor stap.

    ![Azure Data Lake Analytics U-SQL-uitzondering voor foutopsporing](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

### <a name="source-code-is-not-included-in-debugging-solution"></a>Broncode is niet opgenomen in de oplossing voor foutopsporing

Als de gebruikerscode niet in de code-behind-bestand opgenomen is, of u hebt niet de assembly met registreren **fouten opsporen in info**, en vervolgens de broncode niet automatisch in de oplossing voor foutopsporing opgenomen wordt. In dit geval moet u extra stappen voor het toevoegen van uw broncode:

1. Met de rechtermuisknop op **oplossing 'VertexDebug' > toevoegen > bestaand Project...**  de assembly vinden voor de broncode en het project toevoegen aan de oplossing voor foutopsporing.

    ![Azure Data Lake Analytics U-SQL-foutopsporing project toevoegen](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. Ophalen van pad naar het project voor **FailedVertexDebugHost** project. 

3. Met de rechtermuisknop op **het toegevoegde assembly source CodeProject > eigenschappen**, selecteer de **bouwen** tabblad aan de linkerkant en plak de gekopieerde pad eindigt op \bin\debug als **uitvoer > uitvoerpad**. Het pad van de uiteindelijke uitvoer is vergelijkbaar met '<DataLakeTemp path>\fd91dd21-776e-4729-a78b-81ad85a4fba6\loiu0t1y.mfo\FailedVertexDebug\FailedVertexDebugHost\bin\Debug\".

    ![Azure Data Lake Analytics U-SQL-foutopsporing instellen pdb pad](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

Na deze instellingen start debugging met **F5** en onderbrekingspunten. U kunt ook de Visual Studio foutopsporingsprogramma's (controle, variabelen, enzovoort) op het probleem op te lossen.

> [!NOTE]
> Opnieuw het CodeProject van assembly bron telkens nadat u de code voor het genereren van bijgewerkte .pdb bestanden gewijzigd.

## <a name="resubmit-the-job"></a>De taak opnieuw indienen

Als het project is voltooid ziet het uitvoervenster weergegeven na het opsporen van fouten, het volgende bericht:

    The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).

![Azure Data Lake Analytics U-SQL-foutopsporing is mislukt](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

Aan de mislukte taak opnieuw:

1. Voor taken met code-behind-oplossingen, door de C#-code te kopiëren naar de bron-code-behind-bestand (meestal `Script.usql.cs`).

2. Voor taken met assembly's, met de rechtermuisknop op het CodeProject van assembly bron in de oplossing voor foutopsporing en registreer de bijgewerkte .dll-assembly's in uw Azure Data Lake-catalogus.

3. De U-SQL-taak opnieuw indienen.

## <a name="next-steps"></a>Volgende stappen

- [Handleiding voor het programmeren van U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
- [U-SQL-gebruiker gedefinieerde operators ontwikkelen voor Azure Data Lake Analytics-taken](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [U-SQL-taken testen en controleren op fouten met behulp van lokale uitvoering en de Azure Data Lake U-SQL-SDK](data-lake-analytics-data-lake-tools-local-run.md)
- [Problemen met een abnormale terugkerende taak oplossen](data-lake-analytics-data-lake-tools-debug-recurring-job.md)
