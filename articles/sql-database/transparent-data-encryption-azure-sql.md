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
ms.date: 02/20/2019
ms.openlocfilehash: bfceb8feacdad428a6e4c23272fd9092a356f107
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56453321"
---
# <a name="transparent-data-encryption-for-sql-database-and-data-warehouse"></a>Transparante gegevensversleuteling voor SQL-Database en Data Warehouse

Transparante gegevensversleuteling (TDE) helpt Azure SQL Database, Azure SQL Managed Instance en Azure Data Warehouse beveiligen tegen de dreiging van schadelijke activiteiten. Het voert realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing. TDE is standaard ingeschakeld voor alle geïmplementeerde Azure SQL-databases. TDE kan niet worden gebruikt voor het versleutelen van de logische **master** database in SQL-Database.  De **master** -database bevat objecten die nodig zijn om uit te voeren van de TDE-bewerkingen op de gebruikersdatabases.

TDE moet handmatig worden ingeschakeld voor Azure SQL Managed Instance, oudere databases van Azure SQL Database of Azure SQL Data Warehouse.  

Transparante gegevensversleuteling versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel met de naam de databaseversleutelingssleutel. Deze database versleutelingssleutel wordt beveiligd door de transparent data encryption-beveiliging. De beveiliging is een van beide een service beheerde certificaat (beheerde service transparante gegevensversleuteling) of een asymmetrische sleutel opgeslagen in Azure Key Vault (Bring Your Own Key). U kunt de transparent data encryption protector instellen op het niveau van de server voor Azure SQL Database en Data Warehouse, en exemplaar voor Azure SQL Managed Instance. De term *server* verwijst zowel naar de server en het exemplaar in dit document, tenzij anders vermeld.

Bij het opstarten van de database, is de versleutelingssleutel van de versleutelde database ontsleuteld en wordt gebruikt voor ontsleuteling en het opnieuw versleutelen van de databasebestanden in het SQL Server Database Engine-proces. Transparante gegevensversleuteling voert realtime i/o-versleuteling en ontsleuteling van de gegevens op het paginaniveau van de. Elke pagina wordt ontsleuteld wanneer het is gelezen in het geheugen en vervolgens versleuteld voordat deze werd geschreven naar schijf. Zie voor een algemene beschrijving van transparante gegevensversleuteling, [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).

Met SQL Server op een Azure-machine ook kunnen een asymmetrische sleutel uit Key Vault gebruiken. De configuratiestappen zijn anders als een asymmetrische sleutel in SQL Database en SQL Managed Instance. Zie voor meer informatie, [uitbreidbaar Sleutelbeheer met behulp van Azure Key Vault (SQL Server)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server).

## <a name="service-managed-transparent-data-encryption"></a>Beheerde service transparante gegevensversleuteling

In Azure is de standaardinstelling voor transparante gegevensversleuteling of de databaseversleutelingssleutel wordt beveiligd door een ingebouwde servercertificaat. Het certificaat van de ingebouwde is uniek voor elke server. Als een database in een relatie geo-replicatie, wordt de primaire en geo-secundaire database worden beveiligd door de primaire database bovenliggende SLC-sleutel. Als twee databases zijn verbonden met dezelfde server, delen ze ook ingebouwde hetzelfde certificaat. Microsoft draait automatisch deze certificaten in overeenstemming met het interne beveiligingsbeleid en de hoofd-sleutel wordt beveiligd door een Microsoft store voor interne geheim.

Microsoft ook naadloos worden verplaatst en de sleutels indien nodig voor geo-replicatie wordt beheerd en worden hersteld.

> [!IMPORTANT]
> Alle gemaakte SQL-databases worden standaard versleuteld met behulp van de service beheerde transparante gegevensversleuteling. Azure SQL Managed Instance-databases, bestaande SQL-databases die zijn gemaakt vóór mei 2017 en SQL-databases die zijn gemaakt via herstellen, geo-replicatie en database-exemplaar zijn niet standaard versleuteld.

## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Door de klant beheerde transparante gegevensversleuteling - Bring Your Own Key

[TDE met de klant beheerde sleutels in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md) kunt voor het versleutelen van de Database Gegevensversleutelingsleutel (DEK) met een door de klant beheerde asymmetrische sleutel met de naam TDE-beveiliging.  Dit wordt meestal ook hier naar verwezen als Bring Your Own Key (BYOK) ondersteuning voor transparante gegevensversleuteling. De BYOK-scenario, de TDE-beveiliging worden opgeslagen in een klanten en beheerd [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), van het Azure-cloud-gebaseerde externe sleutelbeheersysteem. De TDE-beveiliging kan worden [die worden gegenereerd door de key vault of overgebracht naar de key vault](https://docs.microsoft.com/en-us/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys) van een on-premises HSM-apparaat. De DEK TDE, die is opgeslagen op de opstartpagina van een database, is versleuteld en ontsleuteld door de TDE-beveiliging, die is opgeslagen in Azure Key Vault en nooit verlaat de key vault.  SQL-Database moet machtigingen worden toegekend voor de sleutelkluis te ontsleutelen en de DEK versleutelen met klanten. Als de machtigingen van de logische SQL-server naar de key vault worden ingetrokken, een database zijn niet toegankelijk en alle gegevens worden versleuteld. Voor Azure SQL Database, de TDE-beveiliging is ingesteld op niveau van de logische SQL-server en wordt overgenomen door alle databases die zijn gekoppeld aan die server. Voor [Azure SQL Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-howto-managed-instance), de TDE-beveiliging is ingesteld op het instantieniveau en deze is overgenomen door alle *versleutelde* databases op dat exemplaar. De term *server* verwijst zowel naar de server en het exemplaar in dit document, tenzij anders vermeld.

Gebruikers kunnen met TDE met Azure Key Vault-integratie, essentiële beheertaken zoals sleutelrotaties, key vault-machtigingen, sleutelback-ups beheren en Schakel controle/rapportage over alle TDE beveiligingstoepassingen met behulp van Azure Key Vault-functionaliteit. Key Vault biedt centraal beheer, maakt gebruik van nauw bewaakte hardware security modules (HSM's), en kunt scheiding van functies tussen het beheer van sleutels en gegevens om te voldoen aan de naleving van beveiligingsbeleid.
Zie voor meer informatie over transparante gegevensversleuteling met Azure Key Vault-integratie (Bring Your Own Key-ondersteuning) voor Azure SQL Database, SQL Managed Instance en Data Warehouse, [transparante gegevensversleuteling met Azure Key Vault-integratie](transparent-data-encryption-byok-azure-sql.md).

Als u wilt gaan met transparent data encryption met Azure Key Vault-integratie (ondersteuning voor Bring Your Own Key), Zie de handleiding [transparante gegevensversleuteling inschakelen met behulp van uw eigen sleutel uit Key Vault met behulp van PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Een transparent data encryption beveiligde database verplaatsen

U hoeft niet te ontsleutelen van databases voor bewerkingen in Azure. De transparent data encryption-instellingen op de brondatabase of de primaire database worden transparant overgenomen op de doelcomputer. Bewerkingen die opgenomen zijn, hebben betrekking op:

- Geo-herstel
- Point-in-time Self-service voor herstellen
- Herstel van een verwijderde database
- Actieve geo-replicatie
- Het maken van een databasekopie
- Herstellen van back-upbestand naar Azure SQL Managed Instance

> [!IMPORTANT]
> Handmatige kopie-alleen back-up van een database versleuteld door service beheerde TDE is niet toegestaan in Azure SQL Managed Instance, omdat het certificaat dat wordt gebruikt voor versleuteling is niet toegankelijk. Punt-in-time-restore-functie gebruiken voor dit type database verplaatsen naar een andere Managed Instance.

Wanneer u een database transparent data encryption beschermde exporteert, worden de geëxporteerde inhoud van de database is niet versleuteld. Dit geëxporteerde inhoud is opgeslagen in niet-versleutelde Bacpac-bestanden. Moet u het BACPAC-bestanden op de juiste wijze te beschermen en transparent data encryption inschakelen nadat het importeren van de nieuwe database is voltooid.

Bijvoorbeeld, als het BACPAC-bestand is geëxporteerd uit een on-premises SQL Server-exemplaar, wordt niet de geïmporteerde inhoud van de nieuwe database automatisch versleuteld. Op dezelfde manier als het BACPAC-bestand is geëxporteerd naar een on-premises SQL Server-exemplaar, wordt niet de nieuwe database ook automatisch versleuteld.

De enige uitzondering hierop is wanneer u naar en van een SQL-database exporteert. Transparante gegevensversleuteling is ingeschakeld in de nieuwe database, maar nog steeds het BACPAC-bestand zelf wordt niet versleuteld.

## <a name="manage-transparent-data-encryption-in-the-azure-portal"></a>Transparante gegevensversleuteling in Azure portal beheren

Voor het configureren van transparante gegevensversleuteling via Azure portal, moet verbonden zijn als de Azure-eigenaar, bijdrager of SQL Security Manager.

U transparante gegevensversleuteling in- en uitschakelen op het databaseniveau van de. Om in te schakelen transparante gegevensversleuteling voor een database, gaat u naar de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-beheerder of Inzender. De transparent data encryption-instellingen onder de gebruikersdatabase vinden. Beheerde service transparante gegevensversleuteling wordt standaard gebruikt. Een transparent data encryption-certificaat wordt automatisch gegenereerd voor de server die de database bevat. Gebruik de T-SQL transparante gegevensversleuteling in of uit een database inschakelen voor Azure SQL Managed Instance.

![Beheerde service transparante gegevensversleuteling](./media/transparent-data-encryption-azure-sql/service-managed-tde.png)  

U kunt de hoofdsleutel transparent data encryption, ook wel bekend als de transparent data encryption protector instellen op niveau van de server. Voor het gebruik van transparante gegevensversleuteling met Bring Your Own Key-ondersteuning en beveiligen van uw databases met een sleutel uit Key Vault, open de transparent data encryption-instellingen op uw server.

![Transparante gegevensversleuteling met Bring Your Own Key-ondersteuning](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

## <a name="manage-transparent-data-encryption-by-using-powershell"></a>Transparante gegevensversleuteling met behulp van PowerShell beheren

Voor het configureren van transparante gegevensversleuteling via PowerShell, moet verbonden zijn als de Azure-eigenaar, bijdrager of SQL Security Manager.

### <a name="cmdlets-for-azure-sql-database-and-data-warehouse"></a>Cmdlets voor Azure SQL Database en datawarehouse

Gebruik de volgende cmdlets voor Azure SQL Database en Data Warehouse:

| Cmdlet | Description |
| --- | --- |
| [Set-AzureRmSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) |Hiermee schakelt transparante gegevensversleuteling voor een database of uit|
| [Get-AzureRmSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) |Hiermee haalt u de transparent data encryption-status voor een database |
| [Get-AzureRmSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) |Controleert of de voortgang van de versleuteling voor een database |
| [Add-AzureRmSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) |Voegt een Key Vault-sleutel toe aan een SQL Server-exemplaar |
| [Get-AzureRmSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) |Hiermee haalt u de Key Vault-sleutels voor Azure SQL Database-server  |
| [Set-AzureRmSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) |Hiermee stelt u de transparent data encryption-beveiliging voor een SQL Server-exemplaar |
| [Get-AzureRmSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) |De transparent data encryption protector opgehaald |
| [Remove-AzureRmSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) |Hiermee verwijdert u een Key Vault-sleutel van een SQL Server-exemplaar |
|  | |

> [!IMPORTANT]
> Voor Azure SQL Managed Instance, de T-SQL gebruiken [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) opdracht voor het inschakelen van transparante gegevensversleuteling in- en uitschakelen op het databaseniveau van een en Controleer [PowerShell-script voorbeeld](transparent-data-encryption-byok-azure-sql-configure.md) transparante gegevens beheren versleuteling op het exemplaarniveau van een.

## <a name="manage-transparent-data-encryption-by-using-transact-sql"></a>Transparante gegevensversleuteling beheren met behulp van Transact-SQL

Verbinding maken met de database met behulp van een aanmelding die is een beheerder of lid zijn van de **dbmanager** rol in de database master.

| Opdracht | Description |
| --- | --- |
| [ALTER DATABASE (Azure SQL Database)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | Aan van/op VERSLEUTELING uit ingesteld worden versleuteld of een database wordt ontsleuteld |
| [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Retourneert informatie over de status van de versleuteling van een database en de bijbehorende database versleutelingssleutels |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Retourneert informatie over de status van de versleuteling van elke datawarehouse-knooppunt en de bijbehorende database versleutelingssleutels |
|  | |

U kunt niet de transparent data encryption protector overstappen op een sleutel uit Key Vault met behulp van Transact-SQL. Gebruik PowerShell of Azure portal.

## <a name="manage-transparent-data-encryption-by-using-the-rest-api"></a>Transparante gegevensversleuteling beheren met behulp van de REST-API

Voor het configureren van transparante gegevensversleuteling via de REST-API moet verbonden zijn als de Azure-eigenaar, bijdrager of SQL Security Manager.
Gebruik de volgende reeks opdrachten voor Azure SQL Database en Data Warehouse:

| Opdracht | Description |
| --- | --- |
|[Maken of bijwerken van de Server](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Voegt een Azure Active Directory-identiteit toe aan een SQL Server-exemplaar (die wordt gebruikt om toegang te verlenen tot Key Vault)|
|[Maken of bijwerken van de serversleutel](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Voegt een Key Vault-sleutel toe aan een SQL Server-exemplaar|
|[Serversleutel verwijderen](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Hiermee verwijdert u een Key Vault-sleutel van een SQL Server-exemplaar|
|[Serversleutels ophalen](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Een specifieke Key Vault-sleutel opgehaald uit een SQL Server-exemplaar|
|[Een lijst met serversleutels door Server](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|Haalt de Key Vault sleutels voor een SQL Server-exemplaar |
|[Maken of bijwerken van Encryption Protector](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Hiermee stelt u de transparent data encryption-beveiliging voor een SQL Server-exemplaar|
|[Encryption Protector ophalen](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Hiermee haalt u de transparent data encryption-beveiliging voor een SQL Server-exemplaar|
|[Lijst met versleuteling beveiligingstoepassingen door Server](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Hiermee haalt u de transparent data encryption protector voor een SQL Server-exemplaar |
|[Maken of bijwerken van Transparent Data Encryption-configuratie](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Hiermee schakelt transparante gegevensversleuteling voor een database of uit|
|[Transparent Data Encryption-configuratie ophalen](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Hiermee wordt de transparent data encryption-configuratie voor een database opgehaald|
|[Lijst met Transparent Data Encryption Configuratieresultaten](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Hiermee haalt u het resultaat van de versleuteling voor een database|

## <a name="next-steps"></a>Volgende stappen

- Zie voor een algemene beschrijving van transparante gegevensversleuteling, [transparante gegevensversleuteling](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption).
- Zie voor meer informatie over transparante gegevensversleuteling met Bring Your Own Key-ondersteuning voor Azure SQL Database, Azure SQL Managed Instance en Data Warehouse, [Transparent data encryption met Bring Your Own Key support](transparent-data-encryption-byok-azure-sql.md).
- Als u wilt gaan met transparent data encryption met Bring Your Own Key-ondersteuning, Zie de handleiding [transparante gegevensversleuteling inschakelen met behulp van uw eigen sleutel uit Key Vault met behulp van PowerShell](transparent-data-encryption-byok-azure-sql-configure.md).
- Zie voor meer informatie over Key Vault, de [Key Vault-documentatiepagina](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).
