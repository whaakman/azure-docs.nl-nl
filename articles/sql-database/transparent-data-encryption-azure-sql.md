---
title: Transparante gegevensversleuteling voor Azure SQL Database en Data Warehouse | Microsoft Docs
description: Een overzicht van transparante gegevensversleuteling voor SQL-Database en Data Warehouse. Het document bevat informatie over de voordelen ervan en de opties voor de configuratie, waaronder service beheerde transparante gegevensversleuteling en Bring Your Own Key.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 94a3a502d5756d57cfebdf6698a4435dc1e19948
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353019"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>Transparante gegevensversleuteling voor SQL-Database en Data Warehouse

Transparante gegevensversleuteling (TDE) helpt Azure SQL Database en Azure Data Warehouse beveiligen tegen de dreiging van schadelijke activiteiten. Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing. TDE is standaard ingeschakeld voor alle geïmplementeerde Azure SQL-databases. TDE kan niet worden gebruikt voor het versleutelen van de logische **master** database in SQL-Database.  De **master** -database bevat objecten die nodig zijn om uit te voeren van de TDE-bewerkingen op de gebruikersdatabases.

TDE moet handmatig worden ingeschakeld voor oudere databases of Azure SQL Data Warehouse.  

Transparante gegevensversleuteling versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel met de naam de databaseversleutelingssleutel. Deze database versleutelingssleutel wordt beveiligd door de transparent data encryption-beveiliging. De beveiliging is een van beide een service beheerde certificaat (beheerde service transparante gegevensversleuteling) of een asymmetrische sleutel opgeslagen in Azure Key Vault (Bring Your Own Key). U kunt de transparent data encryption protector instellen op niveau van de server.

Bij het opstarten van de database, is de versleutelingssleutel van de versleutelde database ontsleuteld en wordt gebruikt voor ontsleuteling en het opnieuw versleutelen van de databasebestanden in het SQL Server Database Engine-proces. Transparante gegevensversleuteling voert realtime i/o-versleuteling en ontsleuteling van de gegevens op het paginaniveau van de. Elke pagina wordt ontsleuteld wanneer het is gelezen in het geheugen en vervolgens versleuteld voordat deze werd geschreven naar schijf. Zie voor een algemene beschrijving van transparante gegevensversleuteling, [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

Met SQL Server op een Azure-machine ook kunnen een asymmetrische sleutel uit Key Vault gebruiken. De configuratiestappen zijn anders dan het gebruik van een asymmetrische sleutel in SQL-Database. Zie voor meer informatie, [uitbreidbaar Sleutelbeheer met behulp van Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Beheerde service transparante gegevensversleuteling

In Azure is de standaardinstelling voor transparante gegevensversleuteling of de databaseversleutelingssleutel wordt beveiligd door een ingebouwde servercertificaat. Het certificaat van de ingebouwde is uniek voor elke server. Als een database in een relatie geo-replicatie, wordt de primaire en geo-secundaire database worden beveiligd door de primaire database bovenliggende SLC-sleutel. Als de twee databases zijn verbonden met dezelfde server, delen ze hetzelfde ingebouwde certificaat. Microsoft draait automatisch deze certificaten ten minste elke 90 dagen.

Microsoft ook naadloos worden verplaatst en de sleutels indien nodig voor geo-replicatie wordt beheerd en worden hersteld.

> [!IMPORTANT]
> Alle gemaakte SQL-databases worden standaard versleuteld met behulp van de service beheerde transparante gegevensversleuteling. Bestaande databases vóór mei 2017 en databases die zijn gemaakt via herstellen, geo-replicatie en database-exemplaar niet worden standaard versleuteld.

## <a name="bring-your-own-key"></a>Bring Your Own Key

Met Bring Your Own Key-ondersteuning, u kunt nemen controle over uw versleutelingssleutels transparante en bepalen wie er toegang toe hebben en wanneer. Key Vault, die de Azure cloud-gebaseerde externe sleutelbeheersysteem, is de eerste service voor sleutelbeheer die transparante gegevensversleuteling is geïntegreerd met Bring Your Own Key-ondersteuning. Met Bring Your Own Key-ondersteuning, wordt de databaseversleutelingssleutel beveiligd door een asymmetrische sleutel opgeslagen in Key Vault. De asymmetrische sleutel wordt nooit verlaat Key Vault. Nadat de server machtigingen voor een key vault heeft, verzendt de server basic sleutelbewerking aanvragen toe via Key Vault. U de asymmetrische sleutel instellen op het serverniveau van de en alle databases van die server overnemen.

Met Bring Your Own Key-ondersteuning, kunt u nu essentiële beheertaken zoals sleutelrotaties en key vault-machtigingen beheren. U kunt ook sleutels verwijderen en controle/melden op alle versleutelingssleutels inschakelen. Key Vault biedt centrale sleutelbeheer en maakt gebruik van nauw bewaakte hardware security modules. Key Vault wordt gescheiden van het beheer van sleutels en gegevens om te voldoen aan regelgeving. Zie voor meer informatie over Key Vault, de [Key Vault-documentatiepagina](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

Zie voor meer informatie over transparante gegevensversleuteling met Bring Your Own Key-ondersteuning voor SQL-Database en Data Warehouse, [Transparent data encryption met Bring Your Own Key support](transparent-data-encryption-byok-azure-sql.md).

Als u wilt gaan met transparent data encryption met Bring Your Own Key-ondersteuning, Zie de handleiding [transparante gegevensversleuteling inschakelen met behulp van uw eigen sleutel uit Key Vault met behulp van PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Een transparent data encryption beveiligde database verplaatsen

U hoeft niet te ontsleutelen van databases voor bewerkingen in Azure. De transparent data encryption-instellingen op de brondatabase of de primaire database worden transparant overgenomen op de doelcomputer. Bewerkingen die opgenomen zijn, hebben betrekking op:

- Geo-herstel
- Point-in-time Self-service voor herstellen
- Herstel van een verwijderde database
- Actieve geo-replicatie
- Het maken van een databasekopie

Wanneer u een database transparent data encryption beschermde exporteert, worden de geëxporteerde inhoud van de database is niet versleuteld. Dit geëxporteerde inhoud is opgeslagen in niet-versleutelde Bacpac-bestanden. Moet u het BACPAC-bestanden op de juiste wijze te beschermen en transparent data encryption inschakelen nadat het importeren van de nieuwe database is voltooid.

Bijvoorbeeld, als het BACPAC-bestand is geëxporteerd uit een on-premises SQL Server-exemplaar, wordt niet de geïmporteerde inhoud van de nieuwe database automatisch versleuteld. Op dezelfde manier als het BACPAC-bestand is geëxporteerd naar een on-premises SQL Server-exemplaar, wordt niet de nieuwe database ook automatisch versleuteld.

De enige uitzondering hierop is wanneer u naar en van een SQL-database exporteert. Transparante gegevensversleuteling is ingeschakeld in de nieuwe database, maar nog steeds het BACPAC-bestand zelf wordt niet versleuteld.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Transparante gegevensversleuteling in Azure portal beheren

Voor het configureren van transparante gegevensversleuteling via Azure portal, moet verbonden zijn als de Azure-eigenaar, bijdrager of SQL Security Manager.

U instellen transparante gegevensversleuteling op het databaseniveau van de. Om in te schakelen transparante gegevensversleuteling voor een database, gaat u naar de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-beheerder of Inzender. De transparent data encryption-instellingen onder de gebruikersdatabase vinden. Beheerde service transparante gegevensversleuteling wordt standaard gebruikt. Een transparent data encryption-certificaat wordt automatisch gegenereerd voor de server die de database bevat.

![Beheerde service transparante gegevensversleuteling](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

U kunt de hoofdsleutel transparent data encryption, ook wel bekend als de transparent data encryption protector instellen op niveau van de server. Zie voor het gebruik van transparante gegevensversleuteling met Bring Your Own Key-ondersteuning en beveiligen van uw databases met een sleutel uit Key Vault, de transparent data encryption-instellingen op uw server.

![Transparante gegevensversleuteling met Bring Your Own Key-ondersteuning](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Transparante gegevensversleuteling met behulp van PowerShell beheren

Voor het configureren van transparante gegevensversleuteling via PowerShell, moet verbonden zijn als de Azure-eigenaar, bijdrager of SQL Security Manager.

| Cmdlet | Beschrijving |
| --- | --- |
| [Set-AzureRmSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Hiermee schakelt transparante gegevensversleuteling voor een database of uit|
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Hiermee haalt u de transparent data encryption-status voor een database |
| [Get-Azure-Rm-Sql-Database-Transparent-Data-Encryption-Activity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Controleert of de voortgang van de versleuteling voor een database |
| [Voeg AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |Voegt een Key Vault-sleutel toe aan een SQL Server-exemplaar |
| [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |Haalt de Key Vault sleutels voor een SQL server-exemplaar  |
| [Set-azurermsqlservertransparentdataencryptionprotector is gewijzigd](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |Hiermee stelt u de transparent data encryption-beveiliging voor een SQL Server-exemplaar |
| [Get-azurermsqlservertransparentdataencryptionprotector is gewijzigd](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |De transparent data encryption protector opgehaald |
| [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |Hiermee verwijdert u een Key Vault-sleutel van een SQL Server-exemplaar |
|  | |

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Transparante gegevensversleuteling beheren met behulp van Transact-SQL

Verbinding maken met de database met behulp van een aanmelding die is een beheerder of lid zijn van de **dbmanager** rol in de database master.

| Opdracht | Beschrijving |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) | Aan van/op VERSLEUTELING uit ingesteld worden versleuteld of een database wordt ontsleuteld |
| [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Retourneert informatie over de status van de versleuteling van een database en de bijbehorende database versleutelingssleutels |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Retourneert informatie over de status van de versleuteling van elke datawarehouse-knooppunt en de bijbehorende database versleutelingssleutels |
|  | |

U kunt niet de transparent data encryption protector overstappen op een sleutel uit Key Vault met behulp van Transact-SQL. Gebruik PowerShell of Azure portal.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Transparante gegevensversleuteling beheren met behulp van de REST-API

Voor het configureren van transparante gegevensversleuteling via de REST-API moet verbonden zijn als de Azure-eigenaar, bijdrager of SQL Security Manager.

| Opdracht | Beschrijving |
| --- | --- |
|[Maken of bijwerken van de Server](https://docs.microsoft.com/rest/api/sql/servers/servers_createorupdate)|Voegt een Azure Active Directory-identiteit toe aan een SQL Server-exemplaar (die wordt gebruikt om toegang te verlenen tot Key Vault)|
|[Maken of bijwerken van de serversleutel](https://docs.microsoft.com/rest/api/sql/serverkeys/serverkeys_createorupdate)|Voegt een Key Vault-sleutel toe aan een SQL Server-exemplaar|
|[Serversleutel verwijderen](https://docs.microsoft.com/rest/api/sql/serverkeys/serverkeys_delete)|Hiermee verwijdert u een Key Vault-sleutel van een SQL Server-exemplaar|
|[Serversleutels ophalen](https://docs.microsoft.com/rest/api/sql/serverkeys/serverkeys_get)|Een specifieke Key Vault-sleutel opgehaald uit een SQL Server-exemplaar|
|[Een lijst met serversleutels door Server](https://docs.microsoft.com/rest/api/sql/serverkeys/serverkeys_listbyserver)|Haalt de Key Vault sleutels voor een SQL Server-exemplaar |
|[Maken of bijwerken van Encryption Protector](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/encryptionprotectors_createorupdate)|Hiermee stelt u de transparent data encryption-beveiliging voor een SQL Server-exemplaar|
|[Encryption Protector ophalen](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/encryptionprotectors_get)|Hiermee haalt u de transparent data encryption-beveiliging voor een SQL Server-exemplaar|
|[Lijst met versleuteling beveiligingstoepassingen door Server](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/encryptionprotectors_listbyserver)|Hiermee haalt u de transparent data encryption protector voor een SQL Server-exemplaar |
|[Maken of bijwerken van Transparent Data Encryption-configuratie](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/transparentdataencryptions_createorupdate)|Hiermee schakelt transparante gegevensversleuteling voor een database of uit|
|[Transparent Data Encryption-configuratie ophalen](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/transparentdataencryptions_get)|Hiermee wordt de transparent data encryption-configuratie voor een database opgehaald|
|[Lijst met Transparent Data Encryption Configuratieresultaten](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/transparentdataencryptionactivities_listbyconfiguration)|Hiermee haalt u het resultaat van de versleuteling voor een database|

## <a name="next-steps"></a>Volgende stappen

- Zie voor een algemene beschrijving van transparante gegevensversleuteling, [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Zie voor meer informatie over transparante gegevensversleuteling met Bring Your Own Key-ondersteuning voor SQL-Database en Data Warehouse, [Transparent data encryption met Bring Your Own Key support](transparent-data-encryption-byok-azure-sql.md).
- Als u wilt gaan met transparent data encryption met Bring Your Own Key-ondersteuning, Zie de handleiding [transparante gegevensversleuteling inschakelen met behulp van uw eigen sleutel uit Key Vault met behulp van PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).
- Zie voor meer informatie over Key Vault, de [Key Vault-documentatiepagina](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
