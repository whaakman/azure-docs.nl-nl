---
title: Auditlogboeken voor Azure Database for MySQL
description: Beschrijving van de auditlogboeken beschikbaar zijn in Azure Database voor MySQL en de beschikbare parameters voor het inschakelen van niveaus voor logboekregistratie.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 35cbe04380e2113f986d1e7adf3f7fdf89cb9326
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078870"
---
# <a name="audit-logs-in-azure-database-for-mysql"></a>Auditlogboeken beschikbaar zijn in Azure Database for MySQL

Het controlelogboek is beschikbaar voor gebruikers in Azure Database voor MySQL. Het controlelogboek kan worden gebruikt om bij te houden op databaseniveau activiteit en wordt meestal gebruikt voor naleving.

> [!IMPORTANT]
> Audit log functionaliteit is momenteel in preview.

## <a name="configure-audit-logging"></a>Audit-logboekregistratie configureren

Het controlelogboek is standaard uitgeschakeld. Als u wilt inschakelen, stelt `audit_log_enabled` op ON.

Er zijn andere parameters die u kunt aanpassen:

- `audit_log_events`: Hiermee bepaalt u de gebeurtenissen moeten worden vastgelegd. Zie de onderstaande tabel voor specifieke controlegebeurtenissen.
- `audit_log_exclude_users`: MySQL-gebruikers moeten worden uitgesloten van logboekregistratie. Kunt u maximaal vier gebruikers. Maximale lengte van de parameter is 256 tekens.

| **Gebeurtenis** | **Beschrijving** |
|---|---|
| `CONNECTION` | -Verbinding initiëren (geslaagd of mislukt) <br> -Herauthenticatie gebruiker met een andere gebruiker/wachtwoord tijdens sessie <br> -Connection-beëindiging |
| `DML_SELECT`| SELECT-query 's |
| `DML_NONSELECT` | INSERT/DELETE/UPDATE-query 's |
| `DML` | DML = DML_SELECT + DML_NONSELECT |
| `DDL` | Query's zoals "DROP DATABASE" |
| `DCL` | Query's zoals 'Toestemming geven' |
| `ADMIN` | Query's zoals "STATUS weergeven" |
| `GENERAL` | Alle in DML_SELECT, DML_NONSELECT DML, DDL, Gegevensverbindingsbibliotheek en beheer |
| `TABLE_ACCESS` | -Alleen beschikbaar voor MySQL 5.7 <br> -Tabel meer instructies, zoals selecteren of INSERT INTO... SELECT <br> -Table delete-instructies, zoals verwijderen of TRUNCATE TABLE <br> -Tabel insert-instructies, zoals invoegen of vervangen <br> -Table update-instructies, zoals UPDATE |

## <a name="access-audit-logs"></a>Auditlogboeken inzien

Auditlogboeken zijn geïntegreerd met Azure Monitor diagnostische logboeken. Nadat u de logboeken voor controle hebt ingeschakeld op uw MySQL-server, kunt u deze verzenden naar Azure Monitor-Logboeken, Event Hubs of Azure Storage. Zie voor meer informatie over het inschakelen van diagnostische logboeken in Azure portal, de [audit log portal artikel](howto-configure-audit-logs-portal.md#set-up-diagnostic-logs).

## <a name="schemas"></a>Schema 's

De volgende secties wordt beschreven wat er wordt uitgevoerd door de MySQL-auditlogboeken op basis van het gebeurtenistype. Afhankelijk van de uitvoermethode, zijn de velden die zijn opgenomen en de volgorde waarin ze worden weergegeven kunnen verschillen.

### <a name="connection"></a>Verbinding

| **Eigenschap** | **Beschrijving** |
|---|---|
| `TenantId` | Uw tenant-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| `Type` | Het type van het logboek. Altijd `AzureDiagnostics` |
| `SubscriptionId` | GUID voor het abonnement waartoe de server behoort |
| `ResourceGroup` | Naam van de resourcegroep die de server behoort |
| `ResourceProvider` | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resource-URI |
| `Resource` | Naam van de server |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `connection_log` |
| `event_subclass` | `CONNECT`, `DISCONNECT`, `CHANGE USER` (alleen beschikbaar voor MySQL 5.7) |
| `connection_id` | De unieke verbindings-ID die is gegenereerd door MySQL |
| `host` | Leeg |
| `ip` | IP-adres van de client maakt verbinding met MySQL |
| `user` | Naam van de gebruiker die de query wordt uitgevoerd |
| `db` | Naam van de database verbonden |
| `\_ResourceId` | Resource-URI |

### <a name="general"></a>Algemeen

Onderstaande schema is van toepassing op algemeen, DML_SELECT, DML_NONSELECT, DML, DDL, Gegevensverbindingsbibliotheek en beheerder gebeurtenistypen.

| **Eigenschap** | **Beschrijving** |
|---|---|
| `TenantId` | Uw tenant-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| `Type` | Het type van het logboek. Altijd `AzureDiagnostics` |
| `SubscriptionId` | GUID voor het abonnement waartoe de server behoort |
| `ResourceGroup` | Naam van de resourcegroep die de server behoort |
| `ResourceProvider` | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resource-URI |
| `Resource` | Naam van de server |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `general_log` |
| `event_subclass` | `LOG`, `ERROR`, `RESULT` (alleen beschikbaar voor MySQL 5.6) |
| `event_time` | Query seconden starten in de UNIX-timestamp |
| `error_code` | Foutcode als de query is mislukt. `0` betekent dat er geen fout |
| `thread_id` | ID van de thread die de query wordt uitgevoerd |
| `host` | Leeg |
| `ip` | IP-adres van de client maakt verbinding met MySQL |
| `user` | Naam van de gebruiker die de query wordt uitgevoerd |
| `sql_text` | Volledige querytekst |
| `\_ResourceId` | Resource-URI |

### <a name="table-access"></a>Toegang tot tabellen

| **Eigenschap** | **Beschrijving** |
|---|---|
| `TenantId` | Uw tenant-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated` [UTC] | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| `Type` | Het type van het logboek. Altijd `AzureDiagnostics` |
| `SubscriptionId` | GUID voor het abonnement waartoe de server behoort |
| `ResourceGroup` | Naam van de resourcegroep die de server behoort |
| `ResourceProvider` | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resource-URI |
| `Resource` | Naam van de server |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `event_class` | `table_access_log` |
| `event_subclass` | `READ`, `INSERT`, `UPDATE`, of `DELETE` |
| `connection_id` | De unieke verbindings-ID die is gegenereerd door MySQL |
| `db` | Naam van de database geopend |
| `table` | Naam van de tabel geopend |
| `sql_text` | Volledige querytekst |
| `\_ResourceId` | Resource-URI |

## <a name="next-steps"></a>Volgende stappen

- [Controlelogboeken configureren in Azure portal](howto-configure-audit-logs-portal.md)