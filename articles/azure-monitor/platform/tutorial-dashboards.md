---
title: Diagnostische logboeken in Azure ondersteunde services en schema 's
description: Meer informatie over de ondersteunde services en gebeurtenis-schema voor diagnostische logboeken van Azure.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 10/11/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: e31c957e9ef24079d6917109ec9c5f85928bfbd7
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54260972"
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Ondersteunde services, schema's en categorieën voor diagnostische logboeken van Azure

[Diagnostische logboeken in Azure Monitor](../../azure-monitor/platform/diagnostic-logs-overview.md) zijn logboeken van Azure-services die worden beschreven van de werking van deze services of resources. Alle logboeken met diagnostische gegevens beschikbaar zijn via Azure Monitor delen een gemeenschappelijk schema van de op het hoogste niveau met flexibiliteit voor elke service op de unieke eigenschappen voor hun eigen gebeurtenissen verzenden.

Een combinatie van het resourcetype (beschikbaar in de `resourceId` eigenschap) en de `category` unieke identificatie van een schema. Dit artikel beschrijft het schema op het hoogste niveau voor diagnostische logboeken en koppelingen naar de schema's voor elke service.

## <a name="top-level-diagnostic-logs-schema"></a>Op het hoogste niveau diagnostische logboeken schema

| Name | Vereist/optioneel | Description |
|---|---|---|
| time | Vereist | De tijdstempel (UTC) van de gebeurtenis. |
| resourceId | Vereist | De resource-ID van de resource die de gebeurtenis verzonden. Dit is van het formulier /tenants/tenant-id/providers/provider-name voor tenantservices. |
| tenantId | Vereist voor de tenant-Logboeken | De tenant-ID van de Active Directory-tenant die deze gebeurtenis is gekoppeld aan. Deze eigenschap wordt alleen gebruikt voor op tenantniveau-Logboeken is het niet wordt weergegeven in Logboeken op het niveau van resources. |
| operationName | Vereist | De naam van de bewerking die wordt vertegenwoordigd door deze gebeurtenis. Als de gebeurtenis een RBAC-bewerking stelt, is dit de naam van de RBAC-bewerking (bv. Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Doorgaans zijn gemodelleerd in de vorm van een Resource Manager-bewerking, zelfs als ze niet werkelijke gedocumenteerde Resource Manager-bewerkingen zijn (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Optioneel | De api-versie die is gekoppeld aan de bewerking als de operationName is uitgevoerd met behulp van een API (bijvoorbeeld) `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Als er geen API die overeenkomt met deze bewerking, vertegenwoordigt de versie van de versie van deze bewerking in het geval de eigenschappen die zijn gekoppeld aan de bewerking in de toekomst worden gewijzigd. |
| category | Vereist | De categorie van de logboekbestanden van de gebeurtenis. Categorie is de granulatie waarmee u kunt inschakelen of uitschakelen van Logboeken op een bepaalde resource. De eigenschappen die worden weergegeven in de blob eigenschappen van een gebeurtenis zijn hetzelfde binnen een bepaalde categorie en resource Logboektype. Typische logboekcategorieën zijn 'Controle' "operationele" "Uitvoeren" en "-aanvraag. |
| resultType | Optioneel | De status van de gebeurtenis. Typische waarden zijn gestart, wordt uitgevoerd, is voltooid, is mislukt, actieve en opgelost. |
| resultSignature | Optioneel | De substatus van de gebeurtenis. Als deze bewerking overeenkomt met een REST-API-aanroep, is dit de HTTP-statuscode van de bijbehorende REST-aanroep. |
| resultDescription | Optioneel | De beschrijving van de statische tekst van deze bewerking, bijvoorbeeld. 'Opslagbestand ophalen'. |
| durationMs | Optioneel | De duur van de bewerking in milliseconden. |
| callerIpAddress | Optioneel | De oproepende functie IP-adres als de bewerking komt overeen met een API-aanroep die afkomstig van een entiteit met een openbaar beschikbare IP-adres zijn zou. |
| correlationId | Optioneel | Een GUID die wordt gebruikt voor het groeperen van een set van gerelateerde gebeurtenissen. Normaal gesproken als twee gebeurtenissen de dezelfde operationName maar twee verschillende statussen (bijvoorbeeld hebben) 'Gestart' en 'Geslaagd'), ze delen dezelfde correlatie-ID. Dit kan ook andere relaties tussen gebeurtenissen staan. |
| identity | Optioneel | Een JSON-blob die wordt beschreven van de identiteit van de gebruiker of toepassing die de bewerking heeft uitgevoerd. Meestal dit is inclusief de autorisatie en de claims / JWT-token uit active directory. |
| Niveau | Optioneel | De ernst van de gebeurtenis. Moet een van de informatief, waarschuwing, fout of kritiek. |
| location | Optioneel | De regio van de resource dat de gebeurtenis, bijvoorbeeld verzendt. 'VS-Oost' of "Frankrijk-Zuid" |
| properties | Optioneel | Een uitgebreide eigenschappen met betrekking tot deze rubriek van gebeurtenissen. Alle aangepaste/unieke eigenschappen moeten binnen deze "deel B" van het schema worden geplaatst. |

## <a name="service-specific-schemas-for-resource-diagnostic-logs"></a>Service-specifieke schema's voor diagnostische logboeken van resource
Het schema voor diagnostische logboeken van resources, is afhankelijk van de categorie bron- en logboekbestanden. Deze lijst bevat alle services die de beschikbare diagnostische logboeken en koppelingen naar de service en de categorie-specifieke schema indien beschikbaar.

| Service | Schema en documenten |
| --- | --- |
| Azure Active Directory | [Overzicht](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), [Audit log schema](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) en [aanmeldingen schema](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Analysis Services | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| API Management | [Diagnostische logboeken van API Management](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Toepassingsgateways |[Diagnostische logboekregistratie voor Application Gateway](../../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Logboekanalyse voor Azure Automation](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch-diagnostische logboeken](../../batch/batch-diagnostics.md) |
| Azure Database for MySQL | [Azure Database voor MySQL diagnostische logboeken](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Azure Database for PostgreSQL | [Azure Database for PostgreSQL diagnostische logboeken](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Cognitive Services | Het schema is niet beschikbaar. |
| CDN (Content Delivery Network) | [Diagnostische logboeken voor CDN in Azure](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Azure Cosmos DB-logboekregistratie](../../cosmos-db/logging.md) |
| Data Factory | [Data Factory's met behulp van Azure Monitor bewaken](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Diagnostische logboeken openen voor Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Diagnostische logboeken openen voor Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Diagnostische logboeken in Azure Event Hubs](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Het schema is niet beschikbaar. |
| Azure Firewall | Het schema is niet beschikbaar. |
| IoT Hub | [IoT Hub-bewerkingen](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Logboekregistratie van Azure Key Vault](../../key-vault/key-vault-logging.md) |
| Load Balancer |[Logboekanalyse voor Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Aangepast Logic Apps B2B-volgschema](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Netwerkbeveiligingsgroepen |[Logboekanalyses voor netwerkbeveiligingsgroepen (NSG's)](../../virtual-network/virtual-network-nsg-manage-log.md) |
| DDoS Protection | [Azure DDoS-bescherming standaard beheren](../../virtual-network/manage-ddos-protection.md) |
| Power BI toegewezen | [Diagnostische logboekregistratie voor Power BI Embedded in Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Recovery Services | [Gegevensmodel voor Azure Backup](../../backup/backup-azure-reports-data-model.md)|
| Search |[Inschakelen en gebruiken van Search Traffic Analytics](../../search/search-traffic-analytics.md) |
| Service Bus |[Diagnostische logboeken in Azure Service Bus](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| SQL Database | [Azure SQL Database-diagnostische logboeken](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Diagnostische logboeken van taak](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Traffic Manager | Het schema is niet beschikbaar. |
| Virtuele netwerken | Het schema is niet beschikbaar. |
| Virtuele netwerkgateways | Het schema is niet beschikbaar. |

## <a name="supported-log-categories-per-resource-type"></a>Logboekcategorieën per resourcetype dat wordt ondersteund
|Resourcetype|Categorie|Categorie-weergavenaam|
|---|---|---|
|Microsoft.AnalysisServices/servers|Engine|Engine|
|Microsoft.AnalysisServices/servers|Service|Service|
|Microsoft.ApiManagement/service|GatewayLogs|Logboeken met betrekking tot de ApiManagement-Gateway|
|Microsoft.Automation/automationAccounts|JobLogs|Taaklogboeken|
|Microsoft.Automation/automationAccounts|JobStreams|Taakstromen|
|Microsoft.Automation/automationAccounts|DscNodeStatus|DSC-Knooppuntstatus|
|Microsoft.Batch/batchAccounts|ServiceLog|Servicelogboeken|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Hiermee worden de metrische gegevens van het eindpunt opgehaald, zoals bandbreedte, uitgaand verkeer enzovoort.|
|Microsoft.ClassicNetwork/networksecuritygroups|Gebeurtenis voor stroomregel van netwerkbeveiligingsgroep|Gebeurtenis voor stroomregel van netwerkbeveiligingsgroep|
|Microsoft.CognitiveServices/accounts|Controleren|Controlelogboeken|
|Microsoft.CognitiveServices/accounts|RequestResponse|Aanvraag- en antwoordlogboeken|
|Microsoft.ContainerService/managedClusters|kube-apiserver|Kubernetes API-server|
|Microsoft.ContainerService/managedClusters|kube-controller-manager|Kubernetes Controller Manager|
|Microsoft.ContainerService/managedClusters|cluster-automatisch schalen|Automatische schaalaanpassing van Kubernetes-cluster|
|Microsoft.ContainerService/managedClusters|kube-scheduler|Kubernetes Scheduler|
|Microsoft.ContainerService/managedClusters|GUARD|Verificatiewebhook|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataFactory/factories|ActivityRuns|Uitvoeringen van pijplijn activiteitenlogboek|
|Microsoft.DataFactory/factories|PipelineRuns|Pijplijnuitvoeringen log|
|Microsoft.DataFactory/factories|TriggerRuns|Triggeruitvoeringen log|
|Microsoft.DataLakeAnalytics/accounts|Controleren|Controlelogboeken|
|Microsoft.DataLakeAnalytics/accounts|Aanvragen|Logboeken van de aanvraag|
|Microsoft.DataLakeStore/accounts|Controleren|Controlelogboeken|
|Microsoft.DataLakeStore/accounts|Aanvragen|Logboeken van de aanvraag|
|Microsoft.DBforMySQL/servers|MySqlSlowLogs|MySQL-Server-logboeken|
|Microsoft.DBforPostgreSQL/servers|PostgreSQLLogs|PostgreSQL-Server-logboeken|
|Microsoft.Devices/IotHubs|Verbindingen|Verbindingen|
|Microsoft.Devices/IotHubs|DeviceTelemetry|Telemetrie van apparaten|
|Microsoft.Devices/IotHubs|C2DCommands|C2D-opdrachten|
|Microsoft.Devices/IotHubs|DeviceIdentityOperations|Bewerkingen voor apparaat-id|
|Microsoft.Devices/IotHubs|FileUploadOperations|Bestandsuploadbewerkingen|
|Microsoft.Devices/IotHubs|Routes|Routes|
|Microsoft.Devices/IotHubs|D2CTwinOperations|D2CTwinOperations|
|Microsoft.Devices/IotHubs|C2DTwinOperations|C2D dubbele bewerkingen|
|Microsoft.Devices/IotHubs|TwinQueries|Apparaatdubbel-query 's|
|Microsoft.Devices/IotHubs|JobsOperations|Taakbewerkingen|
|Microsoft.Devices/IotHubs|DirectMethods|Directe methoden|
|Microsoft.Devices/IotHubs|E2EDiagnostics|E2E Diagnostics (Preview)|
|Microsoft.Devices/IotHubs|Configuraties|Configuraties|
|Microsoft.Devices/provisioningServices|DeviceOperations|Apparaatbewerkingen|
|Microsoft.Devices/provisioningServices|ServiceOperations|Servicebewerkingen|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.DocumentDB/databaseAccounts|MongoRequests|MongoRequests|
|Microsoft.DocumentDB/databaseAccounts|QueryRuntimeStatistics|QueryRuntimeStatistics|
|Microsoft.EventHub/namespaces|ArchiveLogs|Logboeken archiveren|
|Microsoft.EventHub/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Logboeken voor automatisch schalen|
|Microsoft.Insights/AutoscaleSettings|AutoscaleEvaluations|Automatische schaalaanpassing gebruiken voor evaluaties|
|Microsoft.Insights/AutoscaleSettings|AutoscaleScaleActions|Automatische schaalaanpassing gebruiken voor schaalacties|
|Microsoft.IoTSpaces/Graph|Tracering|Tracering|
|Microsoft.IoTSpaces/Graph|Operationeel|Operationeel|
|Microsoft.IoTSpaces/Graph|Controleren|Controleren|
|Microsoft.IoTSpaces/Graph|UserDefinedFunction|UserDefinedFunction|
|Microsoft.IoTSpaces/Graph|Inkomend verkeer|Inkomend verkeer|
|Microsoft.IoTSpaces/Graph|Uitgaand verkeer|Uitgaand verkeer|
|Microsoft.KeyVault/vaults|AuditEvent|Controlelogboeken|
|Microsoft.Logic/workflows|WorkflowRuntime|Diagnostische gebeurtenissen van de workflowruntime|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Gebeurtenissen van integratieaccounts bijhouden|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Netwerkbeveiligingsgroepgebeurtenis|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Netwerkbeveiligingsgroep regel teller|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Load Balancer waarschuwingsgebeurtenissen|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|De Integriteitsstatus van de Load Balancer-test|
|Microsoft.Network/publicIPAddresses|DDoSProtectionNotifications|DDoS protection-meldingen|
|Microsoft.Network/publicIPAddresses|DDoSMitigationFlowLogs|Logboeken van de beslissingen voor DDoS-risicobeperking flow|
|Microsoft.Network/publicIPAddresses|DDoSMitigationReports|Rapporten van DDoS-oplossingen|
|Microsoft.Network/virtualNetworks|VMProtectionAlerts|Waarschuwingen voor VM-beveiliging|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Application Gateway-Toegangslogboek|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Prestaties van toepassingsgateway|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Firewall-toepassingsgateway|
|Microsoft.Network/securegateways|AzureFirewallApplicationRule|Azure-toepassing firewallregel|
|Microsoft.Network/securegateways|AzureFirewallNetworkRule|Azure-netwerk firewallregel|
|Microsoft.Network/azurefirewalls|AzureFirewallApplicationRule|Azure-toepassing firewallregel|
|Microsoft.Network/azurefirewalls|AzureFirewallNetworkRule|Azure-netwerk firewallregel|
|Microsoft.Network/virtualNetworkGateways|GatewayDiagnosticLog|Diagnostische logboeken van gateway|
|Microsoft.Network/virtualNetworkGateways|TunnelDiagnosticLog|Tunnel diagnostische logboeken|
|Microsoft.Network/virtualNetworkGateways|RouteDiagnosticLog|Route diagnostische logboeken|
|Microsoft.Network/virtualNetworkGateways|IKEDiagnosticLog|IKE-diagnostische logboeken|
|Microsoft.Network/virtualNetworkGateways|P2SDiagnosticLog|Diagnostische logboeken voor P2S|
|Microsoft.Network/trafficManagerProfiles|ProbeHealthStatusEvents|Traffic Manager-test statusgebeurtenis resultaten|
|Microsoft.Network/expressRouteCircuits|PeeringRouteLog|Peering Route-Table-Logboeken|
|Microsoft.Network/frontdoors|FrontdoorAccessLog|Frontdoor Toegangslogboek|
|Microsoft.Network/frontdoors|FrontdoorWebApplicationFirewallLog|Frontdoor Web Application Firewall-logboek|
|Microsoft.PowerBIDedicated/capacities|Engine|Engine|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Backup waarvoor gegevens zijn gerapporteerd|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery-taken|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery-gebeurtenissen|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery gerepliceerde Items|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationStats|Azure Site Recovery-statistieken|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryRecoveryPoints|Azure Site Recovery-herstelpunten|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicationDataUploadRate|Uploadsnelheid van replicatiegegevens voor Azure Site Recovery|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryProtectedDiskDataChurn|Azure Site Recovery beveiligde Gegevensverloop van bronschijf|
|Microsoft.Search/searchServices|OperationLogs|Bewerkingslogboeken|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Operationele Logboeken|
|Microsoft.Sql/servers/databases|SQLInsights|SQL-inzichten|
|Microsoft.Sql/servers/databases|AutomaticTuning|Automatisch instellen|
|Microsoft.Sql/servers/databases|QueryStoreRuntimeStatistics|Query Store Runtime-statistieken|
|Microsoft.Sql/servers/databases|QueryStoreWaitStatistics|Query Store wacht statistieken|
|Microsoft.Sql/servers/databases|Fouten|Fouten|
|Microsoft.Sql/servers/databases|DatabaseWaitStatistics|Statistieken over de wachttijd|
|Microsoft.Sql/servers/databases|Time-outs|Time-outs|
|Microsoft.Sql/servers/databases|blokken|blokken|
|Microsoft.Sql/servers/databases|Deadlocks|Deadlocks|
|Microsoft.Sql/servers/databases|Controleren|Controlelogboeken|
|Microsoft.Sql/servers/databases|SQLSecurityAuditEvents|Controlegebeurtenis voor SQL-beveiliging|
|Microsoft.Sql/servers/databases|DmsWorkers|DMS werknemers|
|Microsoft.Sql/servers/databases|ExecRequests|Exec aanvragen|
|Microsoft.Sql/servers/databases|RequestSteps|Stappen voor aanvraag|
|Microsoft.Sql/servers/databases|SqlRequests|SQL-aanvragen|
|Microsoft.Sql/servers/databases|Wachten op|Wachten op|
|Microsoft.Sql/managedInstances|ResourceUsageStats|Statistieken voor het gebruik van resource|
|Microsoft.Sql/managedInstances|SQLSecurityAuditEvents|Controlegebeurtenis voor SQL-beveiliging|
|Microsoft.Sql/managedInstances/databases|SQLInsights|SQL-inzichten|
|Microsoft.Sql/managedInstances/databases|QueryStoreRuntimeStatistics|Query Store Runtime-statistieken|
|Microsoft.Sql/managedInstances/databases|QueryStoreWaitStatistics|Query Store wacht statistieken|
|Microsoft.Sql/managedInstances/databases|Fouten|Fouten|
|Microsoft.StreamAnalytics/streamingjobs|Uitvoering|Uitvoering|
|Microsoft.StreamAnalytics/streamingjobs|Ontwerpen|Ontwerpen|
|microsoft.web/sites|FunctionExecutionLogs|Logboekbestanden voor het uitvoeren van functie|
|microsoft.web/sites/slots|FunctionExecutionLogs|Logboekbestanden voor het uitvoeren van functie|

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over diagnostische logboeken](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Diagnostische logboeken van de resource naar Stream **Event Hubs**](../../azure-monitor/platform/diagnostic-logs-stream-event-hubs.md)
* [Diagnostische instellingen voor resources met behulp van de REST-API van Azure Monitor wijzigt](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)
* [Logboeken van Azure storage met Log Analytics analyseren](../../azure-monitor/platform/collect-azure-metrics-logs.md)
