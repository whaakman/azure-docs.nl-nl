---
title: "Kopiëren van een Azure SQL database | Microsoft Docs"
description: Maak een kopie van een Azure SQL database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5aaf6bcd-3839-49b5-8c77-cbdf786e359b
ms.service: sql-database
ms.custom: load & move data
ms.devlang: NA
ms.date: 06/15/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 8c1e3c80b9f24089dc99463d6ea8ae5d0ea7b19d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="copy-an-azure-sql-database"></a>Een Azure SQL database kopiëren

Azure SQL Database biedt verschillende methoden voor het maken van een transactioneel consistent kopie van een bestaande Azure SQL-database op dezelfde server of een andere server. U kunt een SQL-database kopiëren met behulp van de Azure-portal, PowerShell of T-SQL. 

## <a name="overview"></a>Overzicht

Een databasekopie is een momentopname van de brondatabase vanaf het moment van de aanvraag van de kopie. U kunt dezelfde server of een andere server, de servicelaag en prestatieniveau of een andere prestatieniveau binnen de dezelfde servicetier (editie) selecteren. Nadat de kopie voltooid is, wordt een volledig functionele, onafhankelijke database. U kunt op dit moment upgrade of het downgraden naar een versie. De aanmeldingen, gebruikers en machtigingen kunnen afzonderlijk worden beheerd.  

## <a name="logins-in-the-database-copy"></a>Aanmeldingen in de database-exemplaar

Wanneer u een database met dezelfde logische server kopieert, kunnen de dezelfde aanmeldingen op beide databases worden gebruikt. De beveiligings-principal dat u gebruiken om te kopiëren van de database wordt de eigenaar van de database op de nieuwe database. Alle databasegebruikers en hun machtigingen hun beveiligings-id's (SID's) gekopieerd naar het database-exemplaar.  

Wanneer u een database naar een andere logische server kopiëren, wordt de beveiligings-principal op de nieuwe server in de database-eigenaar voor de nieuwe database. Als u [databasegebruikers opgenomen](sql-database-manage-logins.md) voor toegang tot gegevens, zorg ervoor dat de primaire en secundaire databases altijd dezelfde gebruikersreferenties zodat nadat de kopie voltooid is u onmiddellijk toegang met dezelfde aanmeldingsgegevens. 

Als u [Azure Active Directory](../active-directory/active-directory-whatis.md), kunt u de noodzaak voor het beheren van referenties in de kopie volledig elimineren. Echter, wanneer u de database naar een nieuwe server kopiëren, de aanmelding gebaseerde toegang werkt mogelijk niet, omdat de aanmeldingen niet bestaan op de nieuwe server. Zie voor meer informatie over het beheren van aanmeldingen wanneer u een database naar een andere logische server kopiëren, [beveiliging voor Azure SQL database beheren na het herstel na noodgevallen](sql-database-geo-replication-security-config.md). 

Nadat het kopiëren is geslaagd en voordat andere gebruikers worden toegewezen, kunnen alleen de aanmelding die geïnitieerd het kopiëren, de eigenaar van de database zich kunt aanmelden bij de nieuwe database. Om op te lossen aanmeldingen nadat het kopiëren voltooid is, Zie [aanmeldingen oplossen](#resolve-logins).

## <a name="copy-a-database-by-using-the-azure-portal"></a>Kopiëren van een database met behulp van de Azure-portal

Als u wilt kopiëren van een database met behulp van de Azure-portal, open de pagina voor de database en klik vervolgens op **kopie**. 

   ![Database-exemplaar](./media/sql-database-copy/database-copy.png)

## <a name="copy-a-database-by-using-powershell"></a>Een database met behulp van PowerShell kopiëren

Om te kopiëren van een database met behulp van PowerShell, gebruikt de [nieuw AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) cmdlet. 

```PowerShell
New-AzureRmSqlDatabaseCopy -ResourceGroupName "myResourceGroup" `
    -ServerName $sourceserver `
    -DatabaseName "MySampleDatabase" `
    -CopyResourceGroupName "myResourceGroup" `
    -CopyServerName $targetserver `
    -CopyDatabaseName "CopyOfMySampleDatabase"
```

Zie voor een volledige voorbeeldscript [een database kopiëren naar een nieuwe server](scripts/sql-database-copy-database-to-new-server-powershell.md).

## <a name="copy-a-database-by-using-transact-sql"></a>Kopiëren van een database met behulp van Transact-SQL

Aanmelden bij de database master met de principal-aanmelding op serverniveau of de aanmelding die gemaakt van de database die u wilt kopiëren. Aanmeldingen die niet het niveau van de server principal moet voor de database kopiëren te laten slagen, leden van de rol dbmanager. Zie voor meer informatie over aanmeldingen en verbinding maken met de server [aanmeldingen beheren](sql-database-manage-logins.md).

Beginnen met het kopiëren van de brondatabase met de [CREATE DATABASE](https://msdn.microsoft.com/library/ms176061.aspx) instructie. Deze instructie uitvoert, wordt de database kopiëren proces gestart. Omdat het kopiëren van een database een asynchrone is, retourneert de instructie CREATE DATABASE voordat het kopiëren van de database voltooid is.

### <a name="copy-a-sql-database-to-the-same-server"></a>Een SQL-database kopiëren naar dezelfde server
Aanmelden bij de database master met de principal-aanmelding op serverniveau of de aanmelding die gemaakt van de database die u wilt kopiëren. Aanmeldingen die niet het niveau van de server principal moet voor de database kopiëren te laten slagen, leden van de rol dbmanager.

Met deze opdracht wordt Database1 gekopieerd naar een nieuwe database met de naam Database2 op dezelfde server. Afhankelijk van de grootte van uw database, kan de kopiëren bewerking even duren om te voltooien.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Een SQL-database kopiëren naar een andere server

Aanmelden bij de database master van de doelserver, de SQL-databaseserver waar de nieuwe database wordt gemaakt. Gebruik een aanmelding met dezelfde naam en hetzelfde wachtwoord als de database-eigenaar van de brondatabase op de bronserver voor de SQL-database. De aanmelding op de doelserver moet ook lid zijn van de rol dbmanager of worden de principal-aanmelding op serverniveau.

Deze opdracht kopieert Database1 op server1 naar een nieuwe database met de naam Database2 op server2. Afhankelijk van de grootte van uw database, kan de kopiëren bewerking even duren om te voltooien.

    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;


### <a name="monitor-the-progress-of-the-copying-operation"></a>De voortgang van de bewerking uit te kopiëren

Het kopieerproces door het opvragen van de weergaven sys.databases en sys.dm_database_copies controleren. Terwijl het kopiëren uitgevoerd wordt, de **state_desc** kolom van de sys.databases voor de nieuwe database is ingesteld op **kopiëren**.

* Als het kopiëren is mislukt, de **state_desc** kolom van de sys.databases voor de nieuwe database is ingesteld op **VERMOEDT**. De instructie DROP niet uitvoeren op de nieuwe database en probeer het later opnieuw.
* Als het kopiëren is gelukt, de **state_desc** kolom van de sys.databases voor de nieuwe database is ingesteld op **ONLINE**. Het kopiëren is voltooid en de nieuwe database is een reguliere-database die onafhankelijk van de brondatabase kan worden gewijzigd.

> [!NOTE]
> Uitvoeren als u besluit om te annuleren kopiëren terwijl deze uitgevoerd wordt, de [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) instructie op de nieuwe database. U kunt ook annuleert de instructie DROP DATABASE worden uitgevoerd op de brondatabase ook het kopieerproces.
> 

## <a name="resolve-logins"></a>Aanmeldingen oplossen

Nadat de nieuwe database op de doelserver online is, gebruikt u de [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) instructie opnieuw toewijzen van de gebruikers van de nieuwe database aan aanmeldingen op de doelserver. Raadpleeg voor het oplossen van zwevende gebruikers [zwevende gebruikers dat problemen met](https://msdn.microsoft.com/library/ms175475.aspx). Zie ook [beveiliging voor Azure SQL database beheren na het herstel na noodgevallen](sql-database-geo-replication-security-config.md).

Alle gebruikers in de nieuwe database behouden de machtigingen die ze hebben als in de brondatabase. De gebruiker die het database-exemplaar gestart wordt de database-eigenaar van de nieuwe database en een nieuwe beveiligings-id (SID) is toegewezen. Nadat het kopiëren is geslaagd en voordat andere gebruikers worden toegewezen, kunnen alleen de aanmelding die geïnitieerd het kopiëren, de eigenaar van de database zich kunt aanmelden bij de nieuwe database.

Zie voor meer informatie over het beheren van gebruikers en aanmeldingen wanneer u een database naar een andere logische server kopiëren, [beveiliging voor Azure SQL database beheren na het herstel na noodgevallen](sql-database-geo-replication-security-config.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over aanmeldingen [aanmeldingen beheren](sql-database-manage-logins.md) en [beveiliging voor Azure SQL database beheren na het herstel na noodgevallen](sql-database-geo-replication-security-config.md).
* Zie voor het exporteren van een database [exporteren van de database naar een BACPAC](sql-database-export.md).
