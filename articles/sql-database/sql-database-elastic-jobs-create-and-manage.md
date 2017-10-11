---
title: Groepen van Azure SQL-databases beheren | Microsoft Docs
description: Maken en beheren van een taak elastische doorlopen.
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d30cc74778e0b36dd632c2f040ce80d1ca8af5a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Maken en beheren van geschaalde uit Azure SQL-Databases met elastische taken (preview)


**Elastische Database taken** vereenvoudigd beheer van groepen van databases door het uitvoeren van administratieve bewerkingen, zoals wijzigingen in het schema, Referentiebeheer, verwijzing Gegevensupdates, prestatiegegevens verzamelen of telemetrie van de tenant (klant) verzameling. Elastische taken van de Database is momenteel beschikbaar is via de Azure-portal en de PowerShell-cmdlets. Echter, de Azure portal vlakken verminderde functionaliteit beschikken beperkt tot uitvoering voor alle databases in een [elastische pool (preview)](sql-database-elastic-pool.md). Toegang tot extra functies en uitvoering van scripts in een groep van databases met inbegrip van een aangepaste verzameling of een shard ingesteld (gemaakt met behulp van [clientbibliotheek voor elastische Database](sql-database-elastic-scale-introduction.md)), Zie [maken en beheren taken met behulp van PowerShell](sql-database-elastic-jobs-powershell.md). Zie voor meer informatie over taken [elastische Database taken overzicht](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie voor een gratis proefversie [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een elastische pool. Zie [over elastische pools](sql-database-elastic-pool.md).
* Installatie van de onderdelen van de service taak elastische database. Zie [installeren van de service van de taak elastische database](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Taken maken
1. Met behulp van de [Azure-portal](https://portal.azure.com), van een bestaande taak pool voor elastische database, klikt u op **maken taak**.
2. Typ de gebruikersnaam en wachtwoord van de beheerder van de database (gemaakt tijdens de installatie van taken) voor de database taken (metagegevensopslag voor taken).
   
    ![De taak een naam, typ of plak in de code en klik op uitvoeren][1]
3. In de **taak maken** blade, typ een naam voor de taak.
4. Typ de gebruikersnaam en wachtwoord verbinding maken met de doeldatabases op met voldoende machtigingen voor het uitvoeren van scripts.
5. Plakken of typ in het T-SQL-script.
6. Klik op **opslaan** en klik vervolgens op **uitvoeren**.
   
    ![Taken maken en uitvoeren][5]

## <a name="run-idempotent-jobs"></a>De idempotent-taken uitvoeren
Wanneer u een script op een set databases uitvoeren, moet u ervoor dat het script idempotent zijn. Dat wil zeggen, moet het script mogelijk meerdere keren uitvoeren, zelfs als deze is mislukt voordat een onvoltooide status. Bijvoorbeeld, een script is mislukt, de taak wordt automatisch herhaald totdat dit is gelukt (binnen de limieten, als de nieuwe poging logica uiteindelijk stopt het opnieuw proberen). De manier om dit te doen is met het een component 'IF bestaat' en een willekeurig exemplaar gevonden verwijderen voordat u een nieuw object maakt. Hier ziet u een voorbeeld:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Voordat u een nieuw exemplaar in plaats daarvan een 'Bestaat niet als'-component gebruiken:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Dit script werkt vervolgens de tabel die eerder is gemaakt.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Taakstatus controleren
Nadat een taak is gestart, kunt u de voortgang controleren.

1. Klik op de pagina elastische pool **beheren van taken**.
   
    ![Klik op 'Taken beheren'][2]
2. Klik op de naam (a) van een taak. De **STATUS** mag 'Voltooid' of 'Mislukt'. Details van de taak weergegeven (b) met de datum en tijd van het maken en uitvoeren. De onderstaande lijst met (c) dat geeft de voortgang van het script dat op elke database in de groep, geeft de details van de datum en tijd.
   
    ![Een voltooide taak controleren][3]

## <a name="checking-failed-jobs"></a>Controleren van de mislukte taken
Als een taak mislukt, kan een logboek van de uitvoering ervan kan worden gevonden. Klik op de naam van de mislukte taak om de details te bekijken.

![Een mislukte taak controleren][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


