---
title: Ondersteuning voor verplaatsen door Azure-resourcetype
description: Geeft een lijst van de Azure-resource-typen die kunnen worden verplaatst naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 03/22/2019
ms.author: tomfitz
ms.openlocfilehash: d44b1bf778c7ec9551e2fd30f67083f8dded22d1
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438465"
---
# <a name="move-operation-support-for-resources"></a>Ondersteuning voor resources verplaatsen
In dit artikel wordt aangegeven of een Azure-resourcetype de verplaatsing ondersteunt. Hoewel een resourcetype de verplaatsing ondersteunt, kunnen er omstandigheden die verhinderen dat de resource die wordt verplaatst. Zie voor meer informatie over voorwaarden die invloed hebben op verplaatsingsbewerkingen [resources verplaatsen naar een nieuwe resourcegroep of abonnement](resource-group-move-resources.md).

Als u dezelfde gegevens als een bestand met door komma's gescheiden waarden, download [verplaatsen-ondersteuning-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| domainservices | Nee | Nee |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Tenants | Nee | Nee |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| servers | Ja | Ja |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| service | Ja | Ja |

## <a name="microsoftappservice"></a>Microsoft.AppService
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| apiapps | Nee | Nee |
| appidentities | Nee | Nee |
| Gateways | Nee | Nee |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| policyassignments | Nee | Nee |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| automationaccounts | Ja | Ja |
| automationaccounts/configurations | Ja | Ja |
| automationaccounts/runbooks | Ja | Ja |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| b2cdirectories | Ja | Ja |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| registraties | Ja | Ja |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| backupvault | Nee | Nee |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Batchaccounts | Ja | Ja |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Clusters | Nee | Nee |
| bestandsservers | Nee | Nee |
| taken | Nee | Nee |
| werkruimten | Nee | Nee |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| mapapis | Nee | Nee |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| biztalk | Ja | Ja |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| blockchainmembers | Nee | Nee |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| blueprintassignments | Nee | Nee |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| botservices | Ja | Ja |

## <a name="microsoftcache"></a>Microsoft.Cache
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| redis | Ja | Ja |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Profielen | Ja | Ja |
| profielen/eindpunten | Ja | Ja |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| certificateorders | Ja | Ja |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| domainnames | Ja | Nee |
| virtuele machines | Ja | Nee |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| networksecuritygroups | Nee | Nee |
| bereiken | Nee | Nee |
| virtualnetworks | Nee | Nee |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| storageaccounts | Ja | Nee |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| availabilitysets | Ja | Ja |
| schijven | Ja | Ja |
| galerieën | Nee | Nee |
| galleries/images | Nee | Nee |
| galerieën/afbeeldingen/versies | Nee | Nee |
| images | Ja | Ja |
| proximityplacementgroups | Nee | Nee |
| restorepointcollections | Nee | Nee |
| sharedvmimages | Nee | Nee |
| sharedvmimages /-versies | Nee | Nee |
| momentopnamen | Ja | Ja |
| virtuele machines | Ja | Ja |
| virtuele machines/extensies | Ja | Ja |
| virtualmachinescalesets | Ja | Ja |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| containergroups | Nee | Nee |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| containergroups | Nee | Nee |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| registers | Ja | Ja |
| registers/buildtasks | Ja | Ja |
| registers/replicaties | Nee | Nee |
| registers/taken | Ja | Ja |
| registers/webhooks | Ja | Ja |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| containerservices | Nee | Nee |
| managedclusters | Nee | Nee |
| openshiftmanagedclusters | Nee | Nee |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| toepassingen | Ja | Ja |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| connectoren | Ja | Ja |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| hubs | Ja | Ja |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| taken | Nee | Nee |

## <a name="microsoftdataboxedge"></a>Microsoft.DataboxEdge
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| databoxedgedevices | Nee | Nee |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| werkruimten | Nee | Nee |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| catalogi | Ja | Ja |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| connectionmanagers | Nee | Nee |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| pakketten | Nee | Nee |
| Plannen | Nee | Nee |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| datafactories | Ja | Ja |
| factory 's | Ja | Ja |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| datalakeaccounts | Nee | Nee |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| services | Nee | Nee |
| Services-projecten | Nee | Nee |
| sleuven | Nee | Nee |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| servers | Ja | Ja |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| servers | Ja | Ja |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| servergroups | Nee | Nee |
| servers | Ja | Ja |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| artifactsources | Nee | Nee |
| Implementaties | Nee | Nee |
| servicetopologies | Nee | Nee |
| servicetopologies/services | Nee | Nee |
| servicetopologies/services/serviceunits | Nee | Nee |
| stappen | Nee | Nee |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| elasticpools | Nee | Nee |
| elasticpools/iothubtenants | Nee | Nee |
| iothubs | Ja | Ja |
| provisioningservices | Ja | Ja |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Domeincontrollers | Nee | Nee |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| labcenters | Nee | Nee |
| Labs | Ja | Nee |
| Labs/servicerunners | Ja | Ja |
| Labs/virtuele machines | Ja | Nee |
| Schema 's | Nee | Nee |

## <a name="microsoftdns"></a>microsoft.dns
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| dnszones | Nee | Nee |
| dnszones/a | Nee | Nee |
| dnszones/aaaa | Nee | Nee |
| dnszones/cname | Nee | Nee |
| dnszones/mx | Nee | Nee |
| dnszones/ptr | Nee | Nee |
| dnszones/srv | Nee | Nee |
| dnszones/txt | Nee | Nee |
| trafficmanagerprofiles | Nee | Nee |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| databaseaccounts | Ja | Ja |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| domeinen | Ja | Ja |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| domeinen | Ja | Ja |
| onderwerpen | Ja | Ja |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Clusters | Ja | Ja |
| naamruimten | Ja | Ja |

## <a name="microsoftgenomics"></a>Microsoft.Genomics
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| hanainstances | Ja | Ja |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Clusters | Ja | Ja |

## <a name="microsofthybriddata"></a>Microsoft.HybridData
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| datamanagers | Ja | Ja |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| taken | Ja | Ja |

## <a name="microsoftinsights"></a>microsoft.insights
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |
| actiongroups | Ja | Ja |
| activitylogalerts | Nee | Nee |
| alertrules | Ja | Ja |
| autoscalesettings | Ja | Ja |
| Onderdelen | Ja | Ja |
| guestdiagnosticsettings | Nee | Nee |
| metricalerts | Nee | Nee |
| notificationgroups | Nee | Nee |
| notificationrules | Nee | Nee |
| scheduledqueryrules | Nee | Nee |
| webtests | Ja | Ja |
| werkmappen | Ja | Ja |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| iotapps | Ja | Ja |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| checknameavailability | Ja | Ja |
| grafiek | Ja | Ja |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| hsmpools | Nee | Nee |
| Kluizen | Ja | Ja |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Clusters | Ja | Ja |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| labaccounts | Nee | Nee |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| hostingenvironments | Nee | Nee |
| integrationaccounts | Ja | Ja |
| integrationserviceenvironments | Nee | Nee |
| isolatedenvironments | Nee | Nee |
| Werkstromen | Ja | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| commitmentplans | Ja | Ja |
| webservices | Ja | Nee |
| werkruimten | Ja | Ja |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| operationalizationclusters | Ja | Ja |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |
| accounts/werkruimten | Ja | Ja |
| accounts/workspaces/projects | Ja | Ja |
| teamaccounts | Ja | Ja |
| teamaccounts/werkruimten | Ja | Ja |
| teamaccounts/workspaces/projects | Ja | Ja |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| hostingaccounts | Nee | Nee |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| werkruimten | Nee | Nee |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| userassignedidentities | Ja | Ja |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| classicdevservices | Nee | Nee |

## <a name="microsoftmedia"></a>Microsoft.Media
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| mediaservices | Ja | Ja |
| mediaservices/liveevents | Ja | Ja |
| mediaservices/door | Ja | Ja |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| assessmentprojects | Nee | Nee |
| migrateprojects | Nee | Nee |
| projecten | Nee | Nee |

## <a name="microsoftnetapp"></a>Microsoft.NetApp
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| netappaccounts | Nee | Nee |
| netappaccounts/capacitypools | Nee | Nee |
| capacitypools-netappaccounts-volumes | Nee | Nee |
| netappaccounts/capacitypools/volumes/mounttargets | Nee | Nee |
| netappaccounts/capacitypools/volumes/momentopnamen | Nee | Nee |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| applicationgateways | Nee | Nee |
| applicationsecuritygroups | Ja | Ja |
| azurefirewalls | Ja | Ja |
| bastionhosts | Nee | Nee |
| verbindingen | Ja | Ja |
| ddoscustompolicies | Ja | Ja |
| ddosprotectionplans | Nee | Nee |
| dnszones | Ja | Ja |
| expressroutecircuits | Nee | Nee |
| expressroutecrossconnections | Nee | Nee |
| expressroutegateways | Nee | Nee |
| expressrouteports | Nee | Nee |
| ingangen | Ja | Ja |
| frontdoorwebapplicationfirewallpolicies | Ja | Ja |
| interfaceendpoints | Nee | Nee |
| loadbalancers | Ja | Ja |
| localnetworkgateways | Ja | Ja |
| natgateways | Ja | Ja |
| networkintentpolicies | Ja | Ja |
| netwerkinterfaces | Ja | Ja |
| networkprofiles | Nee | Nee |
| networksecuritygroups | Ja | Ja |
| networkwatchers | Ja | Ja |
| networkwatchers/connectionmonitors | Ja | Ja |
| networkwatchers/lenses | Ja | Ja |
| networkwatchers/pingmeshes | Ja | Ja |
| p2svpngateways | Nee | Nee |
| privatelinkservices | Nee | Nee |
| publicipaddresses | Ja | Ja |
| publicipprefixes | Ja | Ja |
| routefilters | Nee | Nee |
| routetables | Ja | Ja |
| securegateways | Nee | Nee |
| serviceendpointpolicies | Ja | Ja |
| trafficmanagerprofiles | Ja | Ja |
| virtualhubs | Nee | Nee |
| virtualnetworkgateways | Ja | Ja |
| virtualnetworks | Ja | Ja |
| virtualnetworktaps | Nee | Nee |
| virtualwans | Nee | Nee |
| vpngateways | Nee | Nee |
| vpnsites | Ja | Ja |
| webapplicationfirewallpolicies | Ja | Ja |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| naamruimten | Ja | Ja |
| namespaces/notificationhubs | Ja | Ja |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| werkruimten | Ja | Ja |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| managementconfigurations | Ja | Ja |
| oplossingen | Ja | Ja |
| weergaven | Ja | Ja |

## <a name="microsoftportal"></a>Microsoft.Portal
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| dashboards | Ja | Ja |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| rootresources | Nee | Nee |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| workspacecollections | Ja | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Capaciteit | Ja | Ja |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Kluizen | Ja | Ja |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| naamruimten | Ja | Ja |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| toepassingen | Ja | Nee |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| stromen | Ja | Ja |
| taakverzamelingen | Ja | Ja |

## <a name="microsoftsearch"></a>Microsoft.Search
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| searchservices | Ja | Ja |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Gateways | Nee | Nee |
| knooppunten | Nee | Nee |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| naamruimten | Ja | Ja |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| toepassingen | Nee | Nee |
| Clusters | Ja | Ja |
| containergroups | Nee | Nee |
| containergroupsets | Nee | Nee |
| edgeclusters | Nee | Nee |
| Netwerken | Nee | Nee |
| secretstores | Nee | Nee |
| volumes | Nee | Nee |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| toepassingen | Ja | Ja |
| containergroups | Nee | Nee |
| Gateways | Ja | Ja |
| Netwerken | Ja | Ja |
| geheimen | Ja | Ja |
| volumes | Ja | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| signalr | Ja | Ja |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| siterecoveryvault | Nee | Nee |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| appliancedefinitions | Nee | Nee |
| apparaten | Nee | Nee |
| applicationdefinitions | Nee | Nee |
| toepassingen | Nee | Nee |
| jitrequests | Nee | Nee |

## <a name="microsoftsql"></a>Microsoft.Sql
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| managedinstances | Ja | Ja |
| managedinstances/databases | Ja | Ja |
| servers | Ja | Ja |
| servers/databases | Ja | Ja |
| servers/elasticpools | Ja | Ja |
| servers/jobaccounts | Nee | Nee |
| servers/jobagents | Nee | Nee |
| virtualclusters | Ja | Ja |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| sqlvirtualmachinegroups | Ja | Ja |
| sqlvirtualmachines | Ja | Ja |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| dwvm | Nee | Nee |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| storageaccounts | Ja | Ja |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| storagesyncservices | Ja | Ja |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| storagesyncservices | Nee | Nee |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| storagesyncservices | Nee | Nee |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| managers | Nee | Nee |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| streamingjobs | Ja | Ja |

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Omgevingen | Nee | Nee |
| omgevingen/eventsources | Nee | Nee |
| instanties | Nee | Nee |
| exemplaren /-omgevingen | Nee | Nee |
| instances/environments/eventsources | Nee | Nee |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| providerregistrations | Nee | Nee |
| bronnen | Nee | Nee |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| Omgevingen | Ja | Ja |
| omgevingen/eventsources | Ja | Ja |
| omgevingen/referencedatasets | Ja | Ja |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| imagetemplates | Nee | Nee |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| account | Ja | Ja |
| -/ accountextensie | Ja | Ja |
| account/project | Ja | Ja |

## <a name="microsoftweb"></a>Microsoft.Web
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| certificaten | Nee | Ja |
| connectiongateways | Ja | Ja |
| verbindingen | Ja | Ja |
| customapis | Ja | Ja |
| hostingenvironments | Nee | Nee |
| serverfarms | Ja | Ja |
| sites | Ja | Ja |
| sites/premieraddons | Ja | Ja |
| sites/slots | Ja | Ja |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Resourcetype | Resourcegroep | Abonnement |
| ------------- | ----------- | ---------- |
| deviceservices | Ja | Ja |

## <a name="third-party-services"></a>Services van derden

Services van derden ondersteuning momenteel geen voor de verplaatsing.

## <a name="next-steps"></a>Volgende stappen
Zie voor de opdrachten om resources te verplaatsen, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](resource-group-move-resources.md).
