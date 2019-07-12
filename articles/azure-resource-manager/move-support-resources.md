---
title: Ondersteuning voor verplaatsen door Azure-resourcetype
description: Geeft een lijst van de Azure-resource-typen die kunnen worden verplaatst naar een nieuwe resourcegroep of abonnement.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 7/9/2019
ms.author: tomfitz
ms.openlocfilehash: 093c20407cb6210125106189f36566f539de0dcc
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67721110"
---
# <a name="move-operation-support-for-resources"></a>Ondersteuning voor resources verplaatsen
In dit artikel wordt aangegeven of een Azure-resourcetype de verplaatsing ondersteunt. Het bevat ook informatie over de speciale voorwaarden om te overwegen bij het verplaatsen van een resource.

Ga naar de naamruimte van de provider van een resource:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Backup](#microsoftbackup)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAI](#microsoftbatchai)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [microsoft.dns](#microsoftdns)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SiteRecovery](#microsoftsiterecovery)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft.AAD
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| domainservices | Nee | Nee |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Tenants | Nee | Nee |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| actionrules | Ja | Ja |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| servers | Ja | Ja |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| service | Ja | Ja |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| configurationstores | Ja | Ja |

## <a name="microsoftappservice"></a>Microsoft.AppService
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| apiapps | Nee | Nee |
| appidentities | Nee | Nee |
| Gateways | Nee | Nee |

> [!IMPORTANT]
> Zie [App Service verplaatsen richtlijnen](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| policyassignments | Nee | Nee |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| automationaccounts | Ja | Ja |
| automationaccounts/configurations | Ja | Ja |
| automationaccounts/runbooks | Ja | Ja |

> [!IMPORTANT]
> Runbooks moeten zich in dezelfde resourcegroep bevinden als het Automation-Account.

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| b2cdirectories | Ja | Ja |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| registraties | Ja | Ja |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| backupvault | Nee | Nee |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Batchaccounts | Ja | Ja |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Clusters | Nee | Nee |
| bestandsservers | Nee | Nee |
| jobs | Nee | Nee |
| workspaces | Nee | Nee |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| mapapis | Nee | Nee |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| biztalk | Ja | Ja |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| blockchainmembers | Ja | Ja |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| blueprintassignments | Nee | Nee |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| botservices | Ja | Ja |

## <a name="microsoftcache"></a>Microsoft.Cache
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| redis | Ja | Ja |

> [!IMPORTANT]
> Als de Azure-Cache voor Redis-exemplaar is geconfigureerd met een virtueel netwerk, kan het exemplaar kan niet worden verplaatst naar een ander abonnement. Zie [virtuele netwerken verplaatsen beperkingen](./move-limitations/virtual-network-move-limitations.md).

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| profiles | Ja | Ja |
| profielen/eindpunten | Ja | Ja |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| certificateorders | Ja | Ja |

> [!IMPORTANT]
> Zie [App Service verplaatsen richtlijnen](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| domainnames | Ja | Nee |
| virtuele machines | Ja | Nee |

> [!IMPORTANT]
> Zie [klassieke implementatie verplaatsen richtlijnen](./move-limitations/classic-model-move-limitations.md). Klassieke resources kunnen worden uitgewisseld tussen abonnementen met een specifieke bewerking voor dit scenario.

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| networksecuritygroups | Nee | Nee |
| bereiken | Nee | Nee |
| virtualnetworks | Nee | Nee |

> [!IMPORTANT]
> Zie [klassieke implementatie verplaatsen richtlijnen](./move-limitations/classic-model-move-limitations.md). Klassieke resources kunnen worden uitgewisseld tussen abonnementen met een specifieke bewerking voor dit scenario.

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Ja | Nee |

> [!IMPORTANT]
> Zie [klassieke implementatie verplaatsen richtlijnen](./move-limitations/classic-model-move-limitations.md). Klassieke resources kunnen worden uitgewisseld tussen abonnementen met een specifieke bewerking voor dit scenario.

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| availabilitysets | Ja | Ja |
| Schijven | Ja | Ja |
| galerieën | Nee | Nee |
| galleries/images | Nee | Nee |
| galerieën/afbeeldingen/versies | Nee | Nee |
| hostgroups | Nee | Nee |
| hostgroups /-hosts | Nee | Nee |
| images | Ja | Ja |
| proximityplacementgroups | Nee | Nee |
| restorepointcollections | Nee | Nee |
| sharedvmimages | Nee | Nee |
| sharedvmimages /-versies | Nee | Nee |
| Momentopnamen | Ja | Ja |
| virtuele machines | Ja | Ja |
| virtuele machines/extensies | Ja | Ja |
| virtualmachinescalesets | Ja | Ja |

> [!IMPORTANT]
> Zie [virtuele Machines verplaatsen richtlijnen](./move-limitations/virtual-machines-move-limitations.md).

## <a name="microsoftcontainer"></a>Microsoft.Container
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| containergroups | Nee | Nee |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| containergroups | Nee | Nee |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| registers | Ja | Ja |
| registers/buildtasks | Ja | Ja |
| registers/replicaties | Ja | Ja |
| registers/taken | Ja | Ja |
| registers/webhooks | Ja | Ja |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| containerservices | Nee | Nee |
| managedclusters | Nee | Nee |
| openshiftmanagedclusters | Nee | Nee |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| toepassingen | Ja | Ja |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| connectoren | Ja | Ja |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hubs | Ja | Ja |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| jobs | Nee | Nee |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| databoxedgedevices | Nee | Nee |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| workspaces | Nee | Nee |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| catalogi | Ja | Ja |
| datacatalogs | Nee | Nee |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| connectionmanagers | Nee | Nee |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Pakketten | Nee | Nee |
| Plannen | Nee | Nee |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| datafactories | Ja | Ja |
| factory 's | Ja | Ja |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| datalakeaccounts | Nee | Nee |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Nee | Nee |
| Services-projecten | Nee | Nee |
| sleuven | Nee | Nee |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| servers | Ja | Ja |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| servers | Ja | Ja |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| servergroups | Nee | Nee |
| servers | Ja | Ja |
| serversv2 | Ja | Ja |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| artifactsources | Ja | Ja |
| Implementaties | Ja | Ja |
| servicetopologies | Ja | Ja |
| servicetopologies/services | Ja | Ja |
| servicetopologies/services/serviceunits | Ja | Ja |
| stappen | Ja | Ja |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| elasticpools | Nee | Nee |
| elasticpools/iothubtenants | Nee | Nee |
| iothubs | Ja | Ja |
| provisioningservices | Ja | Ja |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Domeincontrollers | Nee | Nee |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| labcenters | Nee | Nee |
| Labs | Ja | Nee |
| Labs /-omgevingen | Ja | Ja |
| Labs/servicerunners | Ja | Ja |
| Labs/virtuele machines | Ja | Nee |
| Schema 's | Ja | Ja |

## <a name="microsoftdns"></a>microsoft.dns
| Resourcetype | Resource group | Subscription |
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
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| databaseaccounts | Ja | Ja |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| domeinen | Ja | Ja |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Ja | Ja |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| domeinen | Ja | Ja |
| onderwerpen | Ja | Ja |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Clusters | Ja | Ja |
| Naamruimten | Ja | Ja |

## <a name="microsoftgenomics"></a>Microsoft.Genomics
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hanainstances | Ja | Ja |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Clusters | Ja | Ja |

> [!IMPORTANT]
> U kunt de HDInsight-clusters verplaatsen naar een nieuw abonnement of resourcegroep. U kunt echter niet verplaatsen tussen abonnementen de netwerkresources die worden gekoppeld aan het HDInsight-cluster (zoals het virtuele netwerk, een NIC of een load balancer). Bovendien verplaatsen u niet naar een nieuwe resourcegroep een NIC die is gekoppeld aan een virtuele machine voor het cluster.
>
> Bij het verplaatsen van een HDInsight-cluster naar een nieuw abonnement, verplaatst u eerst andere bronnen (zoals de storage-account). Verplaats het HDInsight-cluster op zichzelf.

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| services | Ja | Ja |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| machines | Nee | Nee |

## <a name="microsofthybriddata"></a>Microsoft.HybridData
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| datamanagers | Ja | Ja |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| jobs | Ja | Ja |

## <a name="microsoftinsights"></a>microsoft.insights
| Resourcetype | Resource group | Subscription |
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
| scheduledqueryrules | Ja | Ja |
| webtests | Ja | Ja |
| werkmappen | Ja | Ja |

> [!IMPORTANT]
> Zorg ervoor dat verplaatsen naar nieuwe abonnement niet groter is dan [abonnementquota](../azure-subscription-service-limits.md#azure-monitor-limits).

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| iotapps | Ja | Ja |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| checknameavailability | Ja | Ja |
| graph | Ja | Ja |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hsmpools | Nee | Nee |
| Kluizen | Ja | Ja |

> [!IMPORTANT]
> Sleutelkluizen gebruikt voor versleuteling van schijf kan niet worden verplaatst naar een resourcegroep in hetzelfde abonnement of tussen abonnementen.

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Clusters | Ja | Ja |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| labaccounts | Nee | Nee |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hostingenvironments | Nee | Nee |
| integrationaccounts | Ja | Ja |
| integrationserviceenvironments | Nee | Nee |
| isolatedenvironments | Nee | Nee |
| Werkstromen | Ja | Ja |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| commitmentplans | Ja | Ja |
| webservices | Ja | Nee |
| workspaces | Ja | Ja |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| operationalizationclusters | Ja | Ja |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |
| accounts/werkruimten | Nee | Nee |
| accounts/workspaces/projects | Nee | Nee |
| teamaccounts | Nee | Nee |
| teamaccounts/werkruimten | Nee | Nee |
| teamaccounts/workspaces/projects | Nee | Nee |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| hostingaccounts | Nee | Nee |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| workspaces | Nee | Nee |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| userassignedidentities | Nee | Nee |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Ja | Ja |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| classicdevservices | Nee | Nee |

## <a name="microsoftmedia"></a>Microsoft.Media
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| mediaservices | Ja | Ja |
| mediaservices/liveevents | Ja | Ja |
| mediaservices/door | Ja | Ja |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| assessmentprojects | Nee | Nee |
| migrateprojects | Nee | Nee |
| projecten | Nee | Nee |

## <a name="microsoftnetapp"></a>Microsoft.NetApp
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| netappaccounts | Nee | Nee |
| netappaccounts/capacitypools | Nee | Nee |
| capacitypools-netappaccounts-volumes | Nee | Nee |
| netappaccounts/capacitypools/volumes/mounttargets | Nee | Nee |
| netappaccounts/capacitypools/volumes/momentopnamen | Nee | Nee |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| applicationgateways | Nee | Nee |
| applicationgatewaywebapplicationfirewallpolicies | Nee | Nee |
| applicationsecuritygroups | Ja | Ja |
| azurefirewalls | Ja | Ja |
| bastionhosts | Nee | Nee |
| Verbindingen | Ja | Ja |
| ddoscustompolicies | Ja | Ja |
| ddosprotectionplans | Nee | Nee |
| dnszones | Ja | Ja |
| expressroutecircuits | Nee | Nee |
| expressroutecrossconnections | Nee | Nee |
| expressroutegateways | Nee | Nee |
| expressrouteports | Nee | Nee |
| frontdoors | Nee | Nee |
| frontdoorwebapplicationfirewallpolicies | Nee | Nee |
| loadbalancers | Ja, basis-SKU<br>Niet - standaard-SKU | Ja, basis-SKU<br>Niet - standaard-SKU |
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
| privatednszones | Ja | Ja |
| privatednszones/virtualnetworklinks | Ja | Ja |
| privateendpoints | Nee | Nee |
| privatelinkservices | Nee | Nee |
| publicipaddresses | Ja, basis-SKU<br>Niet - standaard-SKU | Ja, basis-SKU<br>Niet - standaard-SKU |
| publicipprefixes | Ja | Ja |
| routefilters | Nee | Nee |
| routetables | Ja | Ja |
| securegateways | Ja | Ja |
| serviceendpointpolicies | Ja | Ja |
| trafficmanagerprofiles | Ja | Ja |
| virtualhubs | Nee | Nee |
| virtualnetworkgateways | Ja | Ja |
| virtualnetworks | Ja | Ja |
| virtualnetworktaps | Nee | Nee |
| virtualwans | Nee | Nee |
| vpngateways | Nee | Nee |
| vpnsites | Nee | Nee |
| webapplicationfirewallpolicies | Ja | Ja |

> [!IMPORTANT]
> Zie [virtuele netwerken verplaatsen richtlijnen](./move-limitations/virtual-network-move-limitations.md).

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Naamruimten | Ja | Ja |
| namespaces/notificationhubs | Ja | Ja |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| workspaces | Ja | Ja |

> [!IMPORTANT]
> Zorg ervoor dat verplaatsen naar nieuwe abonnement niet groter is dan [abonnementquota](../azure-subscription-service-limits.md#azure-monitor-limits).

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| managementconfigurations | Ja | Ja |
| oplossingen | Ja | Ja |
| Weergaven | Ja | Ja |

## <a name="microsoftpeering"></a>Microsoft.Peering
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| peerings | Nee | Nee |

## <a name="microsoftportal"></a>Microsoft.Portal
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dashboards | Ja | Ja |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| rootresources | Nee | Nee |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| workspacecollections | Ja | Ja |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Capaciteit | Ja | Ja |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Nee | Nee |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Kluizen | Ja | Ja |

> [!IMPORTANT]
> Zie [Recovery Services verplaatsen richtlijnen](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftrelay"></a>Microsoft.Relay
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Naamruimten | Ja | Ja |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| toepassingen | Ja | Nee |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| stromen | Ja | Ja |
| taakverzamelingen | Ja | Ja |

## <a name="microsoftsearch"></a>Microsoft.Search
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| searchservices | Ja | Ja |

> [!IMPORTANT]
> U kunt verschillende zoeken resources niet verplaatsen in verschillende regio's in één bewerking. In plaats daarvan deze verplaatsen in aparte bewerkingen.

## <a name="microsoftsecurity"></a>Microsoft.Security
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| iotsecuritysolutions | Ja | Ja |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Gateways | Nee | Nee |
| knooppunten | Nee | Nee |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Naamruimten | Ja | Ja |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Resourcetype | Resource group | Subscription |
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
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| toepassingen | Ja | Ja |
| containergroups | Nee | Nee |
| Gateways | Ja | Ja |
| Netwerken | Ja | Ja |
| geheimen | Ja | Ja |
| volumes | Ja | Ja |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| signalr | Ja | Ja |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| siterecoveryvault | Nee | Nee |

> [!IMPORTANT]
> Zie [Recovery Services verplaatsen richtlijnen](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| appliancedefinitions | Nee | Nee |
| apparaten | Nee | Nee |
| applicationdefinitions | Nee | Nee |
| toepassingen | Nee | Nee |
| jitrequests | Nee | Nee |

## <a name="microsoftsql"></a>Microsoft.Sql
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| instancepools | Ja | Ja |
| managedinstances | Ja | Ja |
| managedinstances/databases | Ja | Ja |
| servers | Ja | Ja |
| servers/databases | Ja | Ja |
| servers/elasticpools | Ja | Ja |
| virtualclusters | Ja | Ja |

> [!IMPORTANT]
> Een database en de server moeten zich in dezelfde resourcegroep bevinden. Wanneer u een SQL-server hebt verplaatst, worden ook alle bijbehorende databases verplaatst. Dit gedrag is van toepassing op Azure SQL Database en Azure SQL Data Warehouse-databases.

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| sqlvirtualmachinegroups | Ja | Ja |
| sqlvirtualmachines | Ja | Ja |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dwvm | Nee | Nee |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Ja | Ja |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Caches | Nee | Nee |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Ja | Ja |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Nee | Nee |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Nee | Nee |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| managers | Nee | Nee |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| streamingjobs | Ja | Ja |

> [!IMPORTANT]
> Stream Analytics taken kunnen niet worden verplaatst, bij het uitvoeren van in staat.

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Omgevingen | Nee | Nee |
| omgevingen/eventsources | Nee | Nee |
| instanties | Nee | Nee |
| exemplaren /-omgevingen | Nee | Nee |
| instances/environments/eventsources | Nee | Nee |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| providerregistrations | Nee | Nee |
| resources | Nee | Nee |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Omgevingen | Ja | Ja |
| omgevingen/eventsources | Ja | Ja |
| omgevingen/referencedatasets | Ja | Ja |

## <a name="microsofttoken"></a>Microsoft.Token
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| winkels | Nee | Nee |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| imagetemplates | Nee | Nee |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| account | Ja | Ja |
| -/ accountextensie | Ja | Ja |
| account/project | Ja | Ja |

> [!IMPORTANT]
> Als u wilt wijzigen van het abonnement voor Azure DevOps, Zie [wijzigen van het Azure-abonnement gebruikt voor facturering](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json).

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| dedicatedcloudnodes | Ja | Ja |
| dedicatedcloudservices | Ja | Ja |
| virtuele machines | Ja | Ja |

## <a name="microsoftweb"></a>Microsoft.Web
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| Certificaten | Nee | Ja |
| connectiongateways | Ja | Ja |
| Verbindingen | Ja | Ja |
| customapis | Ja | Ja |
| hostingenvironments | Nee | Nee |
| serverfarms | Ja | Ja |
| sites | Ja | Ja |
| sites/premieraddons | Ja | Ja |
| sites/slots | Ja | Ja |

> [!IMPORTANT]
> Zie [App Service verplaatsen richtlijnen](./move-limitations/app-service-move-limitations.md).

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| deviceservices | Nee | Nee |

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft.WindowsVirtualDesktop
| Resourcetype | Resource group | Subscription |
| ------------- | ----------- | ---------- |
| applicationgroups | Nee | Nee |
| hostpools | Nee | Nee |
| workspaces | Nee | Nee |

## <a name="third-party-services"></a>Services van derden

Services van derden ondersteuning momenteel geen voor de verplaatsing.

## <a name="next-steps"></a>Volgende stappen
Zie voor de opdrachten om resources te verplaatsen, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](resource-group-move-resources.md).

Als u dezelfde gegevens als een bestand met door komma's gescheiden waarden, download [verplaatsen-ondersteuning-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv).
