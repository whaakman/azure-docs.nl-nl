---
title: Het exporteren van U-SQL-databases met Azure Data Lake Tools voor Visual Studio | Microsoft Docs
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio U-SQL-database exporteren en importeren naar een lokaal account op hetzelfde moment.
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: yanacai
ms.openlocfilehash: 9f11e07f7fbaf2b708d6fbc8a746238745132bda
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-export-u-sql-database"></a>Het exporteren van U-SQL-database

In dit document, leert u hoe u [Azure Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs) exporteren van U-SQL-database als een enkel U-SQL-script en gedownloade resources. Importeren van de geëxporteerde database naar een lokale account wordt ook ondersteund in hetzelfde proces.

Klanten beheren meestal meerdere omgevingen voor ontwikkeling, testen en productie. Deze omgevingen worden gehost op zowel lokale account op de lokale computer van de ontwikkelaar en Azure Data Lake Analytics-account in Azure. Bij het ontwikkelen en U-SQL-query's in ontwikkeling en testomgevingen afstemmen, is het gebruikelijk dat ontwikkelaars moeten alles in productiedatabase opnieuw maken. **Database-Wizard exporteren** helpt dit proces versnellen. Ontwikkelaars kunnen met behulp van de wizard kloon de bestaande databaseomgeving en de voorbeeldgegevens aan andere Azure Data Lake Analytics-accounts.

## <a name="export-steps"></a>Stappen exporteren

### <a name="step-1-right-click-the-database-in-server-explorer-and-click-export"></a>Stap 1: Met de rechtermuisknop op de database in Server Explorer en klik op 'Exporteren...'

Alle Azure Data Lake Analytics-accounts u gemachtigd in Server Explorer worden weergegeven. Vouw de bevat de database die u wilt exporteren en met de rechtermuisknop op de database kiezen **exporteren...** . Als u niet het contextmenu vindt, moet u [het hulpprogramma bijwerkt naar de meest recente versie versie](http://aka.ms/adltoolsvs).

![Data Lake Analytics hulpprogramma's voor de Export-Database](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

### <a name="step-2-configure-the-objects-you-want-to-export"></a>Stap 2: Configureer de objecten die u wilt exporteren

Soms een database is een grote, maar hoeft u alleen een klein deel hiervan en vervolgens kunt u de subset met objecten die u wilt exporteren in de wizard exporteren. Houd er rekening mee dat de export-actie is voltooid door een U-SQL-taak wordt uitgevoerd en daarom exporteren uit Azure-account sommige kosten maakt.

![Database-Wizard voor Data Lake Analytics hulpprogramma's exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-more-configurations"></a>Stap 3: Controleer de lijst met objecten en configuraties meer

In deze stap kunt u de geselecteerde objecten aan de bovenkant van het dialoogvenster dubbele controleren. Als er fouten zijn, kunt u Klik op Vorige als u wilt teruggaan en de objecten die u wilt exporteren opnieuw configureren.

U kunt ook andere configuraties over export doel doen, de beschrijvingen van deze configuraties worden vermeld in onderstaande tabel:

|Configuratie|Beschrijving|
|-------------|-----------|
|Doelnaam|Deze naam geeft aan waar u de geëxporteerde database-resources, zoals assembly's, aanvullende bestanden en de voorbeeldgegevens. Een map met deze naam wordt gemaakt onder de hoofdmap van uw lokale gegevens.|
|Projectmap|Dit pad definieert waar u het geëxporteerde U-SQL-script, waaronder alle objectdefinities van de database.|
|Alleen het schema|Deze optie resulteert in enige databasedefinities en bronnen (zoals assembly's en aanvullende bestanden) wordt geëxporteerd.|
|Schema en de gegevens|Deze optie resulteert in databasedefinities, bronnen en gegevens worden geëxporteerd. De bovenste N rijen van tabellen worden geëxporteerd.|
|Automatisch importeren voor lokale-Database|Controleren of deze configuratie betekent dat de geëxporteerde database wordt geïmporteerd met de lokale database automatisch na het exporteren is voltooid.|

![Wizardconfiguratie voor de Database van Data Lake Analytics hulpprogramma's exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Stap 4: Controleer de resultaten exporteren

Na alle deze instellingen en de voortgang van de uitvoer vindt u de geëxporteerde resultaten van het logboekvenster in de wizard. Via het logboek door rode rechthoek in bLaag schermafbeelding gemarkeerd, kunt u de locatie van de geëxporteerde U-SQL-script en database resources inclusief assembly's, aanvullende bestanden en voorbeeldgegevens vinden.

![Data Lake Analytics extra Export Database-Wizard voltooid](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="how-to-import-the-exported-database-to-local-account"></a>Het importeren van de geëxporteerde database naar een lokaal account

Het controleren van de gemakkelijkste manier om te doen dit importeren **importeren in de Database automatisch lokale** tijdens de voortgang van de uitvoer in stap 3. Als u niet vergeten, kunt u zoeken naar het geëxporteerde U-SQL-script via het logboek voor uitvoer en voer het script U-SQL lokaal voor het importeren van de database in uw lokale account.

## <a name="how-to-import-the-exported-database-to-azure-data-lake-analytics-account"></a>Het importeren van de geëxporteerde database naar Azure Data Lake Analytics-account

Voor het importeren van de database aan andere Azure Data Lake Analytics-account, moet u twee stappen:

1. Upload de geëxporteerde resources inclusief assembly's, aanvullende bestanden en voorbeeldgegevens naar het Azure Data Lake Store-standaardaccount van het Azure Data Lake Analytics-account dat u wilt importeren. U kunt zoeken naar de resourcemap geëxporteerde onder de hoofdmap van de lokale gegevens en de hele map uploaden naar de hoofdmap van de store-standaardaccount.
2. Verzenden van het geëxporteerde U-SQL-script op voor het Azure Data Lake Analytics-account dat u wilt importeren van database na het uploaden van voltooid.

## <a name="known-limitation"></a>Bekende beperking

Op dit moment als u hebt geselecteerd **Schema en de gegevens** in de wizard een U-SQL-taak voor het exporteren van de gegevens die zijn opgeslagen in de tabellen het hulpprogramma wordt uitgevoerd. Daarom is de gegevens exporteren proces kan traag verlopen en de kosten in rekening gebracht. 

## <a name="next-steps"></a>Volgende stappen

* [Begrijpt U-SQL-database](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [het testen en foutopsporing van U-SQL-taken met behulp van lokaal uitvoeren en de Azure Data Lake U-SQL-SDK](data-lake-analytics-data-lake-tools-local-run.md)


