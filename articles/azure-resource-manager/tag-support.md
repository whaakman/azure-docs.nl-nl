---
title: Ondersteuning van Azure Resource Manager-Tags voor bronnen
description: Hier wordt weer gegeven welke Azure-resource typen tags ondersteunen. Geeft Details voor alle Azure-Services.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 06/07/2019
ms.author: tomfitz
ms.openlocfilehash: 72bb11cd064c90c2bbe1e9e6452dcbf07fe37817
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304872"
---
# <a name="tag-support-for-azure-resources"></a>Ondersteuning voor labels voor Azure-resources
In dit artikel wordt beschreven of een resource type [labels](resource-group-using-tags.md)ondersteunt. De kolom met de naam **ondersteunt labels** geeft aan of het resource type een eigenschap voor de tag heeft. De kolom label **in het kosten rapport** geeft aan of dit resource type de tag doorgeeft aan het kosten rapport.

Down load [tag-support. CSV](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv)om dezelfde gegevens op te halen als een bestand met door komma's gescheiden waarden.

## <a name="microsoftaad"></a>Microsoft.AAD
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| DomainServices | Ja | Ja |
| DomainServices/oucontainer | Nee | Nee |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftaddons"></a>Microsoft.Addons
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| supportProviders | Nee |  Nee |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| aadsupportcases | Nee |  Nee |
| addsservices | Nee |  Nee |
| middelen | Nee |  Nee |
| anonymousapiusers | Nee |  Nee |
| configuratie | Nee |  Nee |
| logs | Nee |  Nee |
| rapporten | Nee |  Nee |
| services | Nee |  Nee |

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| configuraties | Nee |  Nee |
| generateRecommendations | Nee |  Nee |
| Vereisten | Nee |  Nee |
| onderdrukkingen | Nee |  Nee |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
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
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Server | Ja | Ja |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| reportFeedback | Nee |  Nee |
| service | Ja | Ja |
| validateServiceName | Nee |  Nee |

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| attestationProviders | Nee |  Nee |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| classicAdministrators | Nee |  Nee |
| denyAssignments | Nee |  Nee |
| elevateAccess | Nee |  Nee |
| vergren delingen | Nee |  Nee |
| machtigingen | Nee |  Nee |
| policyAssignments | Nee |  Nee |
| policyDefinitions | Nee |  Nee |
| policySetDefinitions | Nee |  Nee |
| providerOperations | Nee |  Nee |
| roleAssignments | Nee |  Nee |
| roleDefinitions | Nee |  Nee |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| automationAccounts | Ja | Ja |
| automationAccounts/configurations | Ja | Ja |
| automationAccounts/jobs | Nee |  Nee |
| automationAccounts/runbooks | Ja | Ja |
| automationAccounts/softwareUpdateConfigurations | Nee | Nee |
| automationAccounts/webhooks | Nee |  Nee |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| verschillend | Nee |  Nee |
| omgevingen/accounts | Nee |  Nee |
| omgevingen/accounts/naam ruimten | Nee |  Nee |
| omgevingen/accounts/naam ruimten/configuraties | Nee |  Nee |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| b2cDirectories | Ja | Nee |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| registraties | Ja | Ja |
| registrations/customerSubscriptions | Nee |  Nee |
| registraties/producten | Nee |  Nee |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| batchAccounts | Ja | Ja |

## <a name="microsoftbilling"></a>Microsoft.Billing
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
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
| billingAccounts/producten | Nee |  Nee |
| billingAccounts/projects | Nee |  Nee |
| billingAccounts/projects/billingSubscriptions | Nee |  Nee |
| billingAccounts/projects/importRequests | Nee |  Nee |
| billingAccounts/projects/initiateImportRequest | Nee |  Nee |
| billingAccounts/projects/operationStatus | Nee |  Nee |
| billingAccounts/projecten/producten | Nee |  Nee |
| billingAccounts/transactions | Nee |  Nee |
| billingPeriods | Nee |  Nee |
| BillingPermissions | Nee |  Nee |
| billingProperty | Nee |  Nee |
| BillingRoleAssignments | Nee |  Nee |
| BillingRoleDefinitions | Nee |  Nee |
| CreateBillingRoleAssignment | Nee |  Nee |
| afdeling | Nee |  Nee |
| enrollmentAccounts | Nee |  Nee |
| importRequests | Nee |  Nee |
| importRequests/acceptImportRequest | Nee |  Nee |
| importRequests/declineImportRequest | Nee |  Nee |
| factureer | Nee |  Nee |
| Making | Nee |  Nee |
| transfers/acceptTransfer | Nee |  Nee |
| overdrachten/declineTransfer | Nee |  Nee |
| overdrachten/operationStatus | Nee |  Nee |
| usagePlans | Nee |  Nee |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| mapApis | Ja | Ja |
| updateCommunicationPreference | Nee |  Nee |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| BizTalk | Ja | Ja |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| blueprintAssignments | Nee |  Nee |
| blueprintAssignments/assignmentOperations | Nee |  Nee |
| blueprintAssignments/operations | Nee |  Nee |
| blauw drukken | Nee |  Nee |
| blauw drukken/artefacten | Nee |  Nee |
| blauw drukken/versies | Nee |  Nee |
| blauw drukken/versies/artefacten | Nee |  Nee |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| botServices | Ja | Ja |
| botServices/kanalen | Nee |  Nee |
| botServices/verbindingen | Nee |  Nee |

## <a name="microsoftcache"></a>Microsoft.Cache
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Redis | Ja | Ja |
| RedisConfigDefinition | Nee |  Nee |

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| appliedReservations | Nee |  Nee |
| calculatePrice | Nee |  Nee |
| catalogi | Nee |  Nee |
| commercialReservationOrders | Nee |  Nee |
| reservationOrders | Nee |  Nee |
| reservationOrders/calculateRefund | Nee |  Nee |
| reservationOrders/samen voegen | Nee |  Nee |
| reservationOrders/reserve ringen | Nee |  Nee |
| reservationOrders/reserve ringen/revisies | Nee |  Nee |
| reservationOrders/retour neren | Nee |  Nee |
| reservationOrders/splitsen | Nee |  Nee |
| reservationOrders/swap | Nee |  Nee |
| ringen | Nee |  Nee |
| Resources | Nee |  Nee |
| validateReservationOrder | Nee |  Nee |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| edgenodes | Nee |  Nee |
| profiles | Ja | Ja |
| profielen/eind punten | Ja | Ja |
| profielen/eind punten/customdomains | Nee |  Nee |
| profielen/eind punten/oorsprong | Nee |  Nee |
| validateProbe | Nee |  Nee |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| certificateOrders | Ja | Ja |
| certificateOrders/certificaten | Nee |  Nee |
| validateCertificateRegistrationInformation | Nee |  Nee |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Bieden | Nee |  Nee |
| Domein naam | Nee |  Nee |
| Domein naam/mogelijkheden | Nee |  Nee |
| Domein naam/internalLoadBalancers | Nee |  Nee |
| Domein naam/serviceCertificates | Nee |  Nee |
| Domein naam/sleuven | Nee |  Nee |
| Domein naam/sleuven/rollen | Nee |  Nee |
| moveSubscriptionResources | Nee |  Nee |
| operatingSystemFamilies | Nee |  Nee |
| operatingSystems | Nee |  Nee |
| quotas | Nee |  Nee |
| resourceTypes | Nee |  Nee |
| validateSubscriptionMoveAvailability | Nee |  Nee |
| Informatie | Nee |  Nee |
| Informatie/diagnosticSettings | Nee |  Nee |

## <a name="microsoftclassicinfrastructuremigrate"></a>Micro soft. ClassicInfrastructureMigrate
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| classicInfrastructureResources | Nee |  Nee |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Bieden | Nee |  Nee |
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
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Bieden | Nee |  Nee |
| Cd's | Nee |  Nee |
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
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| RateCard | Nee |  Nee |
| UsageAggregates | Nee |  Nee |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Availability sets | Ja | Ja |
| Cd's | Ja | Ja |
| images | Ja | Ja |
| restorePointCollections | Ja | Ja |
| restorePointCollections/restorePoints | Nee |  Nee |
| sharedVMImages | Ja | Ja |
| sharedVMImages/versies | Ja | Ja |
| moment opnamen | Ja | Ja |
| Informatie | Ja | Ja |
| Informatie/diagnosticSettings | Nee |  Nee |
| Informatie/extensies | Ja | Ja |
| virtualMachineScaleSets | Ja | Ja |
| virtualMachineScaleSets/extensions | Nee |  Nee |
| virtualMachineScaleSets/networkInterfaces | Nee |  Nee |
| virtualMachineScaleSets/publicIPAddresses | Nee |  Nee |
| virtualMachineScaleSets/virtualMachines | Nee |  Nee |
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Nee |  Nee |

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| AggregatedCost | Nee |  Nee |
| Tegoeden | Nee |  Nee |
| Budgetten | Nee |  Nee |
| Belastingen | Nee |  Nee |
| CostTags | Nee |  Nee |
| aanvragen | Nee |  Nee |
| events | Nee |  Nee |
| Prognoses | Nee |  Nee |
| extra | Nee |  Nee |
| Markt plaatsen | Nee |  Nee |
| Pricesheets | Nee |  Nee |
| producten | Nee |  Nee |
| ReservationDetails | Nee |  Nee |
| ReservationRecommendations | Nee |  Nee |
| ReservationSummaries | Nee |  Nee |
| ReservationTransactions | Nee |  Nee |
| Labels | Nee |  Nee |
| Voorwaarden | Nee |  Nee |
| UsageDetails | Nee |  Nee |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| containerGroups | Ja | Ja |
| serviceAssociationLinks | Nee |  Nee |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| registers | Ja | Ja |
| registers/builds | Nee |  Nee |
| registers/builds/annuleren | Nee |  Nee |
| registries/builds/getLogLink | Nee |  Nee |
| registers/buildTasks | Ja | Ja |
| registers/buildTasks/stappen | Nee |  Nee |
| registries/eventGridFilters | Nee |  Nee |
| registers/getBuildSourceUploadUrl | Nee |  Nee |
| registers/GetCredentials | Nee |  Nee |
| registries/importImage | Nee |  Nee |
| registries/queueBuild | Nee |  Nee |
| registers/regenerateCredential | Nee |  Nee |
| registers/regenerateCredentials | Nee |  Nee |
| registers/replicaties | Ja | Ja |
| registers/uitvoeringen | Nee |  Nee |
| registers/uitvoeringen/annuleren | Nee |  Nee |
| registers/scheduleRun | Nee |  Nee |
| registers/taken | Ja | Ja |
| registries/updatePolicies | Nee |  Nee |
| registers/webhooks | Ja | Ja |
| registries/webhooks/getCallbackConfig | Nee |  Nee |
| registers/webhooks/ping | Nee |  Nee |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| containerServices | Ja | Ja |
| managedClusters | Ja | Ja |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| toepassingen | Ja | Ja |
| updateCommunicationPreference | Nee |  Nee |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Waarschuwingen | Nee |  Nee |
| BillingAccounts | Nee |  Nee |
| Connectors | Ja | Ja |
| afdeling | Nee |  Nee |
| Dimensies | Nee |  Nee |
| enrollmentAccounts | Nee |  Nee |
| Query’s uitvoeren | Nee |  Nee |
| inschrijving | Nee |  Nee |
| Reportconfigs | Nee |  Nee |
| Rapporten | Nee |  Nee |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| bedrijfs | Ja | Ja |
| hubs/authorizationPolicies | Nee |  Nee |
| hubs/connectors | Nee |  Nee |
| hubs/connectors/toewijzingen | Nee |  Nee |
| hubs/interacties | Nee |  Nee |
| hubs/kpi | Nee |  Nee |
| hubs/links | Nee |  Nee |
| hubs/profielen | Nee |  Nee |
| hubs/roleAssignments | Nee |  Nee |
| hubs/rollen | Nee |  Nee |
| hubs/suggestTypeSchema | Nee |  Nee |
| hubs/weer gaven | Nee |  Nee |
| hubs/widgetTypes | Nee |  Nee |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| functies | Ja | Ja |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| DataBoxEdgeDevices | Ja | Ja |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Werk ruimten | Ja | Nee |
| workspaces/virtualNetworkPeerings | Nee |  Nee |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| catalogi | Ja | Ja |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| connectionManagers | Ja | Ja |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| dataFactories | Ja | Nee |
| dataFactories/diagnosticSettings | Nee |  Nee |
| dataFactorySchema | Nee |  Nee |
| factory's | Ja | Nee |
| fabrieken/integrationRuntimes | Nee |  Nee |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/dataLakeStoreAccounts | Nee |  Nee |
| accounts/storageAccounts | Nee |  Nee |
| accounts/storageAccounts/containers | Nee |  Nee |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/eventGridFilters | Nee |  Nee |
| accounts/firewallRules | Nee |  Nee |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| services | Ja | Ja |
| Services/projecten | Ja | Ja |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Server | Ja | Ja |
| servers/recoverableServers | Nee |  Nee |
| servers/virtualNetworkRules | Nee |  Nee |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Server | Ja | Ja |
| servers/recoverableServers | Nee |  Nee |
| servers/virtualNetworkRules | Nee |  Nee |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Server | Ja | Ja |
| servers/adviseurs | Nee |  Nee |
| servers/queryTexts | Nee |  Nee |
| servers/recoverableServers | Nee |  Nee |
| servers/topQueryStatistics | Nee |  Nee |
| servers/virtualNetworkRules | Nee |  Nee |
| servers/waitStatistics | Nee |  Nee |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| IotHubs | Ja | Ja |
| IotHubs/eventGridFilters | Nee |  Nee |
| ProvisioningServices | Ja | Ja |
| gebruik | Nee |  Nee |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Fungeren | Ja | Ja |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Labs | Ja | Ja |
| Labs-serviceRunners | Ja | Ja |
| Labs-informatie | Ja | Ja |
| schema's | Ja | Ja |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| databaseAccountNames | Nee |  Nee |
| databaseAccounts | Ja | Ja |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| domeinen | Ja | Ja |
| domeinen/domainOwnershipIdentifiers | Nee |  Nee |
| generateSsoRequest | Nee |  Nee |
| topLevelDomains | Nee |  Nee |
| validateDomainRegistrationInformation | Nee |  Nee |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| lcsprojects | Nee |  Nee |
| lcsprojects/clouddeployments | Nee |  Nee |
| lcsprojects/connectors | Nee |  Nee |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| domeinen | Ja | Nee |
| domeinen/onderwerpen | Nee |  Nee |
| eventSubscriptions | Nee |  Nee |
| extensionTopics | Nee |  Nee |
| onderwerp | Ja | Nee |
| topicTypes | Nee |  Nee |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| clusters | Ja | Nee |
| naam ruimten | Ja | Nee |
| naam ruimten/authorizationrules | Nee |  Nee |
| naam ruimten/disasterrecoveryconfigs | Nee |  Nee |
| naam ruimten/Event hubs | Nee |  Nee |
| naam ruimten/Event hubs/authorizationrules | Nee |  Nee |
| naam ruimten/Event hubs/consumergroups | Nee |  Nee |

## <a name="microsoftfeatures"></a>Microsoft.Features
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| database | Nee |  Nee |
| Providers | Nee |  Nee |

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Inge | Nee |  Nee |
| galleryitems | Nee |  Nee |
| generateartifactaccessuri | Nee |  Nee |
| myareas | Nee |  Nee |
| myareas/areas | Nee |  Nee |
| myareas/areas/areas | Nee |  Nee |
| myareas/gebieden/gebieden/galleryitems | Nee |  Nee |
| myareas/areas/galleryitems | Nee |  Nee |
| myareas/galleryitems | Nee |  Nee |
| inschrijving | Nee |  Nee |
| Resources | Nee |  Nee |
| retrieveresourcesbyid | Nee |  Nee |

## <a name="microsoftguestconfiguration"></a>Micro soft. GuestConfiguration
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| guestConfigurationAssignments | Nee |  Nee |
| Software | Nee |  Nee |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| hanaInstances | Ja |  Ja |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| clusters | Ja | Ja |
| clusters/toepassingen | Nee |  Nee |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| functies | Ja | Ja |

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| labelGroups | Nee |  Nee |
| labelGroups/labels | Nee |  Nee |
| labelGroups/labels/voor waarden | Nee |  Nee |
| labelGroups/labels/sublabels | Nee |  Nee |
| labelGroups/labels/subLabels/conditions | Nee |  Nee |

## <a name="microsoftinsights"></a>microsoft.insights
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| actiongroups | Ja | Ja |
| activityLogAlerts | Ja | Ja |
| alertrules | Ja | Ja |
| automatedExportSettings | Nee |  Nee |
| autoscalesettings | Ja | Ja |
| gebonden | Nee |  Nee |
| calculatebaseline | Nee |  Nee |
| Materialen | Ja | Ja |
| onderdelen/gebeurtenissen | Nee |  Nee |
| onderdelen/pricingPlans | Nee |  Nee |
| onderdelen/query | Nee |  Nee |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |
| eventCategories | Nee |  Nee |
| eventtypes | Nee |  Nee |
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
| bladen | Ja | Ja |

## <a name="microsoftintune"></a>Microsoft.Intune
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| IoTApps | Ja | Ja |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Graph | Ja | Ja |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| deletedVaults | Nee |  Nee |
| kluizen | Ja | Ja |
| kluizen/accessPolicies | Nee |  Nee |
| kluizen/geheimen | Nee |  Nee |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| clusters | Ja | Ja |
| clusters/data bases | Nee |  Nee |
| clusters/data bases/dataConnections | Nee |  Nee |
| clusters/data bases/eventhubconnections | Nee |  Nee |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| labaccounts | Ja | Ja |
| gebruikers | Nee |  Nee |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| logs | Nee |  Nee |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| integrationAccounts | Ja | Ja |
| stroom | Ja | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| commitmentPlans | Ja | Ja |
| webServices | Ja | Ja |
| Workspaces | Ja | Ja |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/werk ruimten | Ja | Ja |
| accounts/werk ruimten/projecten | Ja | Ja |
| teamAccounts | Ja | Ja |
| teamAccounts/werk ruimten | Ja | Ja |
| teamAccounts/workspaces/projects | Ja | Ja |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Werk ruimten | Ja | Ja |
| workspaces/computes | Nee |  Nee |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Identiteit | Nee |  Nee |
| userAssignedIdentities | Ja | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| getEntities | Nee |  Nee |
| managementGroups | Nee |  Nee |
| Resources | Nee |  Nee |
| startTenantBackfill | Nee |  Nee |
| tenantBackfillStatus | Nee |  Nee |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |
| accounts/eventGridFilters | Nee |  Nee |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| over | Nee |  Nee |
| offerTypes | Nee |  Nee |
| offerTypes/publishers | Nee |  Nee |
| offerTypes/publishers/offers | Nee |  Nee |
| offerTypes/publishers/offers/plans | Nee |  Nee |
| offerTypes/publishers/offers/plans/agreements | Nee |  Nee |
| offerTypes/publishers/offers/plans/configs | Nee |  Nee |
| offerTypes/publishers/offers/plans/configs/importImage | Nee |  Nee |
| privategalleryitems | Nee |  Nee |
| producten | Nee |  Nee |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| classicDevServices | Ja | Ja |
| updateCommunicationPreference | Nee |  Nee |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| gesloten | Nee |  Nee |
| offertypes | Nee |  Nee |

## <a name="microsoftmedia"></a>Microsoft.Media
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Media Services | Ja | Ja |
| mediaservices/accountFilters | Nee |  Nee |
| Media Services/assets | Nee |  Nee |
| mediaservices/assets/assetFilters | Nee |  Nee |
| Media Services/contentKeyPolicies | Nee |  Nee |
| mediaservices/eventGridFilters | Nee |  Nee |
| mediaservices/liveEventOperations | Nee |  Nee |
| Media Services/liveEvents | Ja | Ja |
| Media Services/liveEvents/liveOutputs | Nee |  Nee |
| mediaservices/liveOutputOperations | Nee |  Nee |
| mediaservices/streamingEndpointOperations | Nee |  Nee |
| mediaservices/streamingEndpoints | Ja | Ja |
| mediaservices/streamingLocators | Nee |  Nee |
| mediaservices/streamingPolicies | Nee |  Nee |
| Media Services/trans formaties | Nee |  Nee |
| Media Services/trans formaties/taken | Nee |  Nee |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| projecten | Ja | Ja |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| applicationGateways | Ja | Nee |
| applicationSecurityGroups | Ja | Ja |
| azureFirewallFqdnTags | Nee |  Nee |
| azureFirewalls | Ja | Nee |
| bgpServiceCommunities | Nee |  Nee |
| inbel | Ja | Ja |
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
| frontdoors | Ja, maar beperkt (Zie [Opmerking hieronder](#frontdoor)) | Ja |
| frontdoorWebApplicationFirewallPolicies | Ja, maar beperkt (Zie [Opmerking hieronder](#frontdoor)) | Ja |
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
| virtualNetworkGateways | Ja | Ja |
| virtualNetworks | Ja | Ja |
| virtualNetworks/subnetten | Nee |  Nee |
| virtualNetworkTaps | Ja | Ja |
| virtualWans | Ja | Ja |
| vpnGateways | Ja | Nee |
| vpnSites | Ja | Ja |
| webApplicationFirewallPolicies | Ja | Ja |

<a id="frontdoor" />

Voor de Azure front-deur service kunt u Tags Toep assen bij het maken van de resource, maar het bijwerken of toevoegen van labels wordt momenteel niet ondersteund.

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| naam ruimten | Ja | Nee |
| namespaces/notificationHubs | Ja | Nee |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| apparaten | Nee |  Nee |
| linkTargets | Nee |  Nee |
| storageInsightConfigs | Nee |  Nee |
| Werk ruimten | Ja | Ja |
| werk ruimten/gegevens bronnen | Nee |  Nee |
| workspaces/linkedServices | Nee |  Nee |
| werk ruimten/query | Nee |  Nee |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| managementassociations | Nee |  Nee |
| managementconfigurations | Ja | Ja |
| oplossingen | Ja | Ja |
| Weergaven | Ja | Ja |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| policyEvents | Nee |  Nee |
| policyStates | Nee |  Nee |
| policyTrackedResources | Nee |  Nee |
| herstel | Nee |  Nee |

## <a name="microsoftportal"></a>Microsoft.Portal
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| consoles | Nee |  Nee |
| Dash boards | Ja | Ja |
| userSettings | Nee |  Nee |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| workspaceCollections | Ja | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| beschikt | Ja | Ja |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| accounts | Ja | Ja |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| backupProtectedItems | Nee |  Nee |
| kluizen | Ja | Ja |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| naam ruimten | Ja | Ja |
| naam ruimten/authorizationrules | Nee |  Nee |
| naam ruimten/hybridconnections | Nee |  Nee |
| naam ruimten/hybridconnections/authorizationrules | Nee |  Nee |
| naam ruimten/wcfrelays | Nee |  Nee |
| naam ruimten/wcfrelays/authorizationrules | Nee |  Nee |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Resources | Nee |  Nee |
| subscriptionsStatus | Nee |  Nee |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| availabilityStatuses | Nee |  Nee |
| childAvailabilityStatuses | Nee |  Nee |
| childResources | Nee |  Nee |
| events | Nee |  Nee |
| impactedResources | Nee |  Nee |
| Meldingen | Nee |  Nee |

## <a name="microsoftresources"></a>Microsoft.Resources
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| implementaties | Nee |  Nee |
| implementaties/bewerkingen | Nee |  Nee |
| koppelen | Nee |  Nee |
| notifyResourceJobs | Nee |  Nee |
| Providers | Nee |  Nee |
| resourceGroups | Nee |  Nee |
| Resources | Nee |  Nee |
| geabonneerd | Nee |  Nee |
| abonnementen/providers | Nee |  Nee |
| abonnementen/resourceGroups | Nee |  Nee |
| abonnementen/ResourceGroups/resources | Nee |  Nee |
| abonnementen/resources | Nee |  Nee |
| abonnementen/TagName | Nee |  Nee |
| subscriptions/tagNames/tagValues | Nee |  Nee |
| tenants | Nee |  Nee |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| toepassingen | Ja | Ja |
| saasresources | Nee |  Nee |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| terugloop | Ja | Ja |
| jobcollections | Ja | Ja |

## <a name="microsoftsearch"></a>Microsoft.Search
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| resourceHealthMetadata | Nee |  Nee |
| searchServices | Ja | Ja |

## <a name="microsoftsecurity"></a>Microsoft.Security
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| advancedThreatProtectionSettings | Nee |  Nee |
| waarschuwingen | Nee |  Nee |
| allowedConnections | Nee |  Nee |
| uitrusting | Nee |  Nee |
| applicationWhitelistings | Nee |  Nee |
| AutoProvisioningSettings | Nee |  Nee |
| Ingebouwde strengste | Nee |  Nee |
| dataCollectionAgents | Nee |  Nee |
| discoveredSecuritySolutions | Nee |  Nee |
| externalSecuritySolutions | Nee |  Nee |
| InformationProtectionPolicies | Nee |  Nee |
| jitNetworkAccessPolicies | Nee |  Nee |
| Bewaking | Nee |  Nee |
| monitoring/antimalware | Nee |  Nee |
| bewaking/basis lijn | Nee |  Nee |
| bewaking/patch | Nee |  Nee |
| Restrictie | Nee |  Nee |
| prijzen | Nee |  Nee |
| securityContacts | Nee |  Nee |
| securitySolutions | Nee |  Nee |
| securitySolutionsReferenceData | Nee |  Nee |
| securityStatus | Nee |  Nee |
| securityStatus/eind punten | Nee |  Nee |
| securityStatus/subnetten | Nee |  Nee |
| securityStatus/informatie | Nee |  Nee |
| securityStatuses | Nee |  Nee |
| securityStatusesSummaries | Nee |  Nee |
| instellingen | Nee |  Nee |
| taken | Nee |  Nee |
| topologieën | Nee |  Nee |
| workspaceSettings | Nee |  Nee |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| naam ruimten | Ja | Nee |
| naam ruimten/authorizationrules | Nee |  Nee |
| naam ruimten/disasterrecoveryconfigs | Nee |  Nee |
| naam ruimten/eventgridfilters | Nee |  Nee |
| naam ruimten/wacht rijen | Nee |  Nee |
| naam ruimten/wacht rijen/authorizationrules | Nee |  Nee |
| naam ruimten/onderwerpen | Nee |  Nee |
| naam ruimten/onderwerpen/authorizationrules | Nee |  Nee |
| naam ruimten/onderwerpen/abonnementen | Nee |  Nee |
| naam ruimten/onderwerpen/abonnementen/regels | Nee |  Nee |
| premiumMessagingRegions | Nee |  Nee |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| clusters | Ja | Ja |
| clusters/toepassingen | Nee |  Nee |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| toepassingen | Ja | Ja |
| gateways | Ja | Ja |
| netwerken | Ja | Ja |
| geheimen | Ja | Ja |
| volumes | Ja | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| SignalR | Ja | Ja |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| applianceDefinitions | Ja | Ja |
| uitrusting | Ja | Ja |
| applicationDefinitions | Ja | Ja |
| toepassingen | Ja | Ja |
| jitRequests | Ja | Ja |

## <a name="microsoftsql"></a>Microsoft.SQL
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| managedInstances | Ja | Ja |
| managedInstances/data bases | Ja (zie opmerking hieronder) | Ja |
| managedInstances/databases/backupShortTermRetentionPolicies | Nee | Nee |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Nee | Nee |
| managedInstances/data bases/vulnerabilityAssessments | Nee | Nee |
| managedInstances/data bases/vulnerabilityAssessments/Rules/basis lijnen | Nee | Nee |
| managedInstances/encryptionProtector | Nee | Nee |
| managedInstances/sleutels | Nee | Nee |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Nee | Nee |
| managedInstances/vulnerabilityAssessments | Nee | Nee |
| Server | Ja | Ja |
| servers/beheerders | Nee |  Nee |
| servers/communicationLinks | Nee |  Nee |
| servers/data bases | Ja (zie opmerking hieronder) | Ja |
| servers/encryptionProtector | Nee |  Nee |
| servers/firewallRules | Nee |  Nee |
| servers/sleutels | Nee |  Nee |
| servers/restorableDroppedDatabases | Nee |  Nee |
| servers/serviceobjectives | Nee |  Nee |
| servers/tdeCertificates | Nee |  Nee |

> [!NOTE]
> De hoofd database ondersteunt geen tags, maar andere data bases, inclusief Azure SQL Data Warehouse data bases, ondersteunings Tags. Azure SQL Data Warehouse-data bases moeten de status actief (niet onderbroken) hebben.


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| SqlVirtualMachineGroups | Ja | Ja |
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Nee |  Nee |
| SqlVirtualMachines | Ja | Ja |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| storageAccounts | Ja | Ja |
| storageAccounts/blobServices | Nee |  Nee |
| storageAccounts/fileServices | Nee |  Nee |
| storageAccounts/queueServices | Nee |  Nee |
| storageAccounts/services | Nee |  Nee |
| storageAccounts/tableServices | Nee |  Nee |
| gebruik | Nee |  Nee |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| storageSyncServices | Ja | Ja |
| storageSyncServices/registeredServers | Nee |  Nee |
| storageSyncServices/syncGroups | Nee |  Nee |
| storageSyncServices/syncGroups/cloudEndpoints | Nee |  Nee |
| storageSyncServices/syncGroups/serverEndpoints | Nee |  Nee |
| storageSyncServices/werk stromen | Nee |  Nee |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| leider | Ja | Ja |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| streamingjobs | Ja (zie opmerking hieronder) | Ja |
| streamingjobs/diagnosticSettings | Nee |  Nee |

> [!NOTE]
> U kunt geen tag toevoegen wanneer streamingjobs wordt uitgevoerd. Stop de resource om een tag toe te voegen.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| CreateSubscription | Nee |  Nee |
| SubscriptionDefinitions | Nee |  Nee |
| SubscriptionOperations | Nee |  Nee |

## <a name="microsoftsupport"></a>microsoft.support
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| supporttickets | Nee |  Nee |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| providerRegistrations | Ja | Ja |
| Resources | Ja | Ja |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| verschillend | Ja | Nee |
| omgevingen/accessPolicies | Nee |  Nee |
| omgevingen/eventsources | Ja | Nee |
| omgevingen/referenceDataSets | Ja | Nee |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| account | Ja | Ja |
| account/extensie | Ja | Ja |
| account/project | Ja | Ja |

## <a name="microsoftweb"></a>Microsoft.Web
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| apiManagementAccounts | Nee |  Nee |
| apiManagementAccounts/apiAcls | Nee |  Nee |
| apiManagementAccounts/apis | Nee |  Nee |
| apiManagementAccounts/apis/apiAcls | Nee |  Nee |
| apiManagementAccounts/apis/connectionAcls | Nee |  Nee |
| apiManagementAccounts/api's/Connections | Nee |  Nee |
| apiManagementAccounts/apis/connections/connectionAcls | Nee |  Nee |
| apiManagementAccounts/api's/localizedDefinitions | Nee |  Nee |
| apiManagementAccounts/connectionAcls | Nee |  Nee |
| apiManagementAccounts/verbindingen | Nee |  Nee |
| billingMeters | Nee |  Nee |
| Bewijzen | Ja | Ja |
| connectionGateways | Ja | Ja |
| inbel | Ja | Ja |
| customApis | Ja | Ja |
| deletedSites | Nee |  Nee |
| functions | Nee |  Nee |
| hostingEnvironments | Ja | Ja |
| hostingEnvironments/multiRolePools | Nee |  Nee |
| hostingEnvironments/multiRolePools/instances | Nee |  Nee |
| hostingEnvironments/workerPools | Nee |  Nee |
| hostingEnvironments/workerPools/instances | Nee |  Nee |
| publishingUsers | Nee |  Nee |
| Vereisten | Nee |  Nee |
| resourceHealthMetadata | Nee |  Nee |
| Runtimes | Nee |  Nee |
| Server farms | Ja | Ja |
| Server farms/werk nemers | Nee |  Nee |
| sites | Ja | Ja |
| sites/domainOwnershipIdentifiers | Nee |  Nee |
| sites/hostNameBindings | Nee |  Nee |
| sites/instanties | Nee |  Nee |
| sites/exemplaren/uitbrei dingen | Nee |  Nee |
| sites/premieraddons | Ja | Ja |
| sites/aanbevelingen | Nee |  Nee |
| sites/resourceHealthMetadata | Nee |  Nee |
| sites/sleuven | Ja | Ja |
| sites/sleuven/hostNameBindings | Nee |  Nee |
| sites/sleuven/instanties | Nee |  Nee |
| sites/sleuven/instanties/uitbrei dingen | Nee |  Nee |
| sourceControls | Nee |  Nee |
| subelementid | Nee |  Nee |
| verifyHostingEnvironmentVnet | Nee |  Nee |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Nee |  Nee |
| diagnosticSettingsCategories | Nee |  Nee |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| DeviceServices | Ja | Ja |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Resourcetype | Ondersteunt labels | Label in kosten rapport |
| ------------- | ----------- | ----------- |
| Materialen | Nee |  Nee |
| componentsSummary | Nee |  Nee |
| monitorInstances | Nee |  Nee |
| monitorInstancesSummary | Nee |  Nee |
| monitors | Nee |  Nee |
| notificationSettings | Nee |  Nee |

## <a name="next-steps"></a>Volgende stappen
Zie [Tags gebruiken om uw Azure-resources te organiseren](resource-group-using-tags.md)voor meer informatie over het Toep assen van tags op resources.
