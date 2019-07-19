---
title: Verwijdering van de modus complete Azure Resource Manager per resource type
description: Toont hoe bron typen het verwijderen van de modus volt ooien in Azure Resource Manager sjablonen verwerken.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/24/2019
ms.author: tomfitz
ms.openlocfilehash: 5ac442c0ae1e397fd1e8b58fdbcf61eb8712046c
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302844"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Verwijdering van Azure-resources voor implementaties in de volledige modus
In dit artikel wordt beschreven hoe bron typen worden verwijderd wanneer dat niet het geval is in een sjabloon die in de modus voltooid wordt geïmplementeerd.

De resource typen die zijn `Yes` gemarkeerd met worden verwijderd wanneer het type niet in de sjabloon staat die is geïmplementeerd met de modus volt ooien. 

De resource typen die zijn `No` gemarkeerd met, worden niet automatisch verwijderd als ze niet in de sjabloon staan, maar ze worden verwijderd als de bovenliggende resource wordt verwijderd. Zie [Azure Resource Manager implementatie modi](deployment-modes.md)voor een volledige beschrijving van het gedrag.


## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | DomainServices | Ja | 
> | DomainServices/oucontainer | Nee | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | diagnosticSettings | Nee | 
> | diagnosticSettingsCategories | Nee | 

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | supportProviders | Nee | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | aadsupportcases | Nee | 
> | addsservices | Nee | 
> | middelen | Nee | 
> | anonymousapiusers | Nee | 
> | configuratie | Nee | 
> | logs | Nee | 
> | rapporten | Nee | 
> | services | Nee | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | configuraties | Nee | 
> | generateRecommendations | Nee | 
> | Vereisten | Nee | 
> | onderdrukkingen | Nee | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | actionRules | Nee | 
> | waarschuwingen | Nee | 
> | alertsList | Nee | 
> | alertsSummary | Nee | 
> | alertsSummaryList | Nee | 
> | smartDetectorAlertRules | Nee | 
> | smartDetectorRuntimeEnvironments | Nee | 
> | smartGroups | Nee | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Server | Ja | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | reportFeedback | Nee | 
> | service | Ja | 
> | validateServiceName | Nee | 

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | attestationProviders | Nee | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | classicAdministrators | Nee | 
> | denyAssignments | Nee | 
> | elevateAccess | Nee | 
> | vergren delingen | Nee | 
> | machtigingen | Nee | 
> | policyAssignments | Nee | 
> | policyDefinitions | Nee | 
> | policySetDefinitions | Nee | 
> | providerOperations | Nee | 
> | roleAssignments | Nee | 
> | roleDefinitions | Nee | 

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | automationAccounts | Ja | 
> | automationAccounts/configurations | Ja | 
> | automationAccounts/jobs | Nee | 
> | automationAccounts/runbooks | Ja | 
> | automationAccounts/softwareUpdateConfigurations | Nee | 
> | automationAccounts/webhooks | Nee | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | verschillend | Nee | 
> | omgevingen/accounts | Nee | 
> | omgevingen/accounts/naam ruimten | Nee | 
> | omgevingen/accounts/naam ruimten/configuraties | Nee | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | b2cDirectories | Ja | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | registraties | Ja | 
> | registrations/customerSubscriptions | Nee | 
> | registraties/producten | Nee | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | batchAccounts | Ja | 

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | billingAccounts | Nee | 
> | billingAccounts/billingProfiles | Nee | 
> | billingAccounts/billingProfiles/billingSubscriptions | Nee | 
> | billingAccounts/billingProfiles/invoices | Nee | 
> | billingAccounts/billingProfiles/invoices/pricesheet | Nee | 
> | billingAccounts/billingProfiles/operationStatus | Nee | 
> | billingAccounts/billingProfiles/paymentMethods | Nee | 
> | billingAccounts/billingProfiles/policies | Nee | 
> | billingAccounts/billingProfiles/pricesheet | Nee | 
> | billingAccounts/billingProfiles/products | Nee | 
> | billingAccounts/billingProfiles/transactions | Nee | 
> | billingAccounts/billingSubscriptions | Nee | 
> | billingAccounts/departments | Nee | 
> | billingAccounts/eligibleOffers | Nee | 
> | billingAccounts/enrollmentAccounts | Nee | 
> | billingAccounts/invoices | Nee | 
> | billingAccounts/invoiceSections | Nee | 
> | billingAccounts/invoiceSections/billingSubscriptions | Nee | 
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Nee | 
> | billingAccounts/invoiceSections/importRequests | Nee | 
> | billingAccounts/invoiceSections/initiateImportRequest | Nee | 
> | billingAccounts/invoiceSections/initiateTransfer | Nee | 
> | billingAccounts/invoiceSections/operationStatus | Nee | 
> | billingAccounts/invoiceSections/products | Nee | 
> | billingAccounts/invoiceSections/transfers | Nee | 
> | billingAccounts/producten | Nee | 
> | billingAccounts/projects | Nee | 
> | billingAccounts/projects/billingSubscriptions | Nee | 
> | billingAccounts/projects/importRequests | Nee | 
> | billingAccounts/projects/initiateImportRequest | Nee | 
> | billingAccounts/projects/operationStatus | Nee | 
> | billingAccounts/projecten/producten | Nee | 
> | billingAccounts/transactions | Nee | 
> | billingPeriods | Nee | 
> | BillingPermissions | Nee | 
> | billingProperty | Nee | 
> | BillingRoleAssignments | Nee | 
> | BillingRoleDefinitions | Nee | 
> | CreateBillingRoleAssignment | Nee | 
> | afdeling | Nee | 
> | enrollmentAccounts | Nee | 
> | importRequests | Nee | 
> | importRequests/acceptImportRequest | Nee | 
> | importRequests/declineImportRequest | Nee | 
> | factureer | Nee | 
> | Making | Nee | 
> | transfers/acceptTransfer | Nee | 
> | overdrachten/declineTransfer | Nee | 
> | overdrachten/operationStatus | Nee | 
> | usagePlans | Nee | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | mapApis | Ja | 
> | updateCommunicationPreference | Nee | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | BizTalk | Ja | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | blueprintAssignments | Nee | 
> | blueprintAssignments/assignmentOperations | Nee | 
> | blueprintAssignments/operations | Nee | 
> | blauw drukken | Nee | 
> | blauw drukken/artefacten | Nee | 
> | blauw drukken/versies | Nee | 
> | blauw drukken/versies/artefacten | Nee | 

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | botServices | Ja | 
> | botServices/kanalen | Nee | 
> | botServices/verbindingen | Nee | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Redis | Ja | 
> | RedisConfigDefinition | Nee | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | appliedReservations | Nee | 
> | calculatePrice | Nee | 
> | catalogi | Nee | 
> | commercialReservationOrders | Nee | 
> | reservationOrders | Nee | 
> | reservationOrders/calculateRefund | Nee | 
> | reservationOrders/samen voegen | Nee | 
> | reservationOrders/reserve ringen | Nee | 
> | reservationOrders/reserve ringen/revisies | Nee | 
> | reservationOrders/retour neren | Nee | 
> | reservationOrders/splitsen | Nee | 
> | reservationOrders/swap | Nee | 
> | ringen | Nee | 
> | Resources | Nee | 
> | validateReservationOrder | Nee | 

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | edgenodes | Nee | 
> | profiles | Ja | 
> | profielen/eind punten | Ja | 
> | profielen/eind punten/customdomains | Nee | 
> | profielen/eind punten/oorsprong | Nee | 
> | validateProbe | Nee | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | certificateOrders | Ja | 
> | certificateOrders/certificaten | Nee | 
> | validateCertificateRegistrationInformation | Nee | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Bieden | Nee | 
> | Domein naam | Nee | 
> | Domein naam/mogelijkheden | Nee | 
> | Domein naam/internalLoadBalancers | Nee | 
> | Domein naam/serviceCertificates | Nee | 
> | Domein naam/sleuven | Nee | 
> | Domein naam/sleuven/rollen | Nee | 
> | moveSubscriptionResources | Nee | 
> | operatingSystemFamilies | Nee | 
> | operatingSystems | Nee | 
> | quotas | Nee | 
> | resourceTypes | Nee | 
> | validateSubscriptionMoveAvailability | Nee | 
> | Informatie | Nee | 
> | Informatie/diagnosticSettings | Nee | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Micro soft. ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | classicInfrastructureResources | Nee | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Bieden | Nee | 
> | expressRouteCrossConnections | Nee | 
> | expressRouteCrossConnections/peerings | Nee | 
> | gatewaySupportedDevices | Nee | 
> | networkSecurityGroups | Nee | 
> | quotas | Nee | 
> | reservedIps | Nee | 
> | virtualNetworks | Nee | 
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Nee | 
> | virtualNetworks/virtualNetworkPeerings | Nee | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Bieden | Nee | 
> | Cd's | Nee | 
> | images | Nee | 
> | osImages | Nee | 
> | osPlatformImages | Nee | 
> | publicImages | Nee | 
> | quotas | Nee | 
> | storageAccounts | Nee | 
> | storageAccounts/services | Nee | 
> | storageAccounts/services/diagnosticSettings | Nee | 
> | storageAccounts/vmImages | Nee | 
> | vmImages | Nee | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | RateCard | Nee | 
> | UsageAggregates | Nee | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Availability sets | Ja | 
> | Cd's | Ja | 
> | images | Ja | 
> | restorePointCollections | Ja | 
> | restorePointCollections/restorePoints | Nee | 
> | sharedVMImages | Ja | 
> | sharedVMImages/versies | Ja | 
> | moment opnamen | Ja | 
> | Informatie | Ja | 
> | Informatie/diagnosticSettings | Nee | 
> | Informatie/extensies | Ja | 
> | virtualMachineScaleSets | Ja | 
> | virtualMachineScaleSets/extensions | Nee | 
> | virtualMachineScaleSets/networkInterfaces | Nee | 
> | virtualMachineScaleSets/publicIPAddresses | Nee | 
> | virtualMachineScaleSets/virtualMachines | Nee | 
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Nee | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | AggregatedCost | Nee | 
> | Tegoeden | Nee | 
> | Budgetten | Nee | 
> | Belastingen | Nee | 
> | CostTags | Nee | 
> | aanvragen | Nee | 
> | events | Nee | 
> | Prognoses | Nee | 
> | extra | Nee | 
> | Markt plaatsen | Nee | 
> | Pricesheets | Nee | 
> | producten | Nee | 
> | ReservationDetails | Nee | 
> | ReservationRecommendations | Nee | 
> | ReservationSummaries | Nee | 
> | ReservationTransactions | Nee | 
> | Labels | Nee | 
> | Voorwaarden | Nee | 
> | UsageDetails | Nee | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | containerGroups | Ja | 
> | serviceAssociationLinks | Nee | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | registers | Ja | 
> | registers/builds | Nee | 
> | registers/builds/annuleren | Nee | 
> | registries/builds/getLogLink | Nee | 
> | registers/buildTasks | Ja | 
> | registers/buildTasks/stappen | Nee | 
> | registries/eventGridFilters | Nee | 
> | registers/getBuildSourceUploadUrl | Nee | 
> | registers/GetCredentials | Nee | 
> | registries/importImage | Nee | 
> | registries/queueBuild | Nee | 
> | registers/regenerateCredential | Nee | 
> | registers/regenerateCredentials | Nee | 
> | registers/replicaties | Ja | 
> | registers/uitvoeringen | Nee | 
> | registers/uitvoeringen/annuleren | Nee | 
> | registers/scheduleRun | Nee | 
> | registers/taken | Ja | 
> | registries/updatePolicies | Nee | 
> | registers/webhooks | Ja | 
> | registries/webhooks/getCallbackConfig | Nee | 
> | registers/webhooks/ping | Nee | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | containerServices | Ja | 
> | managedClusters | Ja | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | toepassingen | Ja | 
> | updateCommunicationPreference | Nee | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Waarschuwingen | Nee | 
> | BillingAccounts | Nee | 
> | Connectors | Ja | 
> | afdeling | Nee | 
> | Dimensies | Nee | 
> | enrollmentAccounts | Nee | 
> | Query’s uitvoeren | Nee | 
> | inschrijving | Nee | 
> | Reportconfigs | Nee | 
> | Rapporten | Nee | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | bedrijfs | Ja | 
> | hubs/authorizationPolicies | Nee | 
> | hubs/connectors | Nee | 
> | hubs/connectors/toewijzingen | Nee | 
> | hubs/interacties | Nee | 
> | hubs/kpi | Nee | 
> | hubs/links | Nee | 
> | hubs/profielen | Nee | 
> | hubs/roleAssignments | Nee | 
> | hubs/rollen | Nee | 
> | hubs/suggestTypeSchema | Nee | 
> | hubs/weer gaven | Nee | 
> | hubs/widgetTypes | Nee | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | functies | Ja | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Ja | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Werk ruimten | Ja | 
> | workspaces/virtualNetworkPeerings | Nee | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | catalogi | Ja | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | connectionManagers | Ja | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | dataFactories | Ja | 
> | dataFactories/diagnosticSettings | Nee | 
> | dataFactorySchema | Nee | 
> | factory's | Ja | 
> | fabrieken/integrationRuntimes | Nee | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/dataLakeStoreAccounts | Nee | 
> | accounts/storageAccounts | Nee | 
> | accounts/storageAccounts/containers | Nee | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/eventGridFilters | Nee | 
> | accounts/firewallRules | Nee | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | services | Ja | 
> | Services/projecten | Ja | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Server | Ja | 
> | servers/recoverableServers | Nee | 
> | servers/virtualNetworkRules | Nee | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Server | Ja | 
> | servers/recoverableServers | Nee | 
> | servers/virtualNetworkRules | Nee | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Server | Ja | 
> | servers/adviseurs | Nee | 
> | servers/queryTexts | Nee | 
> | servers/recoverableServers | Nee | 
> | servers/topQueryStatistics | Nee | 
> | servers/virtualNetworkRules | Nee | 
> | servers/waitStatistics | Nee | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | IotHubs | Ja | 
> | IotHubs/eventGridFilters | Nee | 
> | ProvisioningServices | Ja | 
> | gebruik | Nee | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Fungeren | Ja | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Labs | Ja | 
> | Labs-serviceRunners | Ja | 
> | Labs-informatie | Ja | 
> | schema's | Ja | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | databaseAccountNames | Nee | 
> | databaseAccounts | Ja | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | domeinen | Ja | 
> | domeinen/domainOwnershipIdentifiers | Nee | 
> | generateSsoRequest | Nee | 
> | topLevelDomains | Nee | 
> | validateDomainRegistrationInformation | Nee | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | lcsprojects | Nee | 
> | lcsprojects/clouddeployments | Nee | 
> | lcsprojects/connectors | Nee | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | domeinen | Ja | 
> | domeinen/onderwerpen | Nee | 
> | eventSubscriptions | Nee | 
> | extensionTopics | Nee | 
> | onderwerp | Ja | 
> | topicTypes | Nee | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | clusters | Ja | 
> | naam ruimten | Ja | 
> | naam ruimten/authorizationrules | Nee | 
> | naam ruimten/disasterrecoveryconfigs | Nee | 
> | naam ruimten/Event hubs | Nee | 
> | naam ruimten/Event hubs/authorizationrules | Nee | 
> | naam ruimten/Event hubs/consumergroups | Nee | 

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | database | Nee | 
> | Providers | Nee | 

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Inge | Nee | 
> | galleryitems | Nee | 
> | generateartifactaccessuri | Nee | 
> | myareas | Nee | 
> | myareas/areas | Nee | 
> | myareas/areas/areas | Nee | 
> | myareas/gebieden/gebieden/galleryitems | Nee | 
> | myareas/areas/galleryitems | Nee | 
> | myareas/galleryitems | Nee | 
> | inschrijving | Nee | 
> | Resources | Nee | 
> | retrieveresourcesbyid | Nee | 

## <a name="microsoftguestconfiguration"></a>Micro soft. GuestConfiguration

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | guestConfigurationAssignments | Nee | 
> | Software | Nee | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | hanaInstances | Ja | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | clusters | Ja | 
> | clusters/toepassingen | Nee | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | functies | Ja | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | labelGroups | Nee | 
> | labelGroups/labels | Nee | 
> | labelGroups/labels/voor waarden | Nee | 
> | labelGroups/labels/sublabels | Nee | 
> | labelGroups/labels/subLabels/conditions | Nee | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | actiongroups | Ja | 
> | activityLogAlerts | Ja | 
> | alertrules | Ja | 
> | automatedExportSettings | Nee | 
> | autoscalesettings | Ja | 
> | gebonden | Nee | 
> | calculatebaseline | Nee | 
> | Materialen | Ja | 
> | onderdelen/gebeurtenissen | Nee | 
> | onderdelen/pricingPlans | Nee | 
> | onderdelen/query | Nee | 
> | diagnosticSettings | Nee | 
> | diagnosticSettingsCategories | Nee | 
> | eventCategories | Nee | 
> | eventtypes | Nee | 
> | extendedDiagnosticSettings | Nee | 
> | logDefinitions | Nee | 
> | logprofiles | Nee | 
> | logs | Nee | 
> | metricAlerts | Ja |
> | migrateToNewPricingModel | Nee | 
> | myWorkbooks | Nee | 
> | query's | Nee | 
> | rollbackToLegacyPricingModel | Nee | 
> | scheduledqueryrules | Ja | 
> | vmInsightsOnboardingStatuses | Nee | 
> | webtests | Ja | 
> | bladen | Ja | 

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | diagnosticSettings | Nee | 
> | diagnosticSettingsCategories | Nee | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | IoTApps | Ja | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Graph | Ja | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | deletedVaults | Nee | 
> | kluizen | Ja | 
> | kluizen/accessPolicies | Nee | 
> | kluizen/geheimen | Nee | 

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | clusters | Ja | 
> | clusters/data bases | Nee | 
> | clusters/data bases/dataConnections | Nee | 
> | clusters/data bases/eventhubconnections | Nee | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | labaccounts | Ja | 
> | gebruikers | Nee | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | logs | Nee | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | integrationAccounts | Ja | 
> | stroom | Ja | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | commitmentPlans | Ja | 
> | webServices | Ja | 
> | Workspaces | Ja | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/werk ruimten | Ja | 
> | accounts/werk ruimten/projecten | Ja | 
> | teamAccounts | Ja | 
> | teamAccounts/werk ruimten | Ja | 
> | teamAccounts/workspaces/projects | Ja | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Werk ruimten | Ja | 
> | workspaces/computes | Nee | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Identiteit | Nee | 
> | userAssignedIdentities | Ja | 

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | getEntities | Nee | 
> | managementGroups | Nee | 
> | Resources | Nee | 
> | startTenantBackfill | Nee | 
> | tenantBackfillStatus | Nee | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 
> | accounts/eventGridFilters | Nee | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | over | Nee | 
> | offerTypes | Nee | 
> | offerTypes/publishers | Nee | 
> | offerTypes/publishers/offers | Nee | 
> | offerTypes/publishers/offers/plans | Nee | 
> | offerTypes/publishers/offers/plans/agreements | Nee | 
> | offerTypes/publishers/offers/plans/configs | Nee | 
> | offerTypes/publishers/offers/plans/configs/importImage | Nee | 
> | privategalleryitems | Nee | 
> | producten | Nee | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | classicDevServices | Ja | 
> | updateCommunicationPreference | Nee | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | gesloten | Nee | 
> | offertypes | Nee | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Media Services | Ja | 
> | mediaservices/accountFilters | Nee | 
> | Media Services/assets | Nee | 
> | mediaservices/assets/assetFilters | Nee | 
> | Media Services/contentKeyPolicies | Nee | 
> | mediaservices/eventGridFilters | Nee | 
> | mediaservices/liveEventOperations | Nee | 
> | Media Services/liveEvents | Ja | 
> | Media Services/liveEvents/liveOutputs | Nee | 
> | mediaservices/liveOutputOperations | Nee | 
> | mediaservices/streamingEndpointOperations | Nee | 
> | mediaservices/streamingEndpoints | Ja | 
> | mediaservices/streamingLocators | Nee | 
> | mediaservices/streamingPolicies | Nee | 
> | Media Services/trans formaties | Nee | 
> | Media Services/trans formaties/taken | Nee | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | projecten | Ja | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | applicationGateways | Ja | 
> | applicationSecurityGroups | Ja | 
> | azureFirewallFqdnTags | Nee | 
> | azureFirewalls | Ja | 
> | bgpServiceCommunities | Nee | 
> | inbel | Ja | 
> | ddosCustomPolicies | Ja | 
> | ddosProtectionPlans | Ja | 
> | dnsOperationStatuses | Nee | 
> | dnszones | Ja | 
> | dnszones/A | Nee | 
> | dnszones/AAAA | Nee | 
> | dnszones/all | Nee | 
> | dnszones/CAA | Nee | 
> | dnszones/CNAME | Nee | 
> | dnszones/MX | Nee | 
> | dnszones/NS | Nee | 
> | dnszones/PTR | Nee | 
> | dnszones/recordsets | Nee | 
> | dnszones/SOA | Nee | 
> | dnszones/SRV | Nee | 
> | dnszones/TXT | Nee | 
> | expressRouteCircuits | Ja | 
> | expressRouteServiceProviders | Nee | 
> | frontdoors | Ja | 
> | frontdoorWebApplicationFirewallPolicies | Ja | 
> | getDnsResourceReference | Nee | 
> | interfaceEndpoints | Ja | 
> | internalNotify | Nee | 
> | loadBalancers | Ja | 
> | localNetworkGateways | Ja | 
> | natGateways | Ja | 
> | networkIntentPolicies | Ja | 
> | networkInterfaces | Ja | 
> | networkProfiles | Ja | 
> | networkSecurityGroups | Ja | 
> | networkWatchers | Ja | 
> | networkWatchers/connectionMonitors | Ja | 
> | networkWatchers/lenses | Ja | 
> | networkWatchers/pingMeshes | Ja | 
> | privateLinkServices | Ja | 
> | publicIPAddresses | Ja | 
> | publicIPPrefixes | Ja | 
> | routeFilters | Ja | 
> | routeTables | Ja | 
> | serviceEndpointPolicies | Ja | 
> | trafficManagerGeographicHierarchies | Nee | 
> | trafficmanagerprofiles | Ja | 
> | trafficmanagerprofiles/heatMaps | Nee | 
> | virtualHubs | Ja | 
> | virtualNetworkGateways | Ja | 
> | virtualNetworks | Ja | 
> | virtualNetworkTaps | Ja | 
> | virtualWans | Ja | 
> | vpnGateways | Ja | 
> | vpnSites | Ja | 
> | webApplicationFirewallPolicies | Ja | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | naam ruimten | Ja | 
> | namespaces/notificationHubs | Ja | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | apparaten | Nee | 
> | linkTargets | Nee | 
> | storageInsightConfigs | Nee | 
> | Werk ruimten | Ja | 
> | werk ruimten/gegevens bronnen | Nee | 
> | workspaces/linkedServices | Nee | 
> | werk ruimten/query | Nee | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | managementassociations | Nee | 
> | managementconfigurations | Ja | 
> | oplossingen | Ja | 
> | Weergaven | Ja | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | policyEvents | Nee | 
> | policyStates | Nee | 
> | policyTrackedResources | Nee | 
> | herstel | Nee | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | consoles | Nee | 
> | Dash boards | Ja | 
> | userSettings | Nee | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | workspaceCollections | Ja | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | beschikt | Ja | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | accounts | Ja | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | backupProtectedItems | Nee | 
> | kluizen | Ja | 

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | naam ruimten | Ja | 
> | naam ruimten/authorizationrules | Nee | 
> | naam ruimten/hybridconnections | Nee | 
> | naam ruimten/hybridconnections/authorizationrules | Nee | 
> | naam ruimten/wcfrelays | Nee | 
> | naam ruimten/wcfrelays/authorizationrules | Nee | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Resources | Nee | 
> | subscriptionsStatus | Nee | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | availabilityStatuses | Nee | 
> | childAvailabilityStatuses | Nee | 
> | childResources | Nee | 
> | events | Nee | 
> | impactedResources | Nee | 
> | Meldingen | Nee | 

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | implementaties | Nee | 
> | implementaties/bewerkingen | Nee | 
> | koppelen | Nee | 
> | notifyResourceJobs | Nee | 
> | Providers | Nee | 
> | resourceGroups | Nee | 
> | Resources | Nee | 
> | geabonneerd | Nee | 
> | abonnementen/providers | Nee | 
> | abonnementen/resourceGroups | Nee | 
> | abonnementen/ResourceGroups/resources | Nee | 
> | abonnementen/resources | Nee | 
> | abonnementen/TagName | Nee | 
> | subscriptions/tagNames/tagValues | Nee | 
> | tenants | Nee | 

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | toepassingen | Ja | 
> | saasresources | Nee | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | terugloop | Ja | 
> | jobcollections | Ja | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | resourceHealthMetadata | Nee | 
> | searchServices | Ja | 

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | advancedThreatProtectionSettings | Nee | 
> | waarschuwingen | Nee | 
> | allowedConnections | Nee | 
> | uitrusting | Nee | 
> | applicationWhitelistings | Nee | 
> | AutoProvisioningSettings | Nee | 
> | Ingebouwde strengste | Nee | 
> | dataCollectionAgents | Nee | 
> | discoveredSecuritySolutions | Nee | 
> | externalSecuritySolutions | Nee | 
> | InformationProtectionPolicies | Nee | 
> | jitNetworkAccessPolicies | Nee | 
> | Bewaking | Nee | 
> | monitoring/antimalware | Nee | 
> | bewaking/basis lijn | Nee | 
> | bewaking/patch | Nee | 
> | Restrictie | Nee | 
> | prijzen | Nee | 
> | securityContacts | Nee | 
> | securitySolutions | Nee | 
> | securitySolutionsReferenceData | Nee | 
> | securityStatus | Nee | 
> | securityStatus/eind punten | Nee | 
> | securityStatus/subnetten | Nee | 
> | securityStatus/informatie | Nee | 
> | securityStatuses | Nee | 
> | securityStatusesSummaries | Nee | 
> | instellingen | Nee | 
> | taken | Nee | 
> | topologieën | Nee | 
> | workspaceSettings | Nee | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | diagnosticSettings | Nee | 
> | diagnosticSettingsCategories | Nee | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | naam ruimten | Ja | 
> | naam ruimten/authorizationrules | Nee | 
> | naam ruimten/disasterrecoveryconfigs | Nee | 
> | naam ruimten/eventgridfilters | Nee | 
> | naam ruimten/wacht rijen | Nee | 
> | naam ruimten/wacht rijen/authorizationrules | Nee | 
> | naam ruimten/onderwerpen | Nee | 
> | naam ruimten/onderwerpen/authorizationrules | Nee | 
> | naam ruimten/onderwerpen/abonnementen | Nee | 
> | naam ruimten/onderwerpen/abonnementen/regels | Nee | 
> | premiumMessagingRegions | Nee | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | clusters | Ja | 
> | clusters/toepassingen | Nee | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | toepassingen | Ja | 
> | gateways | Ja | 
> | netwerken | Ja | 
> | geheimen | Ja | 
> | volumes | Ja | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | SignalR | Ja | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | applianceDefinitions | Ja | 
> | uitrusting | Ja | 
> | applicationDefinitions | Ja | 
> | toepassingen | Ja | 
> | jitRequests | Ja | 

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | managedInstances | Ja |
> | managedInstances/data bases | Ja |
> | managedInstances/databases/backupShortTermRetentionPolicies | Nee |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nee |
> | managedInstances/data bases/vulnerabilityAssessments | Nee |
> | managedInstances/data bases/vulnerabilityAssessments/Rules/basis lijnen | Nee |
> | managedInstances/encryptionProtector | Nee |
> | managedInstances/sleutels | Nee |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nee |
> | managedInstances/vulnerabilityAssessments | Nee |
> | Server | Ja | 
> | servers/beheerders | Nee | 
> | servers/communicationLinks | Nee | 
> | servers/data bases | Ja | 
> | servers/encryptionProtector | Nee | 
> | servers/firewallRules | Nee | 
> | servers/sleutels | Nee | 
> | servers/restorableDroppedDatabases | Nee | 
> | servers/serviceobjectives | Nee | 
> | servers/tdeCertificates | Nee | 


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Ja | 
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Nee | 
> | SqlVirtualMachines | Ja | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | storageAccounts | Ja | 
> | storageAccounts/blobServices | Nee | 
> | storageAccounts/fileServices | Nee | 
> | storageAccounts/queueServices | Nee | 
> | storageAccounts/services | Nee | 
> | storageAccounts/tableServices | Nee | 
> | gebruik | Nee | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | storageSyncServices | Ja | 
> | storageSyncServices/registeredServers | Nee | 
> | storageSyncServices/syncGroups | Nee | 
> | storageSyncServices/syncGroups/cloudEndpoints | Nee | 
> | storageSyncServices/syncGroups/serverEndpoints | Nee | 
> | storageSyncServices/werk stromen | Nee | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | leider | Ja | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | streamingjobs | Ja | 
> | streamingjobs/diagnosticSettings | Nee | 

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | CreateSubscription | Nee | 
> | SubscriptionDefinitions | Nee | 
> | SubscriptionOperations | Nee | 

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | supporttickets | Nee | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | providerRegistrations | Ja | 
> | Resources | Ja | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | verschillend | Ja | 
> | omgevingen/accessPolicies | Nee | 
> | omgevingen/eventsources | Ja | 
> | omgevingen/referenceDataSets | Ja | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | account | Ja | 
> | account/extensie | Ja | 
> | account/project | Ja | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | apiManagementAccounts | Nee | 
> | apiManagementAccounts/apiAcls | Nee | 
> | apiManagementAccounts/apis | Nee | 
> | apiManagementAccounts/apis/apiAcls | Nee | 
> | apiManagementAccounts/apis/connectionAcls | Nee | 
> | apiManagementAccounts/api's/Connections | Nee | 
> | apiManagementAccounts/apis/connections/connectionAcls | Nee | 
> | apiManagementAccounts/api's/localizedDefinitions | Nee | 
> | apiManagementAccounts/connectionAcls | Nee | 
> | apiManagementAccounts/verbindingen | Nee | 
> | billingMeters | Nee | 
> | Bewijzen | Ja | 
> | connectionGateways | Ja | 
> | inbel | Ja | 
> | customApis | Ja | 
> | deletedSites | Nee | 
> | functions | Nee | 
> | hostingEnvironments | Ja | 
> | hostingEnvironments/multiRolePools | Nee | 
> | hostingEnvironments/multiRolePools/instances | Nee | 
> | hostingEnvironments/workerPools | Nee | 
> | hostingEnvironments/workerPools/instances | Nee | 
> | publishingUsers | Nee | 
> | Vereisten | Nee | 
> | resourceHealthMetadata | Nee | 
> | Runtimes | Nee | 
> | Server farms | Ja | 
> | Server farms/werk nemers | Nee | 
> | sites | Ja | 
> | sites/domainOwnershipIdentifiers | Nee | 
> | sites/hostNameBindings | Nee | 
> | sites/instanties | Nee | 
> | sites/exemplaren/uitbrei dingen | Nee | 
> | sites/premieraddons | Ja | 
> | sites/aanbevelingen | Nee | 
> | sites/resourceHealthMetadata | Nee | 
> | sites/sleuven | Ja | 
> | sites/sleuven/hostNameBindings | Nee | 
> | sites/sleuven/instanties | Nee | 
> | sites/sleuven/instanties/uitbrei dingen | Nee | 
> | sourceControls | Nee | 
> | subelementid | Nee | 
> | verifyHostingEnvironmentVnet | Nee | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | diagnosticSettings | Nee | 
> | diagnosticSettingsCategories | Nee | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | DeviceServices | Ja | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Resourcetype | Modus voor volledige verwijdering |
> | ------------- | ----------- |
> | Materialen | Nee | 
> | componentsSummary | Nee | 
> | monitorInstances | Nee | 
> | monitorInstancesSummary | Nee | 
> | monitors | Nee | 
> | notificationSettings | Nee | 

## <a name="next-steps"></a>Volgende stappen

Down load [complete-mode-deletion. CSV](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv)om dezelfde gegevens op te halen als een bestand met door komma's gescheiden waarden.