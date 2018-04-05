---
title: Ingebouwde functies voor Azure op rollen gebaseerde toegangsbeheer (RBAC) | Microsoft Docs
description: Dit onderwerp beschrijft de ingebouwde rollen voor op rollen gebaseerde toegangsbeheer (RBAC). De rollen worden continu toegevoegd, dus Controleer de nieuwheid documentatie.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 03/06/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: it-pro
ms.openlocfilehash: 4d9df6743d84310b7db70034d1e84dd3591b3c21
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure
Azure op rollen gebaseerde toegangsbeheer (RBAC) beschikt over de volgende ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en services. U kunt de definities van de ingebouwde rollen niet wijzigen. U kunt echter maken [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) aanpassen aan de specifieke behoeften van uw organisatie.

## <a name="built-in-role-descriptions"></a>Beschrijvingen van de ingebouwde functies
De volgende tabel bevat een korte beschrijving van de ingebouwde rollen. Klik op de naam van de rol voor een gedetailleerd overzicht van **acties** en **notactions** voor de rol. De **acties** eigenschap geeft u de toegestane acties op Azure-resources. Actie tekenreeksen kunnen jokertekens gebruiken. De **notactions** eigenschap geeft u de acties die zijn uitgesloten van de toegestane acties.

De actie definieert wat voor soort bewerkingen die u kunt uitvoeren op een bepaald brontype. Bijvoorbeeld:
- **Schrijven** kunt u PUT, POST, PATCH en DELETE-bewerkingen uit te voeren.
- **Lees** kunt u GET-bewerkingen uitvoeren.

In dit artikel wordt alleen de verschillende rollen die vandaag bestaan. Wanneer u een rol aan een gebruiker toewijst, echter kunt u beperken de toegestane acties verder met het definiëren van een scope. Dit is handig als u ervoor iemand een medewerker van de Website, maar alleen voor één resourcegroep.

> [!NOTE]
> De Azure roldefinities zijn voortdurend in ontwikkeling. In dit artikel is actueel zo mogelijk, maar u vindt altijd de meest recente definities van de rollen in Azure PowerShell. Gebruik de [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) cmdlet voor een lijst met alle beschikbare rollen. U kunt meteen voor het gebruik van een specifieke rol `(get-azurermroledefinition "<role name>").actions` of `(get-azurermroledefinition "<role name>").notactions` indien van toepassing. Gebruik [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) activiteiten van de lijst van specifieke Azure-resourceproviders.


| Ingebouwde rol | Beschrijving |
| --- | --- |
| [Owner](#owner) | Hiermee beheert u alles, inclusief de toegang tot resources. |
| [Contributor](#contributor) | Hiermee beheert u alles, behalve de toegang tot resources. |
| [Reader](#reader) | Hiermee geeft u alles weer, maar kunt u niet wijzigingen aanbrengen. |
| [API Management-Service Inzender](#api-management-service-contributor) | Kan de service en de API's beheren |
| [Rol Operator API Management-Service](#api-management-service-operator-role) | Kan de service beheren, maar niet de API's |
| [Rol van API Management-Service lezer](#api-management-service-reader-role) | Alleen-lezentoegang tot de service en API's |
| [Application Insights-Onderdeelinzender](#application-insights-component-contributor) | Kan onderdelen van Application Insights beheren |
| [Application Insights momentopname-foutopsporingsprogramma](#application-insights-snapshot-debugger) | Hiermee geeft u de gebruiker de machtiging om de functies van Application Insights Snapshot Debugger te gebruiken |
| [Automation-taak Operator](#automation-job-operator) | Taken maken en beheren met Automation-runbooks. |
| [Automation-Operator](#automation-operator) | Operators voor Automation kunnen taken starten, stoppen, onderbreken en hervatten |
| [Automation-Runbook-Operator](#automation-runbook-operator) | Runbook-eigenschappen lezen: hiermee kunnen taken van de runbook worden gemaakt. |
| [De eigenaar van de Azure Stack-registratie](#azure-stack-registration-owner) | Hiermee kunt u Azure Stack-registraties beheren. |
| [Back-up Inzender](#backup-contributor) | Hiermee kunt u de back-upservice beheren, maar u kunt geen kluizen maken of anderen toegang verlenen |
| [Back-upoperator](#backup-operator) | Hiermee kunt u back-upservices beheren, met uitzondering van het verwijderen van back-ups, het maken van kluizen en het verlenen van toegang aan anderen |
| [Back-lezer](#backup-reader) | Kan de back-upservices weergeven, maar kan geen wijzigingen aanbrengen |
| [Lezer facturering](#billing-reader) | Hiermee wordt leestoegang gegeven tot factureringsgegevens |
| [BizTalk Inzender](#biztalk-contributor) | Hiermee beheert u BizTalk-services, maar kunt u niet de toegang tot de services beheren. |
| [CDN-eindpunt Inzender](#cdn-endpoint-contributor) | Kan CDN-eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [CDN-eindpunt lezer](#cdn-endpoint-reader) | Kan CDN-eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
| [CDN Profile Contributor](#cdn-profile-contributor) | Kan CDN-profielen en de bijbehorende eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [CDN-profiel lezer](#cdn-profile-reader) | Kan CDN-profielen en de bijbehorende eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
| [Inzender voor klassieke netwerken](#classic-network-contributor) | Hiermee beheert u klassieke netwerken, maar kunt u niet de toegang tot de netwerken beheren. |
| [Klassieke Storage Account Inzender](#classic-storage-account-contributor) | Hiermee kunt u klassieke opslagaccounts beheren, maar niet de toegang. |
| [Klassieke Storage-Account de Operator-functieservice](#classic-storage-account-key-operator-service-role) | Sleuteloperators voor klassieke opslagaccounts kunnen een lijst met sleutels voor klassieke opslagaccounts maken en de sleutels opnieuw genereren |
| [Klassieke Virtual Machine Contributor](#classic-virtual-machine-contributor) | U kunt klassieke virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of ze zijn verbonden met storage-account beheren. |
| [ClearDB MySQL DB Contributor](#cleardb-mysql-db-contributor) | Hiermee beheert u ClearDB MySQL-databases, maar kunt u niet de toegang tot de databases beheren. |
| [De rol van de lezer-Account cosmos DB](#cosmos-db-account-reader-role) | Gegevens van Azure DB die Cosmos-account kan worden gelezen. Zie [DocumentDB-Account Inzender](#documentdb-account-contributor) voor het beheren van Azure DB die Cosmos-accounts. |
| [Data Factory Inzender](#data-factory-contributor) | Data factory's en de onderliggende resources hierin maken en beheren. |
| [Data Lake Analytics-ontwikkelaars](#data-lake-analytics-developer) | Hiermee kunt u uw eigen taken indienen, controleren en beheren, maar geen Data Lake Analytics-accounts maken of verwijderen. |
| [DevTest Labs User](#devtest-labs-user) | Hiermee kunt u verbinding maken met virtuele machines in Azure DevTest Labs en de virtuele machines starten, opnieuw starten en afsluiten. |
| [DNS-Zone Inzender](#dns-zone-contributor) | Hiermee kunt u DNS-zones en recordsets beheren in Azure DNS, maar kunt u niet bepalen wie toegang heeft. |
| [DocumentDB-Account Inzender](#documentdb-account-contributor) | Kan Azure Cosmos DB accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB. |
| [Intelligente systemen Account Inzender](#intelligent-systems-account-contributor) | Hiermee beheert u Intelligent Systems-accounts, maar kunt u niet de toegang tot de accounts beheren. |
| [Sleutelkluis Inzender](#key-vault-contributor) | Hiermee kunt u sleutelkluizen beheren, maar niet de toegang hiertoe. |
| [Lab Creator](#lab-creator) | Hiermee kunt u maken, beheren en verwijderen van uw beheerde labs in uw Azure Lab-Accounts. |
| [Log Analytics Inzender](#log-analytics-contributor) | Log Analytics Inzender kunnen alle bewakingsgegevens lezen en bewerken van controle-instellingen. Controle-instellingen bewerken bevat de VM-extensie toevoegen aan virtuele machines; lezen van toegangscodes voor opslag om te kunnen verzamelen van Logboeken van Azure Storage; configureren maken en configureren van Automation-accounts; toevoegen van oplossingen; en het configureren van Azure diagnostics op alle Azure-resources. |
| [Log Analytics Reader](#log-analytics-reader) | De lezer van Log Analytics kan alle controlegegevens weergeven en doorzoeken en de controle-instellingen weergeven, inclusief het weergeven van de configuratie van Azure Diagnostics op alle Azure-resources. |
| [Logic App Inzender](#logic-app-contributor) | Hiermee kunt u logische apps beheren, maar niet de toegang hiertoe. |
| [Logic App-Operator](#logic-app-operator) | Hiermee kunt u logische apps lezen, inschakelen en uitschakelen. |
| [Beheerde identiteit Inzender](#managed-identity-contributor) | Maken, lezen, bijwerken en verwijderen van de gebruiker toegewezen identiteit |
| [Beheerde identiteit Operator](#managed-identity-operator) | Lees- en identiteit toegewezen gebruiker toewijzen |
| [Inzender bewaking](#monitoring-contributor) | Kan alle bewakingsgegevens lezen en controle-instellingen bewerken. Zie ook [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Bewaking van de lezer](#monitoring-reader) | Alle bewakingsgegevens (metrische gegevens, Logboeken, enz.) kunnen worden gelezen. Zie ook [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles). |
| [Inzender voor netwerken](#network-contributor) | Hiermee beheert u netwerken, maar kunt u niet de toegang tot de netwerken beheren. |
| [Nieuwe Relic APM-Account Inzender](#new-relic-apm-account-contributor) | Hiermee beheert u New Relic Application Performance Management-accounts en -toepassingen, maar kunt u niet de toegang tot de accounts en toepassingen beheren. |
| [Redis-Cache Inzender](#redis-cache-contributor) | Hiermee beheert u exemplaren van Redis-cache, maar kunt u niet de toegang tot de exemplaren beheren. |
| [Scheduler-taak verzamelingen Inzender](#scheduler-job-collections-contributor) | Hiermee beheert u Scheduler-taakverzamelingen, maar kunt u niet de toegang tot de verzamelingen beheren. |
| [Search Service Inzender](#search-service-contributor) | Hiermee beheert u Search-services, maar kunt u niet de toegang tot de services beheren. |
| [De beheerder beveiliging](#security-admin) | In Security Center alleen: kunt weergeven beveiligingsbeleid, beveiliging statussen weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen |
| [Security Manager](#security-manager) | Hiermee beheert u beveiligingsonderdelen, beveiligingsbeleidsregels en virtuele machines |
| [Beveiliging lezer](#security-reader) | In Security Center alleen: kunt aanbevelingen en waarschuwingen, weergave beveiligingsbeleid weergeven van beveiliging, maar geen wijzigingen aanbrengen weergeven |
| [Site Recovery Inzender](#site-recovery-contributor) | Hiermee kunt u de Site Recovery-service beheren, maar geen kluizen maken of rollen toewijzen |
| [Site Recovery-Operator](#site-recovery-operator) | Hiermee kunt u failover en fallback uitvoeren, maar geen andere beheerbewerkingen voor Site Recovery |
| [Site Recovery lezer](#site-recovery-reader) | Hiermee kunt u de Site Recovery-status weergeven maar geen andere beheerbewerkingen voor Site Recovery uitvoeren |
| [SQL DB Contributor](#sql-db-contributor) | Kunt u SQL-databases, maar niet de toegang tot beheren. U beheren niet ook hun beleid betrekking hebben op beveiliging of hun bovenliggende SQL-servers. |
| [SQL Security Manager](#sql-security-manager) | Hiermee kunt u beveiligingsbeleid van SQL-servers en -databases beheren, maar niet de toegang tot het beleid. |
| [SQL Server Contributor](#sql-server-contributor) | Hiermee kunt u SQL-servers en -databases beheren, maar niet de toegang tot en het beveiligingsbeleid van de servers en databases. |
| [Storage-Account Inzender](#storage-account-contributor) | Hiermee kunt u opslagaccounts beheren, maar niet de toegang. |
| [Storage-Account de Operator-functieservice](#storage-account-key-operator-service-role) | Sleuteloperators voor opslagaccounts kunnen een lijst met sleutels voor opslagaccounts maken en de sleutels opnieuw genereren |
| [Ondersteuning voor aanvraag Inzender](#support-request-contributor) | Hiermee kunt u ondersteuningsaanvragen maken en beheren |
| [Traffic Manager Contributor](#traffic-manager-contributor) | Hiermee kunt u Traffic Manager-profielen beheren, maar kunt u niet bepalen wie toegang heeft. |
| [Beheerder voor gebruikerstoegang](#user-access-administrator) | Hiermee beheert u de gebruikerstoegang tot Azure-resources. |
| [Aanmeldingsnaam van de beheerder van de virtuele Machine](#virtual-machine-administrator-login) | -Gebruikers met deze functie hebben de mogelijkheid om aan te melden met een virtuele machine met Windows-beheerder of gebruiker heeft bevoegdheden die Linux hoofdmap. |
| [Virtual Machine Contributor](#virtual-machine-contributor) | Kunt u virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of ze zijn verbonden met storage-account beheren. |
| [Virtuele Machine gebruikersaanmelding](#virtual-machine-user-login) | Gebruikers met deze functie hebben de mogelijkheid om aan te melden met een virtuele machine als een normale gebruiker. |
| [Web Plan Inzender](#web-plan-contributor) | Hiermee beheert u de webabonnementen voor websites, maar kunt u niet de toegang tot de abonnementen beheren. |
| [Website Inzender](#website-contributor) | Hiermee beheert u websites (niet webabonnementen), maar kunt u niet de toegang tot de websites beheren. |

De volgende tabellen beschrijven de specifieke machtigingen toegekend aan elke rol. Het kan hierbij gaan **acties**, die machtigingen geven en **NotActions**, die ze beperken.

## <a name="owner"></a>Eigenaar
Hiermee beheert u alles, inclusief de toegang tot resources.

| **Acties** |  |
| --- | --- |
| * | Maken en beheren van bronnen van alle typen |

## <a name="contributor"></a>Inzender
Hiermee beheert u alles, behalve de toegang tot resources.

| **Acties** |  |
| --- | --- |
| * | Maken en beheren van bronnen van alle typen |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete | Rollen en roltoewijzingen verwijderen niet |
| Microsoft.Authorization/*/Write | Kan de functies en roltoewijzingen niet maken |
| Microsoft.Authorization/elevateAccess/Action | Hiermee wordt oproepende functie de rechten van Administrator voor gebruikerstoegang gegeven voor het tenantbereik |

## <a name="reader"></a>Lezer
Hiermee geeft u alles weer, maar kunt u niet wijzigingen aanbrengen.

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |

## <a name="api-management-service-contributor"></a>Inzender voor API Management-services
Kan de service en de API's beheren

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/service/* | Maken en beheren van API Management-service |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="api-management-service-operator-role"></a>Operator-rol voor de API Management-service
Kan de service beheren, maar niet de API's

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Lees API Management-Service-exemplaren |
| Microsoft.ApiManagement/service/backup/action | Back-API Management-Service aan de opgegeven container in een gebruiker opgegeven storage-account |
| Microsoft.ApiManagement/service/delete | Verwijderen van exemplaar van API Management-Service |
| Microsoft.ApiManagement/service/managedeployments/action | SKU/eenheden toevoegen of verwijderen van regionale implementaties van API Management-Service wijzigen |
| Microsoft.ApiManagement/service/read | Lezen van metagegevens voor een exemplaar van API Management-Service |
| Microsoft.ApiManagement/service/restore/action | API Management-Service van de opgegeven container in het opgegeven opslagaccount van een gebruiker herstellen |
| Microsoft.ApiManagement/service/updatecertificate/action | SSL-certificaat uploaden voor een API Management-Service |
| Microsoft.ApiManagement/service/updatehostname/action | Setup, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service |
| Microsoft.ApiManagement/service/write | Maak een nieuw exemplaar van API Management-Service |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Lijst met gebruikerssleutels |

## <a name="api-management-service-reader-role"></a>Lezerrol voor de API Management-service
Alleen-lezentoegang tot de service en API's

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Lees API Management-Service-exemplaren |
| Microsoft.ApiManagement/service/read | Lezen van metagegevens voor een exemplaar van API Management-Service |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Lijst met gebruikerssleutels |

## <a name="application-insights-component-contributor"></a>Inzender voor Application Insights-onderdelen
Kan onderdelen van Application Insights beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Insights/components/* | Maken en beheren van Insights-onderdelen |
| Microsoft.Insights/webtests/* | Maken en beheren van webtests |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="application-insights-snapshot-debugger"></a>Application Insights Snapshot Debugger
Hiermee geeft u de gebruiker de machtiging om de functies van Application Insights Snapshot Debugger te gebruiken

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/components/*/read |  |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="automation-job-operator"></a>Operator voor Automation-taak
Taken maken en beheren met Automation-runbooks.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak hervatten |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Hiermee wordt een Azure Automation-taak gestopt |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Een Azure Automation-taak wordt onderbroken |
| Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="automation-operator"></a>Automation-operator
Operators voor Automation kunnen taken starten, stoppen, onderbreken en hervatten

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Automation/automationAccounts/jobs/read | Een Azure Automation-taak opgehaald |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Een Azure Automation-taak hervatten |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Hiermee wordt een Azure Automation-taak gestopt |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Een Azure Automation-taakstroom opgehaald |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Een Azure Automation-taak wordt onderbroken |
| Microsoft.Automation/automationAccounts/jobs/write | Hiermee maakt u een Azure Automation-taak |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Een Azure Automation-taakschema opgehaald |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Hiermee maakt u een Azure Automation-taakschema |
| Microsoft.Automation/automationAccounts/linkedWorkspace/read | De werkruimte die is gekoppeld aan het automation-account opgehaald |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
| Microsoft.Automation/automationAccounts/read | Hiermee ontvangt u een Azure Automation-account |
| Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
| Microsoft.Automation/automationAccounts/schedules/read | Een Azure Automation-planningsasset opgehaald |
| Microsoft.Automation/automationAccounts/schedules/write | Maken of bijwerken van een Azure Automation-planningsasset |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="automation-runbook-operator"></a>Operator voor Automation-runbook
Runbook-eigenschappen lezen: hiermee kunnen taken van de runbook worden gemaakt.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Automation/automationAccounts/runbooks/read | Een Azure Automation-runbook opgehaald |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="azure-stack-registration-owner"></a>Eigenaar Azure Stack-registratie
Hiermee kunt u Azure Stack-registraties beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.AzureStack/registrations/products/listDetails/action | Details voor een Azure-Stack Marketplace-product uitgebreid opgehaald |
| Microsoft.AzureStack/registrations/products/read | Haalt u de eigenschappen van een Azure-Stack Marketplace-product |
| Microsoft.AzureStack/registrations/read | De eigenschappen van een Azure-Stack-registratie opgehaald |

## <a name="backup-contributor"></a>Back-upinzender
Hiermee kunt u de back-upservice beheren, maar u kunt geen kluizen maken of anderen toegang verlenen

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Resultaten van de bewerking op de back-up management beheren |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Maken en beheren van back-containers in de back-fabrics van Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exporttaken |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Maken en beheren van metagegevens met betrekking tot back-management |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van de Backup-beheertaken uit te voeren |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Back-beleidsregels maken en beheren |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Maken en beheren van items die u kunnen een back-up |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Maken en beheren van back-ups van items |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Maken en beheren van containers bewaren van back-artikelen |
| Microsoft.RecoveryServices/Vaults/certificates/* | Maken en beheren van certificaten die zijn gerelateerd aan back-up in de Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van identiteiten geregistreerde |
| Microsoft.RecoveryServices/Vaults/usages/* | Maken en beheren van het gebruik van de Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services. |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van taak exportbewerking. |
| Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="backup-operator"></a>Back-upoperator
Hiermee kunt u back-upservices beheren, met uitzondering van het verwijderen van back-ups, het maken van kluizen en het verlenen van toegang aan anderen

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Retourneert de status van de bewerking |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults leestijd | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ back-up-protectedItems/actie | Hiermee wordt een back-up van het beveiligde item gemaakt. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationResults/leestijd | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationsStatus/leestijd | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems leestijd | Geeft de details van het artikel beveiligd object |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems recoveryPoints/leestijd | Herstelpunten voor beveiligde items ophalen. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints / / herstelbewerking | Herstelpunten voor beveiligde items herstellen. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/schrijven | Een back-up beveiligd Item maken |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Annuleer de taak |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Retourneert alle objecten van de taak |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exporttaken |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Hiermee worden de metagegevens van back-upbeheer voor een Recovery Services-kluis geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van de Backup-beheertaken uit te voeren |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beleidsregels voor beveiliging |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Maken en beheren van items die u kunnen een back-up |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Hiermee wordt een lijst met alle beveiligbare items opgehaald. |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers die horen bij het abonnement |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
| Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resulteert de bewerking kan worden gebruikt ophalen de bewerkingsstatus van en het resultaat voor de bewerking asynchroon ingediende |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen bewerking kan worden gebruikt, krijgen de containers die zijn geregistreerd voor een resource. |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | De bewerking servicecontainer registreren kan worden gebruikt om te registreren van een container met Service voor herstel. |
| Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/provisionInstantItemRecovery, actie | Inrichten Instant Item herstel voor beveiligde Item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints/revokeInstantItemRecovery, actie | Herstel op directe intrekken voor beveiligde Item |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van taak exportbewerking. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationStatus/read |  |
| Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="backup-reader"></a>Back-uplezer
Kan de back-upservices weergeven, maar kan geen wijzigingen aanbrengen

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Retourneert de status van de bewerking |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults leestijd | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op de beveiligde container. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationResults/leestijd | Hiermee wordt het resultaat opgehaald van de bewerking die is uitgevoerd op beveiligde items. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationsStatus/leestijd | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems leestijd | Geeft de details van het artikel beveiligd object |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Retourneert alle geregistreerde containers |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Retourneert alle objecten van de taak |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Exporttaken |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Hiermee worden de metagegevens van back-upbeheer voor een Recovery Services-kluis geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Hiermee wordt het resultaat van de back-upbewerking voor een Recovery Services-kluis geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Hiermee worden de resultaten van de beleidsbewerking opgehaald. |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beleidsregels voor beveiliging |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Hiermee wordt de lijst met alle beveiligde items geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Retourneert alle containers die horen bij het abonnement |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
| Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resulteert de bewerking kan worden gebruikt ophalen de bewerkingsstatus van en het resultaat voor de bewerking asynchroon ingediende |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen bewerking kan worden gebruikt, krijgen de containers die zijn geregistreerd voor een resource. |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems recoveryPoints/leestijd | Herstelpunten voor beveiligde items ophalen. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van taak exportbewerking. |
| Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |

## <a name="billing-reader"></a>Facturering voor lezer
Hiermee wordt leestoegang gegeven tot factureringsgegevens

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Billing/*/read | Factureringsgegevens lezen |
| Microsoft.Consumption/*/read |  |
| Microsoft.Commerce/*/read |  |
| Microsoft.Management/managementGroups/read | Lijst met beheergroepen voor de geverifieerde gebruiker. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="biztalk-contributor"></a>Inzender voor BizTalk
Hiermee beheert u BizTalk-services, maar kunt u niet de toegang tot de services beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.BizTalkServices/BizTalk/* | Maken en beheren van BizTalk services |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-endpoint-contributor"></a>Inzender voor CDN-eindpunt
Kan CDN-eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/* |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-endpoint-reader"></a>Lezer voor CDN-eindpunt
Kan CDN-eindpunten weergeven, maak kan geen wijzigingen aanbrengen.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/endpoints/*/read |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-profile-contributor"></a>Inzender voor CDN-profiel
Kan CDN-profielen en de bijbehorende eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/* |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cdn-profile-reader"></a>Lezer voor CDN-profiel
Kan CDN-profielen en de bijbehorende eindpunten weergeven, maak kan geen wijzigingen aanbrengen.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Cdn/edgenodes/read |  |
| Microsoft.Cdn/operationresults/* |  |
| Microsoft.Cdn/profiles/*/read |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-network-contributor"></a>Inzender voor klassieke netwerken
Hiermee beheert u klassieke netwerken, maar kunt u niet de toegang tot de netwerken beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.ClassicNetwork/* | Maken en beheren van klassieke netwerken |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-storage-account-contributor"></a>Inzender voor klassieke opslagaccounts
Hiermee kunt u klassieke opslagaccounts beheren, maar niet de toegang.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.ClassicStorage/storageAccounts/* | Maken en beheren van de storage-accounts |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-storage-account-key-operator-service-role"></a>De servicerol Sleuteloperator voor klassieke opslagaccounts
Sleuteloperators voor klassieke opslagaccounts kunnen een lijst met sleutels voor klassieke opslagaccounts maken en de sleutels opnieuw genereren

| **Acties** |  |
| --- | --- |
| Microsoft.ClassicStorage/storageAccounts/listkeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
| Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | De bestaande toegangssleutels voor het opslagaccount worden geregenereerd. |

## <a name="classic-virtual-machine-contributor"></a>Inzender voor klassieke virtuele machines
U kunt klassieke virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of ze zijn verbonden met storage-account beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.ClassicCompute/domainNames/* | Maken en beheren van domeinnamen klassieke compute |
| Microsoft.ClassicCompute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
| Microsoft.ClassicNetwork/reservedIps/link/action | Een gereserveerde IP koppelen |
| Microsoft.ClassicNetwork/reservedIps/read | Hiermee worden de gereserveerde IP's opgehaald. |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Hiermee wordt de deelname aan het virtuele netwerk uitgevoerd. |
| Microsoft.ClassicNetwork/virtualNetworks/read | Het virtuele netwerk wordt opgehaald. |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Hiermee wordt de opslagaccountschijf geretourneerd. |
| Microsoft.ClassicStorage/storageAccounts/images/read | Retourneert de installatiekopie van het opslagaccount. |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
| Microsoft.ClassicStorage/storageAccounts/read | Opslagaccount met het opgegeven account wordt geretourneerd. |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cleardb-mysql-db-contributor"></a>Inzender voor ClearDB MySQL-databases
Hiermee beheert u ClearDB MySQL-databases, maar kunt u niet de toegang tot de databases beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| successbricks.cleardb/databases/* | Maken en beheren van ClearDB MySQL-databases |

## <a name="cosmos-db-account-reader-role"></a>Rol van lezer voor Cosmos DB-account
Gegevens van Azure DB die Cosmos-account kan worden gelezen. Zie [DocumentDB-Account Inzender](#documentdb-account-contributor) voor het beheren van Azure DB die Cosmos-accounts.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen kunnen machtigingen die u aan elke gebruiker lezen |
| Microsoft.DocumentDB/*/read | Lezen van een verzameling |
| Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Leest de database alleen-lezen sleutels. |
| Microsoft.Insights/MetricDefinitions/read | Metrische definities lezen |
| Microsoft.Insights/Metrics/read | De metrische gegevens lezen |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="data-factory-contributor"></a>Inzender Data Factory
Data factory's en de onderliggende resources hierin maken en beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.DataFactory/dataFactories/* | Maken en beheren van de gegevensfactory en de onderliggende resources binnen deze. |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="data-lake-analytics-developer"></a>Ontwikkelaar voor Data Lake Analytics
Hiermee kunt u uw eigen taken indienen, controleren en beheren, maar geen Data Lake Analytics-accounts maken of verwijderen.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.BigAnalytics/accounts/* |  |
| Microsoft.DataLakeAnalytics/accounts/* |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.BigAnalytics/accounts/Delete |  |
| Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
| Microsoft.BigAnalytics/accounts/Write |  |
| Microsoft.DataLakeAnalytics/accounts/Delete | Een DataLakeAnalytics-account verwijderen. |
| Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Machtigingen verlenen voor het annuleren van de taken die worden verzonden door andere gebruikers. |
| Microsoft.DataLakeAnalytics/accounts/Write | Maken of bijwerken van een DataLakeAnalytics-account. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Maken of bijwerken van een gekoppelde DataLakeStore-account van een DataLakeAnalytics-account. |
| Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Ontkoppelen van een account DataLakeStore van een DataLakeAnalytics-account. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | Maken of bijwerken van een gekoppelde Storage-account van een DataLakeAnalytics-account. |
| Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Een opslagaccount van een account DataLakeAnalytics ontkoppelen. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Maken of bijwerken van een firewallregel. |
| Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Een firewallregel verwijderen. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Maken of bijwerken van een compute-beleid. |
| Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | Een compute-beleid verwijderen. |

## <a name="devtest-labs-user"></a>DevTest Labs-gebruiker
Hiermee kunt u verbinding maken met virtuele machines in Azure DevTest Labs en de virtuele machines starten, opnieuw starten en afsluiten.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Compute/availabilitySets/read | Hiermee worden de eigenschappen van een beschikbaarheidsset opgehaald |
| Microsoft.Compute/virtualMachines/*/read | Lezen van de eigenschappen van een virtuele machine (VM-grootten, runtimestatus, VM-extensies, enz.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Hiermee wordt de virtuele machine uitgeschakeld en worden de rekenresources vrijgegeven |
| Microsoft.Compute/virtualMachines/read | Hiermee worden de eigenschappen van een virtuele machine opgehaald |
| Microsoft.Compute/virtualMachines/restart/action | Hiermee wordt de virtuele machine opnieuw gestart |
| Microsoft.Compute/virtualMachines/start/action | Hiermee wordt de virtuele machine gestart |
| Microsoft.DevTestLab/*/read | Lezen van de eigenschappen van een testlab |
| Microsoft.DevTestLab/labs/createEnvironment/action | Virtuele machines maken in een testomgeving. |
| Microsoft.DevTestLab/labs/claimAnyVm/action | Een willekeurige claimable virtuele machine in de testomgeving claimen. |
| Microsoft.DevTestLab/labs/formulas/delete | Verwijder de formules. |
| Microsoft.DevTestLab/labs/formulas/read | Lees de formules. |
| Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen van formules. |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Lab beleid evalueert. |
| Microsoft.DevTestLab/labs/virtualMachines/claim/action | Eigenaar worden van een bestaande virtuele machine |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een back-endadresgroep voor load balancer |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel |
| Microsoft.Network/networkInterfaces/*/read | Lezen van de eigenschappen van een netwerkinterface (bijvoorbeeld alle load balancers die de netwerkinterface deel van uitmaakt) |
| Microsoft.Network/networkInterfaces/join/action | Een virtuele Machine koppelt aan een netwerkinterface |
| Microsoft.Network/networkInterfaces/read | Haalt de definitie van een netwerkinterface.  |
| Microsoft.Network/networkInterfaces/write | Een netwerkinterface maken of bijwerken van een bestaande netwerkinterface.  |
| Microsoft.Network/publicIPAddresses/*/read | Lezen van de eigenschappen van een openbaar IP-adres |
| Microsoft.Network/publicIPAddresses/join/action | Koppelt een openbare IP-adres |
| Microsoft.Network/publicIPAddresses/read | De definitie van een openbaar IP-adres ophalen. |
| Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk |
| Microsoft.Resources/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
| Microsoft.Resources/deployments/read | Hiermee kunt u implementaties ophalen of opnemen in een lijst. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/listKeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |

| **NotActions** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/vmSizes/read | Hiermee wordt een lijst weergegeven met de beschikbare grootten waarnaar de virtuele machine kan worden bijgewerkt |

## <a name="dns-zone-contributor"></a>Inzender voor DNS-zone
Hiermee kunt u DNS-zones en recordsets beheren in Azure DNS, maar kunt u niet bepalen wie toegang heeft.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/dnsZones/* | DNS-zones en records maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="documentdb-account-contributor"></a>Inzender voor het DocumentDB-account
Kan Azure Cosmos DB accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.DocumentDb/databaseAccounts/* | Maken en beheren van Azure DB die Cosmos-accounts |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="intelligent-systems-account-contributor"></a>Inzender voor ISS-accounts
Hiermee beheert u Intelligent Systems-accounts, maar kunt u niet de toegang tot de accounts beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.IntelligentSystems/accounts/* | Intelligente systemen rekeningen maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="key-vault-contributor"></a>Inzender voor Key Vault
Hiermee kunt u sleutelkluizen beheren, maar niet de toegang hiertoe.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.KeyVault/* |  |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.KeyVault/locations/deletedVaults/purge/action | Een voorlopig verwijderde sluitelkluis leegmaken |
| Microsoft.KeyVault/hsmPools/* |  |

## <a name="lab-creator"></a>De maker van het Lab
Hiermee kunt u maken, beheren en verwijderen van uw beheerde labs in uw Azure Lab-Accounts.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.ManagedLab/labAccounts/createLab/action | Een lab maken in een lab-account. |
| Microsoft.ManagedLab/labAccounts/*/read |  |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="log-analytics-contributor"></a>Inzender van Log Analytics
Log Analytics Inzender kunnen alle bewakingsgegevens lezen en bewerken van controle-instellingen. Controle-instellingen bewerken bevat de VM-extensie toevoegen aan virtuele machines; lezen van toegangscodes voor opslag om te kunnen verzamelen van Logboeken van Azure Storage; configureren maken en configureren van Automation-accounts; toevoegen van oplossingen; en het configureren van Azure diagnostics op alle Azure-resources.

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.Automation/automationAccounts/* |  |
| Microsoft.ClassicCompute/virtualMachines/extensions/* |  |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
| Microsoft.Compute/virtualMachines/extensions/* |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/diagnosticSettings/* | Gemaakt, bijgewerkt of leest de diagnostische instelling voor Analysis-Server |
| Microsoft.OperationalInsights/* |  |
| Microsoft.OperationsManagement/* |  |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
| Microsoft.Storage/storageAccounts/listKeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="log-analytics-reader"></a>Lezer van Log Analytics
De lezer van Log Analytics kan alle controlegegevens weergeven en doorzoeken en de controle-instellingen weergeven, inclusief het weergeven van de configuratie van Azure Diagnostics op alle Azure-resources.

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.OperationalInsights/workspaces/analytics/query/action | Zoeken met nieuwe-engine. |
| Microsoft.OperationalInsights/workspaces/search/action | Een zoekopdracht wordt uitgevoerd |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.OperationalInsights/workspaces/sharedKeys/read | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte. |

## <a name="logic-app-contributor"></a>Logische app-bijdrager
Hiermee kunt u logische apps beheren, maar niet de toegang hiertoe.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
| Microsoft.ClassicStorage/storageAccounts/read | Opslagaccount met het opgegeven account wordt geretourneerd. |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/diagnosticSettings/* | Gemaakt, bijgewerkt of leest de diagnostische instelling voor Analysis-Server |
| Microsoft.Insights/logdefinitions/* | Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Lijst met logboekbestanden categorieën in het activiteitenlogboek. |
| Microsoft.Insights/metricDefinitions/* | Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
| Microsoft.Logic/* | Logic Apps resources beheert. |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/operationresults/read | Hiermee worden de resultaten van de abonnementsbewerking opgehaald. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/listkeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
| Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/connectionGateways/* | Maken en beheren van een Gateway-verbinding. |
| Microsoft.Web/connections/* | Maken en beheren van een verbinding. |
| Microsoft.Web/customApis/* | Maakt en beheert een aangepaste API. |
| Microsoft.Web/serverFarms/join/action |  |
| Microsoft.Web/serverFarms/read | Opvragen van de eigenschappen voor een App Service-Plan |
| Microsoft.Web/sites/functions/listSecrets/action | Lijst met geheimen Web Apps functies. |

## <a name="logic-app-operator"></a>Logische app-operator
Hiermee kunt u logische apps lezen, inschakelen en uitschakelen.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/*/read | Waarschuwingsregels Insights lezen |
| Microsoft.Insights/diagnosticSettings/*/read | Diagnostische instellingen voor logische Apps opgehaald |
| Microsoft.Insights/metricDefinitions/*/read | Hiermee wordt de beschikbare metrische gegevens voor Logic Apps. |
| Microsoft.Logic/*/read | Logic Apps bronnen leest. |
| Microsoft.Logic/workflows/disable/action | Hiermee wordt de werkstroom uitgeschakeld. |
| Microsoft.Logic/workflows/enable/action | Hiermee wordt de werkstroom ingeschakeld. |
| Microsoft.Logic/workflows/validate/action | Hiermee wordt de werkstroom gevalideerd. |
| Microsoft.Resources/deployments/operations/read | Hiermee kunt u implementatiebewerkingen ophalen of opnemen in een lijst. |
| Microsoft.Resources/subscriptions/operationresults/read | Hiermee worden de resultaten van de abonnementsbewerking opgehaald. |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/connectionGateways/*/read | Lezen verbinding Gateways. |
| Microsoft.Web/connections/*/read | Lezen verbindingen. |
| Microsoft.Web/customApis/*/read | Aangepaste API worden gelezen. |
| Microsoft.Web/serverFarms/read | Opvragen van de eigenschappen voor een App Service-Plan |

## <a name="managed-identity-contributor"></a>Beheerde identiteit Inzender
Maken, lezen, bijwerken en verwijderen van de gebruiker toegewezen identiteit

| **Acties** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/write |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/delete |  |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="managed-identity-operator"></a>Beheerde identiteit Operator
Lees- en identiteit toegewezen gebruiker toewijzen

| **Acties** |  |
| --- | --- |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/read |  |
| Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action |  |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="monitoring-contributor"></a>Controlebijdrager
Kan alle bewakingsgegevens lezen en controle-instellingen bewerken. Zie ook [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.AlertsManagement/alerts/* |  |
| Microsoft.AlertsManagement/alertsSummary/* |  |
| Microsoft.Insights/AlertRules/* | Waarschuwingsregels voor lezen, schrijven, verwijderen. |
| Microsoft.Insights/components/* | Onderdelen van de Application Insights lezen, schrijven, verwijderen. |
| Microsoft.Insights/DiagnosticSettings/* | Diagnostische instellingen voor lezen, schrijven, verwijderen. |
| Microsoft.Insights/eventtypes/* | Activiteitenlogboek gebeurtenissen (management gebeurtenissen) in een abonnement weergeven. Deze machtiging is van toepassing op de portal zowel programmatische toegang tot het activiteitenlogboek. |
| Microsoft.Insights/LogDefinitions/* | Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Lijst met logboekbestanden categorieën in het activiteitenlogboek. |
| Microsoft.Insights/MetricDefinitions/* | Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
| Microsoft.Insights/Metrics/* | Metrische gegevens voor een resource lezen. |
| Microsoft.Insights/Register/Action | De Microsoft Insights-provider registeren |
| Microsoft.Insights/webtests/* | Webtests Application Insights voor lezen, schrijven, verwijderen. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Lezen/schrijven/verwijderen Log Analytics-oplossing packs. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* | Lezen/schrijven/verwijderen Log Analytics opgeslagen zoekopdrachten. |
| Microsoft.OperationalInsights/workspaces/search/action | Een zoekopdracht wordt uitgevoerd |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action | Hiermee haalt u de gedeelde sleutels voor de werkruimte. Deze sleutels worden gebruikt voor het verbinden van Microsoft Operational Insights-agents met de werkruimte. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lezen/schrijven/verwijderen logboekanalyse inzicht opslagconfiguraties. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>Controlelezer
Alle bewakingsgegevens (metrische gegevens, Logboeken, enz.) kunnen worden gelezen. Zie ook [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.OperationalInsights/workspaces/search/action | Een zoekopdracht wordt uitgevoerd |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="network-contributor"></a>Inzender voor netwerken
Hiermee beheert u netwerken, maar kunt u niet de toegang tot de netwerken beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/* | Maken en beheren van netwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="new-relic-apm-account-contributor"></a>Inzender voor het New Relic APM-account
Hiermee beheert u New Relic Application Performance Management-accounts en -toepassingen, maar kunt u niet de toegang tot de accounts en toepassingen beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| NewRelic.APM/accounts/* |  |

## <a name="redis-cache-contributor"></a>Inzender voor Redis-caches
Hiermee beheert u exemplaren van Redis-cache, maar kunt u niet de toegang tot de exemplaren beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Cache/redis/* | Maken en beheren van Redis-caches |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="scheduler-job-collections-contributor"></a>Inzender voor Scheduler-taakverzamelingen
Hiermee beheert u Scheduler-taakverzamelingen, maar kunt u niet de toegang tot de verzamelingen beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Scheduler/jobcollections/* | Maken en beheren van jobverzamelingen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="search-service-contributor"></a>Inzender voor Search-services
Hiermee beheert u Search-services, maar kunt u niet de toegang tot de services beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Search/searchServices/* | Maken en beheren van de search-services |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="security-admin"></a>Beveiligingsbeheerder
In Security Center alleen: kunt weergeven beveiligingsbeleid, beveiliging statussen weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Authorization/policyAssignments/* | Maken en beheren van de toewijzingen van beleid |
| Microsoft.Authorization/policyDefinitions/* | Maken en beheren van beleidsdefinities |
| Microsoft.Authorization/policySetDefinitions/* | Maken en beheren van sets van beleid |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.operationalInsights/workspaces/*/read | Log Analytics-gegevens weergeven |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Security/*/read | Lees beveiligingsonderdelen en beleidsregels |
| Microsoft.Security/locations/alerts/dismiss/action | Een beveiligingswaarschuwing te sluiten |
| Microsoft.Security/locations/tasks/dismiss/action | Een beveiligingsaanbeveling negeren |
| Microsoft.Security/policies/write | Updates van het beveiligingsbeleid |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="security-manager"></a>Beveiligingsbeheer
Hiermee beheert u beveiligingsonderdelen, beveiligingsbeleidsregels en virtuele machines

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.ClassicCompute/*/read | Lezen van configuratiegegevens klassieke virtuele machines |
| Microsoft.ClassicCompute/virtualMachines/*/write | Configuratie voor klassieke virtuele machines schrijven |
| Microsoft.ClassicNetwork/*/read | Configuratie-informatie lezen over klassiek netwerk |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Security/* | Beveiligingsonderdelen en -beleid maken en beheren |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="security-reader"></a>Beveiligingslezer
In Security Center alleen: kunt aanbevelingen en waarschuwingen, weergave beveiligingsbeleid weergeven van beveiliging, maar geen wijzigingen aanbrengen weergeven

| **Acties** |  |
| --- | --- |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.operationalInsights/workspaces/*/read | Log Analytics-gegevens weergeven |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Security/*/read | Lees beveiligingsonderdelen en beleidsregels |

## <a name="site-recovery-contributor"></a>Site Recovery-inzender
Hiermee kunt u de Site Recovery-service beheren, maar geen kluizen maken of rollen toewijzen

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/certificates/write | De bewerking Update Resource certificaat updates het referentiecertificaat van de resource/kluis. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van identiteiten geregistreerde |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Maken of bijwerken van de replicatie-instellingen voor waarschuwingen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Maken en beheren van replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Maken en beheren van replicatietaken |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Replicatie-beleidsregels maken en beheren |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Maken en beheren van herstelplannen |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Maken en beheren van configuratie van de opslag van de Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Retourneert token-informatie voor de Recovery Services-kluis. |
| Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd |  |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="site-recovery-operator"></a>Site Recovery-operator
Hiermee kunt u failover en fallback uitvoeren, maar geen andere beheerbewerkingen voor Site Recovery

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
| Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resulteert de bewerking kan worden gebruikt ophalen de bewerkingsstatus van en het resultaat voor de bewerking asynchroon ingediende |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen bewerking kan worden gebruikt, krijgen de containers die zijn geregistreerd voor een resource. |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Alle instellingen voor waarschuwingen lezen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Hiermee wordt de consistentie van de infrastructuur gecontroleerd |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Gateway opnieuw koppelen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Certificaat vernieuwen voor de infrastructuur |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Geen netwerken lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Alle Netwerktoewijzingen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Beveiliging-Containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare objecten lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Herstelpunt toepassen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover doorvoeren |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplande Failover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Alle beveiligde Items lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Replicatie herstelpunten lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Reparatie replicatie |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Beveiligde Item opnieuw beveiligen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/action | Failover testen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Het opruimen van testfailover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Bijwerken van de Mobility-Service |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle toewijzingen van de Container beveiliging lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders leestijd | Een Recovery Services-Providers lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ refreshProvider-replicationRecoveryServicesProviders/actie | Vernieuw de Provider |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Alle toewijzingen van de classificatie opslag lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lezen van taken |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Maken en beheren van replicatietaken |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lezen van beleid |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Herstelplan-failover doorvoeren |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Plan voor herstel voor geplande Failover |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Alle herstelplannen lezen |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Plan voor herstel opnieuw beveiligen |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test Failover herstelplan |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test het opruimen van herstelplan |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Plan voor herstel van failover |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd |  |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Retourneert token-informatie voor de Recovery Services-kluis. |
| Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen. |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="site-recovery-reader"></a>Site Recovery-lezer
Hiermee kunt u de Site Recovery-status weergeven maar geen andere beheerbewerkingen voor Site Recovery uitvoeren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Met de bewerking Uitgebreide informatie ophalen wordt de uitgebreide informatie opgehaald van een object dat de Azure-resource van het type ?vault? vertegenwoordigt |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd |  |
| Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerking ophalen resulteert de bewerking kan worden gebruikt ophalen de bewerkingsstatus van en het resultaat voor de bewerking asynchroon ingediende |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | De Containers ophalen bewerking kan worden gebruikt, krijgen de containers die zijn geregistreerd voor een resource. |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Alle instellingen voor waarschuwingen lezen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Alle gebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Alle Fabrics lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Geen netwerken lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Alle Netwerktoewijzingen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Beveiliging-Containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Alle beveiligbare objecten lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Alle beveiligde Items lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Replicatie herstelpunten lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Alle toewijzingen van de Container beveiliging lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders leestijd | Een Recovery Services-Providers lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Alle Opslagclassificaties lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Alle toewijzingen van de classificatie opslag lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lezen van taken |
| Microsoft.RecoveryServices/vaults/replicationJobs/read | Lezen van taken |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lezen van beleid |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Alle herstelplannen lezen |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Retourneert token-informatie voor de Recovery Services-kluis. |
| Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="sql-db-contributor"></a>Inzender voor SQL-databases
Kunt u SQL-databases, maar niet de toegang tot beheren. U beheren niet ook hun beleid betrekking hebben op beveiliging of hun bovenliggende SQL-servers.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/databases/* | Maken en beheren van de SQL-databases |
| Microsoft.Sql/servers/read | Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Controlebeleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditingSettings/* | Controle-instellingen kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditRecords/read | De database blob-controlerecords ophalen |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Beleid voor de verbinding kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Gegevensmaskering beleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityMetrics/* | Metrische gegevens voor beveiliging kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |

## <a name="sql-security-manager"></a>SQL-beveiligingsbeheer
Hiermee kunt u beveiligingsbeleid van SQL-servers en -databases beheren, maar niet de toegang tot het beleid.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lees Microsoft autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource zoals opslagaccount of de SQL-database koppelt aan een subnet. |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Sql/servers/auditingPolicies/* | SQL server audit beleidsregels maken en beheren |
| Microsoft.Sql/servers/auditingSettings/* | Maken en beheren van controle instelling van SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Maken en te beheren controlebeleid van de SQL server-database |
| Microsoft.Sql/servers/databases/auditingSettings/* | Maken en beheren van SQL server-database controle-instellingen |
| Microsoft.Sql/servers/databases/auditRecords/read | Lees AuditRecords |
| Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server-database verbinding beleidsregels maken en beheren |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Maken en beheren van SQL server-databasegegevens maskeren van beleid |
| Microsoft.Sql/servers/databases/read | De lijst met databases of haalt de eigenschappen voor de opgegeven database geretourneerd. |
| Microsoft.Sql/servers/databases/schemas/read | Ophalen van lijst met schema's van een database |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Ophalen van lijst met kolommen van een tabel |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/read | Ophalen van lijst met tabellen van een database |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Maken en beheren van de waarschuwing beveiligingsbeleid van SQL server-database |
| Microsoft.Sql/servers/databases/securityMetrics/* | Maken en beheren van SQL server-database beveiliging metrische gegevens |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/firewallRules/* |  |
| Microsoft.Sql/servers/read | Retourneert de lijst met servers of de eigenschappen voor de opgegeven server opgehaald. |
| Microsoft.Sql/servers/securityAlertPolicies/* | Maken en beheren van de waarschuwing beleidsregels voor de beveiliging van SQL server |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="sql-server-contributor"></a>Inzender voor SQL Server
Hiermee kunt u SQL-servers en -databases beheren, maar niet de toegang tot en het beveiligingsbeleid van de servers en databases.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/* | Maken en beheren van de SQL-servers |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* | Controlebeleid van SQL server kan niet worden bewerkt. |
| Microsoft.Sql/servers/auditingSettings/* | Kan geen SQL server-controle-instellingen bewerken |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Controlebeleid van de SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditingSettings/* | Kan geen SQL server-database controle-instellingen bewerken |
| Microsoft.Sql/servers/databases/auditRecords/read | Kan de controlerecords niet lezen. |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Beleidsregels voor SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | SQL server-database gegevensmaskering beleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Waarschuwing beveiligingsbeleid van SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityMetrics/* | Metrische gegevens van SQL server-database beveiliging kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/extendedAuditingSettings/* |  |
| Microsoft.Sql/servers/securityAlertPolicies/* | Waarschuwing beleidsregels voor de beveiliging van SQL server kan niet worden bewerkt. |

## <a name="storage-account-contributor"></a>Inzender voor opslagaccounts
Hiermee kunt u opslagaccounts beheren, maar niet de toegang.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Alle autorisatie lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/diagnosticSettings/* | Diagnostische instellingen beheren |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource zoals opslagaccount of de SQL-database koppelt aan een subnet. |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/* | Maken en beheren van de storage-accounts |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="storage-account-key-operator-service-role"></a>De servicerol Sleuteloperator voor opslagaccounts
Sleuteloperators voor opslagaccounts kunnen een lijst met sleutels voor opslagaccounts maken en de sleutels opnieuw genereren

| **Acties** |  |
| --- | --- |
| Microsoft.Storage/storageAccounts/listkeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
| Microsoft.Storage/storageAccounts/regeneratekey/action | Hiermee genereert u de toegangssleutels voor het opgegeven opslagaccount opnieuw. |

## <a name="support-request-contributor"></a>Inzender voor ondersteuningsaanvragen
Hiermee kunt u ondersteuningsaanvragen maken en beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="traffic-manager-contributor"></a>Inzender voor Traffic Manager
Hiermee kunt u Traffic Manager-profielen beheren, maar kunt u niet bepalen wie toegang heeft.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Network/trafficManagerProfiles/* |  |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="user-access-administrator"></a>Beheerder van gebruikerstoegang
Hiermee beheert u de gebruikerstoegang tot Azure-resources.

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.Authorization/* | Machtigingen beheren |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="virtual-machine-administrator-login"></a>Aanmeldingsnaam van de beheerder van de virtuele Machine
-   Gebruikers met deze functie hebben de mogelijkheid om aan te melden met een virtuele machine met Windows-beheerder of gebruiker heeft bevoegdheden die Linux hoofdmap.

| **Acties** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/loginAsAdmin/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="virtual-machine-contributor"></a>Inzender voor virtuele machines
Kunt u virtuele machines, maar niet de toegang tot, en niet het virtuele netwerk of ze zijn verbonden met storage-account beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Compute/availabilitySets/* | Maken en beheren van de compute-beschikbaarheidssets |
| Microsoft.Compute/locations/* | Maken en beheren van de compute-locaties |
| Microsoft.Compute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.Compute/virtualMachineScaleSets/* | Maken en beheren van virtuele-machineschaalsets |
| Microsoft.DevTestLab/schedules/* |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Een application gateway back-end-adresgroep toevoegen |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid wordt van een back-endadresgroep voor load balancer |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Lid wordt van een load balancer binnenkomende nat-pool |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid wordt van een load balancer binnenkomende nat-regel |
| Microsoft.Network/loadBalancers/probes/join/action | U kunt met behulp van de tests van een load balancer. Bijvoorbeeld: met deze machtiging healthProbe-eigenschap van VM-scale set kan verwijzen naar de test. |
| Microsoft.Network/loadBalancers/read | De definitie van een load balancer ophalen |
| Microsoft.Network/locations/* | Maken en beheren van netwerklocaties |
| Microsoft.Network/networkInterfaces/* | Maken en beheren van de netwerkinterfaces |
| Microsoft.Network/networkSecurityGroups/join/action | Lid wordt van een netwerkbeveiligingsgroep |
| Microsoft.Network/networkSecurityGroups/read | Een definitie van een netwerk opgehaald |
| Microsoft.Network/publicIPAddresses/join/action | Koppelt een openbare IP-adres |
| Microsoft.Network/publicIPAddresses/read | De definitie van een openbaar IP-adres ophalen. |
| Microsoft.Network/virtualNetworks/read | De definitie van het virtuele netwerk ophalen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Lid wordt van een virtueel netwerk |
| Microsoft.RecoveryServices/locations/* |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems / * / lezen |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems leestijd | Geeft de details van het artikel beveiligd object |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/schrijven | Een back-up beveiligd Item maken |
| Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Maak een back-beveiliging wordt intentie |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Retourneert alle beleidsregels voor beveiliging |
| Microsoft.RecoveryServices/Vaults/backupPolicies/write | Hiermee maakt u het beveiligingsbeleid |
| Microsoft.RecoveryServices/Vaults/read | De bewerking kluis ophalen van een object dat de Azure-resource van het type 'kluis' opgehaald |
| Microsoft.RecoveryServices/Vaults/usages/read | Hiermee worden de gebruiksgegevens voor een Recovery Services-kluis geretourneerd. |
| Microsoft.RecoveryServices/Vaults/write | Met de bewerking Kluis maken wordt een Azure-resource van het type vault gemaakt. |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Storage/storageAccounts/listKeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
| Microsoft.Storage/storageAccounts/read | Hiermee retourneert u een lijst met opslagaccounts of haalt u de eigenschappen op voor het opgegeven opslagaccount. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="virtual-machine-user-login"></a>Virtuele Machine gebruikersaanmelding
Gebruikers met deze functie hebben de mogelijkheid om aan te melden met een virtuele machine als een normale gebruiker.

| **Acties** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="web-plan-contributor"></a>Inzender voor webabonnementen
Hiermee beheert u de webabonnementen voor websites, maar kunt u niet de toegang tot de abonnementen beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/serverFarms/* | Maken en beheren van serverfarms |

## <a name="website-contributor"></a>Inzender voor websites
Hiermee beheert u websites (niet webabonnementen), maar kunt u niet de toegang tot de websites beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/components/* | Maken en beheren van Insights-onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read | De beschikbaarheidsstatus ophalen voor alle resources binnen het opgegeven bereik |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Hiermee kunt u resourcegroepen ophalen of opnemen in een lijst. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/certificates/* | Maken en beheren van Websitecertificaten |
| Microsoft.Web/listSitesAssignedToHostName/read | Namen van sites die zijn toegewezen aan de hostnaam ophalen. |
| Microsoft.Web/serverFarms/join/action |  |
| Microsoft.Web/serverFarms/read | Opvragen van de eigenschappen voor een App Service-Plan |
| Microsoft.Web/sites/* | Maken en beheren van websites (maken van de site vereist ook schrijfmachtigingen in voor de bijbehorende App Service-Plan) |

## <a name="see-also"></a>Zie ook
* [Toegangsbeheer op basis van rollen](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
* [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aan uw behoeften toegang.
* [Maken van een geschiedenisrapport voor gewijzigde van toegang](role-based-access-control-access-change-history-report.md): bijhouden van wijzigen van roltoewijzingen in RBAC.
* [Probleemoplossing voor toegangsbeheer op basis van rollen](role-based-access-control-troubleshooting.md): Profiteer van tips voor het oplossen van veelvoorkomende problemen.
* [Machtigingen in Azure Security Center](../security-center/security-center-permissions.md)
