---
title: Azure Resource Manager-tag-ondersteuning voor bronnen
description: Laat zien welke typen Azure-resource bieden ondersteuning voor tags. Bevat details voor alle Azure-services.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 06/07/2019
ms.author: tomfitz
ms.openlocfilehash: 4f1bc1415fbb875120d7b64128cae69e1e3f442c
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67339848"
---
# <a name="tag-support-for-azure-resources"></a>Tag-ondersteuning voor Azure-resources
Dit artikel wordt beschreven of een resourcetype ondersteunt [tags](resource-group-using-tags.md). De kolom met het label **ondersteunt tags** geeft aan of het resourcetype een eigenschap voor het label heeft. De kolom met het label **Tag op in het rapport** geeft aan of dat resourcetype wordt de tag aan het rapport cost doorgegeven.

Als u dezelfde gegevens als een bestand met door komma's gescheiden waarden, download [tag support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| DomainServices | Ja | Ja |
| DomainServices/oucontainer | Nee | Nee |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftaddons"></a>Microsoft.Addons
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| supportProviders | Nee |  Nee |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| aadsupportcases | Nee |  Nee |
| addsservices | Nee |  Nee |
| Agents | Nee |  Nee |
| anonymousapiusers | Nee |  Nee |
| configuratie | Nee |  Nee |
| logs | Nee |  Nee |
| rapporten | Nee |  Nee |
| services | Nee |  Nee |

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Configuraties | Nee |  Nee |
| generateRecommendations | Nee |  Nee |
| Aanbevelingen | Nee |  Nee |
| suppressions | Nee |  Nee |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| actionRules | Nee |  Nee |
| waarschuwingen | Nee |  Nee |
| alertsList | Nee |  Nee |
| alertsSummary | Nee |  Nee |
| alertsSummaryList | Nee |  Nee |
| smartDetectorAlertRules | Nee |  Nee |
| smartDetectorRuntimeEnvironments | Nee |  Nee |
| smartGroups | Nee |  Nee |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| servers | Ja | Ja |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| reportFeedback | Nee |  Nee |
| service | Ja | Ja |
| validateServiceName | Nee |  Nee |

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| attestationProviders | Nee |  Nee |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| classicAdministrators | Nee |  Nee |
| denyAssignments | Nee |  Nee |
| elevateAccess | Nee |  Nee |
| Hiermee vergrendelt u | Nee |  Nee |
| machtigingen | Nee |  Nee |
| policyAssignments | Nee |  Nee |
| policyDefinitions | Nee |  Nee |
| policySetDefinitions | Nee |  Nee |
| providerOperations | Nee |  Nee |
| roleAssignments | Nee |  Nee |
| roleDefinitions | Nee |  Nee |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| automationAccounts | Ja | Ja |
| automationAccounts/configurations | Ja | Ja |
| automationAccounts/jobs | Nee |  Nee |
| automationAccounts/runbooks | Ja | Ja |
| automationAccounts/softwareUpdateConfigurations | Nee | Nee |
| automationAccounts/webhooks | Nee |  Nee |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Omgevingen | Nee |  Nee |
| omgevingen/accounts | Nee |  Nee |
| accounts-omgevingen-naamruimten | Nee |  Nee |
| omgevingen/accounts/naamruimten/configuraties | Nee |  Nee |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| b2cDirectories | Ja | Nee |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| registraties | Ja | Ja |
| registrations/customerSubscriptions | Nee |  Nee |
| registraties/producten | Nee |  Nee |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| batchAccounts | Ja | Ja |

## <a name="microsoftbilling"></a>Microsoft.Billing
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| billingAccounts | Nee |  Nee |
| billingAccounts/billingProfiles | Nee |  Nee |
| billingAccounts/billingProfiles/billingSubscriptions | Nee |  Nee |
| billingAccounts/billingProfiles/invoices | Nee |  Nee |
| billingAccounts/billingProfiles/invoices/pricesheet | Nee |  Nee |
| billingAccounts/billingProfiles/operationStatus | Nee |  Nee |
| billingAccounts/billingProfiles/paymentMethods | Nee |  Nee |
| billingAccounts/billingProfiles/policies | Nee |  Nee |
| billingAccounts/billingProfiles/pricesheet | Nee |  Nee |
| billingAccounts/billingProfiles/products | Nee |  Nee |
| billingAccounts/billingProfiles/transactions | Nee |  Nee |
| billingAccounts/billingSubscriptions | Nee |  Nee |
| billingAccounts/departments | Nee |  Nee |
| billingAccounts/eligibleOffers | Nee |  Nee |
| billingAccounts/enrollmentAccounts | Nee |  Nee |
| billingAccounts/invoices | Nee |  Nee |
| billingAccounts/invoiceSections | Nee |  Nee |
| billingAccounts/invoiceSections/billingSubscriptions | Nee |  Nee |
| billingAccounts/invoiceSections/billingSubscriptions/transfer | Nee |  Nee |
| billingAccounts/invoiceSections/importRequests | Nee |  Nee |
| billingAccounts/invoiceSections/initiateImportRequest | Nee |  Nee |
| billingAccounts/invoiceSections/initiateTransfer | Nee |  Nee |
| billingAccounts/invoiceSections/operationStatus | Nee |  Nee |
| billingAccounts/invoiceSections/products | Nee |  Nee |
| billingAccounts/invoiceSections/transfers | Nee |  Nee |
| billingAccounts/products | Nee |  Nee |
| billingAccounts/projects | Nee |  Nee |
| billingAccounts/projects/billingSubscriptions | Nee |  Nee |
| billingAccounts/projects/importRequests | Nee |  Nee |
| billingAccounts/projects/initiateImportRequest | Nee |  Nee |
| billingAccounts/projects/operationStatus | Nee |  Nee |
| billingAccounts/projects/products | Nee |  Nee |
| billingAccounts/transactions | Nee |  Nee |
| billingPeriods | Nee |  Nee |
| BillingPermissions | Nee |  Nee |
| billingProperty | Nee |  Nee |
| BillingRoleAssignments | Nee |  Nee |
| BillingRoleDefinitions | Nee |  Nee |
| CreateBillingRoleAssignment | Nee |  Nee |
| afdelingen | Nee |  Nee |
| enrollmentAccounts | Nee |  Nee |
| importRequests | Nee |  Nee |
| importRequests/acceptImportRequest | Nee |  Nee |
| importRequests/declineImportRequest | Nee |  Nee |
| Facturen | Nee |  Nee |
| Overdrachten | Nee |  Nee |
| transfers/acceptTransfer | Nee |  Nee |
| overdrachten/declineTransfer | Nee |  Nee |
| transfers/operationStatus | Nee |  Nee |
| usagePlans | Nee |  Nee |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| mapApis | Ja | Ja |
| updateCommunicationPreference | Nee |  Nee |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| BizTalk | Ja | Ja |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| blueprintAssignments | Nee |  Nee |
| blueprintAssignments/assignmentOperations | Nee |  Nee |
| blueprintAssignments/operations | Nee |  Nee |
| blauwdrukken | Nee |  Nee |
| blauwdrukken/artefacten | Nee |  Nee |
| blauwdrukken /-versies | Nee |  Nee |
| versies-blauwdrukken-artefacten | Nee |  Nee |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| botServices | Ja | Ja |
| botServices/channels | Nee |  Nee |
| botServices/connections | Nee |  Nee |

## <a name="microsoftcache"></a>Microsoft.Cache
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Redis | Ja | Ja |
| RedisConfigDefinition | Nee |  Nee |

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| appliedReservations | Nee |  Nee |
| calculatePrice | Nee |  Nee |
| catalogi | Nee |  Nee |
| commercialReservationOrders | Nee |  Nee |
| reservationOrders | Nee |  Nee |
| reservationOrders/calculateRefund | Nee |  Nee |
| reservationOrders/merge | Nee |  Nee |
| reservationOrders/reserveringen | Nee |  Nee |
| reservationOrders/reserveringen/revisies | Nee |  Nee |
| reservationOrders of retourneren | Nee |  Nee |
| reservationOrders/splitsen | Nee |  Nee |
| reservationOrders/wisselen | Nee |  Nee |
| reserveringen | Nee |  Nee |
| resources | Nee |  Nee |
| validateReservationOrder | Nee |  Nee |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| edgenodes | Nee |  Nee |
| Profielen | Ja | Ja |
| profielen/eindpunten | Ja | Ja |
| profiles/endpoints/customdomains | Nee |  Nee |
| profiles/endpoints/origins | Nee |  Nee |
| validateProbe | Nee |  Nee |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| certificateOrders | Ja | Ja |
| certificateOrders/certificaten | Nee |  Nee |
| validateCertificateRegistrationInformation | Nee |  Nee |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Mogelijkheden | Nee |  Nee |
| domainNames | Nee |  Nee |
| domainNames/mogelijkheden | Nee |  Nee |
| domainNames/internalLoadBalancers | Nee |  Nee |
| domainNames/serviceCertificates | Nee |  Nee |
| domainNames/sleuven | Nee |  Nee |
| sleuven-domainNames-rollen | Nee |  Nee |
| moveSubscriptionResources | Nee |  Nee |
| operatingSystemFamilies | Nee |  Nee |
| operatingSystems | Nee |  Nee |
| quotas | Nee |  Nee |
| resourceTypes | Nee |  Nee |
| validateSubscriptionMoveAvailability | Nee |  Nee |
| virtualMachines | Nee |  Nee |
| virtualMachines/diagnosticSettings | Nee |  Nee |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| classicInfrastructureResources | Nee |  Nee |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Mogelijkheden | Nee |  Nee |
| expressRouteCrossConnections | Nee |  Nee |
| expressRouteCrossConnections/peerings | Nee |  Nee |
| gatewaySupportedDevices | Nee |  Nee |
| networkSecurityGroups | Nee |  Nee |
| quotas | Nee |  Nee |
| reservedIps | Nee |  Nee |
| virtualNetworks | Nee |  Nee |
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Nee |  Nee |
| virtualNetworks/virtualNetworkPeerings | Nee |  Nee |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Mogelijkheden | Nee |  Nee |
| Schijven | Nee |  Nee |
| images | Nee |  Nee |
| osImages | Nee |  Nee |
| osPlatformImages | Nee |  Nee |
| publicImages | Nee |  Nee |
| quotas | Nee |  Nee |
| storageAccounts | Nee |  Nee |
| storageAccounts/services | Nee |  Nee |
| storageAccounts/services/diagnosticSettings | Nee |  Nee |
| storageAccounts/vmImages | Nee |  Nee |
| vmImages | Nee |  Nee |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| RateCard | Nee |  Nee |
| UsageAggregates | Nee |  Nee |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| availabilitySets | Ja | Ja |
| Schijven | Ja | Ja |
| images | Ja | Ja |
| restorePointCollections | Ja | Ja |
| restorePointCollections/restorePoints | Nee |  Nee |
| sharedVMImages | Ja | Ja |
| sharedVMImages /-versies | Ja | Ja |
| Momentopnamen | Ja | Ja |
| virtualMachines | Ja | Ja |
| virtualMachines/diagnosticSettings | Nee |  Nee |
| virtualMachines/extensions | Ja | Ja |
| virtualMachineScaleSets | Ja | Ja |
| virtualMachineScaleSets/extensions | Nee |  Nee |
| virtualMachineScaleSets/networkInterfaces | Nee |  Nee |
| virtualMachineScaleSets/publicIPAddresses | Nee |  Nee |
| virtualMachineScaleSets/virtualMachines | Nee |  Nee |
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Nee |  Nee |

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| AggregatedCost | Nee |  Nee |
| Tegoeden | Nee |  Nee |
| Budgetten | Nee |  Nee |
| Kosten in rekening gebracht | Nee |  Nee |
| CostTags | Nee |  Nee |
| credits | Nee |  Nee |
| events | Nee |  Nee |
| Prognoses | Nee |  Nee |
| veel | Nee |  Nee |
| Marktplaatsen | Nee |  Nee |
| Pricesheets | Nee |  Nee |
| Producten | Nee |  Nee |
| ReservationDetails | Nee |  Nee |
| ReservationRecommendations | Nee |  Nee |
| ReservationSummaries | Nee |  Nee |
| ReservationTransactions | Nee |  Nee |
| Tags | Nee |  Nee |
| Voorwaarden | Nee |  Nee |
| UsageDetails | Nee |  Nee |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| containerGroups | Ja | Ja |
| serviceAssociationLinks | Nee |  Nee |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| registers | Ja | Ja |
| registers/builds | Nee |  Nee |
| registers/builds/annuleren | Nee |  Nee |
| registries/builds/getLogLink | Nee |  Nee |
| registers/buildTasks | Ja | Ja |
| registers/buildTasks/stappen | Nee |  Nee |
| registries/eventGridFilters | Nee |  Nee |
| registries/getBuildSourceUploadUrl | Nee |  Nee |
| registries/GetCredentials | Nee |  Nee |
| registries/importImage | Nee |  Nee |
| registries/queueBuild | Nee |  Nee |
| registries/regenerateCredential | Nee |  Nee |
| registries/regenerateCredentials | Nee |  Nee |
| registers/replicaties | Ja | Ja |
| registers/wordt uitgevoerd | Nee |  Nee |
| registers/uitvoeringen/annuleren | Nee |  Nee |
| registries/scheduleRun | Nee |  Nee |
| registers/taken | Ja | Ja |
| registries/updatePolicies | Nee |  Nee |
| registers/webhooks | Ja | Ja |
| registries/webhooks/getCallbackConfig | Nee |  Nee |
| webhooks-registers-ping | Nee |  Nee |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| containerServices | Ja | Ja |
| managedClusters | Ja | Ja |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| toepassingen | Ja | Ja |
| updateCommunicationPreference | Nee |  Nee |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Waarschuwingen | Nee |  Nee |
| BillingAccounts | Nee |  Nee |
| Connectors | Ja | Ja |
| afdelingen | Nee |  Nee |
| Dimensies | Nee |  Nee |
| EnrollmentAccounts | Nee |  Nee |
| Query’s uitvoeren | Nee |  Nee |
| Registreren | Nee |  Nee |
| Reportconfigs | Nee |  Nee |
| Rapporten | Nee |  Nee |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| hubs | Ja | Ja |
| hubs/authorizationPolicies | Nee |  Nee |
| hubs/connectors | Nee |  Nee |
| connectors-hubs-toewijzingen | Nee |  Nee |
| hubs/interacties | Nee |  Nee |
| hubs/kpi | Nee |  Nee |
| hubs/links | Nee |  Nee |
| hubs/profielen | Nee |  Nee |
| hubs/roleAssignments | Nee |  Nee |
| hubs/rollen | Nee |  Nee |
| hubs/suggestTypeSchema | Nee |  Nee |
| hubs/weergaven | Nee |  Nee |
| hubs/widgetTypes | Nee |  Nee |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| jobs | Ja | Ja |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| DataBoxEdgeDevices | Ja | Ja |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| workspaces | Ja | Nee |
| workspaces/virtualNetworkPeerings | Nee |  Nee |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| catalogi | Ja | Ja |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| connectionManagers | Ja | Ja |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| dataFactories | Ja | Nee |
| dataFactories/diagnosticSettings | Nee |  Nee |
| dataFactorySchema | Nee |  Nee |
| factory 's | Ja | Nee |
| factories/integrationRuntimes | Nee |  Nee |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/dataLakeStoreAccounts | Nee |  Nee |
| accounts/storageAccounts | Nee |  Nee |
| accounts/storageAccounts/containers | Nee |  Nee |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/eventGridFilters | Nee |  Nee |
| accounts/firewallRules | Nee |  Nee |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| services | Ja | Ja |
| Services-projecten | Ja | Ja |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| servers | Ja | Ja |
| servers/recoverableServers | Nee |  Nee |
| servers/virtualNetworkRules | Nee |  Nee |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| servers | Ja | Ja |
| servers/recoverableServers | Nee |  Nee |
| servers/virtualNetworkRules | Nee |  Nee |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| servers | Ja | Ja |
| servers/advisors | Nee |  Nee |
| servers/queryTexts | Nee |  Nee |
| servers/recoverableServers | Nee |  Nee |
| servers/topQueryStatistics | Nee |  Nee |
| servers/virtualNetworkRules | Nee |  Nee |
| servers/waitStatistics | Nee |  Nee |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| IotHubs | Ja | Ja |
| IotHubs/eventGridFilters | Nee |  Nee |
| ProvisioningServices | Ja | Ja |
| Het gebruik van | Nee |  Nee |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Domeincontrollers | Ja | Ja |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Labs | Ja | Ja |
| Labs/serviceRunners | Ja | Ja |
| labs/virtualMachines | Ja | Ja |
| Schema 's | Ja | Ja |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| databaseAccountNames | Nee |  Nee |
| databaseAccounts | Ja | Ja |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| domeinen | Ja | Ja |
| domains/domainOwnershipIdentifiers | Nee |  Nee |
| generateSsoRequest | Nee |  Nee |
| topLevelDomains | Nee |  Nee |
| validateDomainRegistrationInformation | Nee |  Nee |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| lcsprojects | Nee |  Nee |
| lcsprojects/clouddeployments | Nee |  Nee |
| lcsprojects/connectors | Nee |  Nee |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| domeinen | Ja | Nee |
| domeinen/onderwerpen | Nee |  Nee |
| eventSubscriptions | Nee |  Nee |
| extensionTopics | Nee |  Nee |
| onderwerpen | Ja | Nee |
| topicTypes | Nee |  Nee |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Clusters | Ja | Nee |
| Naamruimten | Ja | Nee |
| naamruimten/authorizationrules | Nee |  Nee |
| naamruimten/disasterrecoveryconfigs | Nee |  Nee |
| namespaces/eventhubs | Nee |  Nee |
| Event hubs-naamruimten/authorizationrules | Nee |  Nee |
| Event hubs-naamruimten/consumergroups | Nee |  Nee |

## <a name="microsoftfeatures"></a>Microsoft.Features
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| database | Nee |  Nee |
| Providers | Nee |  Nee |

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| registreren | Nee |  Nee |
| galleryitems | Nee |  Nee |
| generateartifactaccessuri | Nee |  Nee |
| myareas | Nee |  Nee |
| myareas/areas | Nee |  Nee |
| myareas/areas/areas | Nee |  Nee |
| myareas/areas/areas/galleryitems | Nee |  Nee |
| myareas/areas/galleryitems | Nee |  Nee |
| myareas/galleryitems | Nee |  Nee |
| Registreren | Nee |  Nee |
| resources | Nee |  Nee |
| retrieveresourcesbyid | Nee |  Nee |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| guestConfigurationAssignments | Nee |  Nee |
| software | Nee |  Nee |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| hanaInstances | Ja |  Ja |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Clusters | Ja | Ja |
| clusters/toepassingen | Nee |  Nee |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| jobs | Ja | Ja |

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| labelGroups | Nee |  Nee |
| labelGroups/labels | Nee |  Nee |
| labelGroups/labels/conditions | Nee |  Nee |
| labelGroups/labels/subLabels | Nee |  Nee |
| labelGroups/labels/subLabels/conditions | Nee |  Nee |

## <a name="microsoftinsights"></a>microsoft.insights
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| actiongroups | Ja | Ja |
| activityLogAlerts | Ja | Ja |
| alertrules | Ja | Ja |
| automatedExportSettings | Nee |  Nee |
| autoscalesettings | Ja | Ja |
| Basislijn | Nee |  Nee |
| calculatebaseline | Nee |  Nee |
| Onderdelen | Ja | Ja |
| onderdelen/gebeurtenissen | Nee |  Nee |
| onderdelen/pricingPlans | Nee |  Nee |
| onderdelen/query | Nee |  Nee |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |
| eventCategories | Nee |  Nee |
| eigenschap EventTypes | Nee |  Nee |
| extendedDiagnosticSettings | Nee |  Nee |
| logDefinitions | Nee |  Nee |
| logprofiles | Nee |  Nee |
| logs | Nee |  Nee |
| metricAlerts | Ja | Ja |
| migrateToNewPricingModel | Nee |  Nee |
| myWorkbooks | Nee |  Nee |
| query's | Nee |  Nee |
| rollbackToLegacyPricingModel | Nee |  Nee |
| scheduledqueryrules | Ja | Ja |
| vmInsightsOnboardingStatuses | Nee |  Nee |
| webtests | Ja | Ja |
| werkmappen | Ja | Ja |

## <a name="microsoftintune"></a>Microsoft.Intune
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| IoTApps | Ja | Ja |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Graph | Ja | Ja |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| deletedVaults | Nee |  Nee |
| Kluizen | Ja | Ja |
| vaults/accessPolicies | Nee |  Nee |
| Kluizen/geheimen | Nee |  Nee |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Clusters | Ja | Ja |
| clusters/databases | Nee |  Nee |
| clusters/databases/met dataconnections | Nee |  Nee |
| clusters/databases/eventhubconnections | Nee |  Nee |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| labaccounts | Ja | Ja |
| gebruikers | Nee |  Nee |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| logs | Nee |  Nee |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| integrationAccounts | Ja | Ja |
| Werkstromen | Ja | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| commitmentPlans | Ja | Ja |
| webServices | Ja | Ja |
| Workspaces | Ja | Ja |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/werkruimten | Ja | Ja |
| accounts/workspaces/projects | Ja | Ja |
| teamAccounts | Ja | Ja |
| teamAccounts/workspaces | Ja | Ja |
| teamAccounts/workspaces/projects | Ja | Ja |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| workspaces | Ja | Ja |
| workspaces/computes | Nee |  Nee |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Identiteiten | Nee |  Nee |
| userAssignedIdentities | Ja | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| getEntities | Nee |  Nee |
| managementGroups | Nee |  Nee |
| resources | Nee |  Nee |
| startTenantBackfill | Nee |  Nee |
| tenantBackfillStatus | Nee |  Nee |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/eventGridFilters | Nee |  Nee |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Aanbiedingen | Nee |  Nee |
| offerTypes | Nee |  Nee |
| offerTypes/publishers | Nee |  Nee |
| offerTypes/publishers/offers | Nee |  Nee |
| offerTypes/publishers/offers/plans | Nee |  Nee |
| offerTypes/publishers/offers/plans/agreements | Nee |  Nee |
| offerTypes/publishers/offers/plans/configs | Nee |  Nee |
| offerTypes/publishers/offers/plans/configs/importImage | Nee |  Nee |
| privategalleryitems | Nee |  Nee |
| Producten | Nee |  Nee |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| classicDevServices | Ja | Ja |
| updateCommunicationPreference | Nee |  Nee |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Overeenkomsten | Nee |  Nee |
| offertypes | Nee |  Nee |

## <a name="microsoftmedia"></a>Microsoft.Media
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| mediaservices | Ja | Ja |
| mediaservices/accountFilters | Nee |  Nee |
| mediaservices/activa | Nee |  Nee |
| mediaservices/assets/assetFilters | Nee |  Nee |
| mediaservices/contentKeyPolicies | Nee |  Nee |
| mediaservices/eventGridFilters | Nee |  Nee |
| mediaservices/liveEventOperations | Nee |  Nee |
| mediaservices/liveEvents | Ja | Ja |
| mediaservices/liveEvents/liveOutputs | Nee |  Nee |
| mediaservices/liveOutputOperations | Nee |  Nee |
| mediaservices/streamingEndpointOperations | Nee |  Nee |
| mediaservices/streamingEndpoints | Ja | Ja |
| mediaservices/streamingLocators | Nee |  Nee |
| mediaservices/streamingPolicies | Nee |  Nee |
| mediaservices/transformaties | Nee |  Nee |
| transformaties-mediaservices-taken | Nee |  Nee |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| projecten | Ja | Ja |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| applicationGateways | Ja | Nee |
| applicationSecurityGroups | Ja | Ja |
| azureFirewallFqdnTags | Nee |  Nee |
| azureFirewalls | Ja | Nee |
| bgpServiceCommunities | Nee |  Nee |
| Verbindingen | Ja | Ja |
| ddosCustomPolicies | Ja | Ja |
| ddosProtectionPlans | Ja | Ja |
| dnsOperationStatuses | Nee |  Nee |
| dnszones | Ja | Ja |
| dnszones/A | Nee |  Nee |
| dnszones/AAAA | Nee |  Nee |
| dnszones/all | Nee |  Nee |
| dnszones/CAA | Nee |  Nee |
| dnszones/CNAME | Nee |  Nee |
| dnszones/MX | Nee |  Nee |
| dnszones/NS | Nee |  Nee |
| dnszones/PTR | Nee |  Nee |
| dnszones/recordsets | Nee |  Nee |
| dnszones/SOA | Nee |  Nee |
| dnszones/SRV | Nee |  Nee |
| dnszones/TXT | Nee |  Nee |
| expressRouteCircuits | Ja  | Nee |
| expressRouteServiceProviders | Nee |  Nee |
| frontdoors | Ja, maar beperkt (Zie [opmerking hieronder](#frontdoor)) | Ja |
| frontdoorWebApplicationFirewallPolicies | Ja, maar beperkt (Zie [opmerking hieronder](#frontdoor)) | Ja |
| getDnsResourceReference | Nee |  Nee |
| interfaceEndpoints | Ja | Ja |
| internalNotify | Nee |  Nee |
| loadBalancers | Ja | Nee |
| localNetworkGateways | Ja | Ja |
| natGateways | Ja | Ja |
| networkIntentPolicies | Ja | Ja |
| networkInterfaces | Ja | Ja |
| networkProfiles | Ja | Ja |
| networkSecurityGroups | Ja | Ja |
| networkWatchers | Ja | Nee |
| networkWatchers/connectionMonitors | Ja | Nee |
| networkWatchers/lenses | Ja | Nee |
| networkWatchers/pingMeshes | Ja | Nee |
| privateLinkServices | Ja | Ja |
| publicIPAddresses | Ja | Ja |
| publicIPPrefixes | Ja | Ja |
| routeFilters | Ja | Ja |
| routeTables | Ja | Ja |
| serviceEndpointPolicies | Ja | Ja |
| trafficManagerGeographicHierarchies | Nee |  Nee |
| trafficmanagerprofiles | Ja | Ja |
| trafficmanagerprofiles/heatMaps | Nee |  Nee |
| virtualHubs | Ja | Ja |
| virtualNetworkGateways | Ja | Nee |
| virtualNetworks | Ja | Ja |
| virtualNetworks/subnetten | Nee |  Nee |
| virtualNetworkTaps | Ja | Ja |
| virtualWans | Ja | Ja |
| vpnGateways | Ja | Nee |
| vpnSites | Ja | Ja |
| webApplicationFirewallPolicies | Ja | Ja |

<a id="frontdoor" />

U kunt tags toepassen bij het maken van de bron voor Azure voordeur Service, maar bij te werken of toevoegen van labels is momenteel niet ondersteund.

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Naamruimten | Ja | Nee |
| namespaces/notificationHubs | Ja | Nee |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| apparaten | Nee |  Nee |
| linkTargets | Nee |  Nee |
| storageInsightConfigs | Nee |  Nee |
| workspaces | Ja | Ja |
| workspaces/dataSources | Nee |  Nee |
| workspaces/linkedServices | Nee |  Nee |
| workspaces/query | Nee |  Nee |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| managementassociations | Nee |  Nee |
| managementconfigurations | Ja | Ja |
| oplossingen | Ja | Ja |
| Weergaven | Ja | Ja |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| policyEvents | Nee |  Nee |
| policyStates | Nee |  Nee |
| policyTrackedResources | Nee |  Nee |
| herstelbewerkingen | Nee |  Nee |

## <a name="microsoftportal"></a>Microsoft.Portal
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| consoles | Nee |  Nee |
| dashboards | Ja | Ja |
| userSettings | Nee |  Nee |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| workspaceCollections | Ja | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Capaciteit | Ja | Ja |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| backupProtectedItems | Nee |  Nee |
| Kluizen | Ja | Ja |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Naamruimten | Ja | Ja |
| naamruimten/authorizationrules | Nee |  Nee |
| namespaces/hybridconnections | Nee |  Nee |
| namespaces/hybridconnections/authorizationrules | Nee |  Nee |
| naamruimten/wcfrelays | Nee |  Nee |
| namespaces/wcfrelays/authorizationrules | Nee |  Nee |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| resources | Nee |  Nee |
| subscriptionsStatus | Nee |  Nee |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| availabilityStatuses | Nee |  Nee |
| childAvailabilityStatuses | Nee |  Nee |
| childResources | Nee |  Nee |
| events | Nee |  Nee |
| impactedResources | Nee |  Nee |
| Meldingen | Nee |  Nee |

## <a name="microsoftresources"></a>Microsoft.Resources
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Implementaties | Nee |  Nee |
| Implementaties/operations | Nee |  Nee |
| Koppelingen | Nee |  Nee |
| notifyResourceJobs | Nee |  Nee |
| Providers | Nee |  Nee |
| resourceGroups | Nee |  Nee |
| resources | Nee |  Nee |
| Abonnementen | Nee |  Nee |
| Abonnementen/providers | Nee |  Nee |
| abonnementen/resourcegroepen | Nee |  Nee |
| abonnementen/resourcegroups/resources | Nee |  Nee |
| Abonnementen/resources | Nee |  Nee |
| abonnementen/tagnames | Nee |  Nee |
| subscriptions/tagNames/tagValues | Nee |  Nee |
| Tenants | Nee |  Nee |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| toepassingen | Ja | Ja |
| saasresources | Nee |  Nee |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| stromen | Ja | Ja |
| taakverzamelingen | Ja | Ja |

## <a name="microsoftsearch"></a>Microsoft.Search
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| resourceHealthMetadata | Nee |  Nee |
| searchServices | Ja | Ja |

## <a name="microsoftsecurity"></a>Microsoft.Security
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| advancedThreatProtectionSettings | Nee |  Nee |
| waarschuwingen | Nee |  Nee |
| allowedConnections | Nee |  Nee |
| apparaten | Nee |  Nee |
| applicationWhitelistings | Nee |  Nee |
| AutoProvisioningSettings | Nee |  Nee |
| Conformiteit | Nee |  Nee |
| dataCollectionAgents | Nee |  Nee |
| discoveredSecuritySolutions | Nee |  Nee |
| externalSecuritySolutions | Nee |  Nee |
| InformationProtectionPolicies | Nee |  Nee |
| jitNetworkAccessPolicies | Nee |  Nee |
| Bewaking | Nee |  Nee |
| monitoring/antimalware | Nee |  Nee |
| bewaking/basislijn | Nee |  Nee |
| bewaking/patch | Nee |  Nee |
| Beleid | Nee |  Nee |
| prijzen | Nee |  Nee |
| securityContacts | Nee |  Nee |
| securitySolutions | Nee |  Nee |
| securitySolutionsReferenceData | Nee |  Nee |
| securityStatus | Nee |  Nee |
| securityStatus/eindpunten | Nee |  Nee |
| securityStatus/subnetten | Nee |  Nee |
| securityStatus/virtuele machines | Nee |  Nee |
| securityStatuses | Nee |  Nee |
| securityStatusesSummaries | Nee |  Nee |
| instellingen | Nee |  Nee |
| taken | Nee |  Nee |
| Topologieën | Nee |  Nee |
| workspaceSettings | Nee |  Nee |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Naamruimten | Ja | Nee |
| naamruimten/authorizationrules | Nee |  Nee |
| naamruimten/disasterrecoveryconfigs | Nee |  Nee |
| naamruimten/eventgridfilters | Nee |  Nee |
| namespaces/queues | Nee |  Nee |
| namespaces/queues/authorizationrules | Nee |  Nee |
| naamruimten/onderwerpen | Nee |  Nee |
| naamruimten/onderwerpen/authorizationrules | Nee |  Nee |
| naamruimten/onderwerpen/abonnementen | Nee |  Nee |
| naamruimten/onderwerpen/abonnementen/regels | Nee |  Nee |
| premiumMessagingRegions | Nee |  Nee |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Clusters | Ja | Ja |
| clusters/toepassingen | Nee |  Nee |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| toepassingen | Ja | Ja |
| Gateways | Ja | Ja |
| Netwerken | Ja | Ja |
| geheimen | Ja | Ja |
| volumes | Ja | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| SignalR | Ja | Ja |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| applianceDefinitions | Ja | Ja |
| apparaten | Ja | Ja |
| applicationDefinitions | Ja | Ja |
| toepassingen | Ja | Ja |
| jitRequests | Ja | Ja |

## <a name="microsoftsql"></a>Microsoft.SQL
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| managedInstances | Ja | Ja |
| managedInstances/databases | Ja (Zie opmerking hieronder) | Ja |
| managedInstances/databases/backupShortTermRetentionPolicies | Nee | Nee |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nee | Nee |
| managedInstances/databases/vulnerabilityAssessments | Nee | Nee |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Nee | Nee |
| managedInstances/encryptionProtector | Nee | Nee |
| managedInstances/sleutels | Nee | Nee |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nee | Nee |
| managedInstances/vulnerabilityAssessments | Nee | Nee |
| servers | Ja | Ja |
| servers/beheerders | Nee |  Nee |
| servers/communicationLinks | Nee |  Nee |
| servers/databases | Ja (Zie opmerking hieronder) | Ja |
| servers/encryptionProtector | Nee |  Nee |
| servers/firewallRules | Nee |  Nee |
| servers/sleutels | Nee |  Nee |
| servers/restorableDroppedDatabases | Nee |  Nee |
| servers/serviceobjectives | Nee |  Nee |
| servers/tdeCertificates | Nee |  Nee |

> [!NOTE]
> De Master database biedt geen ondersteuning voor labels, maar andere databases, met inbegrip van Azure SQL Data Warehouse-databases, ondersteuning voor tags. Azure SQL Data Warehouse-databases moeten zich in actieve (niet onderbroken) staat.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| SqlVirtualMachineGroups | Ja | Ja |
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Nee |  Nee |
| SqlVirtualMachines | Ja | Ja |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| storageAccounts | Ja | Ja |
| storageAccounts/blobServices | Nee |  Nee |
| storageAccounts/fileServices | Nee |  Nee |
| storageAccounts/queueServices | Nee |  Nee |
| storageAccounts/services | Nee |  Nee |
| storageAccounts/tableServices | Nee |  Nee |
| Het gebruik van | Nee |  Nee |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| storageSyncServices | Ja | Ja |
| storageSyncServices/registeredServers | Nee |  Nee |
| storageSyncServices/syncGroups | Nee |  Nee |
| storageSyncServices/syncGroups/cloudEndpoints | Nee |  Nee |
| storageSyncServices/syncGroups/serverEndpoints | Nee |  Nee |
| storageSyncServices/workflows | Nee |  Nee |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| managers | Ja | Ja |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| streamingjobs | Ja (Zie opmerking hieronder) | Ja |
| streamingjobs/diagnosticSettings | Nee |  Nee |

> [!NOTE]
> U kunt een label niet toevoegen als streamingjobs wordt uitgevoerd. Stop de resource als een tag wilt toevoegen.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| CreateSubscription | Nee |  Nee |
| SubscriptionDefinitions | Nee |  Nee |
| SubscriptionOperations | Nee |  Nee |

## <a name="microsoftsupport"></a>microsoft.support
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| supporttickets | Nee |  Nee |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| providerRegistrations | Ja | Ja |
| resources | Ja | Ja |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Omgevingen | Ja | Nee |
| environments/accessPolicies | Nee |  Nee |
| omgevingen/eventsources | Ja | Nee |
| omgevingen/referenceDataSets | Ja | Nee |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| account | Ja | Ja |
| -/ accountextensie | Ja | Ja |
| account/project | Ja | Ja |

## <a name="microsoftweb"></a>Microsoft.Web
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| apiManagementAccounts | Nee |  Nee |
| apiManagementAccounts/apiAcls | Nee |  Nee |
| apiManagementAccounts/apis | Nee |  Nee |
| apiManagementAccounts/apis/apiAcls | Nee |  Nee |
| apiManagementAccounts/apis/connectionAcls | Nee |  Nee |
| apiManagementAccounts/apis/connections | Nee |  Nee |
| apiManagementAccounts/apis/connections/connectionAcls | Nee |  Nee |
| apiManagementAccounts/apis/localizedDefinitions | Nee |  Nee |
| apiManagementAccounts/connectionAcls | Nee |  Nee |
| apiManagementAccounts/connections | Nee |  Nee |
| billingMeters | Nee |  Nee |
| Certificaten | Ja | Ja |
| connectionGateways | Ja | Ja |
| Verbindingen | Ja | Ja |
| customApis | Ja | Ja |
| deletedSites | Nee |  Nee |
| functions | Nee |  Nee |
| hostingEnvironments | Ja | Ja |
| hostingEnvironments/multiRolePools | Nee |  Nee |
| hostingEnvironments/multiRolePools/instances | Nee |  Nee |
| hostingEnvironments/workerPools | Nee |  Nee |
| hostingEnvironments/workerPools/instances | Nee |  Nee |
| publishingUsers | Nee |  Nee |
| Aanbevelingen | Nee |  Nee |
| resourceHealthMetadata | Nee |  Nee |
| runtimes | Nee |  Nee |
| serverFarms | Ja | Ja |
| serverFarms/werkrollen | Nee |  Nee |
| sites | Ja | Ja |
| sites/domainOwnershipIdentifiers | Nee |  Nee |
| sites/hostNameBindings | Nee |  Nee |
| sites/instanties | Nee |  Nee |
| exemplaren-websites-extensies | Nee |  Nee |
| sites/premieraddons | Ja | Ja |
| sites/aanbevelingen | Nee |  Nee |
| sites/resourceHealthMetadata | Nee |  Nee |
| sites/slots | Ja | Ja |
| sites/sleuven/hostNameBindings | Nee |  Nee |
| sleuven-sites-instanties | Nee |  Nee |
| sites/sleuven/exemplaren/extensies | Nee |  Nee |
| sourceControls | Nee |  Nee |
| Valideren | Nee |  Nee |
| verifyHostingEnvironmentVnet | Nee |  Nee |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| DeviceServices | Ja | Ja |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Resourcetype | Ondersteunt tags | Code in het rapport |
| ------------- | ----------- | ----------- |
| Onderdelen | Nee |  Nee |
| componentsSummary | Nee |  Nee |
| monitorInstances | Nee |  Nee |
| monitorInstancesSummary | Nee |  Nee |
| Monitors | Nee |  Nee |
| notificationSettings | Nee |  Nee |

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over tags toepassen op resources, [tags gebruiken om uw Azure-resources te organiseren](resource-group-using-tags.md).
