---
title: Azure Resource Manager-tag-ondersteuning voor bronnen
description: Laat zien welke typen Azure-resource bieden ondersteuning voor tags. Bevat details voor alle Azure-services.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 02/13/2019
ms.author: tomfitz
ms.openlocfilehash: dd6836ef4f859ce77c1a56095d32373d8e08f468
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56270382"
---
# <a name="tag-support-for-azure-resources"></a>Tag-ondersteuning voor Azure-resources
Dit artikel wordt beschreven of een resourcetype ondersteunt [tags](resource-group-using-tags.md).

Als u dezelfde gegevens als een bestand met door komma's gescheiden waarden, download [tag support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| DomainServices | Ja | 
| DomainServices/oucontainer | Nee | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| supportProviders | Nee | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| aadsupportcases | Nee | 
| addsservices | Nee | 
| agents | Nee | 
| anonymousapiusers | Nee | 
| configuratie | Nee | 
| logboeken | Nee | 
| rapporten | Nee | 
| services | Nee | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Configuraties | Nee | 
| generateRecommendations | Nee | 
| aanbevelingen | Nee | 
| suppressions | Nee | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| actionRules | Nee | 
| waarschuwingen | Nee | 
| alertsList | Nee | 
| alertsSummary | Nee | 
| alertsSummaryList | Nee | 
| smartDetectorAlertRules | Nee | 
| smartDetectorRuntimeEnvironments | Nee | 
| smartGroups | Nee | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| reportFeedback | Nee | 
| service | Ja | 
| validateServiceName | Nee | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| attestationProviders | Nee | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| classicAdministrators | Nee | 
| denyAssignments | Nee | 
| elevateAccess | Nee | 
| Hiermee vergrendelt u | Nee | 
| machtigingen | Nee | 
| policyAssignments | Nee | 
| policyDefinitions | Nee | 
| policySetDefinitions | Nee | 
| providerOperations | Nee | 
| roleAssignments | Nee | 
| roleDefinitions | Nee | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| automationAccounts | Ja | 
| automationAccounts/configurations | Ja | 
| automationAccounts/jobs | Nee | 
| automationAccounts/runbooks | Ja | 
| automationAccounts/softwareUpdateConfigurations | Nee | 
| automationAccounts/webhooks | Nee | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Omgevingen | Nee | 
| omgevingen/accounts | Nee | 
| accounts-omgevingen-naamruimten | Nee | 
| omgevingen/accounts/naamruimten/configuraties | Nee | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| b2cDirectories | Ja | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| registraties | Ja | 
| registrations/customerSubscriptions | Nee | 
| registraties/producten | Nee | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| batchAccounts | Ja | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| billingAccounts | Nee | 
| billingAccounts/billingProfiles | Nee | 
| billingAccounts/billingProfiles/billingSubscriptions | Nee | 
| billingAccounts/billingProfiles/invoices | Nee | 
| billingAccounts/billingProfiles/invoices/pricesheet | Nee | 
| billingAccounts/billingProfiles/operationStatus | Nee | 
| billingAccounts/billingProfiles/paymentMethods | Nee | 
| billingAccounts/billingProfiles/policies | Nee | 
| billingAccounts/billingProfiles/pricesheet | Nee | 
| billingAccounts/billingProfiles/products | Nee | 
| billingAccounts/billingProfiles/transactions | Nee | 
| billingAccounts/billingSubscriptions | Nee | 
| billingAccounts/departments | Nee | 
| billingAccounts/eligibleOffers | Nee | 
| billingAccounts/enrollmentAccounts | Nee | 
| billingAccounts/invoices | Nee | 
| billingAccounts/invoiceSections | Nee | 
| billingAccounts/invoiceSections/billingSubscriptions | Nee | 
| billingAccounts/invoiceSections/billingSubscriptions/transfer | Nee | 
| billingAccounts/invoiceSections/importRequests | Nee | 
| billingAccounts/invoiceSections/initiateImportRequest | Nee | 
| billingAccounts/invoiceSections/initiateTransfer | Nee | 
| billingAccounts/invoiceSections/operationStatus | Nee | 
| billingAccounts/invoiceSections/products | Nee | 
| billingAccounts/invoiceSections/transfers | Nee | 
| billingAccounts/products | Nee | 
| billingAccounts/projects | Nee | 
| billingAccounts/projects/billingSubscriptions | Nee | 
| billingAccounts/projects/importRequests | Nee | 
| billingAccounts/projects/initiateImportRequest | Nee | 
| billingAccounts/projects/operationStatus | Nee | 
| billingAccounts/projects/products | Nee | 
| billingAccounts/transactions | Nee | 
| billingPeriods | Nee | 
| BillingPermissions | Nee | 
| billingProperty | Nee | 
| BillingRoleAssignments | Nee | 
| BillingRoleDefinitions | Nee | 
| CreateBillingRoleAssignment | Nee | 
| afdelingen | Nee | 
| enrollmentAccounts | Nee | 
| importRequests | Nee | 
| importRequests/acceptImportRequest | Nee | 
| importRequests/declineImportRequest | Nee | 
| Facturen | Nee | 
| Overdrachten | Nee | 
| transfers/acceptTransfer | Nee | 
| overdrachten/declineTransfer | Nee | 
| transfers/operationStatus | Nee | 
| usagePlans | Nee | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| mapApis | Ja | 
| updateCommunicationPreference | Nee | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| BizTalk | Ja | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| blueprintAssignments | Nee | 
| blueprintAssignments/assignmentOperations | Nee | 
| blueprintAssignments/operations | Nee | 
| blauwdrukken | Nee | 
| blauwdrukken/artefacten | Nee | 
| blauwdrukken /-versies | Nee | 
| versies-blauwdrukken-artefacten | Nee | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| botServices | Ja | 
| botServices/channels | Nee | 
| botServices/connections | Nee | 

## <a name="microsoftcache"></a>Microsoft.Cache
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Redis | Ja | 
| RedisConfigDefinition | Nee | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| appliedReservations | Nee | 
| calculatePrice | Nee | 
| catalogi | Nee | 
| commercialReservationOrders | Nee | 
| reservationOrders | Nee | 
| reservationOrders/calculateRefund | Nee | 
| reservationOrders/merge | Nee | 
| reservationOrders/reserveringen | Nee | 
| reservationOrders/reserveringen/revisies | Nee | 
| reservationOrders of retourneren | Nee | 
| reservationOrders/splitsen | Nee | 
| reservationOrders/wisselen | Nee | 
| reserveringen | Nee | 
| bronnen | Nee | 
| validateReservationOrder | Nee | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| edgenodes | Nee | 
| Profielen | Ja | 
| profielen/eindpunten | Ja | 
| profiles/endpoints/customdomains | Nee | 
| profiles/endpoints/origins | Nee | 
| validateProbe | Nee | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| certificateOrders | Ja | 
| certificateOrders/certificaten | Nee | 
| validateCertificateRegistrationInformation | Nee | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Mogelijkheden | Nee | 
| domainNames | Nee | 
| domainNames/mogelijkheden | Nee | 
| domainNames/internalLoadBalancers | Nee | 
| domainNames/serviceCertificates | Nee | 
| domainNames/sleuven | Nee | 
| sleuven-domainNames-rollen | Nee | 
| moveSubscriptionResources | Nee | 
| operatingSystemFamilies | Nee | 
| operatingSystems | Nee | 
| quotas | Nee | 
| resourceTypes | Nee | 
| validateSubscriptionMoveAvailability | Nee | 
| virtualMachines | Nee | 
| virtualMachines/diagnosticSettings | Nee | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| classicInfrastructureResources | Nee | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Mogelijkheden | Nee | 
| expressRouteCrossConnections | Nee | 
| expressRouteCrossConnections/peerings | Nee | 
| gatewaySupportedDevices | Nee | 
| networkSecurityGroups | Nee | 
| quotas | Nee | 
| reservedIps | Nee | 
| virtualNetworks | Nee | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nee | 
| virtualNetworks/virtualNetworkPeerings | Nee | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Mogelijkheden | Nee | 
| Schijven | Nee | 
| images | Nee | 
| osImages | Nee | 
| osPlatformImages | Nee | 
| publicImages | Nee | 
| quotas | Nee | 
| storageAccounts | Nee | 
| storageAccounts/services | Nee | 
| storageAccounts/services/diagnosticSettings | Nee | 
| storageAccounts/vmImages | Nee | 
| vmImages | Nee | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| RateCard | Nee | 
| UsageAggregates | Nee | 

## <a name="microsoftcompute"></a>Microsoft.Compute
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
| virtualMachines | Ja | 
| virtualMachines/diagnosticSettings | Nee | 
| virtualMachines/extensions | Ja | 
| virtualMachineScaleSets | Ja | 
| virtualMachineScaleSets/extensions | Nee | 
| virtualMachineScaleSets/networkInterfaces | Nee | 
| virtualMachineScaleSets/publicIPAddresses | Nee | 
| virtualMachineScaleSets/virtualMachines | Nee | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Nee | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| AggregatedCost | Nee | 
| Tegoeden | Nee | 
| Budgetten | Nee | 
| Kosten | Nee | 
| CostTags | Nee | 
| credits | Nee | 
| events | Nee | 
| Prognoses | Nee | 
| veel | Nee | 
| Marktplaatsen | Nee | 
| Pricesheets | Nee | 
| producten | Nee | 
| ReservationDetails | Nee | 
| ReservationRecommendations | Nee | 
| ReservationSummaries | Nee | 
| ReservationTransactions | Nee | 
| Tags | Nee | 
| Voorwaarden | Nee | 
| UsageDetails | Nee | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| containerGroups | Ja | 
| serviceAssociationLinks | Nee | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| registers | Ja | 
| registers/builds | Nee | 
| registers/builds/annuleren | Nee | 
| registries/builds/getLogLink | Nee | 
| registers/buildTasks | Ja | 
| registers/buildTasks/stappen | Nee | 
| registries/eventGridFilters | Nee | 
| registries/getBuildSourceUploadUrl | Nee | 
| registries/GetCredentials | Nee | 
| registries/importImage | Nee | 
| registries/queueBuild | Nee | 
| registries/regenerateCredential | Nee | 
| registries/regenerateCredentials | Nee | 
| registers/replicaties | Ja | 
| registers/wordt uitgevoerd | Nee | 
| registers/uitvoeringen/annuleren | Nee | 
| registries/scheduleRun | Nee | 
| registers/taken | Ja | 
| registries/updatePolicies | Nee | 
| registers/webhooks | Ja | 
| registries/webhooks/getCallbackConfig | Nee | 
| webhooks-registers-ping | Nee | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| containerServices | Ja | 
| managedClusters | Ja | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| toepassingen | Ja | 
| updateCommunicationPreference | Nee | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Waarschuwingen | Nee | 
| BillingAccounts | Nee | 
| Connectors | Ja | 
| Afdelingen | Nee | 
| Dimensies | Nee | 
| EnrollmentAccounts | Nee | 
| Query’s uitvoeren | Nee | 
| Registreren | Nee | 
| Reportconfigs | Nee | 
| Rapporten | Nee | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| hubs | Ja | 
| hubs/authorizationPolicies | Nee | 
| hubs/connectors | Nee | 
| connectors-hubs-toewijzingen | Nee | 
| hubs/interacties | Nee | 
| hubs/kpi | Nee | 
| hubs/links | Nee | 
| hubs/profielen | Nee | 
| hubs/roleAssignments | Nee | 
| hubs/rollen | Nee | 
| hubs/suggestTypeSchema | Nee | 
| hubs/weergaven | Nee | 
| hubs/widgetTypes | Nee | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| taken | Ja | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| DataBoxEdgeDevices | Ja | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| werkruimten | Ja | 
| workspaces/virtualNetworkPeerings | Nee | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| catalogi | Ja | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| connectionManagers | Ja | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| dataFactories | Ja | 
| dataFactories/diagnosticSettings | Nee | 
| dataFactorySchema | Nee | 
| factory 's | Ja | 
| factories/integrationRuntimes | Nee | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/dataLakeStoreAccounts | Nee | 
| accounts/storageAccounts | Nee | 
| accounts/storageAccounts/containers | Nee | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/eventGridFilters | Nee | 
| accounts/firewallRules | Nee | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| services | Ja | 
| Services-projecten | Ja | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers/recoverableServers | Nee | 
| servers/virtualNetworkRules | Nee | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers/recoverableServers | Nee | 
| servers/virtualNetworkRules | Nee | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| servers | Ja | 
| servers/advisors | Nee | 
| servers/queryTexts | Nee | 
| servers/recoverableServers | Nee | 
| servers/topQueryStatistics | Nee | 
| servers/virtualNetworkRules | Nee | 
| servers/waitStatistics | Nee | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| IotHubs | Ja | 
| IotHubs/eventGridFilters | Nee | 
| ProvisioningServices | Ja | 
| Het gebruik van | Nee | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Domeincontrollers | Ja | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Labs | Ja | 
| Labs/serviceRunners | Ja | 
| labs/virtualMachines | Ja | 
| Schema 's | Ja | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| databaseAccountNames | Nee | 
| databaseAccounts | Ja | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| domeinen | Ja | 
| domains/domainOwnershipIdentifiers | Nee | 
| generateSsoRequest | Nee | 
| topLevelDomains | Nee | 
| validateDomainRegistrationInformation | Nee | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| lcsprojects | Nee | 
| lcsprojects/clouddeployments | Nee | 
| lcsprojects/connectors | Nee | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| domeinen | Ja | 
| domeinen/onderwerpen | Nee | 
| eventSubscriptions | Nee | 
| extensionTopics | Nee | 
| onderwerpen | Ja | 
| topicTypes | Nee | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| naamruimten | Ja | 
| naamruimten/authorizationrules | Nee | 
| naamruimten/disasterrecoveryconfigs | Nee | 
| namespaces/eventhubs | Nee | 
| Event hubs-naamruimten/authorizationrules | Nee | 
| Event hubs-naamruimten/consumergroups | Nee | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| database | Nee | 
| Providers | Nee | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| registreren | Nee | 
| galleryitems | Nee | 
| generateartifactaccessuri | Nee | 
| myareas | Nee | 
| myareas/areas | Nee | 
| myareas/areas/areas | Nee | 
| myareas/areas/areas/galleryitems | Nee | 
| myareas/areas/galleryitems | Nee | 
| myareas/galleryitems | Nee | 
| Registreren | Nee | 
| bronnen | Nee | 
| retrieveresourcesbyid | Nee | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| guestConfigurationAssignments | Nee | 
| software | Nee | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| hanaInstances | Ja | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/toepassingen | Nee | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| taken | Ja | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| labelGroups | Nee | 
| labelGroups/labels | Nee | 
| labelGroups/labels/conditions | Nee | 
| labelGroups/labels/subLabels | Nee | 
| labelGroups/labels/subLabels/conditions | Nee | 

## <a name="microsoftinsights"></a>microsoft.insights
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| actiongroups | Ja | 
| activityLogAlerts | Ja | 
| alertrules | Ja | 
| automatedExportSettings | Nee | 
| autoscalesettings | Ja | 
| basislijn | Nee | 
| calculatebaseline | Nee | 
| Onderdelen | Ja | 
| onderdelen/gebeurtenissen | Nee | 
| onderdelen/pricingPlans | Nee | 
| onderdelen/query | Nee | 
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 
| eventCategories | Nee | 
| eigenschap EventTypes | Nee | 
| extendedDiagnosticSettings | Nee | 
| logDefinitions | Nee | 
| logprofiles | Nee | 
| logboeken | Nee | 
| metricAlerts | Ja |
| migrateToNewPricingModel | Nee | 
| myWorkbooks | Nee | 
| query's | Nee | 
| rollbackToLegacyPricingModel | Nee | 
| scheduledqueryrules | Ja | 
| vmInsightsOnboardingStatuses | Nee | 
| webtests | Ja | 
| werkmappen | Ja | 

## <a name="microsoftintune"></a>Microsoft.Intune
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| IoTApps | Ja | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Graph | Ja | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| deletedVaults | Nee | 
| Kluizen | Ja | 
| vaults/accessPolicies | Nee | 
| Kluizen/geheimen | Nee | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/databases | Nee | 
| clusters/databases/met dataconnections | Nee | 
| clusters/databases/eventhubconnections | Nee | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| labaccounts | Ja | 
| gebruikers | Nee | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| logboeken | Nee | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| integrationAccounts | Ja | 
| Werkstromen | Ja | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| commitmentPlans | Ja | 
| webServices | Ja | 
| Workspaces | Ja | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/werkruimten | Ja | 
| accounts/workspaces/projects | Ja | 
| teamAccounts | Ja | 
| teamAccounts/workspaces | Ja | 
| teamAccounts/workspaces/projects | Ja | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| werkruimten | Ja | 
| workspaces/computes | Nee | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Identiteiten | Nee | 
| userAssignedIdentities | Ja | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| getEntities | Nee | 
| managementGroups | Nee | 
| bronnen | Nee | 
| startTenantBackfill | Nee | 
| tenantBackfillStatus | Nee | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 
| accounts/eventGridFilters | Nee | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Aanbiedingen | Nee | 
| offerTypes | Nee | 
| offerTypes/publishers | Nee | 
| offerTypes/publishers/offers | Nee | 
| offerTypes/publishers/offers/plans | Nee | 
| offerTypes/publishers/offers/plans/agreements | Nee | 
| offerTypes/publishers/offers/plans/configs | Nee | 
| offerTypes/publishers/offers/plans/configs/importImage | Nee | 
| privategalleryitems | Nee | 
| producten | Nee | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| classicDevServices | Ja | 
| updateCommunicationPreference | Nee | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Overeenkomsten | Nee | 
| offertypes | Nee | 

## <a name="microsoftmedia"></a>Microsoft.Media
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| mediaservices | Ja | 
| mediaservices/accountFilters | Nee | 
| mediaservices/activa | Nee | 
| mediaservices/assets/assetFilters | Nee | 
| mediaservices/contentKeyPolicies | Nee | 
| mediaservices/eventGridFilters | Nee | 
| mediaservices/liveEventOperations | Nee | 
| mediaservices/liveEvents | Ja | 
| mediaservices/liveEvents/liveOutputs | Nee | 
| mediaservices/liveOutputOperations | Nee | 
| mediaservices/streamingEndpointOperations | Nee | 
| mediaservices/streamingEndpoints | Ja | 
| mediaservices/streamingLocators | Nee | 
| mediaservices/streamingPolicies | Nee | 
| mediaservices/transformaties | Nee | 
| transformaties-mediaservices-taken | Nee | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| projecten | Ja | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| applicationGateways | Ja | 
| applicationSecurityGroups | Ja | 
| azureFirewallFqdnTags | Nee | 
| azureFirewalls | Ja | 
| bgpServiceCommunities | Nee | 
| verbindingen | Ja | 
| ddosCustomPolicies | Ja | 
| ddosProtectionPlans | Ja | 
| dnsOperationStatuses | Nee | 
| dnszones | Ja | 
| dnszones/A | Nee | 
| dnszones/AAAA | Nee | 
| dnszones/all | Nee | 
| dnszones/CAA | Nee | 
| dnszones/CNAME | Nee | 
| dnszones/MX | Nee | 
| dnszones/NS | Nee | 
| dnszones/PTR | Nee | 
| dnszones/recordsets | Nee | 
| dnszones/SOA | Nee | 
| dnszones/SRV | Nee | 
| dnszones/TXT | Nee | 
| expressRouteCircuits | Ja | 
| expressRouteServiceProviders | Nee | 
| frontdoors | Ja | 
| frontdoorWebApplicationFirewallPolicies | Ja | 
| getDnsResourceReference | Nee | 
| interfaceEndpoints | Ja | 
| internalNotify | Nee | 
| loadBalancers | Ja | 
| localNetworkGateways | Ja | 
| natGateways | Ja | 
| networkIntentPolicies | Ja | 
| networkInterfaces | Ja | 
| networkProfiles | Ja | 
| networkSecurityGroups | Ja | 
| networkWatchers | Ja | 
| networkWatchers/connectionMonitors | Ja | 
| networkWatchers/lenses | Ja | 
| networkWatchers/pingMeshes | Ja | 
| privateLinkServices | Ja | 
| publicIPAddresses | Ja | 
| publicIPPrefixes | Ja | 
| routeFilters | Ja | 
| routeTables | Ja | 
| serviceEndpointPolicies | Ja | 
| trafficManagerGeographicHierarchies | Nee | 
| trafficmanagerprofiles | Ja | 
| trafficmanagerprofiles/heatMaps | Nee | 
| virtualHubs | Ja | 
| virtualNetworkGateways | Ja | 
| virtualNetworks | Ja | 
| virtualNetworkTaps | Ja | 
| virtualWans | Ja | 
| vpnGateways | Ja | 
| vpnSites | Ja | 
| webApplicationFirewallPolicies | Ja | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| naamruimten | Ja | 
| namespaces/notificationHubs | Ja | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| apparaten | Nee | 
| linkTargets | Nee | 
| storageInsightConfigs | Nee | 
| werkruimten | Ja | 
| workspaces/dataSources | Nee | 
| workspaces/linkedServices | Nee | 
| workspaces/query | Nee | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| managementassociations | Nee | 
| managementconfigurations | Ja | 
| oplossingen | Ja | 
| weergaven | Ja | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| policyEvents | Nee | 
| policyStates | Nee | 
| policyTrackedResources | Nee | 
| herstelbewerkingen | Nee | 

## <a name="microsoftportal"></a>Microsoft.Portal
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| consoles | Nee | 
| dashboards | Ja | 
| userSettings | Nee | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| workspaceCollections | Ja | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Capaciteit | Ja | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| accounts | Ja | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| backupProtectedItems | Nee | 
| Kluizen | Ja | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| naamruimten | Ja | 
| naamruimten/authorizationrules | Nee | 
| namespaces/hybridconnections | Nee | 
| namespaces/hybridconnections/authorizationrules | Nee | 
| naamruimten/wcfrelays | Nee | 
| namespaces/wcfrelays/authorizationrules | Nee | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| bronnen | Nee | 
| subscriptionsStatus | Nee | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| availabilityStatuses | Nee | 
| childAvailabilityStatuses | Nee | 
| childResources | Nee | 
| events | Nee | 
| impactedResources | Nee | 
| meldingen | Nee | 

## <a name="microsoftresources"></a>Microsoft.Resources
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| implementaties | Nee | 
| Implementaties/operations | Nee | 
| Koppelingen | Nee | 
| notifyResourceJobs | Nee | 
| Providers | Nee | 
| resourceGroups | Nee | 
| bronnen | Nee | 
| abonnementen | Nee | 
| Abonnementen/providers | Nee | 
| abonnementen/resourcegroepen | Nee | 
| abonnementen/resourcegroups/resources | Nee | 
| Abonnementen/resources | Nee | 
| abonnementen/tagnames | Nee | 
| subscriptions/tagNames/tagValues | Nee | 
| Tenants | Nee | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| toepassingen | Ja | 
| saasresources | Nee | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| stromen | Ja | 
| taakverzamelingen | Ja | 

## <a name="microsoftsearch"></a>Microsoft.Search
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| resourceHealthMetadata | Nee | 
| searchServices | Ja | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| advancedThreatProtectionSettings | Nee | 
| waarschuwingen | Nee | 
| allowedConnections | Nee | 
| apparaten | Nee | 
| applicationWhitelistings | Nee | 
| AutoProvisioningSettings | Nee | 
| Conformiteit | Nee | 
| dataCollectionAgents | Nee | 
| discoveredSecuritySolutions | Nee | 
| externalSecuritySolutions | Nee | 
| InformationProtectionPolicies | Nee | 
| jitNetworkAccessPolicies | Nee | 
| controle | Nee | 
| monitoring/antimalware | Nee | 
| bewaking/basislijn | Nee | 
| bewaking/patch | Nee | 
| beleidsregels | Nee | 
| prijzen | Nee | 
| securityContacts | Nee | 
| securitySolutions | Nee | 
| securitySolutionsReferenceData | Nee | 
| securityStatus | Nee | 
| securityStatus/eindpunten | Nee | 
| securityStatus/subnetten | Nee | 
| securityStatus/virtuele machines | Nee | 
| securityStatuses | Nee | 
| securityStatusesSummaries | Nee | 
| instellingen | Nee | 
| taken | Nee | 
| Topologieën | Nee | 
| workspaceSettings | Nee | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| naamruimten | Ja | 
| naamruimten/authorizationrules | Nee | 
| naamruimten/disasterrecoveryconfigs | Nee | 
| naamruimten/eventgridfilters | Nee | 
| namespaces/queues | Nee | 
| namespaces/queues/authorizationrules | Nee | 
| naamruimten/onderwerpen | Nee | 
| naamruimten/onderwerpen/authorizationrules | Nee | 
| naamruimten/onderwerpen/abonnementen | Nee | 
| naamruimten/onderwerpen/abonnementen/regels | Nee | 
| premiumMessagingRegions | Nee | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Clusters | Ja | 
| clusters/toepassingen | Nee | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| toepassingen | Ja | 
| Gateways | Ja | 
| Netwerken | Ja | 
| geheimen | Ja | 
| volumes | Ja | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| SignalR | Ja | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| applianceDefinitions | Ja | 
| apparaten | Ja | 
| applicationDefinitions | Ja | 
| toepassingen | Ja | 
| jitRequests | Ja | 

## <a name="microsoftsql"></a>Microsoft.SQL
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| managedInstances | Ja |
| managedInstances/databases | Ja (Zie opmerking hieronder) |
| managedInstances/databases/backupShortTermRetentionPolicies | Nee |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nee |
| managedInstances/databases/vulnerabilityAssessments | Nee |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Nee |
| managedInstances/encryptionProtector | Nee |
| managedInstances/sleutels | Nee |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nee |
| managedInstances/vulnerabilityAssessments | Nee |
| servers | Ja | 
| servers/beheerders | Nee | 
| servers/communicationLinks | Nee | 
| servers/databases | Ja (Zie opmerking hieronder) | 
| servers/encryptionProtector | Nee | 
| servers/firewallRules | Nee | 
| servers/sleutels | Nee | 
| servers/restorableDroppedDatabases | Nee | 
| servers/serviceobjectives | Nee | 
| servers/tdeCertificates | Nee | 

> [!NOTE]
> De Master database biedt geen ondersteuning voor labels, maar andere databases, met inbegrip van Azure SQL Data Warehouse-databases, ondersteuning voor tags. Azure SQL Data Warehouse-databases moeten zich in actieve (niet onderbroken) staat.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| SqlVirtualMachineGroups | Ja | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Nee | 
| SqlVirtualMachines | Ja | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageAccounts | Ja | 
| storageAccounts/blobServices | Nee | 
| storageAccounts/fileServices | Nee | 
| storageAccounts/queueServices | Nee | 
| storageAccounts/services | Nee | 
| storageAccounts/tableServices | Nee | 
| Het gebruik van | Nee | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| storageSyncServices | Ja | 
| storageSyncServices/registeredServers | Nee | 
| storageSyncServices/syncGroups | Nee | 
| storageSyncServices/syncGroups/cloudEndpoints | Nee | 
| storageSyncServices/syncGroups/serverEndpoints | Nee | 
| storageSyncServices/workflows | Nee | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| managers | Ja | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| streamingjobs | Ja (Zie opmerking hieronder) | 
| streamingjobs/diagnosticSettings | Nee | 

> [!NOTE]
> U kunt een label niet toevoegen als streamingjobs wordt uitgevoerd. Stop de resource als een tag wilt toevoegen.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| CreateSubscription | Nee | 
| SubscriptionDefinitions | Nee | 
| SubscriptionOperations | Nee | 

## <a name="microsoftsupport"></a>microsoft.support
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| supporttickets | Nee | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| providerRegistrations | Ja | 
| bronnen | Ja | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Omgevingen | Ja | 
| environments/accessPolicies | Nee | 
| omgevingen/eventsources | Ja | 
| omgevingen/referenceDataSets | Ja | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| account | Ja | 
| -/ accountextensie | Ja | 
| account/project | Ja | 

## <a name="microsoftweb"></a>Microsoft.Web
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| apiManagementAccounts | Nee | 
| apiManagementAccounts/apiAcls | Nee | 
| apiManagementAccounts/apis | Nee | 
| apiManagementAccounts/apis/apiAcls | Nee | 
| apiManagementAccounts/apis/connectionAcls | Nee | 
| apiManagementAccounts/apis/connections | Nee | 
| apiManagementAccounts/apis/connections/connectionAcls | Nee | 
| apiManagementAccounts/apis/localizedDefinitions | Nee | 
| apiManagementAccounts/connectionAcls | Nee | 
| apiManagementAccounts/connections | Nee | 
| billingMeters | Nee | 
| certificaten | Ja | 
| connectionGateways | Ja | 
| verbindingen | Ja | 
| customApis | Ja | 
| deletedSites | Nee | 
| functions | Nee | 
| hostingEnvironments | Ja | 
| hostingEnvironments/multiRolePools | Nee | 
| hostingEnvironments/multiRolePools/instances | Nee | 
| hostingEnvironments/workerPools | Nee | 
| hostingEnvironments/workerPools/instances | Nee | 
| publishingUsers | Nee | 
| aanbevelingen | Nee | 
| resourceHealthMetadata | Nee | 
| runtimes | Nee | 
| serverFarms | Ja | 
| serverFarms/werkrollen | Nee | 
| sites | Ja | 
| sites/domainOwnershipIdentifiers | Nee | 
| sites/hostNameBindings | Nee | 
| sites/instanties | Nee | 
| exemplaren-websites-extensies | Nee | 
| sites/premieraddons | Ja | 
| sites/aanbevelingen | Nee | 
| sites/resourceHealthMetadata | Nee | 
| sites/slots | Ja | 
| sites/sleuven/hostNameBindings | Nee | 
| sleuven-sites-instanties | Nee | 
| sites/sleuven/exemplaren/extensies | Nee | 
| sourceControls | Nee | 
| Valideren | Nee | 
| verifyHostingEnvironmentVnet | Nee | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| diagnosticSettings | Nee | 
| diagnosticSettingsCategories | Nee | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| DeviceServices | Ja | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Resourcetype | Ondersteunt tags |
| ------------- | ----------- |
| Onderdelen | Nee | 
| componentsSummary | Nee | 
| monitorInstances | Nee | 
| monitorInstancesSummary | Nee | 
| Monitors | Nee | 
| notificationSettings | Nee | 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tags toepassen op resources, [tags gebruiken om uw Azure-resources te organiseren](resource-group-using-tags.md).
