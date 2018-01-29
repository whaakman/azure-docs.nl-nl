---
title: Azure diagnostische logboeken ondersteunde Services en schema's | Microsoft Docs
description: De ondersteunde schema van de services en evenementen voor Azure diagnostische logboeken kennen.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/24/2018
ms.author: johnkem
ms.openlocfilehash: 3e2b5305b969c96e6b14122af03da9249373094a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Ondersteunde services, schema's en categorieën voor Azure diagnostische logboeken

[Diagnostische logboeken van Azure-resource](monitoring-overview-of-diagnostic-logs.md) logboeken die door uw Azure-resources die de werking van die bron te beschrijven. Deze logboeken vormen een resourcetype specifieke. In dit artikel, geven we een overzicht op de set ondersteunde services en het schema van de gebeurtenis voor gebeurtenissen die door elke service. Dit artikel bevat ook een volledige lijst met beschikbare logboek categorieën per resourcetype.

## <a name="supported-services-and-schemas-for-resource-diagnostic-logs"></a>Ondersteunde services en schema's voor resource diagnostische logboeken
Het schema voor resource diagnostische logboeken varieert, afhankelijk van de bron- en logboekbestanden categorie.   

| Service | Schema & Docs |
| --- | --- |
| Analysis Services | Het schema is niet beschikbaar. |
| API Management | [Diagnostische logboeken van API Management](../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Toepassingsgateways |[Logboekregistratie van diagnostische gegevens voor de toepassingsgateway](../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Log analytics voor Azure Automation](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Diagnostische logboekregistratie van Azure Batch](../batch/batch-diagnostics.md) |
| Customer Insights | Het schema is niet beschikbaar. |
| CDN (Content Delivery Network) | Het schema is niet beschikbaar. |
| CosmosDB | [Azure Cosmos DB Logging](../cosmos-db/logging.md) |
| Data Lake Analytics |[Diagnostische logboeken openen voor Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Toegang tot diagnoselogboeken voor Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Diagnostische logboeken van Azure Event Hubs](../event-hubs/event-hubs-diagnostic-logs.md) |
| IoT Hub | [Bewerkingen van de IoT-Hub](../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Logboekregistratie van Azure Key Vault](../key-vault/key-vault-logging.md) |
| Load Balancer |[Logboekanalyse voor Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
| Logische apps |[Aangepast Logic Apps B2B-volgschema](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Netwerkbeveiligingsgroepen |[Logboekanalyses voor netwerkbeveiligingsgroepen (NSG's)](../virtual-network/virtual-network-nsg-manage-log.md) |
| DDOS-beveiliging | Het schema is niet beschikbaar. |
| Recovery Services | [Het gegevensmodel voor Azure Backup](../backup/backup-azure-reports-data-model.md)|
| Search |[Inschakelen en gebruiken van Search Traffic Analytics](../search/search-traffic-analytics.md) |
| Server Management | Het schema is niet beschikbaar. |
| Service Bus |[Diagnostische logboeken van Azure Service Bus](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Diagnostische logboekregistratie van Azure SQL Database](../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Diagnostische logboeken van taak](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Virtuele netwerken | Het schema is niet beschikbaar. |

## <a name="supported-log-categories-per-resource-type"></a>Logboek categorieën per resourcetype ondersteund
|Brontype|Categorie|Weergavenaam van de categorie|
|---|---|---|
|Microsoft.AnalysisServices/servers|Engine|Engine|
|Microsoft.AnalysisServices/servers|Service|Service|
|Microsoft.ApiManagement/service|GatewayLogs|Logboeken die betrekking hebben op ApiManagement Gateway|
|Microsoft.Automation/automationAccounts|JobLogs|Taaklogboeken|
|Microsoft.Automation/automationAccounts|JobStreams|Job Streams|
|Microsoft.Automation/automationAccounts|DscNodeStatus|Status van de DSC-knooppunt|
|Microsoft.Batch/batchAccounts|ServiceLog|Service Logs|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Hiermee worden de metrische gegevens van het eindpunt opgehaald, zoals bandbreedte, uitgaand verkeer enzovoort.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories|ActivityRuns|Pipeline-activiteitenlogboek wordt uitgevoerd|
|Microsoft.DataFactory/factories|PipelineRuns|Pijplijn uitgevoerd logboek|
|Microsoft.DataFactory/factories|TriggerRuns|Trigger voert logboek|
|Microsoft.DataLakeAnalytics/accounts|Controle|Auditlogboeken|
|Microsoft.DataLakeAnalytics/accounts|Aanvragen|Logboeken aanvragen|
|Microsoft.DataLakeStore/accounts|Controle|Auditlogboeken|
|Microsoft.DataLakeStore/accounts|Aanvragen|Logboeken aanvragen|
|Microsoft.Devices/IotHubs|Verbindingen|Verbindingen|
|Microsoft.Devices/IotHubs|DeviceTelemetry|De Apparaattelemetrie|
|Microsoft.Devices/IotHubs|C2DCommands|C2D-opdrachten|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Bewerkingen voor apparaat-id|
|Microsoft.Devices/IotHubs|FileUploadOperations|Bewerkingen voor het uploaden van een bestandsserver|
|Microsoft.Devices/IotHubs|Routes|Routes|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|Bewerkingen voor C2D-Twin|
|Microsoft.Devices/IotHubs|TwinQueries|Dubbele query 's|
|Microsoft.Devices/IotHubs|JobsOperations|Bewerkingen voor taken|
|Microsoft.Devices/IotHubs|DirectMethods|Rechtstreekse methoden|
|Microsoft.Devices/provisioningServices|DeviceOperations|Apparaatbewerkingen|
|Microsoft.Devices/provisioningServices|ServiceOperations|Servicebewerkingen|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archief Logboeken|
|Microsoft.EventHub/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Logboeken voor automatisch schalen|
|Microsoft.KeyVault/vaults|AuditEvent|Auditlogboeken|
|Microsoft.Logic/workflows|WorkflowRuntime|Diagnostische gebeurtenissen van de workflowruntime|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Gebeurtenissen van integratieaccounts bijhouden|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Gebeurtenis van Netwerkbeveiligingsgroep|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Regelteller van Netwerkbeveiligingsgroep|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Netwerkbeveiligingsgroep regel stroom gebeurtenis|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Load Balancer waarschuwingsgebeurtenissen|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Gezondheidsstatus van Load Balancer-test|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS-bescherming-meldingen|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|VM protection-waarschuwingen|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Toegangslogboek voor Application Gateway|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Toepassingslogboek van het Gateway-prestaties|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Toepassingslogboek van het Gateway-Firewall|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Diagnostische logboeken gateway|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Diagnostische logboeken tunnel|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Logboeken met diagnostische gegevens routeren|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Traffic Manager-test statusgebeurtenis resultaten|
|Microsoft.Network/expressRouteCircuits|GWMCountersTable|Tabel met GWM prestatiemeteritems|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Backup rapportgegevens|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery-taken|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery-gebeurtenissen|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery gerepliceerde Items|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery replicatie statistieken|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery-herstelpunten|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Azure Site Recovery replicatiegegevens uploaden snelheid|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Recovery beveiligde schijf Gegevensverloop|
|Microsoft.Search/searchServices|OperationLogs|Bewerkingslogboeken|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Query Store Runtime-statistieken|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Query Store wacht statistieken|
|Microsoft.Sql/servers/databases|Fouten|Fouten|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Wacht databasestatistieken|
|Microsoft.Sql/servers/databases|Time-outs|Time-outs|
|Microsoft.Sql/servers/databases|Blokken|Blokken|
|Microsoft.Sql/servers/databases|SQLInsights|SQL-inzichten|
|Microsoft.Sql/servers/databases|Controle|Auditlogboeken|
|Microsoft.StreamAnalytics/streamingjobs|Uitvoering|Uitvoering|
|Microsoft.StreamAnalytics/streamingjobs|Ontwerpen|Ontwerpen|

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over diagnostische logboeken](monitoring-overview-of-diagnostic-logs.md)
* [Diagnostische logboeken van de resource te streamen **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Diagnostische instellingen voor bronnen met de REST-API van Azure Monitor wijzigen](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Logboeken van de Azure-opslag met logboekanalyse analyseren](../log-analytics/log-analytics-azure-storage.md)
