---
title: Groepen van Azure SQL-databases beheren | Microsoft Docs
description: Helpen bij het maken en beheren van een elastische taak.
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: sstein
ms.openlocfilehash: 8c83513838170d18cfc2f1d4f7cd42df2ec617e1
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44717485"
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Maken en beheren van schaal van Azure SQL-databases met elastische taken (preview)


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


**Taken voor elastic Database** Vereenvoudig het beheer van groepen van databases door het uitvoeren van administratieve bewerkingen, zoals wijzigingen in het schema, beheren van referenties, bijwerken van verwijzingsgegevens, verzamelen van prestatiegegevens of telemetrie van tenants (klanten) verzameling. Taken voor elastic Database is momenteel beschikbaar via Azure portal en PowerShell-cmdlets. Echter de Azure portal oppervlakken verminderde functionaliteit beperkt tot het uitvoeren van alle databases in een [elastische pool](sql-database-elastic-pool.md). Instellen voor toegang tot extra functies en uitvoering van scripts voor een groep databases met inbegrip van een aangepaste verzameling of een shard (gemaakt met behulp van [Elastic Database-clientbibliotheek](sql-database-elastic-scale-introduction.md)), Zie [maken en beheren taken met behulp van PowerShell](sql-database-elastic-jobs-powershell.md). Zie voor meer informatie over taken [overzicht van de taken van Elastic Database](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Zie voor een gratis proefversie [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een elastische pool. Zie [over elastische pools](sql-database-elastic-pool.md).
* Installatie van de elastische taak serviceonderdelen. Zie [installatie van de elastische taak service](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Het maken van taken
1. Met behulp van de [Azure-portal](https://portal.azure.com), van een bestaande taak pool voor elastische database, klikt u op **maken taak**.
2. Typ de gebruikersnaam en het wachtwoord van de beheerder van de database (gemaakt bij de installatie van taken) voor de taken database (metagegevensopslag voor taken).
   
    ![Naam van de taak, typ of plak in de code en klik op uitvoeren][1]
3. In de **taak maken** blade, typ een naam voor de taak.
4. Typ de gebruikersnaam en wachtwoord voor het verbinding maken met de doeldatabases op met voldoende machtigingen voor de uitvoering van het script te voltooien.
5. Plak of typ in de T-SQL-script.
6. Klik op **opslaan** en klik vervolgens op **uitvoeren**.
   
    ![Taken maken en uitvoeren][5]

## <a name="run-idempotent-jobs"></a>Idempotent-taken uitvoeren
Als u een script in een set van databases uitvoert, moet u ervoor dat het script idempotent is zijn. Dat wil zeggen, moet het script worden meerdere keren uitvoeren, zelfs als deze is mislukt voordat een onvoltooide status. Bijvoorbeeld, wanneer u een script is mislukt, de taak wordt automatisch opnieuw geprobeerd totdat het is gelukt (binnen de grenzen, als de nieuwe poging logica wordt uiteindelijk niet meer het opnieuw proberen). De manier om dit te doen is met het een component 'IF bestaat' en alle gevonden instantie verwijderen voordat u een nieuw object maakt. Hier ziet u een voorbeeld:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Of gebruik een component 'Bestaat niet als' voordat u een nieuwe instantie:

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

Met dit script werkt vervolgens de tabel die u eerder hebt gemaakt.

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

1. Klik op de pagina elastische pool **taken beheren**.
   
    ![Klik op "Beheren taken"][2]
2. Klik op de naam (a) van een taak. De **STATUS** mag "Voltooid" of "Mislukt". (B) weergegeven de details van de taak met de datum en tijd van maken en uitvoeren. De lijst (c) onder de die toont de voortgang van het script voor elke database in de pool, geeft de details van datum en tijd.
   
    ![Een voltooide taak controleren][3]

## <a name="checking-failed-jobs"></a>Controleren op mislukte taken
Als een taak mislukt, wordt een logboek van de uitvoering ervan te vinden. Klik op de naam van de mislukte taak om de details te bekijken.

![Een mislukte taak controleren][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


