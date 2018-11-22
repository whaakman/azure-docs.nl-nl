---
title: Azure Resource Manager-tag-ondersteuning voor bronnen
description: Laat zien welke typen Azure-resource bieden ondersteuning voor tags. Bevat details voor alle Azure-services.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 11/20/2018
ms.author: tomfitz
ms.openlocfilehash: a4bb423dc5eddde0fd2d2b9b4f263ab39dbd801f
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284979"
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
| services | Nee | 
| addsservices | Nee | 
| configuratie | Nee | 
| agents | Nee | 
| aadsupportcases | Nee | 
| Rapporten | Nee | 
| servicehealthmetrics | Nee | 
| logboeken | Nee | 
| anonymousapiusers | Nee | 

## <a name="analysis-services"></a>Analysis Services
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 

## <a name="api-hubs"></a>API-Hubs
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| apiManagementAccounts | Nee | 
| apiManagementAccounts/connectionProviders | Nee | 
| apiManagementAccounts/verbindingen | Nee | 
| apiManagementAccounts/connectionAcls | Nee | 
| apiManagementAccounts/connectionProviderAcls | Nee | 
| apiManagementAccounts/API 's | Nee | 

## <a name="api-management"></a>API Management
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| service | Ja | 

## <a name="automation"></a>Automation
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| AutomationAccounts | Ja | 
| automationAccounts/runbooks | Ja | 
| automationAccounts/configuraties | Ja | 
| automationAccounts/webhooks | Nee | 
| automationAccounts/softwareUpdateConfigurations | Nee | 
| automationAccounts/taken | Nee | 

## <a name="batch"></a>Batch
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| batchAccounts | Ja | 

## <a name="batch-ai"></a>Batch AI
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| taken | Ja | 
| bestandsservers | Ja | 
| werkruimten | Ja | 
| werkruimten/clusters | Nee | 
| werkruimten/bestandsservers | Nee | 
| werkruimten/experimenten | Nee | 
| experimenten-werkruimten-taken | Nee | 

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
| Profielen | Ja | 
| profielen/eindpunten | Ja | 
| profielen/eindpunten/oorsprongen | Nee | 
| profielen/eindpunten/customdomains | Nee | 
| validateProbe | Nee | 
| edgenodes | Nee | 

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
| virtualNetworks/virtualNetworkPeerings | Nee | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nee | 

## <a name="classic-storage"></a>Klassieke opslag
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageAccounts/services | Nee | 
| storageAccounts/services/diagnosticSettings | Nee | 

## <a name="compute"></a>Compute
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| availabilitySets | Ja | 
| virtuele machines | Ja | 
| virtuele machines/extensies | Ja | 
| virtualMachineScaleSets | Ja | 
| virtualMachineScaleSets/extensies | Nee | 
| virtualMachineScaleSets/virtuele machines | Nee | 
| virtualMachineScaleSets/netwerkinterfaces | Nee | 
| virtualMachineScaleSets/virtuele machines/netwerkinterfaces | Nee | 
| virtualMachineScaleSets/publicIPAddresses | Nee | 
| restorePointCollections | Ja | 
| restorePointCollections/restorePoints | Nee | 
| virtuele machines/diagnosticSettings | Nee | 
| virtuele machines/metricDefinitions | Nee | 
| sharedVMImages | Ja | 
| sharedVMImages /-versies | Ja | 
| Schijven | Ja | 
| momentopnamen | Ja | 
| images | Ja | 

## <a name="container"></a>Container
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| containerGroups | Ja | 

## <a name="container-instance"></a>Container-exemplaar
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| containerGroups | Ja | 
| serviceAssociationLinks | Nee | 

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
| factory 's | Ja | 
| factory's / integrationRuntimes | Nee | 
| dataFactories/diagnosticSettings | Nee | 
| dataFactories/metricDefinitions | Nee | 
| dataFactorySchema | Nee | 

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
| Schema 's | Ja | 
| Labs/virtuele machines | Ja | 
| Labs/serviceRunners | Ja | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| lcsprojects | Nee | 
| lcsprojects/connectors | Nee | 
| lcsprojects/clouddeployments | Nee | 

## <a name="event-grid"></a>Event Grid
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| eventSubscriptions | Nee | 
| onderwerpen | Ja | 
| domeinen | Ja | 
| domeinen/onderwerpen | Nee | 
| topicTypes | Nee | 
| extensionTopics | Nee | 

## <a name="event-hub"></a>Event Hub
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Naamruimten | Ja | 
| Clusters | Ja | 

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
| Onderdelen | Ja | 
| onderdelen/query | Nee | 
| onderdelen/metrische gegevens | Nee | 
| onderdelen/gebeurtenissen | Nee | 
| webtests | Ja | 
| query's | Nee | 
| scheduledqueryrules | Ja | 
| onderdelen/pricingPlans | Nee | 
| migrateToNewPricingModel | Nee | 
| rollbackToLegacyPricingModel | Nee | 
| automatedExportSettings | Nee | 
| werkmappen | Ja | 
| myWorkbooks | Nee | 
| logboeken | Nee | 

## <a name="key-vault"></a>Key Vault
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Kluizen | Ja | 
| Kluizen/geheimen | Nee | 
| kluizen/accessPolicies | Nee | 
| deletedVaults | Nee | 

## <a name="log-analytics"></a>Log Analytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| logboeken | Nee | 

## <a name="logic"></a>Logica
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Werkstromen | Ja | 
| integrationAccounts | Ja | 

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

## <a name="mariadb"></a>MariaDB
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers/recoverableServers | Nee | 
| servers/virtualNetworkRules | Nee | 

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
| mediaservices/activa | Nee | 
| mediaservices/contentKeyPolicies | Nee | 
| mediaservices/streamingLocators | Nee | 
| mediaservices/streamingPolicies | Nee | 
| mediaservices/eventGridFilters | Nee | 
| mediaservices/transformaties | Nee | 
| transformaties-mediaservices-taken | Nee | 
| mediaservices/door | Ja | 
| mediaservices/liveEvents | Ja | 
| mediaservices/liveEvents/liveOutputs | Nee | 
| mediaservices/streamingEndpointOperations | Nee | 
| mediaservices/liveEventOperations | Nee | 
| mediaservices/liveOutputOperations | Nee | 
| activa-mediaservices/assetFilters | Nee | 
| mediaservices/accountFilters | Nee | 

## <a name="mysql"></a>MySQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers/recoverableServers | Nee | 
| servers/virtualNetworkRules | Nee | 

## <a name="network"></a>Netwerk
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| virtualNetworks | Ja | 
| publicIPAddresses | Ja | 
| Netwerkinterfaces | Ja | 
| interfaceEndpoints | Ja | 
| load balancers | Ja | 
| networkSecurityGroups | Ja | 
| applicationSecurityGroups | Ja | 
| serviceEndpointPolicies | Ja | 
| networkIntentPolicies | Ja | 
| routeTables | Ja | 
| publicIPPrefixes | Ja | 
| networkWatchers | Ja | 
| networkWatchers/connectionMonitors | Ja | 
| networkWatchers/lenzen | Ja | 
| networkWatchers/pingMeshes | Ja | 
| virtualNetworkGateways | Ja | 
| localNetworkGateways | Ja | 
| verbindingen | Ja | 
| applicationGateways | Ja | 
| expressRouteCircuits | Ja | 
| routeFilters | Ja | 
| virtualWans | Ja | 
| vpnSites | Ja | 
| virtualHubs | Ja | 
| vpnGateways | Ja | 
| azureFirewalls | Ja | 
| virtualNetworkTaps | Ja | 
| privateLinkServices | Ja | 
| ddosProtectionPlans | Ja | 
| networkProfiles | Ja | 
| frontdoors | Ja | 
| frontdoorWebApplicationFirewallPolicies | Ja | 
| webApplicationFirewallPolicies | Ja | 

## <a name="notification-hubs"></a>Notification Hubs
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Naamruimten | Ja | 
| naamruimten/notificationHubs | Ja | 

## <a name="portal"></a>Portal
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Dashboards | Ja | 

## <a name="portal-sdk"></a>Portal-SDK
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| rootResources | Ja | 

## <a name="postgresql"></a>PostgreSQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers/recoverableServers | Nee | 
| servers/virtualNetworkRules | Nee | 
| servers/topQueryStatistics | Nee | 
| servers/queryTexts | Nee | 
| servers/waitStatistics | Nee | 
| servers/adviseurs | Nee | 

## <a name="power-bi"></a>Power BI
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| workspaceCollections | Ja | 

## <a name="recovery-services"></a>Recovery Services
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Kluizen | Ja | 
| backupProtectedItems | Nee | 

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
| searchServices | Ja | 
| resourceHealthMetadata | Nee | 

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

## <a name="sql-virtual-machine"></a>Virtuele machine van SQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| DWVM | Ja | 

## <a name="storage"></a>Storage
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageAccounts | Ja | 
| storageAccounts/blobServices | Nee | 
| storageAccounts/tableServices | Nee | 
| storageAccounts/queueServices | Nee | 
| storageAccounts/fileServices | Nee | 
| storageAccounts/services | Nee | 
| storageAccounts/services/metricDefinitions | Nee | 

## <a name="storage-sync"></a>Opslagsynchronisatieservice
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageSyncServices | Ja | 
| storageSyncServices/syncGroups | Nee | 
| storageSyncServices/syncGroups/cloudEndpoints | Nee | 
| storageSyncServices/syncGroups/serverEndpoints | Nee | 
| storageSyncServices/een geregistreerde server | Nee | 
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
| account of het aangevraagde project | Ja | 
| -/ accountextensie | Ja | 
| account | Ja | 
| account of het aangevraagde project | Ja | 
| -/ accountextensie | Ja | 

## <a name="web"></a>Web
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| sites/instanties | Nee | 
| sleuven-sites-instanties | Nee | 
| exemplaren-websites-extensies | Nee | 
| sites/sleuven/exemplaren/extensies | Nee | 
| publishingUsers | Nee | 
| Valideren | Nee | 
| sourceControls | Nee | 
| sites/hostNameBindings | Nee | 
| sites/domainOwnershipIdentifiers | Nee | 
| sites/sleuven/hostNameBindings | Nee | 
| certificaten | Ja | 
| serverFarms | Ja | 
| serverFarms/werkrollen | Nee | 
| Sites | Ja | 
| sites/sleuven | Ja | 
| sites/metrische gegevens | Nee | 
| sites/sleuven/metrische gegevens | Nee | 
| sites/premieraddons | Ja | 
| hostingEnvironments | Ja | 
| hostingEnvironments/multiRolePools | Nee | 
| hostingEnvironments/workerPools | Nee | 
| hostingEnvironments/metrische gegevens | Nee | 
| functions | Nee | 
| deletedSites | Nee | 
| apiManagementAccounts | Nee | 
| apiManagementAccounts/verbindingen | Nee | 
| apiManagementAccounts/connectionAcls | Nee | 
| apiManagementAccounts/API/verbindingen/connectionAcls | Nee | 
| apiManagementAccounts/API/connectionAcls | Nee | 
| apiManagementAccounts/apiAcls | Nee | 
| apiManagementAccounts/API/apiAcls | Nee | 
| apiManagementAccounts/API 's | Nee | 
| apiManagementAccounts/API/localizedDefinitions | Nee | 
| API's-apiManagementAccounts-verbindingen | Nee | 
| verbindingen | Ja | 
| customApis | Ja | 
| connectionGateways | Ja | 
| billingMeters | Nee | 
| verifyHostingEnvironmentVnet | Nee | 

## <a name="xrm"></a>XRM
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| organizations | Nee | 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tags toepassen op resources, [tags gebruiken om uw Azure-resources te organiseren](resource-group-using-tags.md).
