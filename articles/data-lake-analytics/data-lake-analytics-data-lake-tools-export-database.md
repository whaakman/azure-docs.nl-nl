---
title: Exporteren van een U-SQL-database met behulp van Azure Data Lake Tools voor Visual Studio
description: Informatie over het gebruik van Azure Data Lake Tools voor Visual Studio een U-SQL-database exporteert en importeert u het automatisch naar een lokaal account.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 11/27/2017
ms.openlocfilehash: e4eea3cb4b16460c7e17bb6575c4e6cf8dda5a0a
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047251"
---
# <a name="export-a-u-sql-database"></a>Een U-SQL-database exporteren

In dit artikel leert u hoe u [Azure Data Lake Tools voor Visual Studio](http://aka.ms/adltoolsvs) voor het exporteren van een U-SQL-database als een enkel U-SQL-script en gedownloade resources. U kunt de geëxporteerde database importeren in een lokaal account in hetzelfde proces.

Klanten beheren meestal meerdere omgevingen gebruiken voor ontwikkeling, testen en productie. Deze omgevingen worden gehost op zowel een lokaal account, op de lokale computer van een ontwikkelaar en in een Azure Data Lake Analytics-account in Azure. 

Bij het ontwikkelen en af te stemmen U-SQL-query's in een ontwikkel- en testomgevingen, moeten ontwikkelaars vaak hun werk in een productiedatabase opnieuw te maken. De Wizard voor het exporteren van Database helpt dit proces versnellen. Met behulp van de wizard, kunnen ontwikkelaars de bestaande databaseomgeving en de voorbeeldgegevens om andere Data Lake Analytics-accounts te klonen.

## <a name="export-steps"></a>Stappen exporteren

### <a name="step-1-export-the-database-in-server-explorer"></a>Stap 1: De database in Server Explorer exporteren

Alle Data Lake Analytics-accounts die u hebt machtigingen voor worden weergegeven in Server Explorer. Voor het exporteren van de database:

1. Vouw in Server Explorer, het account dat met de database die u wilt exporteren.
2. Met de rechtermuisknop op de database en selecteer vervolgens **exporteren**. 
   
    ![Server Explorer - een database exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     Als de **exporteren** menu-optie niet beschikbaar is, moet u [bijwerken van het hulpprogramma naar de meest recente versie release](http://aka.ms/adltoolsvs).

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>Stap 2: Configureer de objecten die u wilt exporteren

Als u slechts een klein deel van een grote database, kunt u een subset van objecten die u wilt exporteren in de wizard Exporteren kunt configureren. 

De export-actie is voltooid door het uitvoeren van een U-SQL-taak. Daarom exporteren vanuit een Azure-account leidt tot enige kosten.

![Wizard voor het exporteren van database - Selecteer objecten exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>Stap 3: Controleer de lijst met objecten en andere configuraties

In deze stap controleert u of de geselecteerde objecten in de **Export objectenlijst** vak. Als er fouten zijn, selecteert u **vorige** teruggaan en de objecten die u wilt exporteren de juiste wijze configureren.

U kunt ook andere instellingen voor de doel-export configureren. Beschrijvingen van de configuratie worden weergegeven in de volgende tabel:

|Configuratie|Beschrijving|
|-------------|-----------|
|Doelnaam|Deze naam wordt aangegeven waar u de geëxporteerde database-resources. Voorbeelden zijn assembly's, aanvullende bestanden en voorbeeldgegevens. Een map met deze naam wordt gemaakt onder de hoofdmap van uw lokale gegevens.|
|Projectmap|Dit pad definieert waar u het geëxporteerde U-SQL-script. Alle objectdefinities van de database worden opgeslagen op deze locatie.|
|Alleen schema|Als u deze optie selecteert, zijn de enige databasedefinities en resources (zoals assembly's en aanvullende bestanden) worden geëxporteerd.|
|Schema en gegevens|Als u deze optie selecteert, zijn de databasedefinities, resources en gegevens worden geëxporteerd. De bovenste N rijen van tabellen worden geëxporteerd.|
|Automatisch naar lokale-Database importeren|Als u deze optie selecteert, wordt de geëxporteerde database wordt automatisch geïmporteerd om de lokale database bij het exporteren is voltooid.|

![Wizard Database exporteren - lijst met objecten voor exporteren en andere configuraties](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Stap 4: Controleer de resultaten exporteren

Bij het exporteren is voltooid, kunt u de geëxporteerde resultaten weergeven in het logboekvenster in de wizard. Het volgende voorbeeld laat zien hoe geëxporteerde U-SQL script en de database om Gebruikersbronnen te vinden, met inbegrip van assembly's, aanvullende bestanden en voorbeeldgegevens:

![Wizard Database exporteren - resultaten exporteren](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>De geëxporteerde database importeren in een lokaal account

De gemakkelijkste manier om de geëxporteerde database importeren om te selecteren wordt de **importeren naar een lokale Database automatisch** selectievakje tijdens het exporteren in stap 3. Als u dit selectievakje niet inschakelt, moet u eerst het geëxporteerde U-SQL-script in het logboek voor uitvoer vinden. Voer vervolgens de U-SQL-script lokaal voor het importeren van de database in uw lokale account.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>De geëxporteerde database importeren in een Data Lake Analytics-account

Voor het importeren van de database naar een ander Data Lake Analytics-account:

1. De geëxporteerde resources, met inbegrip van assembly's, aanvullende bestanden en gegevens naar de Azure Data Lake Store-standaardaccount van het Data Lake Analytics-account dat u importeren wilt naar uploaden. Hier vindt u de geëxporteerde resource-map onder de hoofdmap van de lokale gegevens. De hele map uploaden naar de hoofdmap van het Data Lake Store-standaardaccount.
2. Bij het uploaden is voltooid, verzend het geëxporteerde U-SQL-script naar het Data Lake Analytics-account dat u wilt importeren van de database.

## <a name="known-limitations"></a>Bekende beperkingen

Op dit moment als u selecteert de **Schema en gegevens** optie in stap 3, het hulpprogramma wordt uitgevoerd een U-SQL-taak voor het exporteren van de gegevens die zijn opgeslagen in tabellen. De gegevens exporteren van proces wordt mogelijk traag als gevolg hiervan, en worden er mogelijk kosten. 

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over U-SQL-databases](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [U-SQL-taken testen en controleren op fouten met behulp van lokale uitvoering en de Azure Data Lake U-SQL-SDK](data-lake-analytics-data-lake-tools-local-run.md)


