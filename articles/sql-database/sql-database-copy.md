---
title: Een Azure SQL database kopiëren | Microsoft Docs
description: Maak transactioneel consistente kopie van een bestaande Azure SQL-database op dezelfde server of een andere server.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 2ce86bee96f22b4079afee3eacbeee7ea15a6ffa
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226004"
---
# <a name="copy-an-azure-sql-database"></a>Een Azure SQL database kopiëren

Azure SQL Database biedt verschillende methoden voor het maken van een transactioneel consistente kopie van een bestaande Azure SQL-database op dezelfde server of een andere server. U kunt een SQL-database kopiëren met behulp van de Azure portal, PowerShell of T-SQL. 

## <a name="overview"></a>Overzicht

Een databasekopie is een momentopname van de brondatabase vanaf het moment van de kopie-aanvraag. U kunt dezelfde server of een andere server, de servicelaag en compute-grootte of een andere compute-grootte binnen dezelfde servicelaag (edition) selecteren. Nadat de kopie voltooid is, wordt een volledig functionele, onafhankelijke database. Op dit moment kunt u upgraden en downgraden deze op elke editie. De aanmeldingen, gebruikers en machtigingen kunnen onafhankelijk van elkaar worden beheerd.  

## <a name="logins-in-the-database-copy"></a>Aanmeldingen in de database-exemplaar

Wanneer u een database naar dezelfde logische server kopiëren, kunnen de dezelfde aanmeldingen op beide databases worden gebruikt. De beveiligings-principal dat u gebruiken om te kopiëren van de database wordt de eigenaar van de database op de nieuwe database. Alle databasegebruikers, de machtigingen en de beveiligings-id's (SID's) worden gekopieerd naar het database-exemplaar.  

Wanneer u een database naar een andere logische server kopiëren, wordt de beveiligings-principal op de nieuwe server de eigenaar van de database op de nieuwe database. Als u [ingesloten databasegebruikers](sql-database-manage-logins.md) voor gegevenstoegang, zorg ervoor dat de primaire en secundaire databases altijd de referenties van de dezelfde gebruiker, zodat nadat de kopie voltooid is u onmiddellijk toegang hebben, met dezelfde referenties . 

Als u [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md), u kunt volledig elimineert de noodzaak voor het beheren van referenties in de kopie. Echter, wanneer u de database naar een nieuwe server kopieert, de toegang op basis van aanmelding werkt mogelijk niet, omdat de aanmeldingen bestaan niet op de nieuwe server. Zie voor meer informatie over het beheren van aanmeldingen wanneer u een database naar een andere logische server kopiëren, [over het beheren van Azure SQL database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md). 

Nadat het kopiëren is voltooid en voordat andere gebruikers zijn toegewezen, kunnen alleen de aanmelding die geïnitieerd het kopiëren, de eigenaar van de database zich kunt aanmelden bij de nieuwe database. Om op te lossen aanmeldingen nadat de bewerking kopiëren voltooid is, Zie [oplossen aanmeldingen](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Een database kopiëren met behulp van Azure portal

Als u wilt kopiëren van een database met behulp van Azure portal, open de pagina voor uw database en klik vervolgens op **kopie**. 

   ![Database-exemplaar](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Een database kopiëren met behulp van PowerShell

Als u wilt kopiëren van een database met behulp van PowerShell, gebruikt u de [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet. 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Zie voor een compleet voorbeeld van een script, [een database kopiëren naar een nieuwe server](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Een database kopiëren met behulp van Transact-SQL

Meld u aan bij de database master met de principal-aanmelding op serverniveau of de aanmelding die het maken van de database die u wilt kopiëren. Voor de database kopiëren om te kunnen slagen, moeten aanmeldingen die niet de principal op serverniveau lid zijn van de rol dbmanager. Zie voor meer informatie over aanmeldingen en verbinding maken met de server, [aanmeldingen beheren](sql-database-manage-logins.md).

Beginnen met het kopiëren van de brondatabase met de [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) instructie. Deze instructie uitvoert, wordt de database kopiëren proces gestart. Omdat een database kopieert een asynchroon proces is, retourneert de instructie CREATE DATABASE is voordat het kopiëren van de database voltooid is.

### <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database kopiëren naar dezelfde server
Meld u aan bij de database master met de principal-aanmelding op serverniveau of de aanmelding die het maken van de database die u wilt kopiëren. Voor de database kopiëren om te kunnen slagen, moeten aanmeldingen die niet de principal op serverniveau lid zijn van de rol dbmanager.

Met deze opdracht worden Database1 gekopieerd naar een nieuwe database met de naam Database2 op dezelfde server. De bewerking kopiëren kan enige tijd duren, afhankelijk van de grootte van uw database.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database2 AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Een SQL-database kopiëren naar een andere server

Meld u aan bij de hoofddatabase van de doelserver, de SQL database-server waar de nieuwe database wordt gemaakt. Gebruik een aanmelding met dezelfde naam en hetzelfde wachtwoord als de database-eigenaar van de brondatabase op de bronserver van de SQL-database. De aanmelding op de doelserver moet ook lid zijn van de rol dbmanager of worden de principal-aanmelding op serverniveau.

Met deze opdracht kopieert Database1 op server1 naar een nieuwe database met de naam Database2 op server2. De bewerking kopiëren kan enige tijd duren, afhankelijk van de grootte van uw database.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database2 AS COPY OF server1.Database1;


### <a name="monitor-the-progress-of-the-copying-operation"></a>De voortgang van de bewerking kopiëren

Het kopieerproces bewaken door het opvragen van de sys.databases en sys.dm_database_copies weergaven. Terwijl het kopiëren uitgevoerd wordt, de **state_desc** kolom van de sys.databases voor de nieuwe database is ingesteld op **kopiëren**.

* Als het kopiëren is mislukt, de **state_desc** kolom van de sys.databases voor de nieuwe database is ingesteld op **VERMOEDT**. Voer de instructie DROP uit op de nieuwe database en probeer het later opnieuw.
* Als het kopiëren is voltooid, de **state_desc** kolom van de sys.databases voor de nieuwe database is ingesteld op **ONLINE**. Het kopiëren is voltooid en de nieuwe database is een normale database die onafhankelijk van de brondatabase kan worden gewijzigd.

> [!NOTE]
> Als u besluit om te annuleren de kopiëren terwijl deze uitgevoerd wordt, uitvoeren van de [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) instructie op de nieuwe database. U kunt ook annuleert de instructie DROP DATABASE wordt uitgevoerd op de brondatabase ook het kopieerproces.
> 

## <a name="resolve-logins"></a>Aanmeldingen oplossen

Nadat de nieuwe database op de doelserver online is, gebruikt u de [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) instructie opnieuw toewijzen van de gebruikers van de nieuwe database aanmeldingen op de doelserver. Raadpleeg voor het oplossen van zwevende gebruikers [zwevende gebruikers oplossen](https://msdn.microsoft.com/library/ms175475.aspx). Zie ook [over het beheren van Azure SQL database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).

Alle gebruikers in de nieuwe database behouden de machtigingen die ze in de brondatabase hebben. De gebruiker die het database-exemplaar wordt gestart, wordt de eigenaar van de database van de nieuwe database en een nieuwe beveiligings-id (SID) is toegewezen. Nadat het kopiëren is voltooid en voordat andere gebruikers zijn toegewezen, kunnen alleen de aanmelding die geïnitieerd het kopiëren, de eigenaar van de database zich kunt aanmelden bij de nieuwe database.

Zie voor meer informatie over het beheren van gebruikers en aanmeldgegevens wanneer u een database naar een andere logische server kopiëren, [over het beheren van Azure SQL database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over aanmeldingen [aanmeldingen beheren](sql-database-manage-logins.md) en [over het beheren van Azure SQL database-beveiliging na herstel na noodgevallen](sql-database-geo-replication-security-config.md).
* Zie voor het exporteren van een database, [exporteren van de database naar een BACPAC](sql-database-export.md).
