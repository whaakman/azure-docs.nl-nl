---
title: Artikel over het oplossen van veelvoorkomende problemen/fouten die zijn gekoppeld aan met behulp van Azure Database Migration Service bekende | Microsoft Docs
description: Meer informatie over het oplossen van veelvoorkomende bekende problemen/fouten die zijn gekoppeld aan met behulp van Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/18/2019
ms.openlocfilehash: 1d639a8b1d5c7a5dd2b7bac7c5e020be7c8b1c50
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190949"
---
# <a name="troubleshoot-common-azure-database-migration-service-issues-and-errors"></a>Veelvoorkomende problemen met Azure Database Migration Service en-fouten oplossen

Dit artikel worden enkele veelvoorkomende problemen en fouten die gebruikers van Azure Database Migration Service kunnen gehoord. Het artikel bevat ook informatie over hoe u deze problemen en fouten op te lossen.

## <a name="migration-activity-in-queued-state"></a>Migratieactiviteiten in in de wachtrij staat

Wanneer u nieuwe activiteiten in een Azure Database Migration Service-project maakt, blijven de activiteiten in een in de wachtrij staat.

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Dit probleem treedt op wanneer de Azure Database Migration Service-exemplaar heeft bereikt de maximumcapaciteit voor lopende taken die gelijktijdig worden uitgevoerd. Een nieuwe activiteit is in de wachtrij geplaatst totdat de capaciteit beschikbaar. | Valideer de Data Migration Service-exemplaar heeft het uitvoeren van activiteiten in projecten. U kunt blijven maken van nieuwe activiteiten automatisch toegevoegd aan de wachtrij voor uitvoering. Als een van de bestaande uitvoeren van activiteiten hebt voltooid, wordt de volgende activiteit in de wachtrij wordt uitgevoerd en de status gewijzigd in staat automatisch worden uitgevoerd. U hoeft te ondernemen actie meer om de migratie van activiteit in de wachtrij te starten.<br><br> |

## <a name="max-number-of-databases-selected-for-migration"></a>Max. aantal databases die zijn geselecteerd voor migratie

De volgende fout treedt op wanneer het maken van een activiteit voor een database migration-project voor het verplaatsen naar Azure SQL Database of een Azure SQL-Database beheerd exemplaar:

* **Fout**: Validatiefout van de migratie-instellingen', "errorDetail": "objecten meer dan maximum aantal '4' van 'Databases' is geselecteerd voor migratie."

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout wordt weergegeven wanneer u meer dan vier databases voor een enkele migratieactiviteit hebt geselecteerd. Op dit moment is elke migratieactiviteit beperkt tot vier databases. | Selecteer vier of minder databases per migratieactiviteit. Als u meer dan vier databases parallel te migreren wilt, richt u een ander exemplaar van Azure Database Migration Service. Elk abonnement ondersteunt momenteel maximaal twee Azure Database Migration Service-exemplaren.<br><br> |

## <a name="errors-for-mysql-migration-to-azure-mysql-with-recovery-failures"></a>Fouten voor MySQL-migratie met Azure MySQL met mislukte herstelbewerkingen

Wanneer u van MySQL met Azure Database voor MySQL met behulp van Azure Database Migration Service migreert, wordt de migratieactiviteit mislukt met de volgende fout:

* **Fout**: Fout in de database-migratie - taak 'TaskID' is onderbroken vanwege [n] opeenvolgende herstel fouten.

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout kan optreden wanneer de gebruiker die de migratie in de rol ReplicationAdmin en/of de bevoegdheden van de replicatie-CLIENT, REPLICA-replicatie en SUPER (eerdere versies dan MySQL 5.6.6 ontbreekt).<br><br><br><br><br><br><br><br><br><br><br><br><br> | Zorg ervoor dat de [vereiste bevoegdheden](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#prerequisites) voor de gebruiker account zijn geconfigureerd nauwkeurig op de Azure Database for MySQL-exemplaar. Bijvoorbeeld, kunnen de volgende stappen worden uitgevoerd voor het maken van een gebruiker met de naam 'migrateuser' met de vereiste bevoegdheden:<br>1. CREATE USER migrateuser@'%' geïdentificeerd door 'geheim'; <br>2. Verleent alle rechten op db_name.* naar migrateuser'@'% aangeduid met 'geheim'; Herhaal deze stap voor het verlenen van toegang tot meer databases <br>3. Verleen replicatie slave op *.* naar migrateuser'@'% aangeduid met 'geheim';<br>4. Replicatie-client verlenen op *.* naar migrateuser'@'% aangeduid met 'geheim';<br>5. Bevoegdheden; leegmaken |

## <a name="error-when-attempting-to-stop-azure-database-migration-service"></a>Fout bij het stoppen van de Azure Database Migration Service

U ontvangt de volgende fout bij het stoppen van de Azure Database Migration Service-exemplaar:

* **Fout**: Service niet stoppen. Fout: {'fout': {"code": "InvalidRequest", "message": "een of meer activiteiten die momenteel worden uitgevoerd. U kunt de service stoppen, wacht u totdat de activiteiten zijn voltooid of stopt u deze activiteiten handmatig en probeer het opnieuw. "}}

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout wordt weergegeven wanneer de service-exemplaar dat u probeert te stoppen bevat activiteiten die nog wordt uitgevoerd of die aanwezig zijn in migration-projecten. <br><br><br><br><br><br> | Zorg ervoor dat er geen activiteiten worden uitgevoerd in het exemplaar van Azure Database Migration Service u probeert te stoppen. U kunt ook de activiteiten of projecten verwijderen voordat u probeert de service te stoppen. De volgende stappen laten zien hoe u projecten voor het opschonen van het exemplaar van de migratie door het verwijderen van alle actieve taken verwijderen:<br>1. Install-Module -Name AzureRM.DataMigration <br>2. Login-AzureRmAccount <br>3. Select-AzureRmSubscription -SubscriptionName "<subName>" <br> 4. Remove-AzureRmDataMigrationProject -Name <projectName> -ResourceGroupName <rgName> -ServiceName <serviceName> -DeleteRunningTask |

## <a name="error-when-attempting-to-start-azure-database-migration-service"></a>Fout bij het starten van de Azure Database Migration Service

U ontvangt de volgende fout bij het starten van de Azure Database Migration Service-exemplaar:

* **Fout**: Service niet starten. Fout: {'errorDetail': de service niet starten, neem contact op met Microsoft ondersteuning}

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout wordt weergegeven wanneer het vorige exemplaar intern is mislukt. Deze fout treedt op zelden en het technische team is op de hoogte van het. <br> | Verwijder het exemplaar van de service die u kunt geen start en vervolgens inrichten nieuw wachtwoord om deze te vervangen. |

## <a name="error-restoring-database-while-migrating-sql-to-azure-sql-db-managed-instance"></a>Fout bij het herstellen van database tijdens het migreren van SQL met Azure SQL DB beheerd exemplaar

Wanneer u een online migratie van SQL Server naar een beheerd exemplaar voor Azure SQL Database uitvoeren, mislukt de cutover met de volgende fout:

* **Fout**: De herstelbewerking is mislukt voor de bewerkings-Id 'bewerkings-id'. Code 'AuthorizationFailed' bericht ' de 'clientId' met object-id 'object-id'-client is niet gemachtigd om uit te voeren actie 'Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/read' over scope ' /subscriptions/ abonnements-id '.'.

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Deze fout wijst op de toepassings-principal wordt gebruikt voor online migratie van SQL Server naar een beheerd exemplaar voor Azure SQL Database biedt geen machtiging voor het abonnement hebt bijdragen. Bepaalde API-aanroepen met Managed Instance op dit moment moeten deze machtiging op abonnement voor de herstelbewerking. <br><br><br><br><br><br><br><br><br><br><br><br><br><br> | Gebruik de `Get-AzureADServicePrincipal` PowerShell-cmdlet met `-ObjectId` beschikbaar is via het foutbericht om de weergavenaam van de toepassings-ID wordt gebruikt.<br><br> De machtigingen om deze toepassing te valideren en zorg ervoor dat er de [rol van Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) op het abonnementsniveau. <br><br> De Azure Database Migration Service Engineering-Team werkt aan beperkt de vereiste toegang vanaf de huidige rol op abonnement bijdragen. Als u een bedrijfsvereiste die geen gebruik van rol bijdragen, neem contact op met de ondersteuning van Azure voor meer informatie. |

## <a name="error-when-deleting-nic-associated-with-azure-database-migration-service"></a>Fout bij het verwijderen van de NIC die is gekoppeld aan Azure Database Migration Service

Wanneer u probeert te verwijderen van een netwerkkaart die zijn gekoppeld aan Azure Database Migration Service, wordt de verwijdering poging mislukt vanwege de volgende fout:

* **Fout**: De NIC die is gekoppeld aan Azure Database Migration Service vanwege de DMS-service met behulp van de NIC kan niet worden verwijderd.

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Dit probleem treedt op wanneer de Azure Database Migration Service-exemplaar mogelijk nog steeds aanwezig en gebruiken van de NIC. <br><br><br><br><br><br><br><br> | Als u wilt verwijderen van deze NIC, verwijdert u de DMS-service-exemplaar dat de NIC die wordt gebruikt door de service automatisch verwijderd.<br><br> **Belangrijke**: Zorg ervoor dat de Azure Database Migration Service-exemplaar wordt verwijderd, heeft geen actieve activiteiten.<br><br> Nadat alle projecten en activiteiten die zijn gekoppeld aan het Azure Database Migration Service-exemplaar zijn verwijderd, kunt u het service-exemplaar verwijderen. De NIC die wordt gebruikt door de service-exemplaar is automatisch opgeschoond als onderdeel van de service verwijderen. |

## <a name="connection-error-when-using-expressroute"></a>Verbindingsfout bij het gebruik van ExpressRoute

Wanneer u probeert verbinding maken met de gegevensbron in de wizard Azure Database Migration service-project, mislukt de verbinding na langere time-out als bron wordt ExpressRoute gebruikt voor connectiviteit.

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Bij het gebruik van [ExpressRoute](https://azure.microsoft.com/services/expressroute/), Azure Database Migration Service [vereist](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) drie service-eindpunten op het subnet van het Virtueelnetwerk dat is gekoppeld aan de service wordt ingericht:<br> --Service Bus-eindpunt<br> --Opslageindpunt<br> --Gericht op database-eindpunt (bijvoorbeeld SQL-eindpunt, Cosmos DB-eindpunt)<br><br><br><br><br> | [Schakel](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) de vereiste service-eindpunten voor ExpressRoute-connectiviteit tussen de bron- en Azure Database Migration Service. <br><br><br><br><br><br><br><br> |

## <a name="timeout-error-when-migrating-a-mysql-database-to-azure-mysql"></a>Time-out bij het migreren van een MySQL-database naar Azure MySQL

Wanneer u een MySQL-database naar een Azure Database for MySQL-exemplaar via Azure Database Migration Service migreert, faalt de migratie met de volgende time-outfout:

* **Fout**: Database migration-fout: kan niet worden geladen van bestand - belasting kan proces niet starten voor het bestand "n" retcode is: SQL_ERROR SqlState: HY000 NativeError: 1205 bericht: [MySQL] [ODBC-stuurprogramma] [mysqld] vergrendeling wacht time-out overschreden; Probeer het opnieuw opstarten van transactie

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Deze fout treedt op wanneer de migratie mislukt vanwege de time-out voor wachten op vergrendeling tijdens de migratie. | Vergroot de waarde van serverparameter **'innodb_lock_wait_timeout'** . De hoogste toegestane waarde is 1073741824. |

## <a name="error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance"></a>Fout bij het verbinding maken met de bron-SQL Server bij het gebruik van dynamische poort of benoemd exemplaar

Wanneer u probeert verbinding maken met Azure Database Migration Service SQL Server-bron die wordt uitgevoerd op een benoemd exemplaar of een dynamische poort, mislukt de verbinding vanwege de volgende fout:

* **Fout**: -1 - SQL-verbinding is mislukt. Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server wordt niet gevonden of toegang tot de server is niet mogelijk. Controleer of dat de exemplaarnaam juist is en dat SQL Server is geconfigureerd voor het toestaan van externe verbindingen. (provider: SQL-netwerkinterfaces, fout: 26 - fout bij het zoeken van de opgegeven Server/exemplaar)

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Dit probleem treedt op wanneer de bron SQL Server-exemplaar dat Azure Database Migration Service probeert verbinding maken met een dynamische poort is of van een benoemd exemplaar gebruikmaakt. De SQL Server Browser-service luistert op UDP-poort 1434 voor binnenkomende verbindingen op een benoemd exemplaar of bij het gebruik van een dynamische poort. De dynamische poort veranderen telkens wanneer SQL Server-service opnieuw wordt opgestart. U kunt de dynamische poort is toegewezen aan een exemplaar via network configuration in SQL Server Configuration Manager controleren.<br><br><br> |Controleer of dat Azure Database Migration Service verbinding met de bron SQL Server Browser-service op UDP-poort 1434 en de SQL Server-exemplaar via de dynamisch toegewezen TCP-poort, zoals van toepassing maken kan. |

## <a name="additional-known-issues"></a>Aanvullende bekende problemen

* [Bekende problemen/migratiebeperkingen met online migratie naar Azure SQL Database](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [Bekende problemen/migratiebeperkingen met online migratie naar Azure Database for MySQL](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [Bekende problemen/migratiebeperkingen met online migratie naar Azure Database for PostgreSQL](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Volgende stappen

* Artikel [Azure Database Migration Service PowerShell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* Artikel [parameters van de server configureren in Azure Database voor MySQL met behulp van de Azure-portal](https://docs.microsoft.com/azure/mysql/howto-server-parameters).
* Artikel [overzicht van vereisten voor het gebruik van Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs).
* Zie de [Veelgestelde vragen over het gebruik van Azure Database Migration Service](https://docs.microsoft.com/azure/dms/faq).
