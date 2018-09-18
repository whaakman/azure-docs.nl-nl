---
title: Ingebouwde rollen voor Azure-resources | Microsoft Docs
description: Beschrijving van de ingebouwde functies voor op rollen gebaseerd toegangsbeheer (RBAC) en Azure-resources. De acties, NotActions, DataActions en NotDataActions bevat.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 09/15/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: 0b7933df1e9f250430800b5b7deba06239cb6fd1
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736668"
---
# <a name="built-in-roles-for-azure-resources"></a>Ingebouwde rollen voor Azure-resources
[Op rollen gebaseerd toegangsbeheer (RBAC)](overview.md) heeft diverse ingebouwde roldefinities die u aan gebruikers, groepen en service-principals toewijzen kunt. Roltoewijzingen zijn de manier waarop u de toegang tot resources in Azure. Als de ingebouwde rollen niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen [aangepaste rollen](custom-roles.md) maken.

De ingebouwde rollen zijn altijd nog in ontwikkeling. Als u de meest recente definities van gebruikersrollen, gebruikt [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) of [az role definitielijst](/cli/azure/role/definition#az-role-definition-list).

## <a name="built-in-role-descriptions"></a>Beschrijvingen van de ingebouwde functies
De volgende tabel bevat korte beschrijvingen van de ingebouwde rollen. Klik op de naam van de rol voor een overzicht van `Actions`, `NotActions`, `DataActions`, en `NotDataActions` voor elke rol.


| Ingebouwde rol | Beschrijving |
| --- | --- |
| [Eigenaar](#owner) | Hiermee beheert u alles, inclusief de toegang tot resources. |
| [Inzender](#contributor) | Hiermee beheert u alles, behalve de toegang tot resources. |
| [Lezer](#reader) | Hiermee geeft u alles weer, maar kunt u niet wijzigingen aanbrengen. |
| [AcrImageSigner](#acrimagesigner) | acr-afbeeldingsondertekenaar |
| [AcrQuarantineReader](#acrquarantinereader) | acr-quarantainegegevenslezer |
| [AcrQuarantineWriter](#acrquarantinewriter) | acr-quarantainegegevensschrijver |
| [Inzender voor API Management-Services](#api-management-service-contributor) | Hiermee beheert u API Management-services, maar kunt u niet de toegang tot de services beheren. |
| [Rol Operator API Management-Service](#api-management-service-operator-role) | Kan de service beheren, maar niet de API's |
| [Rol Lezer API Management-Service](#api-management-service-reader-role) | Alleen-lezentoegang tot de service en API's |
| [Application Insights-Onderdeelinzender](#application-insights-component-contributor) | Kan onderdelen van Application Insights beheren |
| [Application Insights Snapshot Debugger](#application-insights-snapshot-debugger) | Biedt de gebruiker toestemming voor het weergeven en downloaden van momentopnamen voor foutopsporing die zijn verzameld met Application Insights Snapshot Debugger. Houd er rekening mee dat deze machtigingen zijn niet opgenomen in de [eigenaar](#owner) of [Inzender](#contributor) rollen. |
| [Operator voor Automation](#automation-job-operator) | Taken maken en beheren met Automation-runbooks. |
| [Automation-Operator](#automation-operator) | Operators voor Automation kunnen taken starten, stoppen, onderbreken en hervatten |
| [Operator voor Automation-Runbook](#automation-runbook-operator) | Runbook-eigenschappen lezen: hiermee kunnen taken van de runbook worden gemaakt. |
| [Beheerdersrol voor Azure Kubernetes Service-Cluster](#azure-kubernetes-service-cluster-admin-role) | Lijst met cluster admin credential actie. |
| [Azure Kubernetes Service-Cluster-gebruikersrol](#azure-kubernetes-service-cluster-user-role) | Lijst met cluster referentie actie van de gebruiker. |
| [De eigenaar van de Azure Stack-registratie](#azure-stack-registration-owner) | Hiermee kunt u Azure Stack-registraties beheren. |
| [Back-Inzender](#backup-contributor) | Hiermee kunt u de back-upservice beheren, maar u kunt geen kluizen maken of anderen toegang verlenen |
| [Back-upoperator](#backup-operator) | Hiermee kunt u back-upservices beheren, met uitzondering van het verwijderen van back-ups, het maken van kluizen en het verlenen van toegang aan anderen |
| [Back-Uplezer](#backup-reader) | Kan de back-upservices weergeven, maar kan geen wijzigingen aanbrengen |
| [Facturering voor lezer](#billing-reader) | Hiermee kunt u lezen factureringsgegevens |
| [Inzender voor BizTalk](#biztalk-contributor) | Hiermee beheert u BizTalk-services, maar kunt u niet de toegang tot de services beheren. |
| [Inzender voor CDN-eindpunt](#cdn-endpoint-contributor) | Kan CDN-eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [Lezer voor CDN-eindpunt](#cdn-endpoint-reader) | Kan CDN-eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
| [Inzender voor CDN-profiel](#cdn-profile-contributor) | Kan CDN-profielen en de bijbehorende eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [Lezer voor CDN-profiel](#cdn-profile-reader) | Kan CDN-profielen en de bijbehorende eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
| [Inzender voor klassieke netwerken](#classic-network-contributor) | Hiermee beheert u klassieke netwerken, maar kunt u niet de toegang tot de netwerken beheren. |
| [Inzender voor klassieke Opslagaccounts](#classic-storage-account-contributor) | Hiermee kunt u klassieke opslagaccounts beheren, maar niet de toegang. |
| [Klassieke opslag Account servicerol Sleuteloperator](#classic-storage-account-key-operator-service-role) | Sleuteloperators voor klassieke opslagaccounts kunnen een lijst met sleutels voor klassieke opslagaccounts maken en de sleutels opnieuw genereren |
| [Inzender voor klassieke virtuele machines](#classic-virtual-machine-contributor) | Hiermee beheert u klassieke virtuele machines, maar kunt u niet de toegang tot de virtuele machines of het virtuele netwerk of opslagaccount beheren waaraan de virtuele machines zijn gekoppeld. |
| [Cognitive Services-gebruiker](#cognitive-services-user) | Hiermee kunt u lees- en sleutels van Cognitive Services weergeven. |
| [Rol van lezer voor cosmos DB-Account](#cosmos-db-account-reader-role) | Kan Azure Cosmos DB-accountgegevens lezen. Zie [Inzender voor DocumentDB-Account](#documentdb-account-contributor) voor het beheren van Azure Cosmos DB-accounts. |
| [Inzender voor Data Box](#data-box-contributor) | Hiermee beheert u alles onder de Data Box-Service, behalve het verlenen van toegang aan anderen. |
| [Data Box-lezer](#data-box-reader) | U kunt Data Box-Service met uitzondering van het maken van de volgorde of ordergegevens bewerken en toegang geven tot andere beheren. |
| [Inzender Data Factory](#data-factory-contributor) | Hiermee beheert u data factory's, maar kunt u niet de toegang tot de data factory's beheren. |
| [Data Lake Analytics-ontwikkelaar](#data-lake-analytics-developer) | Hiermee kunt u uw eigen taken indienen, controleren en beheren, maar geen Data Lake Analytics-accounts maken of verwijderen. |
| [Gegevens Purger](#data-purger) | Kan analytische gegevens verwijderen |
| [DevTest Labs-gebruiker](#devtest-labs-user) | Hiermee kunt u verbinding kunt maken, starten, opnieuw opstarten en afsluiten van virtuele machines in Azure DevTest Labs. |
| [Inzender voor DNS-Zone](#dns-zone-contributor) | Hiermee kunt u DNS-zones en recordsets beheren in Azure DNS, maar kunt u niet bepalen wie toegang heeft. |
| [Inzender voor het DocumentDB-Account](#documentdb-account-contributor) | Kan Azure Cosmos DB-accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB. |
| [Inzender voor het Account van de intelligente systemen](#intelligent-systems-account-contributor) | Hiermee beheert u Intelligent Systems-accounts, maar kunt u niet de toegang tot de accounts beheren. |
| [Inzender voor Key Vault](#key-vault-contributor) | Hiermee kunt u sleutelkluizen beheren, maar niet de toegang hiertoe. |
| [Labmaker](#lab-creator) | Hiermee kunt u beheerde labs maken, beheren en verwijderen in uw Azure Lab-accounts. |
| [Inzender van log Analytics](#log-analytics-contributor) | Inzender van log Analytics kan alle controlegegevens lezen en bewerken van instellingen voor controle. Bewerken van instellingen voor controle houdt het toevoegen van de VM-extensie voor virtuele machines; lezen van opslagaccountsleutels om te kunnen verzamelen van Logboeken van Azure Storage; configureren het maken en configureren van Automation-accounts; toevoegen van oplossingen en Azure diagnostics configureren op alle Azure-resources. |
| [Lezer van log Analytics](#log-analytics-reader) | De lezer van Log Analytics kan alle controlegegevens weergeven en doorzoeken en de controle-instellingen weergeven, inclusief het weergeven van de configuratie van Azure Diagnostics op alle Azure-resources. |
| [Logische App-bijdrager](#logic-app-contributor) | Hiermee kunt u logische apps beheren, maar niet de toegang hiertoe. |
| [Logische App-Operator](#logic-app-operator) | Hiermee kunt u logische apps lezen, inschakelen en uitschakelen. |
| [De Operatorrol beheerde toepassing](#managed-application-operator-role) | Hiermee kunt u lees- en acties uitvoeren op resources van beheerde toepassingen |
| [Beheerde toepassingen-lezer](#managed-applications-reader) | Hiermee kunt u lezen van resources in een beheerde app en de aanvraag voor JIT-toegang. |
| [Inzender beheerde identiteit](#managed-identity-contributor) | Door gebruiker toegewezen identiteit maken, lezen, bijwerken en verwijderen |
| [Operator beheerde identiteit](#managed-identity-operator) | Door gebruiker toegewezen identiteit maken en toewijzen |
| [Beheergroep-Inzender](#management-group-contributor) | De rol Inzender beheergroep |
| [Lezer van de beheergroep](#management-group-reader) | Rol Lezer beheergroep |
| [Controlebijdrager](#monitoring-contributor) | Kan alle controlegegevens lezen en bewerken van instellingen voor controle. Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Uitgever van de metrische gegevens controleren](#monitoring-metrics-publisher) | Hiermee schakelt het publiceren van metrische gegevens bij Azure-resources |
| [Controlelezer](#monitoring-reader) | Kan alle controlegegevens lezen (metrische gegevens, Logboeken, enz.). Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Inzender voor netwerken](#network-contributor) | Hiermee beheert u netwerken, maar kunt u niet de toegang tot de netwerken beheren. |
| [Nieuwe Relic APM-Account Inzender](#new-relic-apm-account-contributor) | Hiermee beheert u New Relic Application Performance Management-accounts en -toepassingen, maar kunt u niet de toegang tot de accounts en toepassingen beheren. |
| [Lezer en toegang tot gegevens](#reader-and-data-access) | Hiermee kunt u Alles weergeven, maar kunt u verwijderen of te maken van een storage-account of een ingesloten resource niet. Er kunnen ook toegang tot alle gegevens in een opslagaccount verleend via toegang tot opslagaccountsleutels lezen/schrijven. |
| [Redis-Cache-Inzender](#redis-cache-contributor) | Hiermee beheert u exemplaren van Redis-cache, maar kunt u niet de toegang tot de exemplaren beheren. |
| [Inzender voor Resourcebeleid (Preview)](#resource-policy-contributor-preview) | (Preview) Gebruikers gevuld vanuit EA, met rechten voor het maken/wijzigen voor resourcebeleid, supportticket maken en het lezen van resources/hiërarchie. |
| [Inzender voor Scheduler-taak](#scheduler-job-collections-contributor) | Hiermee beheert u Scheduler-taakverzamelingen, maar kunt u niet de toegang tot de verzamelingen beheren. |
| [Inzender voor Search-Services](#search-service-contributor) | Hiermee beheert u Search-services, maar kunt u niet de toegang tot de services beheren. |
| [Beveiligingsbeheerder](#security-admin) | In Security Center alleen: kunt weergeven beveiligingsbeleid, security status weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, waarschuwingen en aanbevelingen negeren |
| [Security Manager](#security-manager) | Hiermee beheert u beveiligingsonderdelen, beveiligingsbeleidsregels en virtuele machines |
| [Beveiligingslezer](#security-reader) | In Security Center alleen: aanbevelingen en waarschuwingen, weergave beveiligingsbeleid van de status van de beveiliging weergeven, maar kan geen wijzigingen aanbrengen kunt weergeven |
| [Site Recovery-Inzender](#site-recovery-contributor) | Hiermee kunt u de Site Recovery-service beheren, maar geen kluizen maken of rollen toewijzen |
| [Site Recovery-Operator](#site-recovery-operator) | Hiermee kunt u failover en fallback uitvoeren, maar geen andere beheerbewerkingen voor Site Recovery |
| [Site Recovery-lezer](#site-recovery-reader) | Hiermee kunt u de Site Recovery-status weergeven maar geen andere beheerbewerkingen voor Site Recovery uitvoeren |
| [' SQL DB Contributor '](#sql-db-contributor) | Kunt u SQL-databases, maar niet de toegang tot beheren. U beheren niet ook hun beveiligingsbeleid of de bovenliggende SQL-servers. |
| [SQL Security Manager](#sql-security-manager) | Hiermee kunt u beveiligingsbeleid van SQL-servers en -databases beheren, maar niet de toegang tot het beleid. |
| [Inzender voor SQL Server](#sql-server-contributor) | Hiermee kunt u SQL-servers en -databases beheren, maar niet de toegang tot en het beveiligingsbeleid van de servers en databases. |
| [Inzender voor opslagaccounts](#storage-account-contributor) | Hiermee kunt u opslagaccounts beheren, maar niet de toegang. |
| [Storage-Account servicerol Sleuteloperator](#storage-account-key-operator-service-role) | Sleuteloperators voor opslagaccounts kunnen een lijst met sleutels voor opslagaccounts maken en de sleutels opnieuw genereren |
| [Gegevensbijdrager voor Blob (Preview)](#storage-blob-data-contributor-preview) | Hiermee wordt lees-, schrijf- en verwijdertoegang tot Azure Storage-blobcontainers en -gegevens verleend. |
| [Gegevenslezer voor Opslagblob (Preview)](#storage-blob-data-reader-preview) | Hiermee wordt leestoegang tot Azure Storage-blobcontainers en -gegevens verleend. |
| [Gegevensbijdrager voor wachtrij (Preview)](#storage-queue-data-contributor-preview) | Hiermee wordt lees-, schrijf- en verwijdertoegang tot Azure Storage-wachtrijen en -wachtrijberichten verleend. |
| [Gegevenslezer voor Opslagwachtrij (Preview)](#storage-queue-data-reader-preview) | Hiermee wordt leestoegang verleend voor Azure Storage-wachtrijen en -wachtrijberichten. |
| [Inzender voor ondersteuningsaanvragen](#support-request-contributor) | Hiermee kunt u ondersteuningsaanvragen maken en beheren |
| [Inzender voor Traffic Manager](#traffic-manager-contributor) | Hiermee kunt u Traffic Manager-profielen beheren, maar kunt u niet bepalen wie toegang heeft. |
| [Beheerder van gebruikerstoegang](#user-access-administrator) | Hiermee beheert u de gebruikerstoegang tot Azure-resources. |
| [Beheerdersaanmelding bij virtuele Machine](#virtual-machine-administrator-login) | Virtuele Machines weergeven in de portal en meld u aan als beheerder |
| [Inzender voor virtuele machines](#virtual-machine-contributor) | Hiermee kunt u beheren van virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of storage-account die zijn gekoppeld. |
| [Gebruikersaanmelding bij virtuele Machine](#virtual-machine-user-login) | Virtuele Machines in de portal en meld u aan als een gewone gebruiker weergeven. |
| [Inzender voor webabonnementen](#web-plan-contributor) | Hiermee beheert u de webabonnementen voor websites, maar kunt u niet de toegang tot de abonnementen beheren. |
| [Inzender voor websites](#website-contributor) | Hiermee beheert u websites (niet webabonnementen), maar kunt u niet de toegang tot de websites beheren. |


## <a name="owner"></a>Eigenaar
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u alles, inclusief de toegang tot resources. |
> | **Id** | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | **Acties** |  |
> | * | Maken en beheren van resources van alle typen |

## <a name="contributor"></a>Inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u alles, behalve de toegang tot resources. |
> | **Id** | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | **Acties** |  |
> | * | Maken en beheren van resources van alle typen |
> | **notActions** |  |
> | Microsoft.Authorization/*/Delete | Functies en roltoewijzingen verwijderen niet |
> | Microsoft.Authorization/*/Write | Kan de rollen en roltoewijzingen niet maken |
> | Microsoft.Authorization/elevateAccess/Action | Hiermee wordt oproepende functie de rechten van Administrator voor gebruikerstoegang gegeven voor het tenantbereik |
> | Microsoft.Blueprint/blueprintAssignments/write |  |
> | Microsoft.Blueprint/blueprintAssignments/delete |  |

## <a name="reader"></a>Lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee geeft u alles weer, maar kunt u niet wijzigingen aanbrengen. |
> | **Id** | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |

## <a name="acrimagesigner"></a>AcrImageSigner
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | acr-afbeeldingsondertekenaar |
> | **Id** | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |

## <a name="acrquarantinereader"></a>AcrQuarantineReader
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | acr-quarantainegegevenslezer |
> | **Id** | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="acrquarantinewriter"></a>AcrQuarantineWriter
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | acr-quarantainegegevensschrijver |
> | **Id** | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | **Acties** |  |
> | Microsoft.ContainerRegistry/registries/*/write |  |
> | Microsoft.ContainerRegistry/registries/*/read |  |

## <a name="api-management-service-contributor"></a>Inzender voor API Management-services
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u API Management-services, maar kunt u niet de toegang tot de services beheren. |
> | **Id** | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | **Acties** |  |
> | Microsoft.ApiManagement/service/* | Maken en beheren van API Management-service |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="api-management-service-operator-role"></a>Operator-rol voor de API Management-service
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan de service beheren, maar niet de API's |
> | **Id** | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | **Acties** |  |
> | Microsoft.ApiManagement/service/*/read | Lezen API Management Service-exemplaren |
> | Microsoft.ApiManagement/service/backup/action | Back-API Management-Service voor de opgegeven container in een gebruiker opgegeven opslagaccount |
> | Microsoft.ApiManagement/service/delete | API Management-Service-exemplaar verwijderen |
> | Microsoft.ApiManagement/service/managedeployments/action | Wijzigen van SKU /-eenheden, regionale implementaties van API Management-Service toevoegen/verwijderen |
> | Microsoft.ApiManagement/service/read | Metagegevens voor een exemplaar van API Management-Service lezen |
> | Microsoft.ApiManagement/service/restore/action | API Management-Service van de opgegeven container in een door de gebruiker opgegeven storage-account herstellen |
> | Microsoft.ApiManagement/service/updatecertificate/action | SSL-certificaat uploaden voor een API Management-Service |
> | Microsoft.ApiManagement/service/updatehostname/action | Installeren, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service |
> | Microsoft.ApiManagement/service/write | Maak een nieuw exemplaar van API Management-Service |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **notActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Lijst met gebruikerssleutels ophalen |

## <a name="api-management-service-reader-role"></a>Lezerrol voor de API Management-service
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Alleen-lezentoegang tot de service en API's |
> | **Id** | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | **Acties** |  |
> | Microsoft.ApiManagement/service/*/read | Lezen API Management Service-exemplaren |
> | Microsoft.ApiManagement/service/read | Metagegevens voor een exemplaar van API Management-Service lezen |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **notActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Lijst met gebruikerssleutels ophalen |

## <a name="application-insights-component-contributor"></a>Inzender voor Application Insights-onderdelen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan onderdelen van Application Insights beheren |
> | **Id** | ae349356-3a1b-4a5e-921d-050484c6347e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Insights/components/* | Maken en beheren van inzicht onderdelen |
> | Microsoft.Insights/webtests/* | Maken en beheren van webtests |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="application-insights-snapshot-debugger"></a>Application Insights Snapshot Debugger
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Biedt de gebruiker toestemming voor het weergeven en downloaden van momentopnamen voor foutopsporing die zijn verzameld met Application Insights Snapshot Debugger. Houd er rekening mee dat deze machtigingen zijn niet opgenomen in de [eigenaar](#owner) of [Inzender](#contributor) rollen. |
> | **Id** | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="automation-job-operator"></a>Operator voor Automation-taak
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Taken maken en beheren met Automation-runbooks. |
> | **Id** | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
> | Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak wordt hervat |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Een Azure Automation-taak gestopt |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hiermee wordt een Azure Automation-taak onderbroken |
> | Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
> | Microsoft.Automation/automationAccounts/jobs/output/read | De uitvoer van een taak opgehaald |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="automation-operator"></a>Automation-operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Operators voor Automation kunnen taken starten, stoppen, onderbreken en hervatten |
> | **Id** | d3881f73-407a-4167-8283-e981cbba0404 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
> | Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak wordt hervat |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Een Azure Automation-taak gestopt |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Hiermee wordt een Azure Automation-taak onderbroken |
> | Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Een Azure Automation-taakschema opgehaald |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Hiermee maakt u een Azure Automation-taakschema |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | De werkruimte die is gekoppeld aan het automation-account opgehaald |
> | Microsoft.Automation/automationAccounts/read | Een Azure Automation-account opgehaald |
> | Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
> | Microsoft.Automation/automationAccounts/schedules/read | Een Azure Automation-planningsasset opgehaald |
> | Microsoft.Automation/automationAccounts/schedules/write | Hiermee maakt of een Azure Automation-planningsasset werkt |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Automation/automationAccounts/jobs/output/read | De uitvoer van een taak opgehaald |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="automation-runbook-operator"></a>Operator voor Automation-runbook
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Runbook-eigenschappen lezen: hiermee kunnen taken van de runbook worden gemaakt. |
> | **Id** | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="azure-kubernetes-service-cluster-admin-role"></a>Beheerdersrol voor Azure Kubernetes Service-Cluster
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lijst met cluster admin credential actie. |
> | **Id** | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | **Acties** |  |
> | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Lijst van de referentie clusterAdmin van een beheerde cluster |

## <a name="azure-kubernetes-service-cluster-user-role"></a>Azure Kubernetes Service-Cluster-gebruikersrol
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Lijst met cluster referentie actie van de gebruiker. |
> | **Id** | 4abbcc35-e782-43d8-92c5-2d3f1bd2253f |
> | **Acties** |  |
> | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Lijst van de referentie clusterUser van een beheerde cluster |

## <a name="azure-stack-registration-owner"></a>Eigenaar Azure Stack-registratie
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Azure Stack-registraties beheren. |
> | **Id** | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | **Acties** |  |
> | Microsoft.AzureStack/registrations/products/listDetails/action | Details voor een Azure Stack Marketplace-product uitgebreid opgehaald |
> | Microsoft.AzureStack/registrations/products/read | Haalt u de eigenschappen van een Azure Stack Marketplace-product |
> | Microsoft.AzureStack/registrations/read | Haalt u de eigenschappen van een Azure Stack-registratie |

## <a name="backup-contributor"></a>Back-upinzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u de back-upservice beheren, maar u kunt geen kluizen maken of anderen toegang verlenen |
> | **Id** | 5e467623-bb1f-42f4-a55d-6e525e11384b |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Resultaten van de bewerking op de back-upbeheer beheren |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Back-containers in de back-fabrics van Recovery Services-kluis maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Taken exporteren |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van Exporttaakbewerking. |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Maken en beheren van meta-gegevens met betrekking tot back-upbeheer |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van back-upbeheer bewerkingen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Back-beleid maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Artikelen die u kunnen een back-up maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Maken en beheren van back-ups van items |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Maken en beheren van containers met back-upitems |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Hiermee retourneert u overzichten van beschermde Items en beschermde Servers van Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Maken en beheren van certificaten met betrekking tot back-up in Recovery Services-kluis |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie met betrekking tot de kluis |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/* | Beheren van detectiebewerking voor het ophalen van containers van een nieuw gemaakt |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van geregistreerde identiteiten |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/usages/* | Maken en beheren van het gebruik van Recovery Services-kluis |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="backup-operator"></a>Back-upoperator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u back-upservices beheren, met uitzondering van het verwijderen van back-ups, het maken van kluizen en het verlenen van toegang aan anderen |
> | **Id** | 00c29273-979b-4161-815c-10b084fb9324 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Status van de bewerking geretourneerd |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Hiermee wordt een back-up van het beveiligde item gemaakt. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Retourneert de objectgegevens van het beveiligde Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Direct Itemherstel inrichten voor beveiligd Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Herstelpunten voor beveiligde items ophalen. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Herstelpunten voor beveiligde items herstellen. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Direct Itemherstel intrekken voor beveiligd Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een beveiligd back-upitem maken |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
> | Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | De taak annuleren |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Alle taakobjecten geretourneerd |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Taken exporteren |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van Exporttaakbewerking. |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Hiermee worden de metagegevens van back-upbeheer voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van back-upbeheer bewerkingen |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beveiligingsbeleid voor apps |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Artikelen die u kunnen een back-up maken en beheren |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Hiermee wordt een lijst met alle beveiligbare items opgehaald. |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers van het abonnement |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Hiermee retourneert u overzichten van beschermde Items en beschermde Servers van Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Resourcecertificaat bijwerken de bron-/ kluisreferentiecertificaat bijgewerkt. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Hiermee vernieuwt u de containerlijst |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | De bewerking servicecontainer registreren kan worden gebruikt voor het registreren van een container met de Recovery-Service. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="backup-reader"></a>Back-uplezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan de back-upservices weergeven, maar kan geen wijzigingen aanbrengen |
> | **Id** | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Status van de bewerking geretourneerd |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Retourneert de objectgegevens van het beveiligde Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
> | Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupJobs/read | Alle taakobjecten geretourneerd |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Taken exporteren |
> | Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Hiermee worden de metagegevens van back-upbeheer voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beveiligingsbeleid voor apps |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers van het abonnement |
> | Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Hiermee retourneert u overzichten van beschermde Items en beschermde Servers van Recovery Services. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Herstelpunten voor beveiligde items ophalen. |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van Exporttaakbewerking. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |

## <a name="billing-reader"></a>Facturering voor lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lezen factureringsgegevens |
> | **Id** | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Billing/*/read | Facturering-gegevens lezen |
> | Microsoft.Consumption/*/read |  |
> | Microsoft.Commerce/*/read |  |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="biztalk-contributor"></a>Inzender voor BizTalk
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u BizTalk-services, maar kunt u niet de toegang tot de services beheren. |
> | **Id** | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.BizTalkServices/BizTalk/* | BizTalk services maken en beheren |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-endpoint-contributor"></a>Inzender voor CDN-eindpunt
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan CDN-eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
> | **Id** | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-endpoint-reader"></a>Lezer voor CDN-eindpunt
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan CDN-eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
> | **Id** | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/read |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-profile-contributor"></a>Inzender voor CDN-profiel
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan CDN-profielen en de bijbehorende eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
> | **Id** | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-profile-reader"></a>Lezer voor CDN-profiel
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan CDN-profielen en de bijbehorende eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
> | **Id** | 8f96442b-4075-438f-813d-ad51ab4019af |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/read |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-network-contributor"></a>Inzender voor klassieke netwerken
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u klassieke netwerken, maar kunt u niet de toegang tot de netwerken beheren. |
> | **Id** | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.ClassicNetwork/* | Maken en beheren van klassieke netwerken |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-storage-account-contributor"></a>Inzender voor klassieke opslagaccounts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u klassieke opslagaccounts beheren, maar niet de toegang. |
> | **Id** | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.ClassicStorage/storageAccounts/* | Maken en storage-accounts beheren |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-storage-account-key-operator-service-role"></a>De servicerol Sleuteloperator voor klassieke opslagaccounts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Sleuteloperators voor klassieke opslagaccounts kunnen een lijst met sleutels voor klassieke opslagaccounts maken en de sleutels opnieuw genereren |
> | **Id** | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | **Acties** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | De bestaande toegangssleutels voor het opslagaccount worden geregenereerd. |

## <a name="classic-virtual-machine-contributor"></a>Inzender voor klassieke virtuele machines
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u klassieke virtuele machines, maar kunt u niet de toegang tot de virtuele machines of het virtuele netwerk of opslagaccount beheren waaraan de virtuele machines zijn gekoppeld. |
> | **Id** | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.ClassicCompute/domainNames/* | Maken en beheren van domeinnamen van klassieke berekening |
> | Microsoft.ClassicCompute/virtualMachines/* | Virtuele machines maken en beheren |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Een gereserveerde IP koppelen |
> | Microsoft.ClassicNetwork/reservedIps/read | Hiermee worden de gereserveerde IP's opgehaald. |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Hiermee wordt de deelname aan het virtuele netwerk uitgevoerd. |
> | Microsoft.ClassicNetwork/virtualNetworks/read | Het virtuele netwerk wordt opgehaald. |
> | Microsoft.ClassicStorage/storageAccounts/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Retourneert de installatiekopie van het opslagaccount. (Afgeschaft. Gebruik 'Microsoft.ClassicStorage/storageAccounts/vmImages') |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
> | Microsoft.ClassicStorage/storageAccounts/read | Opslagaccount met het opgegeven account wordt geretourneerd. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cognitive-services-user"></a>Cognitive Services-gebruiker
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lees- en sleutels van Cognitive Services weergeven. |
> | **Id** | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Acties** |  |
> | Microsoft.CognitiveServices/*/read |  |
> | Microsoft.CognitiveServices/accounts/listkeys/action | Een lijst met sleutels maken |
> | Microsoft.Insights/metricdefinitions/read | Metrische definities lezen |
> | Microsoft.Insights/metrics/read | De metrische gegevens lezen |
> | Microsoft.Insights/alertRules/read | Een klassieke waarschuwing voor metrische gegevens lezen |
> | Microsoft.Insights/diagnosticSettings/read | Een diagnostische instelling voor de resource lezen |
> | Microsoft.Insights/logDefinitions/read | Logboekdefinities lezen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
> | Microsoft.Resources/subscriptions/operationresults/read | Hiermee worden de resultaten van de abonnementsbewerking opgehaald. |
> | Microsoft.Resources/subscriptions/read | Hiermee kunt u de lijst met abonnementen ophalen. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cosmos-db-account-reader-role"></a>Rol van lezer voor Cosmos DB-account
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan Azure Cosmos DB-accountgegevens lezen. Zie [Inzender voor DocumentDB-Account](#documentdb-account-contributor) voor het beheren van Azure Cosmos DB-accounts. |
> | **Id** | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen zijn toegewezen, kan machtigingen die aan elke gebruiker lezen |
> | Microsoft.DocumentDB/*/read | Een verzameling lezen |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Leest de database alleen-lezen-accountsleutels. |
> | Microsoft.Insights/MetricDefinitions/read | Metrische definities lezen |
> | Microsoft.Insights/Metrics/read | De metrische gegevens lezen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="data-box-contributor"></a>Inzender voor Data Box
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u alles onder de Data Box-Service, behalve het verlenen van toegang aan anderen. |
> | **Id** | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Databox/* |  |

## <a name="data-box-reader"></a>Data Box-lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | U kunt Data Box-Service met uitzondering van het maken van de volgorde of ordergegevens bewerken en toegang geven tot andere beheren. |
> | **Id** | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Databox/*/read |  |
> | Microsoft.Databox/jobs/listsecrets/action | Hiermee wordt een lijst gemaakt van niet-versleutelde geheimen die betrekking hebben op de order. |
> | Microsoft.Databox/locations/availableSkus/action | Met deze methode wordt de lijst met beschikbare SKU's geretourneerd. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="data-factory-contributor"></a>Inzender Data Factory
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u data factory's, maar kunt u niet de toegang tot de data factory's beheren. |
> | **Id** | 673868aa-7521-48A0-acc6-0f60742d39f5 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.DataFactory/dataFactories/* | Maken en beheren van data factory's en onderliggende resources hierin. |
> | Microsoft.DataFactory/factories/* | Maken en beheren van data factory's en onderliggende resources hierin. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="data-lake-analytics-developer"></a>Ontwikkelaar voor Data Lake Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u uw eigen taken indienen, controleren en beheren, maar geen Data Lake Analytics-accounts maken of verwijderen. |
> | **Id** | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.BigAnalytics/accounts/* |  |
> | Microsoft.DataLakeAnalytics/accounts/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **notActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | Een account DataLakeAnalytics verwijderen. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Machtigingen verlenen voor het annuleren van de taken die worden ingediend door andere gebruikers. |
> | Microsoft.DataLakeAnalytics/accounts/Write | Maken of bijwerken van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Maken of bijwerken van een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Een DataLakeStore-account loskoppelen van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Maken of bijwerken van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Een Storage-account loskoppelen van een DataLakeAnalytics-account. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Maken of bijwerken van een firewallregel. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Een firewallregel verwijderen. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Maken of bijwerken van een compute-beleid. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Een compute-beleid niet verwijderen. |

## <a name="data-purger"></a>Gegevensverwijderaar
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan analytische gegevens verwijderen |
> | **Id** | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | **Acties** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/purge/action | Gegevens uit Application Insights leegmaken |
> | Microsoft.OperationalInsights/workspaces/*/read |  |
> | Microsoft.OperationalInsights/workspaces/purge/action | Opgegeven gegevens uit de werkruimte verwijderen |

## <a name="devtest-labs-user"></a>DevTest Labs-gebruiker
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u verbinding kunt maken, starten, opnieuw opstarten en afsluiten van virtuele machines in Azure DevTest Labs. |
> | **Id** | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Compute/availabilitySets/read | Hiermee worden de eigenschappen van een beschikbaarheidsset opgehaald |
> | Microsoft.Compute/virtualMachines/*/read | Lezen van de eigenschappen van een virtuele machine (VM-grootten, runtimestatus, VM-extensies, enz.) |
> | Microsoft.Compute/virtualMachines/deallocate/action | Hiermee wordt de virtuele machine uitgeschakeld en worden de rekenresources vrijgegeven |
> | Microsoft.Compute/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine opgehaald |
> | Microsoft.Compute/virtualMachines/restart/action | Hiermee wordt de virtuele machine opnieuw gestart |
> | Microsoft.Compute/virtualMachines/start/action | Hiermee wordt de virtuele machine gestart |
> | Microsoft.DevTestLab/*/read | De eigenschappen van een lab lezen |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Virtuele machines in een lab maken. |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Claim een willekeurige claimbare virtuele machine in het lab. |
> | Microsoft.DevTestLab/labs/formulas/delete | Verwijderen van formules. |
> | Microsoft.DevTestLab/labs/formulas/read | Formules worden gelezen. |
> | Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen van formules. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Lab-beleid evalueert. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Eigenaar worden van een bestaande virtuele machine |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een load balancer-back-end-adresgroep |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel |
> | Microsoft.Network/networkInterfaces/*/read | Lezen van de eigenschappen van een netwerkinterface (bijvoorbeeld alle load balancers die de netwerkinterface een deel van is) |
> | Microsoft.Network/networkInterfaces/join/action | Lid wordt van een virtuele Machine aan een netwerkinterface |
> | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Microsoft.Network/networkInterfaces/write | Hiermee maakt u een netwerkinterface gemaakt of bijgewerkt van een bestaande netwerkinterface.  |
> | Microsoft.Network/publicIPAddresses/*/read | Lezen van de eigenschappen van een openbaar IP-adres |
> | Microsoft.Network/publicIPAddresses/join/action | Lid wordt van een openbaar ip-adres |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk |
> | Microsoft.Resources/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
> | Microsoft.Resources/deployments/read | Hiermee kunt u implementaties ophalen of opnemen in een lijst. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | **notActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Hiermee wordt een lijst weergegeven met de beschikbare grootten waarnaar de virtuele machine kan worden bijgewerkt |

## <a name="dns-zone-contributor"></a>Inzender voor DNS-zone
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u DNS-zones en recordsets beheren in Azure DNS, maar kunt u niet bepalen wie toegang heeft. |
> | **Id** | befefa01-2a29-4197-83a8-272ff33ce314 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/dnsZones/* | DNS-zones en records maken en beheren |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="documentdb-account-contributor"></a>Inzender voor het DocumentDB-account
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan Azure Cosmos DB-accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB. |
> | **Id** | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.DocumentDb/databaseAccounts/* | Maken en beheren van Azure Cosmos DB-accounts |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="intelligent-systems-account-contributor"></a>Inzender voor ISS-accounts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u Intelligent Systems-accounts, maar kunt u niet de toegang tot de accounts beheren. |
> | **Id** | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.IntelligentSystems/accounts/* | Bouw intelligente systemen rekeningen maken en beheren |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="key-vault-contributor"></a>Inzender voor Key Vault
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u sleutelkluizen beheren, maar niet de toegang hiertoe. |
> | **Id** | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **notActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/purge/action | Een voorlopig verwijderde sluitelkluis leegmaken |
> | Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>Labmaker
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheerde labs maken, beheren en verwijderen in uw Azure Lab-accounts. |
> | **Id** | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.LabServices/labAccounts/*/read |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Een lab maken in een lab-account. |
> | Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action |  |
> | Microsoft.LabServices/labAccounts/getRegionalAvailability/action | Informatie over de regionale beschikbaarheid voor elke grootte categorie geconfigureerd onder een lab-account ophalen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="log-analytics-contributor"></a>Inzender van Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Inzender van log Analytics kan alle controlegegevens lezen en bewerken van instellingen voor controle. Bewerken van instellingen voor controle houdt het toevoegen van de VM-extensie voor virtuele machines; lezen van opslagaccountsleutels om te kunnen verzamelen van Logboeken van Azure Storage; configureren het maken en configureren van Automation-accounts; toevoegen van oplossingen en Azure diagnostics configureren op alle Azure-resources. |
> | **Id** | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/* | Gemaakt, bijgewerkt of de diagnostische instelling voor analyseserver lezen |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="log-analytics-reader"></a>Lezer van Log Analytics
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | De lezer van Log Analytics kan alle controlegegevens weergeven en doorzoeken en de controle-instellingen weergeven, inclusief het weergeven van de configuratie van Azure Diagnostics op alle Azure-resources. |
> | **Id** | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.OperationalInsights/workspaces/analytics/query/action | Zoeken met de nieuwe engine. |
> | Microsoft.OperationalInsights/workspaces/search/action | Een zoekquery uitgevoerd |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **notActions** |  |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |

## <a name="logic-app-contributor"></a>Logische app-bijdrager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u logische apps beheren, maar niet de toegang hiertoe. |
> | **Id** | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
> | Microsoft.ClassicStorage/storageAccounts/read | Opslagaccount met het opgegeven account wordt geretourneerd. |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/* | Gemaakt, bijgewerkt of de diagnostische instelling voor analyseserver lezen |
> | Microsoft.Insights/logdefinitions/* | Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Logboekcategorieën lijst in het activiteitenlogboek. |
> | Microsoft.Insights/metricDefinitions/* | Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
> | Microsoft.Logic/* | Logic Apps-resources beheert. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/operationresults/read | Hiermee worden de resultaten van de abonnementsbewerking opgehaald. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/connectionGateways/* | Maken en beheren van een Gateway-verbinding. |
> | Microsoft.Web/connections/* | Maken en beheren van een verbinding. |
> | Microsoft.Web/customApis/* | Maakt en beheert een aangepaste API. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Hiermee worden de eigenschappen van een App Service-Plan |
> | Microsoft.Web/sites/functions/listSecrets/action | Lijst met geheimen Web Apps-functies. |

## <a name="logic-app-operator"></a>Logische app-operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u logische apps lezen, inschakelen en uitschakelen. |
> | **Id** | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/*/read | Lezen Insights-waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/*/read | Diagnostische instellingen voor logische Apps worden opgehaald |
> | Microsoft.Insights/metricDefinitions/*/read | Hiermee haalt de beschikbare metrische gegevens voor Logic Apps. |
> | Microsoft.Logic/*/read | Hiermee leest u Logic Apps-resources. |
> | Microsoft.Logic/workflows/disable/action | Hiermee wordt de werkstroom uitgeschakeld. |
> | Microsoft.Logic/workflows/enable/action | Hiermee wordt de werkstroom ingeschakeld. |
> | Microsoft.Logic/workflows/validate/action | Hiermee wordt de werkstroom gevalideerd. |
> | Microsoft.Resources/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
> | Microsoft.Resources/subscriptions/operationresults/read | Hiermee worden de resultaten van de abonnementsbewerking opgehaald. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/connectionGateways/*/read | Lees de Gateways verbinding. |
> | Microsoft.Web/connections/*/read | Verbindingen lezen. |
> | Microsoft.Web/customApis/*/read | Lees de aangepaste API. |
> | Microsoft.Web/serverFarms/read | Hiermee worden de eigenschappen van een App Service-Plan |

## <a name="managed-application-operator-role"></a>De Operatorrol beheerde toepassing
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lees- en acties uitvoeren op resources van beheerde toepassingen |
> | **Id** | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | **Acties** |  |
> | Microsoft.Solutions/applications/read | Hiermee wordt een lijst met toepassingen opgehaald. |

## <a name="managed-applications-reader"></a>Beheerde toepassingen-lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u lezen van resources in een beheerde app en de aanvraag voor JIT-toegang. |
> | **Id** | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Solutions/jitRequests/* |  |

## <a name="managed-identity-contributor"></a>Inzender beheerde identiteit
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Door gebruiker toegewezen identiteit maken, lezen, bijwerken en verwijderen |
> | **Id** | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | **Acties** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="managed-identity-operator"></a>Operator beheerde identiteit
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Door gebruiker toegewezen identiteit maken en toewijzen |
> | **Id** | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Acties** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="management-group-contributor"></a>Inzender beheergroep
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | De rol Inzender beheergroep |
> | **Id** | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | **Acties** |  |
> | Microsoft.Management/managementGroups/delete | Beheergroep verwijderen. |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | Microsoft.Management/managementGroups/subscriptions/delete | Hiermee koppelt u abonnement uit de beheergroep ongedaan maken. |
> | Microsoft.Management/managementGroups/subscriptions/write | Collega's het bestaande abonnement met de beheergroep. |
> | Microsoft.Management/managementGroups/write | Maken of bijwerken van een beheergroep. |

## <a name="management-group-reader"></a>Lezer beheergroep
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Rol Lezer beheergroep |
> | **Id** | ac63b705-f282-497d-ac71-919bf39d939d |
> | **Acties** |  |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |

## <a name="monitoring-contributor"></a>Controlebijdrager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan alle controlegegevens lezen en bewerken van instellingen voor controle. Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/AlertRules/* | Waarschuwingsregels voor lezen/schrijven/verwijderen. |
> | Microsoft.Insights/components/* | Onderdelen van de Application Insights lezen, schrijven en verwijderen. |
> | Microsoft.Insights/DiagnosticSettings/* | Diagnostische instellingen lezen, schrijven en verwijderen. |
> | Microsoft.Insights/eventtypes/* | Lijst met gebeurtenissen in activiteitenlogboeken (van Beheergebeurtenissen) in een abonnement. Deze machtiging is van toepassing op zowel programmatische als portal toegang tot het activiteitenlogboek. |
> | Microsoft.Insights/LogDefinitions/* | Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Logboekcategorieën lijst in het activiteitenlogboek. |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/MetricDefinitions/* | Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
> | Microsoft.Insights/Metrics/* | De metrische gegevens voor een resource lezen. |
> | Microsoft.Insights/Register/Action | De Microsoft Insights-provider registreren |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.Insights/webtests/* | Lezen/schrijven/verwijderen Application Insights-webtests. |
> | Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Oplossingspakketten voor lezen/schrijven/verwijderen Log Analytics. |
> | Microsoft.OperationalInsights/workspaces/savedSearches/* | Lezen, schrijven en verwijderen met Log Analytics opgeslagen zoekopdrachten. |
> | Microsoft.OperationalInsights/workspaces/search/action | Een zoekquery uitgevoerd |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt om agents van Microsoft Operational Insights verbinding naar de werkruimte te maken. |
> | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lezen/schrijven/verwijderen Log Analytics insight opslagconfiguraties. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.WorkloadMonitor/monitors/* |  |
> | Microsoft.WorkloadMonitor/notificationSettings/* |  |
> | Microsoft.WorkloadMonitor/workloadInsights/* |  |

## <a name="monitoring-metrics-publisher"></a>Uitgever van de metrische gegevens controleren
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee schakelt het publiceren van metrische gegevens bij Azure-resources |
> | **Id** | 3913510d-42f4-4E42-8a64-420c390055eb |
> | **Acties** |  |
> | Microsoft.Insights/Register/Action | De Microsoft Insights-provider registreren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | **DataActions** |  |
> | Microsoft.Insights/Metrics/Write | Metrische gegevens schrijven |

## <a name="monitoring-reader"></a>Controlelezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kan alle controlegegevens lezen (metrische gegevens, Logboeken, enz.). Zie ook [aan de slag met rollen, machtigingen en beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
> | **Id** | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.OperationalInsights/workspaces/search/action | Een zoekquery uitgevoerd |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="network-contributor"></a>Inzender voor netwerken
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u netwerken, maar kunt u niet de toegang tot de netwerken beheren. |
> | **Id** | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/* | Maken en beheren van netwerken |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="new-relic-apm-account-contributor"></a>Inzender voor het New Relic APM-account
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u New Relic Application Performance Management-accounts en -toepassingen, maar kunt u niet de toegang tot de accounts en toepassingen beheren. |
> | **Id** | 5d28c62d-5b37-4476-8438-e587778df237 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | NewRelic.APM/accounts/* |  |

## <a name="reader-and-data-access"></a>Lezer- en gegevenstoegang
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Alles weergeven, maar kunt u verwijderen of te maken van een storage-account of een ingesloten resource niet. Er kunnen ook toegang tot alle gegevens in een opslagaccount verleend via toegang tot opslagaccountsleutels lezen/schrijven. |
> | **Id** | c12c1c16-33A1-487b-954d-41c89c60f349 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |

## <a name="redis-cache-contributor"></a>Inzender voor Redis-caches
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u exemplaren van Redis-cache, maar kunt u niet de toegang tot de exemplaren beheren. |
> | **Id** | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Cache/redis/* | Maken en beheren van Redis-caches |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="resource-policy-contributor-preview"></a>Inzender voor resourcebeleid (preview-versie)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | (Preview) Gebruikers gevuld vanuit EA, met rechten voor het maken/wijzigen voor resourcebeleid, supportticket maken en het lezen van resources/hiërarchie. |
> | **Id** | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Authorization/policyassignments/* | Maken en beheren van de toewijzingen van beleid |
> | Microsoft.Authorization/policydefinitions/* | Maken en beheren van beleidsdefinities |
> | Microsoft.Authorization/policysetdefinitions/* | Maken en beheren van beleid instellen |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="scheduler-job-collections-contributor"></a>Inzender voor Scheduler-taakverzamelingen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u Scheduler-taakverzamelingen, maar kunt u niet de toegang tot de verzamelingen beheren. |
> | **Id** | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Scheduler/jobcollections/* | Maken en beheren van taakcollecties |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="search-service-contributor"></a>Inzender voor Search-services
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u Search-services, maar kunt u niet de toegang tot de services beheren. |
> | **Id** | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Search/searchServices/* | Search-services maken en beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="security-admin"></a>Beveiligingsbeheerder
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | In Security Center alleen: kunt weergeven beveiligingsbeleid, security status weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, waarschuwingen en aanbevelingen negeren |
> | **Id** | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Authorization/policyAssignments/* | Maken en beheren van de toewijzingen van beleid |
> | Microsoft.Authorization/policyDefinitions/* | Maken en beheren van beleidsdefinities |
> | Microsoft.Authorization/policySetDefinitions/* | Maken en beheren van beleid instellen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |
> | Microsoft.operationalInsights/workspaces/*/read | Log Analytics-gegevens weergeven |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Security/*/read | Onderdelen van de veiligheid lezen en het beleid |
> | Microsoft.Security/locations/alerts/activate/action | Een beveiligingswaarschuwing geactiveerd |
> | Microsoft.Security/locations/alerts/dismiss/action | Een beveiligingswaarschuwing negeren |
> | Microsoft.Security/locations/tasks/activate/action | Een beveiligingsaanbeveling activeren |
> | Microsoft.Security/locations/tasks/dismiss/action | Een beveiligingsaanbeveling negeren |
> | Microsoft.Security/policies/write | Updates van het beveiligingsbeleid |
> | Microsoft.Security/securityContacts/write | Updates van de contactpersoon voor beveiliging |
> | Microsoft.Security/securityContacts/delete | Hiermee verwijdert u de contactpersoon voor beveiliging |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="security-manager"></a>Beveiligingsbeheer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u beveiligingsonderdelen, beveiligingsbeleidsregels en virtuele machines |
> | **Id** | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.ClassicCompute/*/read | Configuratie-informatie lezen klassieke virtuele machines |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Configuratie voor klassieke virtuele machines schrijven |
> | Microsoft.ClassicNetwork/*/read | Configuratie-informatie lezen over klassieke netwerk |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Security/* | Security-onderdelen en -beleid maken en beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="security-reader"></a>Beveiligingslezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | In Security Center alleen: aanbevelingen en waarschuwingen, weergave beveiligingsbeleid van de status van de beveiliging weergeven, maar kan geen wijzigingen aanbrengen kunt weergeven |
> | **Id** | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.operationalInsights/workspaces/*/read | Log Analytics-gegevens weergeven |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Security/*/read | Onderdelen van de veiligheid lezen en het beleid |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Management/managementGroups/read | Lijst met beheergroepen omwille van de geverifieerde gebruiker. |

## <a name="site-recovery-contributor"></a>Site Recovery-inzender
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u de Site Recovery-service beheren, maar geen kluizen maken of rollen toewijzen |
> | **Id** | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Resourcecertificaat bijwerken de bron-/ kluisreferentiecertificaat bijgewerkt. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie met betrekking tot de kluis |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van geregistreerde identiteiten |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Maken of bijwerken van de replicatie-instellingen voor waarschuwingen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Maken en replicatie-infrastructuren beheren |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Replicatietaken maken en beheren |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Replicatiebeleid maken en beheren |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Maken en beheren van herstelplannen |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Maken en beheren van configuratie van de opslag van Recovery Services-kluis |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Retourneert de tokengegevens voor Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking Kluistoken kan worden gebruikt om op te halen Kluistoken voor kluis op back-end-bewerkingen. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="site-recovery-operator"></a>Site Recovery-operator
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u failover en fallback uitvoeren, maar geen andere beheerbewerkingen voor Site Recovery |
> | **Id** | 494ae006-db33-4328-bf46-533a6560a3ca |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lezen van alle instellingen voor waarschuwingen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Hiermee wordt de consistentie van de infrastructuur gecontroleerd |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Gateway opnieuw koppelen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Certificaat vernieuwen voor de infrastructuur |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Alle netwerken lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Netwerktoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Lezen van alle Beveiligingscontainers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare Items lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Herstelpunt toepassen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover doorvoeren |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplande Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lezen van alle beveiligde Items |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lezen van alle herstelpunten voor replicatie |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Replicatie herstellen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Beveiligd Item opnieuw beveiligen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Failover testen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Failovertest opschonen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobility-Service bijwerken |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle Beveiligingscontainertoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lees een Recovery Services-Providers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Provider vernieuwen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lees elk Opslagclassificatietoewijzingen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Alle vCenters lezen |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Replicatietaken maken en beheren |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Elk beleid lezen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Herstelplan voor failover doorvoeren |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Herstelplan voor geplande Failover |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lees elk plannen voor herstel |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Herstelplan voor opnieuw beveiligen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Herstelplan voor Failover testen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Plan voor herstel van test voor de failovertest |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Herstelplan voor failover |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Retourneert de tokengegevens voor Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking Kluistoken kan worden gebruikt om op te halen Kluistoken voor kluis op back-end-bewerkingen. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="site-recovery-reader"></a>Site Recovery-lezer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u de Site Recovery-status weergeven maar geen andere beheerbewerkingen voor Site Recovery uitvoeren |
> | **Id** | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resultaten bewerking kan worden gebruikt ophalen de bewerkingsstatus en het resultaat van de asynchroon ingediende bewerking |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen op de bewerking kan worden gebruikt. Haal de containers die zijn geregistreerd voor een resource. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Lezen van alle instellingen voor waarschuwingen |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Alle netwerken lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Netwerktoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Lezen van alle Beveiligingscontainers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare Items lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Lezen van alle beveiligde Items |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lezen van alle herstelpunten voor replicatie |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle Beveiligingscontainertoewijzingen lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Lees een Recovery Services-Providers |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Lees elk Opslagclassificatietoewijzingen |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Alle vCenters lezen |
> | Microsoft.RecoveryServices/vaults/replicationJobs/read | Alle taken lezen |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Elk beleid lezen |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Lees elk plannen voor herstel |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read | Retourneert de tokengegevens voor Recovery Services-kluis. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | De bewerking Kluistoken kan worden gebruikt om op te halen Kluistoken voor kluis op back-end-bewerkingen. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="sql-db-contributor"></a>Inzender voor SQL-databases
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Kunt u SQL-databases, maar niet de toegang tot beheren. U beheren niet ook hun beveiligingsbeleid of de bovenliggende SQL-servers. |
> | **Id** | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en rollen toewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/databases/* | SQL-databases maken en beheren |
> | Microsoft.Sql/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **notActions** |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Auditbeleid kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Controle-instellingen bewerken niet |
> | Microsoft.Sql/servers/databases/auditRecords/read | De controlerecords voor database-blob ophalen |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Kan geen verbinding beleidsregels kunt bewerken |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Gegevens maskeren beleidsregels kunt bewerken |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing-beveiligingsbeleid kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Over de beveiliging kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>SQL-beveiligingsbeheer
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beveiligingsbeleid van SQL-servers en -databases beheren, maar niet de toegang tot het beleid. |
> | **Id** | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Alleen Microsoft-authorisatie |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource, zoals storage-account of SQL-database koppelt naar een subnet. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server audit beleid maken en beheren |
> | Microsoft.Sql/servers/auditingSettings/* | Maken en beheren van SQL server-controle-instellingen |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Maken en beheren van controlebeleid van de SQL server-database |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Maken en beheren van SQL server-database controle-instellingen |
> | Microsoft.Sql/servers/databases/auditRecords/read | Controlerecords lezen |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server-database verbinding beleid maken en beheren |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Maken en beheren van SQL server-databasegegevens maskeren beleid |
> | Microsoft.Sql/servers/databases/read | Retourneert de lijst met databases of haalt de eigenschappen voor de opgegeven database. |
> | Microsoft.Sql/servers/databases/schemas/read | Lijst van schema's van een database ophalen |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Lijst met kolommen van een tabel ophalen |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Lijst met tabellen van een database ophalen |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Maken en beheren van waarschuwingen beveiligingsbeleid van SQL server-database |
> | Microsoft.Sql/servers/databases/securityMetrics/* | Maken en beheren van SQL server-database security metrische gegevens |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Retourneert de lijst met servers of haalt de eigenschappen voor de opgegeven server. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL server security waarschuwing beleid maken en beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="sql-server-contributor"></a>Inzender voor SQL Server
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u SQL-servers en -databases beheren, maar niet de toegang tot en het beveiligingsbeleid van de servers en databases. |
> | **Id** | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Sql/locations/*/read |  |
> | Microsoft.Sql/servers/* | Maken en beheren van de SQL-servers |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | **notActions** |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL server audit beleid kan niet worden bewerkt. |
> | Microsoft.Sql/servers/auditingSettings/* | Instellingen voor SQL server audit kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Controlebeleid van de SQL server-database kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Kan geen SQL server-database controle-instellingen bewerken |
> | Microsoft.Sql/servers/databases/auditRecords/read | Kan de controlerecords niet lezen. |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Beleidsregels voor SQL server-database kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server-databasegegevens maskeren beleid kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid van SQL server-database kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/securityMetrics/* | SQL server-database over de beveiliging kan niet worden bewerkt. |
> | Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
> | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | Waarschuwing beleidsregels voor de beveiliging van SQL server kan niet worden bewerkt. |

## <a name="storage-account-contributor"></a>Inzender voor opslagaccounts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u opslagaccounts beheren, maar niet de toegang. |
> | **Id** | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Alle autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/diagnosticSettings/* | Diagnostische instellingen beheren |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource, zoals storage-account of SQL-database koppelt naar een subnet. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Storage/storageAccounts/* | Maken en storage-accounts beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="storage-account-key-operator-service-role"></a>De servicerol Sleuteloperator voor opslagaccounts
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Sleuteloperators voor opslagaccounts kunnen een lijst met sleutels voor opslagaccounts maken en de sleutels opnieuw genereren |
> | **Id** | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Hiermee genereert u de toegangssleutels voor het opgegeven opslagaccount opnieuw. |

## <a name="storage-blob-data-contributor-preview"></a>Gegevensbijdrager voor opslagblob (preview-versie)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee wordt lees-, schrijf- en verwijdertoegang tot Azure Storage-blobcontainers en -gegevens verleend. |
> | **Id** | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Hiermee wordt het resultaat van het verwijderen van een container geretourneerd |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Hiermee wordt een container of een lijst containers geretourneerd |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Hiermee wordt het resultaat van het plaatsen of in lease geven van de blobcontainer geretourneerd |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Hiermee wordt het resultaat van het verwijderen van een blob geretourneerd |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Hiermee wordt een blob of een lijst met blobs geretourneerd |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Hiermee wordt het resultaat van het schrijven van een blob geretourneerd |

## <a name="storage-blob-data-reader-preview"></a>Gegevenslezer voor opslagblob (preview-versie)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee wordt leestoegang tot Azure Storage-blobcontainers en -gegevens verleend. |
> | **Id** | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Hiermee wordt een container of een lijst containers geretourneerd |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Hiermee wordt een blob of een lijst met blobs geretourneerd |

## <a name="storage-queue-data-contributor-preview"></a>Gegevensbijdrager voor opslagwachtrij (preview-versie)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee wordt lees-, schrijf- en verwijdertoegang tot Azure Storage-wachtrijen en -wachtrijberichten verleend. |
> | **Id** | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Hiermee wordt het resultaat van een wachtrij verwijderen geretourneerd |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Hiermee wordt een wachtrij of een lijst met wachtrijen geretourneerd. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | Hiermee wordt het resultaat van het schrijven van een wachtrij geretourneerd |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Hiermee wordt het resultaat van een bericht verwijderen geretourneerd |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Hiermee wordt een bericht geretourneerd |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Hiermee wordt het resultaat van het schrijven van een bericht geretourneerd |

## <a name="storage-queue-data-reader-preview"></a>Gegevenslezer voor opslagwachtrij (preview-versie)
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee wordt leestoegang verleend voor Azure Storage-wachtrijen en -wachtrijberichten. |
> | **Id** | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Acties** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Hiermee wordt een wachtrij of een lijst met wachtrijen geretourneerd. |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Hiermee wordt een bericht geretourneerd |

## <a name="support-request-contributor"></a>Inzender voor ondersteuningsaanvragen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u ondersteuningsaanvragen maken en beheren |
> | **Id** | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="traffic-manager-contributor"></a>Inzender voor Traffic Manager
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u Traffic Manager-profielen beheren, maar kunt u niet bepalen wie toegang heeft. |
> | **Id** | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Meer functies en roltoewijzingen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="user-access-administrator"></a>Beheerder van gebruikerstoegang
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u de gebruikerstoegang tot Azure-resources. |
> | **Id** | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **Acties** |  |
> | * / lezen | Bronnen van alle typen, met uitzondering van geheimen worden gelezen. |
> | Microsoft.Authorization/* | Machtigingen beheren |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="virtual-machine-administrator-login"></a>Beheerdersaanmelding bij virtuele Machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Virtuele Machines weergeven in de portal en meld u aan als beheerder |
> | **Id** | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | **Acties** |  |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/loadBalancers/read | De definitie van een load balancer opgehaald |
> | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Als een gewone gebruiker aanmelden bij een virtuele machine |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Aanmelden bij een virtuele machine met Windows-beheerders- of Linux-rootgebruikersbevoegdheden |

## <a name="virtual-machine-contributor"></a>Inzender voor virtuele machines
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee kunt u beheren van virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of storage-account die zijn gekoppeld. |
> | **Id** | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Compute/availabilitySets/* | Maken en beheren van compute-beschikbaarheidssets |
> | Microsoft.Compute/locations/* | Maken en beheren van compute-locaties |
> | Microsoft.Compute/virtualMachines/* | Virtuele machines maken en beheren |
> | Microsoft.Compute/virtualMachineScaleSets/* | Maken en beheren van virtuele-machineschaalsets |
> | Microsoft.DevTestLab/schedules/* |  |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Lid wordt van een application gateway back-end-adresgroep |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een load balancer-back-end-adresgroep |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Lid wordt van een load balancer binnenkomende nat-pool |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel |
> | Microsoft.Network/loadBalancers/probes/join/action | U kunt met behulp van tests van een load balancer. Bijvoorbeeld: met deze machtiging Statustest-eigenschap van de VM-schaalset set kan verwijzen naar de test. |
> | Microsoft.Network/loadBalancers/read | De definitie van een load balancer opgehaald |
> | Microsoft.Network/locations/* | Maken en beheren van netwerklocaties |
> | Microsoft.Network/networkInterfaces/* | Maken en beheren van netwerkinterfaces |
> | Microsoft.Network/networkSecurityGroups/join/action | Lid wordt van een netwerkbeveiligingsgroep |
> | Microsoft.Network/networkSecurityGroups/read | De definitie van een network security opgehaald |
> | Microsoft.Network/publicIPAddresses/join/action | Lid wordt van een openbaar ip-adres |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Een back-Upbeveiligingsintentie maken |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Retourneert de objectgegevens van het beveiligde Item |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een beveiligd back-upitem maken |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beveiligingsbeleid voor apps |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Hiermee maakt u beleid voor beveiliging |
> | Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen wordt een object waarmee de Azure-resource van het type 'kluis' |
> | Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
> | Microsoft.RecoveryServices/Vaults/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.SqlVirtualMachine/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
> | Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="virtual-machine-user-login"></a>Gebruikersaanmelding bij virtuele machine
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Virtuele Machines in de portal en meld u aan als een gewone gebruiker weergeven. |
> | **Id** | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Acties** |  |
> | Microsoft.Network/publicIPAddresses/read | Hiermee haalt u de definitie van een openbaar IP-adres. |
> | Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
> | Microsoft.Network/loadBalancers/read | De definitie van een load balancer opgehaald |
> | Microsoft.Network/networkInterfaces/read | Hiermee haalt u de definitie van een netwerk-interface.  |
> | Microsoft.Compute/virtualMachines/*/read |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Als een gewone gebruiker aanmelden bij een virtuele machine |

## <a name="web-plan-contributor"></a>Inzender voor webabonnementen
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u de webabonnementen voor websites, maar kunt u niet de toegang tot de abonnementen beheren. |
> | **Id** | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/serverFarms/* | Maken en beheren van server-farms |

## <a name="website-contributor"></a>Inzender voor websites
> [!div class="mx-tableFixed"]
> | | |
> | --- | --- |
> | **Beschrijving** | Hiermee beheert u websites (niet webabonnementen), maar kunt u niet de toegang tot de websites beheren. |
> | **Id** | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Acties** |  |
> | Microsoft.Authorization/*/read | Autorisatie lezen |
> | Microsoft.Insights/alertRules/* | Maken en beheren van inzicht waarschuwingsregels |
> | Microsoft.Insights/components/* | Maken en beheren van inzicht onderdelen |
> | Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
> | Microsoft.Resources/deployments/* | Maken en beheren van brongroepimplementaties |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
> | Microsoft.Support/* | Maken en ondersteuningstickets beheren |
> | Microsoft.Web/certificates/* | Maken en beheren van Websitecertificaten |
> | Microsoft.Web/listSitesAssignedToHostName/read | Namen van sites die zijn toegewezen aan de hostnaam ophalen. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/read | Hiermee worden de eigenschappen van een App Service-Plan |
> | Microsoft.Web/sites/* | Websites (het maken van site ook moeten schrijfmachtigingen voor de bijbehorende App Service-Plan) maken en beheren |

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste rollen](custom-roles.md)
- [Met behulp van de Azure portal-roltoewijzingen beheren](role-assignments-portal.md)
- [Machtigingen in Azure Security Center](../security-center/security-center-permissions.md)
