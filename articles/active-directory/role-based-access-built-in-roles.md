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
ms.openlocfilehash: 0e8f1d97f51fbfedbca1619ef5e7f28a3a0570b9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
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
| [Owner](#owner) | Kunnen alles beheren, inclusief toegang |
| [Contributor](#contributor) | Kunnen alles beheren behalve toegang |
| [Reader](#reader) | Alles weergeven, maar u kunt geen wijzigingen aanbrengen |
| [API Management-Service Inzender](#api-management-service-contributor) | API Management-services kunt beheren |
| [Rol Operator API Management-Service](#api-management-service-operator-role) | API Management-services kunt beheren |
| [Rol van API Management-Service lezer](#api-management-service-reader-role) | API Management-services kunt beheren |
| [Application Insights-Onderdeelinzender](#application-insights-component-contributor) | Kan onderdelen van Application Insights beheren |
| [Application Insights momentopname-foutopsporingsprogramma](#application-insights-snapshot-debugger) | Hiermee geeft u de gebruiker de machtiging om de functies van Application Insights Snapshot Debugger te gebruiken |
| [Automation-taak Operator](#automation-job-operator) | Taken maken en beheren met Automation-runbooks. |
| [Automation-Operator](#automation-operator) | Kunnen starten, stoppen, onderbreken en hervatten van taken |
| [Automation-Runbook-Operator](#automation-runbook-operator) | Runbook-eigenschappen lezen: hiermee kunnen taken van de runbook worden gemaakt. |
| [De eigenaar van de Azure Stack-registratie](#azure-stack-registration-owner) | Hiermee kunt u Azure Stack-registraties beheren. |
| [Back-up Inzender](#backup-contributor) | Alle back-management-acties, met uitzondering van de Recovery Services-kluis maken en toegang geven aan anderen kunt beheren |
| [Back-upoperator](#backup-operator) | Alle back-acties, behalve voor het maken van kluizen, back-up en geven toegang tot andere verwijderen kunt beheren |
| [Back-lezer](#backup-reader) | Back-beheer in de Recovery Services-kluis kunt bewaken |
| [Lezer facturering](#billing-reader) | Alle factureringsgegevens kunt weergeven |
| [BizTalk Inzender](#biztalk-contributor) | BizTalk services kunt beheren |
| [CDN-eindpunt Inzender](#cdn-endpoint-contributor) | Kan CDN-eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [CDN-eindpunt lezer](#cdn-endpoint-reader) | Kan CDN-eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
| [CDN Profile Contributor](#cdn-profile-contributor) | Kan CDN-profielen en de bijbehorende eindpunten beheren, maar kan geen toegang verlenen aan andere gebruikers. |
| [CDN-profiel lezer](#cdn-profile-reader) | Kan CDN-profielen en de bijbehorende eindpunten weergeven, maak kan geen wijzigingen aanbrengen. |
| [Inzender voor klassieke netwerken](#classic-network-contributor) | Klassieke virtuele netwerken en gereserveerde IP's kunt beheren |
| [Klassieke Storage Account Inzender](#classic-storage-account-contributor) | Klassieke opslagaccounts kunt beheren |
| [Klassieke Storage-Account de Operator-functieservice](#classic-storage-account-key-operator-service-role) | Sleuteloperators voor klassieke opslagaccounts kunnen een lijst met sleutels voor klassieke opslagaccounts maken en de sleutels opnieuw genereren |
| [Klassieke Virtual Machine Contributor](#classic-virtual-machine-contributor) | Klassieke virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden kunt beheren |
| [ClearDB MySQL DB Contributor](#cleardb-mysql-db-contributor) | ClearDB MySQL-databases kunt beheren |
| [De rol van de lezer-Account cosmos DB](#cosmos-db-account-reader-role) | Gegevens van Azure DB die Cosmos-account kan worden gelezen. Zie [DocumentDB-Account Inzender](#documentdb-account-contributor) voor het beheren van Azure DB die Cosmos-accounts. |
| [Data Factory Inzender](#data-factory-contributor) | Maken en beheren van de gegevensfactory en de onderliggende resources binnen deze. |
| [Data Lake Analytics-ontwikkelaars](#data-lake-analytics-developer) | Hiermee kunt u uw eigen taken indienen, controleren en beheren, maar geen Data Lake Analytics-accounts maken of verwijderen. |
| [DevTest Labs User](#devtest-labs-user) | Kunnen alles weergeven en verbinding maken, starten, opnieuw opstarten en afsluiten van de virtuele machines |
| [DNS-Zone Inzender](#dns-zone-contributor) | Kunnen DNS-zones en -records beheren. |
| [DocumentDB-Account Inzender](#documentdb-account-contributor) | Kan Azure Cosmos DB accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB. |
| [Intelligente systemen Account Inzender](#intelligent-systems-account-contributor) | Intelligente systemen accounts kunnen beheren |
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
| [Inzender voor netwerken](#network-contributor) | Alle netwerkbronnen kunt beheren |
| [Nieuwe Relic APM-Account Inzender](#new-relic-apm-account-contributor) | Hiermee beheert u New Relic Application Performance Management-accounts en -toepassingen, maar kunt u niet de toegang tot de accounts en toepassingen beheren. |
| [Redis-Cache Inzender](#redis-cache-contributor) | Redis-caches kunt beheren |
| [Scheduler-taak verzamelingen Inzender](#scheduler-job-collections-contributor) | Scheduler-taakverzamelingen kunt beheren |
| [Search Service Inzender](#search-service-contributor) | Search-services kunt beheren |
| [De beheerder beveiliging](#security-admin) | In Security Center alleen: kunt weergeven beveiligingsbeleid, beveiliging statussen weergeven, bewerken beveiligingsbeleid, waarschuwingen weergeven en aanbevelingen, negeren van waarschuwingen en aanbevelingen |
| [Security Manager](#security-manager) | Beveiligingsonderdelen, beveiligingsbeleid en virtuele machines beheren |
| [Beveiliging lezer](#security-reader) | In Security Center alleen: kunt aanbevelingen en waarschuwingen, weergave beveiligingsbeleid weergeven van beveiliging, maar geen wijzigingen aanbrengen weergeven |
| [Site Recovery Inzender](#site-recovery-contributor) | Alle beheeracties van Site Recovery, met uitzondering van de Recovery Services-kluis maken en toewijzen van rechten aan andere gebruikers kunnen beheren |
| [Site Recovery-Operator](#site-recovery-operator) | Kan Failover en Failback kan maar niet andere Site Recovery management acties worden uitgevoerd of toegang toewijzen aan andere gebruikers |
| [Site Recovery lezer](#site-recovery-reader) | Kan de status van de Site Recovery in de Recovery Services-kluis bewaken en ondersteuningstickets genereren |
| [SQL DB Contributor](#sql-db-contributor) | Kan SQL-databases, maar niet hun beleid betrekking hebben op beveiliging beheren |
| [SQL Security Manager](#sql-security-manager) | Het beleid betrekking hebben op de beveiliging van SQL-servers en databases kunt beheren |
| [SQL Server Contributor](#sql-server-contributor) | Kan SQL-servers en databases, maar niet hun beleid betrekking hebben op beveiliging beheren |
| [Storage-Account Inzender](#storage-account-contributor) | Kan storage-accounts beheren, maar geen toegang tot deze. |
| [Storage-Account de Operator-functieservice](#storage-account-key-operator-service-role) | Sleuteloperators voor opslagaccounts kunnen een lijst met sleutels voor opslagaccounts maken en de sleutels opnieuw genereren |
| [Ondersteuning voor aanvraag Inzender](#support-request-contributor) | Kunt maken en beheren van ondersteuningstickets bij het abonnementsbereik |
| [Traffic Manager Contributor](#traffic-manager-contributor) | Hiermee kunt u Traffic Manager-profielen beheren, maar kunt u niet bepalen wie toegang heeft. |
| [Beheerder voor gebruikerstoegang](#user-access-administrator) | Gebruikerstoegang tot Azure-resources kunt beheren |
| [Aanmeldingsnaam van de beheerder van de virtuele Machine](#virtual-machine-administrator-login) | -Gebruikers met deze functie hebben de mogelijkheid om aan te melden met een virtuele machine met Windows-beheerder of gebruiker heeft bevoegdheden die Linux hoofdmap. |
| [Virtual Machine Contributor](#virtual-machine-contributor) | Beheren van virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden |
| [Virtuele Machine gebruikersaanmelding](#virtual-machine-user-login) | Gebruikers met deze functie hebben de mogelijkheid om aan te melden met een virtuele machine als een normale gebruiker. |
| [Web Plan Inzender](#web-plan-contributor) | Kunt u web-abonnementen beheren |
| [Website Inzender](#website-contributor) | Kunt beheren, websites, maar niet de web-abonnementen waaraan ze zijn verbonden |

De volgende tabellen beschrijven de specifieke machtigingen toegekend aan elke rol. Het kan hierbij gaan **acties**, die machtigingen geven en **NotActions**, die ze beperken.

## <a name="owner"></a>Eigenaar
Kunnen alles beheren, inclusief toegang

| **Acties** |  |
| --- | --- |
| * | Maken en beheren van bronnen van alle typen |

## <a name="contributor"></a>Inzender
Kunnen alles beheren behalve toegang

| **Acties** |  |
| --- | --- |
| * | Maken en beheren van bronnen van alle typen |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete | Rollen en roltoewijzingen verwijderen niet |
| Microsoft.Authorization/*/Write | Kan de functies en roltoewijzingen niet maken |
| Microsoft.Authorization/elevateAccess/Action | Hiermee wordt oproepende functie de rechten van Administrator voor gebruikerstoegang gegeven voor het tenantbereik |

## <a name="reader"></a>Lezer
Alles weergeven, maar u kunt geen wijzigingen aanbrengen

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |

## <a name="api-management-service-contributor"></a>Inzender voor API Management-services
API Management-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/service/* | Maken en beheren van API Management-service |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="api-management-service-operator-role"></a>Operator-rol voor de API Management-service
API Management-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Lees API Management-Service-exemplaren |
| Microsoft.ApiManagement/service/backup/action | Back-up API Management-Service op de opgegeven container in een opslagaccount opgegeven gebruiker |
| Microsoft.ApiManagement/service/delete | Verwijderen van een exemplaar van API Management-Service |
| Microsoft.ApiManagement/service/managedeployments/action | Wijziging SKU/eenheden; toevoegen of verwijderen van regionale implementaties van API Management-Service |
| Microsoft.ApiManagement/service/read | Lezen van metagegevens voor een exemplaar van API Management-Service |
| Microsoft.ApiManagement/service/restore/action | API Management-Service van de opgegeven container in het opgegeven opslagaccount van een gebruiker herstellen |
| Microsoft.ApiManagement/service/updatecertificate/action | SSL-certificaat uploaden voor een API Management-Service |
| Microsoft.ApiManagement/service/updatehostname/action | Instellen, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service |
| Microsoft.ApiManagement/service/write | Maak een nieuw exemplaar van API Management-Service |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.ApiManagement/service/users/keys/read | Lijst met gebruikerssleutels |

## <a name="api-management-service-reader-role"></a>Lezerrol voor de API Management-service
API Management-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/service/*/read | Lees API Management-Service-exemplaren |
| Microsoft.ApiManagement/service/read | Lezen van metagegevens voor een exemplaar van API Management-Service |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van rollen en roltoewijzingen |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
Kunnen starten, stoppen, onderbreken en hervatten van taken

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Automation/automationAccounts/jobs/read | Automation-account taken lezen |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Een automation-account-taak hervatten |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Een automation-account-taak stoppen |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Automation-account taak streams lezen |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Een automation-account-taak onderbreken |
| Microsoft.Automation/automationAccounts/jobs/write | Automation-account taken schrijven |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Een automation-account-taakschema lezen |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Een automation-account-taakschema lezen |
| Microsoft.Automation/automationAccounts/linkedWorkspace/read | De werkruimte die is gekoppeld aan het automation-account opgehaald |
| Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Resources voor de Hybrid Runbook Worker gelezen |
| Microsoft.Automation/automationAccounts/read | Automation-accounts lezen |
| Microsoft.Automation/automationAccounts/runbooks/read | Automation-runbooks lezen |
| Microsoft.Automation/automationAccounts/schedules/read | Automation-account planningen lezen |
| Microsoft.Automation/automationAccounts/schedules/write | Automation-account schema's schrijven |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
Alle back-management-acties, met uitzondering van de Recovery Services-kluis maken en toegang geven aan anderen kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Resultaten van de bewerking op de back-up management beheren |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Maken en beheren van back-containers in de back-fabrics van Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Back-uptaken exporteren naar een excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/* | Maken en beheren van metagegevens met betrekking tot back-management |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van de Backup-beheertaken uit te voeren |
| Microsoft.RecoveryServices/Vaults/backupPolicies/* | Back-beleidsregels maken en beheren |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Maken en beheren van items die u kunnen een back-up |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Maken en beheren van back-ups van items |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Maken en beheren van containers bewaren van back-artikelen |
| Microsoft.RecoveryServices/Vaults/certificates/* | Maken en beheren van certificaten die zijn gerelateerd aan back-up in de Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/read | Recovery services-kluizen lezen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van identiteiten geregistreerde |
| Microsoft.RecoveryServices/Vaults/usages/* | Maken en beheren van het gebruik van de Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services. |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
| Microsoft.RecoveryServices/Vaults/storageConfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/* |  |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van taak exportbewerking. |
| Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="backup-operator"></a>Back-upoperator
Alle back-acties, behalve voor het maken van kluizen, back-up en geven toegang tot andere verwijderen kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Lezen van de resultaten van de bewerking op de back-management |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults leestijd | Resultaat van de bewerking op de beveiliging containers lezen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ back-up-protectedItems/actie | Back-upbewerking op een item voor back-ups op aanvraag uitvoeren |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationResults/leestijd | Lees-resultaat van bewerking uitgevoerd op een back-up item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationsStatus/leestijd | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems leestijd | Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems recoveryPoints/leestijd | Lezen van het herstelpunt van een back-up-item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/recoveryPoints / / herstelbewerking | Een herstelbewerking met behulp van een herstelpunt wordt gemaakt van een back-up item uitvoeren |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems/schrijven | Een back-item maken |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Back-upitems houden-containers gelezen |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobs/cancel/action | Annuleer de taak |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Hiermee wordt het resultaat van de taakbewerking geretourneerd. |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Retourneert alle objecten van de taak |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Back-uptaken exporteren naar een excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Lees metagegevens met betrekking tot back-management |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van de Backup-beheertaken uit te voeren |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Resultaten van de bewerkingen die worden uitgevoerd op de back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Maken en beheren van items die u kunnen een back-up |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/read | Hiermee wordt een lijst met alle beveiligbare items opgehaald. |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Alleen een back-up containers bewaren van back-artikelen |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lezen van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/extendedInformation/write | Uitgebreide gegevens gerelateerd aan de kluis schrijven |
| Microsoft.RecoveryServices/Vaults/read | Recovery services-kluizen lezen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/* | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De resultaten van het lezen van bewerking uitgevoerd op de geregistreerde items van de kluis |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Geregistreerde items van de kluis lezen |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Geregistreerde items naar de kluis schrijven |
| Microsoft.RecoveryServices/Vaults/usages/read | Lees de informatie over het gebruik van de Recovery Services-kluis |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
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
Back-beheer in de Recovery Services-kluis kunt bewaken

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Lezen van de resultaten van de bewerking op de back-management |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ operationResults leestijd | Resultaat van de bewerking op de beveiliging containers lezen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationResults/leestijd | Lees-resultaat van bewerking uitgevoerd op een back-up item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems operationsStatus/leestijd | Hiermee wordt de status geretourneerd van de bewerking die is uitgevoerd op beveiligde items. |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems leestijd | Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Back-upitems houden-containers gelezen |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read | Lezen van de resultaten van de back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobs/read | Lezen van de back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Back-uptaken exporteren naar een excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Lees metagegevens met betrekking tot back-management |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Lees de back-beheer Bewerkingsresultaten |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Resultaten van de bewerkingen die worden uitgevoerd op de back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Alleen een back-up containers bewaren van back-artikelen |
| Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read | Samenvattingen retourneert voor beveiligde Items en beveiligde Servers voor een Recovery Services. |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lezen van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/read | Recovery services-kluizen lezen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Resultaat van de detectiebewerking aan voor het ophalen van een nieuw gemaakt containers lezen |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De resultaten van het lezen van bewerking uitgevoerd op de geregistreerde items van de kluis |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Geregistreerde items van de kluis lezen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd |  |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Hiermee haalt de waarschuwingen voor de Recovery services-kluis. |
| Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupconfig/vaultconfig/read |  |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/ protectedItems recoveryPoints/leestijd | Herstelpunten voor beveiligde items ophalen. |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/operationResults/read | Retourneert het resultaat van taak exportbewerking. |
| Microsoft.RecoveryServices/Vaults/usages/read | Lees de informatie over het gebruik van de Recovery Services-kluis |

## <a name="billing-reader"></a>Facturering voor lezer
Alle factureringsgegevens kunt weergeven

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Billing/*/read | Factureringsgegevens lezen |
| Microsoft.Consumption/*/read |  |
| Microsoft.Commerce/*/read |  |
| Microsoft.Management/managementGroups/read | Lijst met beheergroepen voor de geverifieerde gebruiker. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="biztalk-contributor"></a>Inzender voor BizTalk
BizTalk services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.BizTalkServices/BizTalk/* | Maken en beheren van BizTalk services |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
Klassieke virtuele netwerken en gereserveerde IP's kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.ClassicNetwork/* | Maken en beheren van klassieke netwerken |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-storage-account-contributor"></a>Inzender voor klassieke opslagaccounts
Klassieke opslagaccounts kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.ClassicStorage/storageAccounts/* | Maken en beheren van de storage-accounts |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="classic-storage-account-key-operator-service-role"></a>De servicerol Sleuteloperator voor klassieke opslagaccounts
Sleuteloperators voor klassieke opslagaccounts kunnen een lijst met sleutels voor klassieke opslagaccounts maken en de sleutels opnieuw genereren

| **Acties** |  |
| --- | --- |
| Microsoft.ClassicStorage/storageAccounts/listkeys/action | Hiermee worden de toegangssleutels voor de opslagaccounts weergegeven. |
| Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | De bestaande toegangssleutels voor het opslagaccount worden geregenereerd. |

## <a name="classic-virtual-machine-contributor"></a>Inzender voor klassieke virtuele machines
Klassieke virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.ClassicCompute/domainNames/* | Maken en beheren van domeinnamen klassieke compute |
| Microsoft.ClassicCompute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Deelnemen aan netwerkbeveiligingsgroepen |
| Microsoft.ClassicNetwork/reservedIps/link/action | Gereserveerde IP's koppelen |
| Microsoft.ClassicNetwork/reservedIps/read | Lees gereserveerde IP-adressen |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Virtuele netwerken koppelen |
| Microsoft.ClassicNetwork/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Schijven met opslagruimte lezen |
| Microsoft.ClassicStorage/storageAccounts/images/read | Installatiekopieën van de storage-account lezen |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lijst met sleutels voor opslagaccount |
| Microsoft.ClassicStorage/storageAccounts/read | Klassieke opslagaccounts lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="cleardb-mysql-db-contributor"></a>Inzender voor ClearDB MySQL-databases
ClearDB MySQL-databases kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| successbricks.cleardb/databases/* | Maken en beheren van ClearDB MySQL-databases |

## <a name="cosmos-db-account-reader-role"></a>Rol van lezer voor Cosmos DB-account
Gegevens van Azure DB die Cosmos-account kan worden gelezen. Zie [DocumentDB-Account Inzender](#documentdb-account-contributor) voor het beheren van Azure DB die Cosmos-accounts.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen kunnen machtigingen die u aan elke gebruiker lezen |
| Microsoft.DocumentDB/*/read | Lezen van een verzameling |
| Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Het kenmerk alleen-lezen sleutels deelvenster lezen |
| Microsoft.Insights/MetricDefinitions/read | Metrische definities lezen |
| Microsoft.Insights/Metrics/read | Metrische gegevens lezen-account |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="data-factory-contributor"></a>Inzender Data Factory
Maken en beheren van de gegevensfactory en de onderliggende resources binnen deze.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.DataFactory/dataFactories/* | Maken en beheren van de gegevensfactory en de onderliggende resources binnen deze. |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
Kunnen alles weergeven en verbinding maken, starten, opnieuw opstarten en afsluiten van de virtuele machines

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Compute/availabilitySets/read | Lezen van de eigenschappen van beschikbaarheidssets |
| Microsoft.Compute/virtualMachines/*/read | Lezen van de eigenschappen van een virtuele machine (VM-grootten, runtimestatus, VM-extensies, enz.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Toewijzing van virtuele machines |
| Microsoft.Compute/virtualMachines/read | De eigenschappen van een virtuele machine lezen |
| Microsoft.Compute/virtualMachines/restart/action | Virtuele machines opnieuw opstarten |
| Microsoft.Compute/virtualMachines/start/action | Virtuele machines starten |
| Microsoft.DevTestLab/*/read | Lezen van de eigenschappen van een testlab |
| Microsoft.DevTestLab/labs/createEnvironment/action | Een testomgeving maken |
| Microsoft.DevTestLab/labs/claimAnyVm/action | Een willekeurige claimable virtuele machine in de testomgeving claimen. |
| Microsoft.DevTestLab/labs/formulas/delete | Formules verwijderen |
| Microsoft.DevTestLab/labs/formulas/read | Formules lezen |
| Microsoft.DevTestLab/labs/formulas/write | Toevoegen of wijzigen van formules |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Labbeleidsregels evalueren |
| Microsoft.DevTestLab/labs/virtualMachines/claim/action | Eigenaar worden van een bestaande virtuele machine |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Deelnemen aan een load balancer back-end-adresgroep |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Deelnemen aan een load balancer binnenkomende NAT-regel |
| Microsoft.Network/networkInterfaces/*/read | Lezen van de eigenschappen van een netwerkinterface (bijvoorbeeld alle load balancers die de netwerkinterface deel van uitmaakt) |
| Microsoft.Network/networkInterfaces/join/action | Een virtuele Machine toevoegen aan een netwerkinterface |
| Microsoft.Network/networkInterfaces/read | Netwerkinterfaces lezen |
| Microsoft.Network/networkInterfaces/write | Netwerkinterfaces schrijven |
| Microsoft.Network/publicIPAddresses/*/read | Lezen van de eigenschappen van een openbaar IP-adres |
| Microsoft.Network/publicIPAddresses/join/action | Lid worden van een openbaar IP-adres |
| Microsoft.Network/publicIPAddresses/read | Lezen van het openbare IP-netwerkadressen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Een virtueel netwerk koppelen |
| Microsoft.Resources/deployments/operations/read | Implementatie leesbewerkingen |
| Microsoft.Resources/deployments/read | Lezen van implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/listKeys/action | Lijst met sleutels voor opslagaccount |

| **NotActions** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/vmSizes/read | Hiermee wordt een lijst weergegeven met de beschikbare grootten waarnaar de virtuele machine kan worden bijgewerkt |

## <a name="dns-zone-contributor"></a>Inzender voor DNS-zone
Kunnen DNS-zones en -records beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/dnsZones/* | DNS-zones en records maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Lezen van de status van de resources |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="documentdb-account-contributor"></a>Inzender voor het DocumentDB-account
Kan Azure Cosmos DB accounts beheren. Azure Cosmos DB is voorheen bekend als DocumentDB.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.DocumentDb/databaseAccounts/* | Maken en beheren van Azure DB die Cosmos-accounts |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="intelligent-systems-account-contributor"></a>Inzender voor ISS-accounts
Intelligente systemen accounts kunnen beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.IntelligentSystems/accounts/* | Intelligente systemen rekeningen maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
| Microsoft.Insights/Register/Action | Registreer de provider Microsoft.Insights. |
| Microsoft.Insights/webtests/* | Webtests Application Insights voor lezen, schrijven, verwijderen. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* | Lezen/schrijven/verwijderen Log Analytics-oplossing packs. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* | Lezen/schrijven/verwijderen Log Analytics opgeslagen zoekopdrachten. |
| Microsoft.OperationalInsights/workspaces/search/action | Log Analytics-werkruimten zoeken. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action | Lijst met sleutels voor een werkruimte voor logboekanalyse. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* | Lezen/schrijven/verwijderen logboekanalyse inzicht opslagconfiguraties. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.WorkloadMonitor/workloads/* |  |

## <a name="monitoring-reader"></a>Controlelezer
Alle bewakingsgegevens (metrische gegevens, Logboeken, enz.) kunnen worden gelezen. Zie ook [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](../monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Acties** |  |
| --- | --- |
| * / lezen | Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.OperationalInsights/workspaces/search/action | Log Analytics zoekgegevens |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="network-contributor"></a>Inzender voor netwerken
Alle netwerkbronnen kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/* | Maken en beheren van netwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
Redis-caches kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Cache/redis/* | Maken en beheren van Redis-caches |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="scheduler-job-collections-contributor"></a>Inzender voor Scheduler-taakverzamelingen
Scheduler-taakverzamelingen kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Scheduler/jobcollections/* | Maken en beheren van jobverzamelingen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="search-service-contributor"></a>Inzender voor Search-services
Search-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Security/*/read | Lees beveiligingsonderdelen en beleidsregels |
| Microsoft.Security/locations/alerts/dismiss/action | Een beveiligingswaarschuwing te sluiten |
| Microsoft.Security/locations/tasks/dismiss/action | Een beveiligingsaanbeveling negeren |
| Microsoft.Security/policies/write | Updates van het beveiligingsbeleid |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="security-manager"></a>Beveiligingsbeheer
Beveiligingsonderdelen, beveiligingsbeleid en virtuele machines beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.ClassicCompute/*/read | Lezen van configuratiegegevens klassieke virtuele machines |
| Microsoft.ClassicCompute/virtualMachines/*/write | Configuratie voor klassieke virtuele machines schrijven |
| Microsoft.ClassicNetwork/*/read | Configuratie-informatie lezen over klassiek netwerk |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Security/*/read | Lees beveiligingsonderdelen en beleidsregels |

## <a name="site-recovery-contributor"></a>Site Recovery-inzender
Alle beheeracties van Site Recovery, met uitzondering van de Recovery Services-kluis maken en toewijzen van rechten aan andere gebruikers kunnen beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/certificates/write | Het referentiecertificaat van de kluis bijgewerkt |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/read | Lees Recovery Services-kluizen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Maken en beheren van identiteiten geregistreerde |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Maken of bijwerken van de replicatie-instellingen voor waarschuwingen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Replicatiegebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/* | Maken en beheren van replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Maken en beheren van replicatietaken |
| Microsoft.RecoveryServices/vaults/replicationPolicies/* | Replicatie-beleidsregels maken en beheren |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Maken en beheren van herstelplannen |
| Microsoft.RecoveryServices/Vaults/storageConfig/* | Maken en beheren van configuratie van de opslag van de Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Lees Recovery Services-kluis token-informatie |
| Microsoft.RecoveryServices/Vaults/usages/read | Details van het gebruik van een Recovery Services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen. |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd | Meldingsconfiguratie voor lezen Recovery services-kluis |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="site-recovery-operator"></a>Site Recovery-operator
Kan Failover en Failback kan maar niet andere Site Recovery management acties worden uitgevoerd of toegang toewijzen aan andere gebruikers

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lezen van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/read | Lees Recovery Services-kluizen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerkingsstatus en resultaat voor het uitvoeren van een ingediende lezen |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Geregistreerd voor een resource-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Replicatie-instellingen voor waarschuwingen lezen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Replicatiegebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Consistentie van de fabrics controleren |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Lees replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Replicatie-gateway opnieuw koppelen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Replicatie fabric certificaat vernieuwen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Lezen van replicatie fabric-netwerken |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Lees replicatie fabric netwerktoewijzing |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Beveiliging-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Lijst met alle beveiligbare objecten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Herstelpunt toepassen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover doorvoeren |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Geplande Failover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Lijst met alle beveiligde items |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lijst met beschikbare herstelpunten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Reparatie replicatie |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/reProtect/action | Start voor een beveiligde item opnieuw te beveiligen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover/action | Testfailover van een beveiligde item starten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Het opruimen van testfailover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Failover |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Bijwerken van de Mobility-Service |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Beveiliging lezen container toewijzingen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders leestijd | Lees Recovery Services-providers |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ refreshProvider-replicationRecoveryServicesProviders/actie | Vernieuw Recovery Services-provider |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Alleen opslagclassificaties voor replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Toewijzingen voor classificatie van bestanden lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lees de vCenter-gegevens geregistreerd |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Maken en beheren van replicatietaken |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lezen van replicatiebeleid |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Failover voor een herstelplan-failover doorvoeren |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Failover van een herstelplan starten |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Plannen voor herstel te lezen |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Start opnieuw beveiligen van een herstelplan |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Testfailover van een herstelplan starten |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Start het opruimen van een herstelplan-testfailover |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Niet-geplande failover van een herstelplan starten |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd | Meldingsconfiguratie voor lezen Recovery services-kluis |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Lezen van configuratie van de opslag van een Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Lees Recovery Services-kluis token-informatie |
| Microsoft.RecoveryServices/Vaults/usages/read | Details van het gebruik van een Recovery Services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="site-recovery-reader"></a>Site Recovery-lezer
Kan de status van de Site Recovery in de Recovery Services-kluis bewaken en ondersteuningstickets genereren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp is een interne bewerking die wordt gebruikt door de service |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lezen van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd | Meldingsconfiguratie voor lezen Recovery services-kluis |
| Microsoft.RecoveryServices/Vaults/read | Lees Recovery Services-kluizen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerkingsstatus en resultaat voor het uitvoeren van een ingediende lezen |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Geregistreerd voor een resource-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Replicatie-instellingen voor waarschuwingen lezen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Replicatiegebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Lees replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Lezen van replicatie fabric-netwerken |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks/replicationNetworkMappings/read | Lees replicatie fabric netwerktoewijzing |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/read | Beveiliging-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectableItems/read | Lijst met alle beveiligbare objecten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/read | Lijst met alle beveiligde items |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Lijst met beschikbare herstelpunten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectionContainerMappings/read | Beveiliging lezen container toewijzingen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders leestijd | Lees Recovery Services-providers |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/read | Alleen opslagclassificaties voor replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications/replicationStorageClassificationMappings/read | Toewijzingen voor classificatie van bestanden lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lees de vCenter-gegevens geregistreerd |
| Microsoft.RecoveryServices/vaults/replicationJobs/read | Status van replicatietaken lezen |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lezen van replicatiebeleid |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Plannen voor herstel te lezen |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Lezen van configuratie van de opslag van een Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Lees Recovery Services-kluis token-informatie |
| Microsoft.RecoveryServices/Vaults/usages/read | Details van het gebruik van een Recovery Services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/vaultTokens/read | Het Token kluis opnieuw kan worden gebruikt voor bewerkingen van kluis niveau back-end-kluis Token ophalen. |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="sql-db-contributor"></a>Inzender voor SQL-databases
Kan SQL-databases, maar niet hun beleid betrekking hebben op beveiliging beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Sql/locations/*/read |  |
| Microsoft.Sql/servers/databases/* | Maken en beheren van de SQL-databases |
| Microsoft.Sql/servers/read | Lezen van de SQL-Servers |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Controlebeleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditingSettings/* | Controle-instellingen kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditRecords/read | Kan de controlerecords niet lezen. |
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
Het beleid betrekking hebben op de beveiliging van SQL-servers en databases kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lees Microsoft autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource zoals opslagaccount of de SQL-database koppelt aan een subnet. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Sql/servers/auditingPolicies/* | SQL server audit beleidsregels maken en beheren |
| Microsoft.Sql/servers/auditingSettings/* | Maken en beheren van controle instelling van SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Maken en te beheren controlebeleid van de SQL server-database |
| Microsoft.Sql/servers/databases/auditingSettings/* | Maken en beheren van SQL server-database controle-instellingen |
| Microsoft.Sql/servers/databases/auditRecords/read | Lees AuditRecords |
| Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server-database verbinding beleidsregels maken en beheren |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Maken en beheren van SQL server-databasegegevens maskeren van beleid |
| Microsoft.Sql/servers/databases/read | Lees SQL-databases |
| Microsoft.Sql/servers/databases/schemas/read | Schema's lezen SQL server-database |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Tabelkolommen voor lezen SQL server-database |
| Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/schemas/tables/read | Lees SQL server-databasetabellen |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Maken en beheren van de waarschuwing beveiligingsbeleid van SQL server-database |
| Microsoft.Sql/servers/databases/securityMetrics/* | Maken en beheren van SQL server-database beveiliging metrische gegevens |
| Microsoft.Sql/servers/databases/sensitivityLabels/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessments/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/* |  |
| Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/* |  |
| Microsoft.Sql/servers/firewallRules/* |  |
| Microsoft.Sql/servers/read | Lezen van de SQL-Servers |
| Microsoft.Sql/servers/securityAlertPolicies/* | Maken en beheren van de waarschuwing beleidsregels voor de beveiliging van SQL server |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="sql-server-contributor"></a>Inzender voor SQL Server
Kan SQL-servers en databases, maar niet hun beleid betrekking hebben op beveiliging beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
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
Kan storage-accounts beheren, maar geen toegang tot deze.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Alle autorisatie lezen |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/diagnosticSettings/* | Diagnostische instellingen beheren |
| Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Resource zoals opslagaccount of de SQL-database koppelt aan een subnet. |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/* | Maken en beheren van de storage-accounts |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="storage-account-key-operator-service-role"></a>De servicerol Sleuteloperator voor opslagaccounts
Sleuteloperators voor opslagaccounts kunnen een lijst met sleutels voor opslagaccounts maken en de sleutels opnieuw genereren

| **Acties** |  |
| --- | --- |
| Microsoft.Storage/storageAccounts/listkeys/action | Hiermee retourneert u de toegangssleutels voor het opgegeven opslagaccount. |
| Microsoft.Storage/storageAccounts/regeneratekey/action | Hiermee genereert u de toegangssleutels voor het opgegeven opslagaccount opnieuw. |

## <a name="support-request-contributor"></a>Inzender voor ondersteuningsaanvragen
Kunt maken en beheren van ondersteuningstickets bij het abonnementsbereik

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van rollen en roltoewijzingen |
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
Gebruikerstoegang tot Azure-resources kunt beheren

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
Beheren van virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Compute/availabilitySets/* | Maken en beheren van de compute-beschikbaarheidssets |
| Microsoft.Compute/locations/* | Maken en beheren van de compute-locaties |
| Microsoft.Compute/virtualMachines/* | Maken en beheren van virtuele machines |
| Microsoft.Compute/virtualMachineScaleSets/* | Maken en beheren van virtuele-machineschaalsets |
| Microsoft.DevTestLab/schedules/* |  |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Deelnemen aan netwerk application gateway back-end-adresgroepen |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Lid load balancer back-end-adresgroepen |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Lid load balancer inkomende NAT-pools |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Lid load balancer NAT-regels voor binnenkomende verbindingen |
| Microsoft.Network/loadBalancers/probes/join/action | U kunt met behulp van de tests van een load balancer. Bijvoorbeeld: met deze machtiging healthProbe-eigenschap van VM-scale set kan verwijzen naar de test. |
| Microsoft.Network/loadBalancers/read | Taakverdelers lezen |
| Microsoft.Network/locations/* | Maken en beheren van netwerklocaties |
| Microsoft.Network/networkInterfaces/* | Maken en beheren van de netwerkinterfaces |
| Microsoft.Network/networkSecurityGroups/join/action | Deelnemen aan netwerkbeveiligingsgroepen |
| Microsoft.Network/networkSecurityGroups/read | Lezen van netwerkbeveiligingsgroepen |
| Microsoft.Network/publicIPAddresses/join/action | Deelnemen aan het openbare IP-netwerkadressen |
| Microsoft.Network/publicIPAddresses/read | Lezen van het openbare IP-netwerkadressen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.Network/virtualNetworks/subnets/join/action | Virtueel netwerksubnetten toevoegen |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/listKeys/action | Lijst met sleutels voor opslagaccount |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

## <a name="virtual-machine-user-login"></a>Virtuele Machine gebruikersaanmelding
Gebruikers met deze functie hebben de mogelijkheid om aan te melden met een virtuele machine als een normale gebruiker.

| **Acties** |  |
| --- | --- |
| Microsoft.Compute/virtualMachines/login/action |  |
| Microsoft.Compute/virtualMachine/logon/action |  |

## <a name="web-plan-contributor"></a>Inzender voor webabonnementen
Kunt u web-abonnementen beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/serverFarms/* | Maken en beheren van serverfarms |

## <a name="website-contributor"></a>Inzender voor websites
Kunt beheren, websites, maar niet de web-abonnementen waaraan ze zijn verbonden

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Insights/alertRules/* | Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/components/* | Maken en beheren van Insights-onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Web/certificates/* | Maken en beheren van Websitecertificaten |
| Microsoft.Web/listSitesAssignedToHostName/read | Lezen van de sites die zijn toegewezen aan een host-naam |
| Microsoft.Web/serverFarms/join/action | Deelnemen aan serverfarms |
| Microsoft.Web/serverFarms/read | Serverfarms lezen |
| Microsoft.Web/sites/* | Maken en beheren van websites (maken van de site vereist ook schrijfmachtigingen in voor de bijbehorende App Service-Plan) |

## <a name="see-also"></a>Zie ook
* [Toegangsbeheer op basis van rollen](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
* [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aan uw behoeften toegang.
* [Maken van een geschiedenisrapport voor gewijzigde van toegang](role-based-access-control-access-change-history-report.md): bijhouden van wijzigen van roltoewijzingen in RBAC.
* [Probleemoplossing voor toegangsbeheer op basis van rollen](role-based-access-control-troubleshooting.md): Profiteer van tips voor het oplossen van veelvoorkomende problemen.
* [Machtigingen in Azure Security Center](../security-center/security-center-permissions.md)
