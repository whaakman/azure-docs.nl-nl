---
title: Exporteren van een U-SQL-database met Azure Data Lake Tools voor Visual Studio | Microsoft Docs
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio automatisch op een lokale account importeren en exporteren van een U-SQL-database.
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
ms.openlocfilehash: 441606258f9541c9552925e7c0cbc9b3a9effb4d
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="export-a-u-sql-database"></a>Exporteren van een U-SQL-database

Informatie over het gebruik in dit artikel [Azure Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs) te exporteren van een U-SQL-database als een enkel U-SQL-script en gedownloade resources. U kunt de geëxporteerde database importeren naar een lokaal account in hetzelfde proces.

Klanten beheren meestal meerdere omgevingen voor ontwikkeling, testen en productie. Deze omgevingen worden gehost op zowel een lokale account op de lokale computer een ontwikkelaar en in een Azure Data Lake Analytics-account in Azure. 

Bij het ontwikkelen en het afstemmen van U-SQL-query's in ontwikkeling en testomgevingen moeten ontwikkelaars vaak opnieuw maken van hun werk in een productiedatabase. De Wizard voor het exporteren van Database kunt dit proces versnellen. Ontwikkelaars kunnen met behulp van de wizard kloon de bestaande databaseomgeving en de voorbeeldgegevens aan andere Data Lake Analytics-accounts.

## <a name="export-steps"></a>Stappen exporteren

### <a name="step-1-export-the-database-in-server-explorer"></a>Stap 1: De database in Server Explorer exporteren

Alle Data Lake Analytics-accounts die u machtigingen hebt voor worden vermeld in Server Explorer. Exporteren van de database:

1. Vouw in Server Explorer het account dat met de database die u wilt exporteren.
2. Met de rechtermuisknop op de database en selecteer vervolgens **exporteren**. 
   
    ![Server Explorer - een database exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     Als de **exporteren** menuoptie niet beschikbaar is, moet u [het hulpprogramma bijwerkt naar de meest recente versie versie](http://aka.ms/adltoolsvs).

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>Stap 2: Configureer de objecten die u wilt exporteren

Als u slechts een klein deel van een grote database, kunt u een subset van objecten die u wilt exporteren in de wizard Exporteren kunt configureren. 

De export-actie is voltooid door een U-SQL-taak wordt uitgevoerd. Exporteren van een Azure-account maakt daarom enkele kosten.

![Wizard voor het exporteren van database - Selecteer objecten exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>Stap 3: Controleer de lijst met objecten en andere configuraties

In deze stap controleert u of de geselecteerde objecten in de **Export objectenlijst** vak. Als er fouten zijn, selecteert u **vorige** teruggaan en de objecten die u wilt exporteren de juiste wijze configureren.

U kunt ook andere instellingen voor de export-doel configureren. Beschrijvingen van de configuratie worden vermeld in de volgende tabel:

|Configuratie|Beschrijving|
|-------------|-----------|
|Doelnaam|Deze naam geeft aan waar u de geëxporteerde database-resources. Voorbeelden zijn assembly's, aanvullende bestanden en voorbeeldgegevens. Een map met deze naam wordt gemaakt onder de hoofdmap van uw lokale gegevens.|
|Projectmap|Dit pad definieert waar u het geëxporteerde U-SQL-script. Alle objectdefinities van de database worden opgeslagen op deze locatie.|
|Alleen het schema|Als u deze optie selecteert, worden alleen de databasedefinities en bronnen (zoals assembly's en aanvullende bestanden) geëxporteerd.|
|Schema en de gegevens|Als u deze optie selecteert, zijn de databasedefinities, bronnen en gegevens worden geëxporteerd. De bovenste N rijen van tabellen worden geëxporteerd.|
|Automatisch importeren voor lokale-Database|Als u deze optie selecteert, wordt de geëxporteerde database wordt automatisch geïmporteerd in uw lokale database bij het exporteren is voltooid.|

![Wizard Database exporteren - lijst met objecten voor exporteren en andere configuraties](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Stap 4: Controleer de resultaten exporteren

Bij het exporteren is voltooid, kunt u de geëxporteerde resultaten weergeven in het logboekvenster in de wizard. Het volgende voorbeeld ziet u hoe zoeken naar geëxporteerde U-SQL-script en database bronnen, met inbegrip van assembly's, aanvullende bestanden en voorbeeldgegevens:

![Wizard voor het exporteren van database - resultaten exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>De geëxporteerde database importeren naar een lokaal account

De gemakkelijkste manier om te importeren van de geëxporteerde database is het selecteren van de **importeren naar een lokale Database automatisch** selectievakje tijdens het exportproces in stap 3. Als u dit selectievakje niet inschakelt, moet u eerst het geëxporteerde U-SQL-script vinden in het logboek voor uitvoer. Voer vervolgens de U-SQL-script lokaal voor het importeren van de database in uw lokale account.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>De geëxporteerde database importeren naar een Data Lake Analytics-account

Voor het importeren van de database naar andere Data Lake Analytics-account:

1. Upload de geëxporteerde resources, met inbegrip van assembly's, aanvullende bestanden en voorbeeldgegevens naar het Azure Data Lake Store-standaardaccount van het Data Lake Analytics-account dat u wilt importeren. U vindt de geëxporteerde resourcemap onder de hoofdmap van de lokale gegevens. Upload de hele map naar de hoofdmap van het Data Lake Store-standaardaccount.
2. Wanneer het uploaden is voltooid, verzenden van het geëxporteerde U-SQL-script op voor het Data Lake Analytics-account dat u wilt importeren van de database.

## <a name="known-limitations"></a>Bekende beperkingen

Op dit moment als u selecteert de **Schema en de gegevens** optie in stap 3, het hulpprogramma wordt uitgevoerd een U-SQL-taak voor het exporteren van de gegevens die zijn opgeslagen in de tabellen. De gegevens exporteren proces wordt mogelijk traag als gevolg hiervan, en u kosten met zich kan brengen. 

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over U-SQL-databases](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [U-SQL-taken testen en controleren op fouten met behulp van lokale uitvoering en de Azure Data Lake U-SQL-SDK](data-lake-analytics-data-lake-tools-local-run.md)


