---
title: Azure Resource Manager-tag-ondersteuning voor bronnen
description: Laat zien welke typen Azure-resource bieden ondersteuning voor tags. Bevat details voor alle Azure-services.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/02/2019
ms.author: tomfitz
ms.openlocfilehash: 50ea7a2446b5560bd208b2da128fa877068ce452
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000288"
---
# <a name="tag-support-for-azure-resources"></a>Tag-ondersteuning voor Azure-resources
Dit artikel wordt beschreven of een resourcetype ondersteunt [tagging](resource-group-using-tags.md).

## <a name="aad-domain-services"></a>AAD Domain Services
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| domeinen | Nee | 

## <a name="ad-hybrid-health-service"></a>Hybride AD Health-Service
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| addsservices | Nee |
| aadsupportcases | Nee | 
| agents | Nee | 
| anonymousapiusers | Nee | 
| configuratie | Nee | 
| logboeken | Nee | 
| rapporten | Nee | 
| services | Nee | 
| servicehealthmetrics | Nee | 

## <a name="aks"></a>AKS
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| managedClusters | Ja | 

## <a name="analysis-services"></a>Analysis Services
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 

## <a name="api-hubs"></a>API-Hubs
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| apiManagementAccounts | Nee | 
| apiManagementAccounts/API 's | Nee | 
| apiManagementAccounts/connectionAcls | Nee | 
| apiManagementAccounts/connectionProviders | Nee | 
| apiManagementAccounts/connectionProviderAcls | Nee | 
| apiManagementAccounts/verbindingen | Nee | 

## <a name="api-management"></a>API Management
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| service | Ja | 

## <a name="automation"></a>Automation
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| AutomationAccounts | Ja | 
| automationAccounts/configuraties | Ja | 
| automationAccounts/taken | Nee | 
| automationAccounts/runbooks | Ja | 
| automationAccounts/softwareUpdateConfigurations | Nee | 
| automationAccounts/webhooks | Nee | 

## <a name="azure-database-for-mariadb"></a>Azure Database for MariaDB
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers-configuraties | Nee |
| servers/databases | Nee |
| servers/firewallRules | Nee |
| servers/recoverableServers | Nee | 
| servers/securityAlertPolicies | Nee |
| servers/virtualNetworkRules | Nee | 

## <a name="azure-database-for-mysql"></a>Azure Database for MySQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers-configuraties | Nee |
| servers/databases | Nee |
| servers/firewallRules | Nee |
| servers/recoverableServers | Nee | 
| servers/securityAlertPolicies | Nee |
| servers/virtualNetworkRules | Nee | 

## <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers/adviseurs | Nee | 
| servers-configuraties | Nee |
| servers/databases | Nee |
| servers/firewallRules | Nee |
| servers/queryTexts | Nee | 
| servers/recoverableServers | Nee | 
| servers/securityAlertPolicies | Nee |
| servers/topQueryStatistics | Nee | 
| servers/virtualNetworkRules | Nee | 
| servers/waitStatistics | Nee | 

## <a name="batch"></a>Batch
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| batchAccounts | Ja | 

## <a name="bing-maps"></a>Bing Maps
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| mapApis | Ja | 

## <a name="biztalk-services"></a>BizTalk Services
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| BizTalk | Ja | 

## <a name="cache"></a>Cache
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Redis | Ja | 

## <a name="cdn"></a>CDN
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| edgenodes | Nee | 
| Profielen | Ja | 
| profielen/eindpunten | Ja | 
| profielen/eindpunten/customdomains | Nee | 
| profielen/eindpunten/oorsprongen | Nee | 
| validateProbe | Nee | 

## <a name="classic-compute"></a>Klassieke Compute
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| domainNames | Nee | 
| domainNames/sleuven | Nee | 
| sleuven-domainNames-rollen | Nee | 
| virtuele machines | Nee | 
| virtuele machines/diagnosticSettings | Nee | 
| virtuele machines/metricDefinitions | Nee | 
| virtuele machines/metrische gegevens | Nee | 

## <a name="classic-infrastructure-migrate"></a>Migreren van klassieke infrastructuur
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| classicInfrastructureResources | Nee | 

## <a name="classic-network"></a>Klassiek netwerk
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| virtualNetworks | Nee | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nee | 
| virtualNetworks/virtualNetworkPeerings | Nee | 

## <a name="classic-storage"></a>Klassieke opslag
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageAccounts/services | Nee | 
| storageAccounts/services/diagnosticSettings | Nee | 

## <a name="compute"></a>Compute
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| availabilitySets | Ja | 
| Schijven | Ja | 
| images | Ja | 
| restorePointCollections | Ja | 
| restorePointCollections/restorePoints | Nee | 
| sharedVMImages | Ja | 
| sharedVMImages /-versies | Ja | 
| momentopnamen | Ja | 
| virtuele machines | Ja | 
| virtuele machines/diagnosticSettings | Nee | 
| virtuele machines/extensies | Ja | 
| virtuele machines/metricDefinitions | Nee | 
| virtualMachineScaleSets | Ja | 
| virtualMachineScaleSets/extensies | Nee | 
| virtualMachineScaleSets/netwerkinterfaces | Nee | 
| virtualMachineScaleSets/publicIPAddresses | Nee | 
| virtualMachineScaleSets/virtuele machines | Nee | 
| virtualMachineScaleSets/virtuele machines/netwerkinterfaces | Nee | 

## <a name="container"></a>Container
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| containerGroups | Ja | 

## <a name="container-instance"></a>Container-exemplaar
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| containerGroups | Ja | 
| serviceAssociationLinks | Nee | 

## <a name="container-registry"></a>Container Registry
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| registers | Ja | 
| registers/replicaties | Ja |
| registers/taken | Ja |
| registers/webhooks | Ja |

## <a name="container-service"></a>Container Service
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| containerServices | Ja | 

## <a name="cortana-analytics"></a>Cortana Analytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="cosmos-db"></a>Cosmos DB
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| databaseAccounts | Ja | 
| databaseAccountNames | Nee | 

## <a name="cost-management"></a>Cost Management
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Connectors | Ja | 

## <a name="data-box"></a>Data Box
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| taken | Ja | 

## <a name="data-box-edge"></a>Data Box Edge
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| DataBoxEdgeDevices | Ja | 

## <a name="data-catalog"></a>Data Catalog
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| catalogi | Ja | 

## <a name="data-connect"></a>Gegevens verbinden
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| connectionManagers | Ja | 

## <a name="data-factory"></a>Data Factory
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| dataFactories | Ja | 
| dataFactories/diagnosticSettings | Nee | 
| dataFactories/metricDefinitions | Nee | 
| dataFactorySchema | Nee | 
| factory 's | Ja | 
| factory's / integrationRuntimes | Nee | 

## <a name="devices"></a>Apparaten
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| IotHubs | Ja | 
| IotHubs/eventGridFilters | Nee | 
| ProvisioningServices | Ja | 

## <a name="devspaces"></a>Devspaces
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Domeincontrollers | Ja | 

## <a name="devtest-lab"></a>Devtest Lab
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Labs | Ja | 
| Labs/artifactsources | Ja |
| Labs/kosten | Ja |
| Labs/customimages | Ja |
| Labs/formules | Ja |
| Labs/notificationchannels | Ja |
| policysets-Labs-beleid | Ja |
| Labs/schema 's | Ja |
| Labs/serviceRunners | Ja | 
| Labs/gebruikers | Ja |
| gebruikers-Labs-schijven | Ja |
| gebruikers-Labs-omgevingen | Ja |
| gebruikers-Labs-geheimen | Ja |
| Labs/gebruikers/servicefabrics | Ja |
| Labs/gebruikers/servicefabrics/schema 's | Ja |
| Labs/virtuele machines | Ja | 
| Labs/virtuele machines/schema 's | Ja |
| Labs/virtualnetworks | Ja |
| Schema 's | Ja | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| lcsprojects | Nee | 
| lcsprojects/connectors | Nee | 
| lcsprojects/clouddeployments | Nee | 

## <a name="event-grid"></a>Event Grid
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| domeinen | Ja | 
| domeinen/onderwerpen | Nee | 
| eventSubscriptions | Nee | 
| extensionTopics | Nee | 
| onderwerpen | Ja | 
| topicTypes | Nee | 

## <a name="event-hub"></a>Event Hub
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| Naamruimten | Ja | 
| naamruimten/AuthorizationRules | Nee |
| naamruimten/disasterRecoveryConfigs | Nee |
| naamruimten/Event hubs | Nee |
| Event hubs-naamruimten/authorizationRules | Nee |
| Event hubs-naamruimten/consumergroups | Nee |

## <a name="hana-on-azure"></a>Hana op Azure
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| hanaInstances | Ja | 

## <a name="hdinsight"></a>HDInsight
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/toepassingen | Nee | 

## <a name="import-export"></a>Importeren en exporteren
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| taken | Ja | 

## <a name="insights"></a>Inzichten
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| actionGroups | Ja |
| activityLogAlerts | Ja |
| alertrules | Ja |
| automatedExportSettings | Nee | 
| Onderdelen | Ja | 
| onderdelen/gebeurtenissen | Nee | 
| onderdelen/metrische gegevens | Nee | 
| onderdelen/pricingPlans | Nee | 
| onderdelen/query | Nee | 
| logboeken | Nee | 
| metricAlerts | Ja |
| migrateToNewPricingModel | Nee | 
| myWorkbooks | Nee | 
| query's | Nee | 
| rollbackToLegacyPricingModel | Nee | 
| scheduledqueryrules | Ja | 
| webtests | Ja | 
| werkmappen | Ja | 

## <a name="key-vault"></a>Key Vault
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| deletedVaults | Nee | 
| Kluizen | Ja | 
| kluizen/accessPolicies | Nee | 
| Kluizen/geheimen | Nee | 

## <a name="log-analytics"></a>Log Analytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| logboeken | Nee | 

## <a name="logic"></a>Logica
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| integrationAccounts | Ja | 
| Werkstromen | Ja | 

## <a name="machine-learning-services"></a>Machine Learning Services
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| werkruimten | Ja | 
| werkruimten/berekeningen | Nee | 

## <a name="managed-identity"></a>Beheerde identiteit
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Identiteiten | Nee | 
| userAssignedIdentities | Ja | 

## <a name="marketplace-apps"></a>Marketplace-Apps
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| classicDevServices | Ja | 

## <a name="marketplace-ordering"></a>Marketplace-bestellingen
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Overeenkomsten | Nee | 
| offertypes | Nee | 

## <a name="media"></a>Media
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| mediaservices | Ja | 
| mediaservices/accountFilters | Nee | 
| mediaservices/activa | Nee | 
| activa-mediaservices/assetFilters | Nee | 
| mediaservices/contentKeyPolicies | Nee | 
| mediaservices/eventGridFilters | Nee | 
| mediaservices/liveEventOperations | Nee | 
| mediaservices/liveEvents | Ja | 
| mediaservices/liveEvents/liveOutputs | Nee | 
| mediaservices/liveOutputOperations | Nee | 
| mediaservices/door | Ja | 
| mediaservices/streamingEndpointOperations | Nee | 
| mediaservices/streamingLocators | Nee | 
| mediaservices/streamingPolicies | Nee | 
| mediaservices/transformaties | Nee | 
| transformaties-mediaservices-taken | Nee | 

## <a name="network"></a>Netwerk
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| applicationGateways | Ja | 
| applicationSecurityGroups | Ja | 
| azureFirewalls | Ja | 
| verbindingen | Ja | 
| ddosProtectionPlans | Ja | 
| expressRouteCircuits | Ja | 
| frontdoors | Ja | 
| frontdoorWebApplicationFirewallPolicies | Ja | 
| interfaceEndpoints | Ja | 
| load balancers | Ja | 
| localNetworkGateways | Ja | 
| networkIntentPolicies | Ja | 
| Netwerkinterfaces | Ja | 
| networkProfiles | Ja | 
| networkSecurityGroups | Ja | 
| networkWatchers | Ja | 
| networkWatchers/connectionMonitors | Ja | 
| networkWatchers/lenzen | Ja | 
| networkWatchers/pingMeshes | Ja | 
| privateLinkServices | Ja | 
| publicIPAddresses | Ja | 
| publicIPPrefixes | Ja | 
| routeFilters | Ja | 
| routeTables | Ja | 
| serviceEndpointPolicies | Ja | 
| virtualHubs | Ja | 
| virtualNetworks | Ja | 
| virtualNetworkGateways | Ja | 
| virtualNetworkTaps | Ja | 
| virtualWans | Ja | 
| vpnGateways | Ja | 
| vpnSites | Ja | 
| webApplicationFirewallPolicies | Ja | 

## <a name="notification-hubs"></a>Notification Hubs
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Naamruimten | Ja | 
| naamruimten/notificationHubs | Ja | 

## <a name="operational-insights"></a>Operational Insights
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| werkruimten | Ja |
| werkruimten/gegevensbronnen | Ja |
| werkruimten/linkedServices | Ja |
| werkruimten/savedSearches | Nee |
| werkruimten/storageInsightConfigs | Ja |

## <a name="operations-management"></a>Operations Management
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| oplossingen | Nee |

## <a name="portal"></a>Portal
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Dashboards | Ja | 

## <a name="portal-sdk"></a>Portal-SDK
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| rootResources | Ja | 

## <a name="power-bi"></a>Power BI
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| workspaceCollections | Ja | 

## <a name="recovery-services"></a>Recovery Services
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| backupProtectedItems | Nee | 
| Kluizen | Ja | 

## <a name="relay"></a>Relay
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Naamruimten | Ja | 

## <a name="resources"></a>Resources
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Resourcegroepen | Ja | 
| abonnementen/resourcegroepen | Ja | 

## <a name="scheduler"></a>Scheduler
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| taakverzamelingen | Ja | 
| stromen | Ja | 

## <a name="search"></a>Search
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| resourceHealthMetadata | Nee | 
| searchServices | Ja | 

## <a name="security"></a>Beveiliging
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| dataCollectionAgents | Nee | 

## <a name="service-bus"></a>Service Bus
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Naamruimten | Ja | 
| naamruimten/eventgridfilters | Nee | 

## <a name="service-fabric"></a>Service Fabric
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/toepassingen | Nee | 

## <a name="service-fabric-mesh"></a>Service Fabric Mesh
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| toepassingen | Ja | 
| Netwerken | Ja | 
| volumes | Ja | 

## <a name="signalr-service"></a>SignalR Service
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| SignalR | Ja | 

## <a name="site-recovery"></a>Site Recovery
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| SiteRecoveryVault | Ja | 

## <a name="solutions"></a>Oplossingen
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| toepassingen | Ja | 
| applicationDefinitions | Ja | 
| jitRequests | Ja | 

## <a name="sql"></a>SQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| locaties/instanceFailoverGroups | Nee |
| managedInstances | Ja |
| managedInstances/databases | Ja |
| managedInstances/databases/backupShortTermRetentionPolicies | Nee |
| databases/managedInstances/schema's / kolommen/tabellen/sensitivityLabels | Nee |
| managedInstances/databases/vulnerabilityAssessments | Nee |
| managedInstances/databases/vulnerabilityAssessments/regels/basislijnen | Nee |
| managedInstances/encryptionProtector | Nee |
| managedInstances/sleutels | Nee |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nee |
| managedInstances/vulnerabilityAssessments | Nee |
| servers | Ja |
| servers/beheerders | Nee |
| servers/adviseurs | Nee |
| servers/auditingSettings | Nee |
| servers/backupLongTermRetentionVaults | Nee |
| servers/communicationLinks | Nee |
| servers/connectionPolicies | Nee |
| servers/databases | Ja |
| databases-servers/adviseurs | Nee |
| servers/databases/auditingSettings | Nee |
| servers/databases/backupLongTermRetentionPolicies | Nee |
| servers/databases/backupShortTermRetentionPolicies | Nee |
| servers/databases/connectionPolicies | Nee |
| servers/databases/dataMaskingPolicies | Nee |
| servers/databases/dataMaskingPolicies/regels | Nee |
| servers/databases/extendedAuditingSettings | Nee |
| databases-servers-extensies | Nee |
| servers/databases/geoBackupPolicies | Nee |
| databases/servers/schema's / kolommen/tabellen/sensitivityLabels | Nee |
| servers/databases/securityAlertPolicies | Nee |
| servers/databases/syncGroups | Nee |
| servers/databases/syncGroups/syncMembers | Nee |
| servers/databases/transparentDataEncryption | Nee |
| servers/databases/vulnerabilityAssessments | Nee |
| servers/databases/vulnerabilityAssessments/regels/basislijnen | Nee |
| servers/disasterRecoveryConfiguration | Nee |
| servers/dnsAliases | Nee |
| servers/elasticPools | Ja |
| servers/encryptionProtector | Nee |
| servers/extendedAuditingSettings | Nee |
| servers/failoverGroups | Ja |
| servers/firewallRules | Nee |
| servers/jobAgents | Ja |
| jobAgents-servers-referenties | Nee |
| jobAgents-servers-taken | Nee |
| servers/jobAgents/taken/uitvoeringen | Nee |
| servers/jobAgents/taken/stappen | Nee |
| servers/jobAgents/targetGroups | Nee |
| servers/sleutels | Nee |
| servers/securityAlertPolicies | Nee |
| servers/syncAgents | Nee |
| servers/virtualNetworkRules | Nee |
| servers/vulnerabilityAssessments | Nee |

## <a name="sql-virtual-machine"></a>Virtuele machine van SQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| DWVM | Ja | 

## <a name="storage"></a>Storage
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageAccounts | Ja | 
| storageAccounts/blobServices | Nee | 
| storageAccounts/fileServices | Nee | 
| storageAccounts/queueServices | Nee | 
| storageAccounts/services | Nee | 
| storageAccounts/services/metricDefinitions | Nee | 
| storageAccounts/tableServices | Nee | 

## <a name="storage-sync"></a>Opslagsynchronisatieservice
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageSyncServices | Ja | 
| storageSyncServices/een geregistreerde server | Nee | 
| storageSyncServices/syncGroups | Nee | 
| storageSyncServices/syncGroups/cloudEndpoints | Nee | 
| storageSyncServices/syncGroups/serverEndpoints | Nee | 
| storageSyncServices/werkstromen | Nee | 

## <a name="storsimple"></a>Storsimple
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| managers | Ja | 

## <a name="stream-analytics"></a>Stream Analytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| streamingjobs | Ja | 
| streamingjobs/diagnosticSettings | Nee | 
| streamingjobs/metricDefinitions | Nee | 

## <a name="subscription"></a>Abonnement
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| SubscriptionDefinitions | Nee | 
| SubscriptionOperations | Nee | 

## <a name="support"></a>Ondersteuning
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| supporttickets | Nee | 

## <a name="visual-studio"></a>Visual Studio
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| account | Ja | 
| -/ accountextensie | Ja | 
| account of het aangevraagde project | Ja | 

## <a name="web"></a>Web
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| apiManagementAccounts | Nee | 
| apiManagementAccounts/apiAcls | Nee | 
| apiManagementAccounts/API 's | Nee | 
| apiManagementAccounts/API/apiAcls | Nee | 
| apiManagementAccounts/API/connectionAcls | Nee | 
| API's-apiManagementAccounts-verbindingen | Nee | 
| apiManagementAccounts/API/verbindingen/connectionAcls | Nee | 
| apiManagementAccounts/API/localizedDefinitions | Nee | 
| apiManagementAccounts/connectionAcls | Nee | 
| apiManagementAccounts/verbindingen | Nee | 
| billingMeters | Nee | 
| certificaten | Ja | 
| connectionGateways | Ja | 
| verbindingen | Ja | 
| customApis | Ja | 
| deletedSites | Nee | 
| functions | Nee | 
| hostingEnvironments | Ja | 
| hostingEnvironments/metrische gegevens | Nee | 
| hostingEnvironments/multiRolePools | Nee | 
| hostingEnvironments/workerPools | Nee | 
| publishingUsers | Nee | 
| serverFarms | Ja | 
| serverFarms/werkrollen | Nee | 
| Sites | Ja | 
| sites/domainOwnershipIdentifiers | Nee | 
| sites/hostNameBindings | Nee | 
| sites/instanties | Nee | 
| exemplaren-websites-extensies | Nee | 
| sites/metrische gegevens | Nee | 
| sites/premieraddons | Ja | 
| sites/sleuven | Ja | 
| sites/sleuven/hostNameBindings | Nee | 
| sleuven-sites-instanties | Nee | 
| sites/sleuven/exemplaren/extensies | Nee | 
| sites/sleuven/metrische gegevens | Nee | 
| sourceControls | Nee | 
| Valideren | Nee | 
| verifyHostingEnvironmentVnet | Nee | 

## <a name="xrm"></a>XRM
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| organizations | Nee | 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tags toepassen op resources, [tags gebruiken om uw Azure-resources te organiseren](resource-group-using-tags.md).
