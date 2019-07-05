---
title: Auditlogboeken voor Azure Database for MySQL
description: Beschrijving van de auditlogboeken beschikbaar zijn in Azure Database voor MySQL en de beschikbare parameters voor het inschakelen van niveaus voor logboekregistratie.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: 86750cea5e7f0d4726f3e0e9a03795ef2a602d8b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443847"
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

## <a name="diagnostic-logs-schemas"></a>Schema's voor diagnostische logboeken

De volgende secties wordt beschreven wat er wordt uitgevoerd door de MySQL-auditlogboeken op basis van het gebeurtenistype. Afhankelijk van de uitvoermethode, zijn de velden die zijn opgenomen en de volgorde waarin ze worden weergegeven kunnen verschillen.

### <a name="connection"></a>Verbinding

| **Eigenschap** | **Beschrijving** |
|---|---|
| `TenantId` | Uw tenant-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| `Type` | Het type van het logboek. Altijd `AzureDiagnostics` |
| `SubscriptionId` | GUID voor het abonnement waartoe de server behoort |
| `ResourceGroup` | Naam van de resourcegroep die de server behoort |
| `ResourceProvider` | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resource-URI |
| `Resource` | Naam van de server |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Naam van de server |
| `event_class_s` | `connection_log` |
| `event_subclass_s` | `CONNECT`, `DISCONNECT`, `CHANGE USER` (alleen beschikbaar voor MySQL 5.7) |
| `connection_id_d` | De unieke verbindings-ID die is gegenereerd door MySQL |
| `host_s` | Leeg |
| `ip_s` | IP-adres van de client maakt verbinding met MySQL |
| `user_s` | Naam van de gebruiker die de query wordt uitgevoerd |
| `db_s` | Naam van de database verbonden |
| `\_ResourceId` | Resource-URI |

### <a name="general"></a>Algemeen

Onderstaande schema is van toepassing op algemeen, DML_SELECT, DML_NONSELECT, DML, DDL, Gegevensverbindingsbibliotheek en beheerder gebeurtenistypen.

| **Eigenschap** | **Beschrijving** |
|---|---|
| `TenantId` | Uw tenant-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| `Type` | Het type van het logboek. Altijd `AzureDiagnostics` |
| `SubscriptionId` | GUID voor het abonnement waartoe de server behoort |
| `ResourceGroup` | Naam van de resourcegroep die de server behoort |
| `ResourceProvider` | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resource-URI |
| `Resource` | Naam van de server |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Naam van de server |
| `event_class_s` | `general_log` |
| `event_subclass_s` | `LOG`, `ERROR`, `RESULT` (alleen beschikbaar voor MySQL 5.6) |
| `event_time` | Query seconden starten in de UNIX-timestamp |
| `error_code_d` | Foutcode als de query is mislukt. `0` betekent dat er geen fout |
| `thread_id_d` | ID van de thread die de query wordt uitgevoerd |
| `host_s` | Leeg |
| `ip_s` | IP-adres van de client maakt verbinding met MySQL |
| `user_s` | Naam van de gebruiker die de query wordt uitgevoerd |
| `sql_text_s` | Volledige querytekst |
| `\_ResourceId` | Resource-URI |

### <a name="table-access"></a>Toegang tot tabellen

| **Eigenschap** | **Beschrijving** |
|---|---|
| `TenantId` | Uw tenant-ID |
| `SourceSystem` | `Azure` |
| `TimeGenerated [UTC]` | Tijdstempel wanneer het logboek is vastgelegd in UTC |
| `Type` | Het type van het logboek. Altijd `AzureDiagnostics` |
| `SubscriptionId` | GUID voor het abonnement waartoe de server behoort |
| `ResourceGroup` | Naam van de resourcegroep die de server behoort |
| `ResourceProvider` | De naam van de resourceprovider. Altijd `MICROSOFT.DBFORMYSQL` |
| `ResourceType` | `Servers` |
| `ResourceId` | Resource-URI |
| `Resource` | Naam van de server |
| `Category` | `MySqlAuditLogs` |
| `OperationName` | `LogEvent` |
| `LogicalServerName_s` | Naam van de server |
| `event_class_s` | `table_access_log` |
| `event_subclass_s` | `READ`, `INSERT`, `UPDATE`, of `DELETE` |
| `connection_id_d` | De unieke verbindings-ID die is gegenereerd door MySQL |
| `db_s` | Naam van de database geopend |
| `table_s` | Naam van de tabel geopend |
| `sql_text_s` | Volledige querytekst |
| `\_ResourceId` | Resource-URI |

## <a name="next-steps"></a>Volgende stappen

- [Controlelogboeken configureren in Azure portal](howto-configure-audit-logs-portal.md)