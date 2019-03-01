---
title: SQL Database-Audit-logboekindeling | Microsoft Docs
description: Krijg inzicht in hoe SQL Database-auditlogboeken worden opgebouwd.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 915ccc60216f3f206bcdc53825decac4d6d020d0
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57012026"
---
# <a name="sql-database-audit-log-format"></a>SQL Database-Audit-logboekindeling

[Met Azure SQL Database auditing](sql-database-auditing.md) houdt databasegebeurtenissen en geschreven naar een auditlogboek in uw Azure storage-account, of deze wordt verzonden naar Event Hub of Log Analytics voor downstream-verwerkingen en analyses.

## <a name="naming-conventions"></a>Naamconventies

### <a name="blob-audit"></a>BLOB-controle

Auditlogboeken die zijn opgeslagen in Blob-opslag zijn opgeslagen in een container met de naam `sqldbauditlogs` in de Azure Storage-account. De directory-hiërarchie in de container heeft de vorm `<ServerName>/<DatabaseName>/<AuditName>/<Date>/`. De indeling van de Blob filename is `<CreationTime>_<FileNumberInSession>.xel`, waarbij `CreationTime` is ingesteld op UTC `hh_mm_ss_ms` -indeling en `FileNumberInSession` is een index in het geval sessie reeksen in meerdere bestanden van de Blob-Logboeken.

Bijvoorbeeld, voor de database `Database1` op `Server1` is mogelijk geldig pad van het volgende:

    Server1/Database1/SqlDbAuditing_ServerAudit_NoRetention/2019-02-03/12_23_30_794_0.xel

### <a name="event-hub"></a>Event Hub

Auditgebeurtenissen worden geschreven naar de naamruimte en event hub die is gedefinieerd tijdens het configureren van controle en worden vastgelegd in de hoofdtekst van [Apache Avro](http://avro.apache.org/) gebeurtenissen en opgeslagen met behulp van JSON opmaak met UTF-8-codering. De om controlelogboeken te lezen, kunt u [Avro-hulpprogramma's](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview#use-avro-tools) of een vergelijkbaar hulpprogramma's waarmee deze indeling worden verwerkt.

### <a name="log-analytics"></a>Log Analytics

Auditgebeurtenissen worden geschreven naar Log Analytics-werkruimte gedefinieerd tijdens het configureren van controle op de `AzureDiagnostics` tabel met de categorie `SQLSecurityAuditEvents`. Zie voor meer nuttige informatie over opdrachten en Log Analytics-zoektaal, [Log Analytics zoeken verwijzing](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search).

## <a id="subheading-1"></a>Audit logboekvelden

| Naam (Blob) | Naam (Event Hubs/Log Analytics) | Description | Blob Type | Event Hubs/Log Analytics-Type |
|-------------|---------------------------------|-------------|-----------|-------------------------------|
| action_id | action_id_s | ID van de actie | varchar(4) | string |
| action_name | action_name_s | Naam van de actie | N/A | string |
| additional_information | additional_information_s | Aanvullende informatie over de gebeurtenis, opgeslagen als XML | nvarchar(4000) | string |
| affected_rows | affected_rows_d | Aantal rijen beïnvloed door de query | bigint | int |
| toepassingsnaam | application_name_s| Naam van de clienttoepassing | nvarchar(128) | string |
| audit_schema_version | audit_schema_version_d | Altijd 1 | int | int |
| class_type | class_type_s | Type controleerbare entiteit die de controle wordt uitgevoerd op | varchar(2) | string |
| class_type_desc | class_type_description_s | Beschrijving van controleerbare entiteit die de controle wordt uitgevoerd op | N/A | string |
| client_ip | client_ip_s | Bron-IP-adres van de clienttoepassing | nvarchar(128) | string |
| connection_id | N/A | ID van de verbinding in de server | GUID | N/A |
| data_sensitivity_information | data_sensitivity_information_s | Gegevenstypen en gevoeligheidslabels die zijn geretourneerd door de gecontroleerde query, op basis van de geclassificeerde kolommen in de database. Meer informatie over [Azure SQL-Database gegevens ontdekken en classificatie](sql-database-data-discovery-and-classification.md) | nvarchar(4000) | string |
| database_name | database_name_s | De databasecontext waarin de actie is opgetreden | Sysname | string |
| database_principal_id | database_principal_id_d | ID van de context van de database-gebruiker die de actie wordt uitgevoerd in | int | int |
| database_principal_name | database_principal_name_s | Naam van de database-gebruikerscontext waarin de actie wordt uitgevoerd | Sysname | string |
| duration_milliseconds | duration_milliseconds_d | Duur voor het uitvoeren van query's in milliseconden | bigint | int |
| event_time | event_time_t | Datum en tijd wanneer de controleerbare actie wordt geactiveerd | datetime2 | datum/tijd |
| host_name | N/A | Client-hostnaam | string | N/A |
| is_column_permission | is_column_permission_s | Vlag die aangeeft of dit de machtiging voor een kolom is. 1 = true, 0 = false | bits | string |
| N/A | is_server_level_audit_s | Vlag die aangeeft of deze controle op serverniveau is | N/A | string |
| object_-id | object_id_d | De ID van de entiteit op waarmee de controle is opgetreden. Dit omvat de: server-objecten, databases, database-objecten en schema-objecten. 0 als de entiteit de server zelf is of als de controle niet is uitgevoerd op het objectniveau van een | int | int |
| object_name | object_name_s | De naam van de entiteit op waarmee de controle is opgetreden. Dit omvat de: server-objecten, databases, database-objecten en schema-objecten. 0 als de entiteit de server zelf is of als de controle niet is uitgevoerd op het objectniveau van een | Sysname | string |
| permission_bitmask | permission_bitmask_s | Indien van toepassing is, ziet u de machtigingen die zijn verleend, geweigerd of ingetrokken | varbinary(16) | string |
| response_rows | response_rows_d | Aantal rijen in de resultatenset geretourneerd | bigint | int |
| schema_name | schema_name_s | De schemacontext waarin de actie is opgetreden. Null zijn voor controles plaatsvinden buiten een schema | Sysname | string |
| N/A | securable_class_type_s | Beveiligbaar object dat is toegewezen aan de class_type wordt gecontroleerd | N/A | string |
| sequence_group_id | sequence_group_id_g | Unieke id | varbinary | GUID |
| sequence_number | sequence_number_d | De volgorde van de records in een enkele audit-record die te groot is voor in de schrijfbuffer voor controles passen wordt bijgehouden | int | int |
| server_instance_name | server_instance_name_s | Naam van het serverexemplaar waarop de controle is opgetreden | Sysname | string |
| server_principal_id | server_principal_id_d | ID van de aanmeldingscontext waarin de actie wordt uitgevoerd | int | int |
| server_principal_name | server_principal_name_s | Huidige aanmelding | Sysname | string |
| server_principal_sid | server_principal_sid_s | Huidige aanmeldings-SID | varbinary | string |
| session_id | session_id_d | ID van de sessie waarop de gebeurtenis heeft plaatsgevonden | smallint | int |
| session_server_principal_name | session_server_principal_name_s | Server-principal voor sessie | Sysname | string |
| Instructie | statement_s | T-SQL-instructie die is uitgevoerd (indien aanwezig) | nvarchar(4000) | string |
| Is voltooid | succeeded_s | Geeft aan of de actie die de gebeurtenis is geslaagd. Voor gebeurtenissen dan aanmelding en batch rapport dit alleen of de machtigingscontrole is geslaagd of mislukt, niet de bewerking. 1 = voltooid, 0 = mislukt | bits | string |
| target_database_principal_id | target_database_principal_id_d | De databaseprincipal de weigeren-GRANT/REVOKE-bewerking wordt uitgevoerd op. 0 indien niet van toepassing | int | int |
| target_database_principal_name | target_database_principal_name_s | Doelgebruiker van de actie. NULL als niet van toepassing | string | string |
| target_server_principal_id | target_server_principal_id_d | Server-principal die de bewerking weigeren-GRANT/REVOKE is uitgevoerd op. Retourneert 0 als niet van toepassing | int | int |
| target_server_principal_name | target_server_principal_name_s | Doel-aanmelding van de actie. NULL als niet van toepassing | Sysname | string |
| target_server_principal_sid | target_server_principal_sid_s | De SID van de doel-aanmelding. NULL als niet van toepassing | varbinary | string |
| transaction_id | transaction_id_d | Alleen SQL Server (vanaf 2016) - 0 voor Azure SQL DB | bigint | int |
| user_defined_event_id | user_defined_event_id_d | Door gebruiker gedefinieerde gebeurtenis-id als een argument doorgegeven aan sp_audit_write. NULL voor systeemgebeurtenissen (standaard) en niet-nul voor de gebruiker gedefinieerde gebeurtenis. Zie voor meer informatie, [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | smallint | int |
| user_defined_information | user_defined_information_s | Door gebruiker gedefinieerde gegevens als een argument doorgegeven aan sp_audit_write. NULL voor systeemgebeurtenissen (standaard) en niet-nul voor de gebruiker gedefinieerde gebeurtenis. Zie voor meer informatie, [sp_audit_write (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-audit-write-transact-sql) | nvarchar(4000) | string |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure SQL Database auditing](sql-database-auditing.md).