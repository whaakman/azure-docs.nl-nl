---
title: Acties en NotActions - Azure op rollen gebaseerde toegangsbeheer (RBAC) | Microsoft Docs
description: Dit onderwerp beschrijft de ingebouwde rollen voor op rollen gebaseerde toegangsbeheer (RBAC). De rollen worden continu toegevoegd, dus Controleer de nieuwheid documentatie.
services: active-directory
documentationcenter: 
author: andredm7
manager: femila
editor: 
ms.assetid: b547c5a5-2da2-4372-9938-481cb962d2d6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/28/2017
ms.author: andredm
ms.reviewer: 
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a5de00793621cfdecea887c53a22d482a25d1b8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="built-in-roles-for-azure-role-based-access-control"></a>Ingebouwde functies voor op rollen gebaseerd toegangsbeheer van Azure
Azure op rollen gebaseerde toegangsbeheer (RBAC) beschikt over de volgende ingebouwde rollen die kunnen worden toegewezen aan gebruikers, groepen en services. U kunt de definities van de ingebouwde rollen niet wijzigen. U kunt echter maken [aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md) aanpassen aan de specifieke behoeften van uw organisatie.

## <a name="roles-in-azure"></a>Rollen in Azure
De volgende tabel bevat een korte beschrijving van de ingebouwde rollen. Klik op de naam van de rol voor een gedetailleerd overzicht van **acties** en **notactions** voor de rol. De **acties** eigenschap geeft u de toegestane acties op Azure-resources. Actie tekenreeksen kunnen jokertekens gebruiken. De **notactions** eigenschap geeft u de acties die zijn uitgesloten van de toegestane acties.

De actie definieert wat voor soort bewerkingen die u kunt uitvoeren op een bepaald brontype. Bijvoorbeeld:
- **Schrijven** kunt u PUT, POST, PATCH en DELETE-bewerkingen uit te voeren.
- **Lees** kunt u GET-bewerkingen uitvoeren.

In dit artikel wordt alleen de verschillende rollen die vandaag bestaan. Wanneer u een rol aan een gebruiker toewijst, echter kunt u beperken de toegestane acties verder met het definiëren van een scope. Dit is handig als u ervoor iemand een medewerker van de Website, maar alleen voor één resourcegroep.

> [!NOTE]
> De Azure roldefinities zijn voortdurend in ontwikkeling. In dit artikel is actueel zo mogelijk, maar u vindt altijd de meest recente definities van de rollen in Azure PowerShell. Gebruik de [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) cmdlet voor een lijst met alle beschikbare rollen. U kunt meteen voor het gebruik van een specifieke rol `(get-azurermroledefinition "<role name>").actions` of `(get-azurermroledefinition "<role name>").notactions` indien van toepassing. Gebruik [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) activiteiten van de lijst van specifieke Azure-resourceproviders.


| Rolnaam | Beschrijving |
| --- | --- |
| [API Management-Service Inzender](#api-management-service-contributor) |API Management-service en de API's kunt beheren |
| [Rol Operator API Management-Service](#api-management-service-operator-role) | API Management-service, maar niet de API's zelf kunt beheren |
| [Rol van API Management-Service lezer](#api-management-service-reader-role) | Alleen-lezen toegang tot API Management-service en API 's |
| [Application Insights-Onderdeelinzender](#application-insights-component-contributor) |Application Insights-onderdelen kunt beheren |
| [Automation-Operator](#automation-operator) |Kunnen starten, stoppen, onderbreken en hervatten van taken |
| [Back-up Inzender](#backup-contributor) | Back-up in de Recovery Services-kluis kunnen beheren |
| [Back-upoperator](#backup-operator) | Back-up met uitzondering van het verwijderen van back-up, in Recovery Services-kluis kunnen beheren |
| [Back-lezer](#backup-reader) | Alle back-management-services kunt weergeven  |
| [Lezer facturering](#billing-reader) | Alle factureringsgegevens kunt weergeven  |
| [BizTalk Inzender](#biztalk-contributor) |BizTalk services kunt beheren |
| [ClearDB MySQL DB Contributor](#cleardb-mysql-db-contributor) |ClearDB MySQL-databases kunt beheren |
| [Inzender](#contributor) |Kunnen alles beheren behalve toegang. |
| [Data Factory Inzender](#data-factory-contributor) |Kan maken en beheren van gegevensfactory's en onderliggende resources binnen deze. |
| [DevTest Labs gebruiker](#devtest-labs-user) |Kunnen alles weergeven en verbinding maken, starten, opnieuw opstarten en afsluiten van de virtuele machines |
| [DNS-Zone Inzender](#dns-zone-contributor) |DNS-zones en -records beheren |
| [Azure Cosmos DB Account Inzender](#documentdb-account-contributor) |Azure DB die Cosmos-accounts kunnen beheren |
| [Intelligente systemen Account Inzender](#intelligent-systems-account-contributor) |Intelligente systemen accounts kunnen beheren |
| Logic App Inzender | Kan alle aspecten van een logische App beheren, maar niet een nieuwe maken. |
| Logic App-Operator |Kunt starten en stoppen van werkstromen die zijn gedefinieerd in een logische App. |
| [Bewaking van de lezer](#monitoring-reader) |Alle bewakingsgegevens kunnen lezen |
| [Inzender bewaking](#monitoring-contributor) |Kunnen lezen bewakingsgegevens en controle-instellingen bewerken |
| [Inzender voor netwerken](#network-contributor) |Alle netwerkbronnen kunt beheren |
| [Nieuwe Relic APM-Account Inzender](#new-relic-apm-account-contributor) |Nieuwe Relic prestaties Toepassingsbeheer accounts en -toepassingen kunnen beheren |
| [Eigenaar](#owner) |Kunnen alles beheren, inclusief toegang |
| [Lezer](#reader) |Alles weergeven, maar u kunt geen wijzigingen aanbrengen |
| [Redis-Cache Inzender](#redis-cache-contributor) |Redis-caches kunt beheren |
| [Scheduler-taak verzamelingen Inzender](#scheduler-job-collections-contributor) |Scheduler-taakverzamelingen kunt beheren |
| [Search Service Inzender](#search-service-contributor) |Search-services kunt beheren |
| [Beveiligingsbeheer](#security-manager) |Beveiligingsonderdelen, beveiligingsbeleid en virtuele machines beheren |
| [Site Recovery Inzender](#site-recovery-contributor) | Site Recovery kunt beheren in Recovery Services-kluis |
| [Site Recovery-Operator](#site-recovery-operator) | Failover en failback bewerkingen Site Recovery in de Recovery Services-kluis kunnen beheren |
| [Site Recovery lezer](#site-recovery-reader) | Alle beheerbewerkingen van Site Recovery kunt weergeven  |
| [SQL DB Contributor](#sql-db-contributor) |Kan SQL-databases, maar niet hun beleid betrekking hebben op beveiliging beheren |
| [SQL Security Manager](#sql-security-manager) |Het beleid betrekking hebben op de beveiliging van SQL-servers en databases kunt beheren |
| [SQL Server Inzender](#sql-server-contributor) |Kan SQL-servers en databases, maar niet hun beleid betrekking hebben op beveiliging beheren |
| [Klassieke Storage Account Inzender](#classic-storage-account-contributor) |Klassieke opslagaccounts kunt beheren |
| [Storage-Account Inzender](#storage-account-contributor) |Storage-accounts kunnen beheren |
| [Ondersteuning voor aanvraag Inzender](#support-request-contributor) | Kunt maken en beheren van aanvragen voor ondersteuning |
| [Beheerder voor gebruikerstoegang](#user-access-administrator) |Gebruikerstoegang tot Azure-resources kunt beheren |
| [Klassieke Virtual Machine Contributor](#classic-virtual-machine-contributor) |Klassieke virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden kunt beheren |
| [Virtual Machine Contributor](#virtual-machine-contributor) |Beheren van virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden |
| [Inzender voor klassieke netwerken](#classic-network-contributor) |Klassieke virtuele netwerken en gereserveerde IP's kunt beheren |
| [Web Plan Inzender](#web-plan-contributor) |Kunt u web-abonnementen beheren |
| [Website Inzender](#website-contributor) |Kunt beheren, websites, maar niet de web-abonnementen waaraan ze zijn verbonden |

## <a name="role-permissions"></a>Rolmachtigingen
De volgende tabellen beschrijven de specifieke machtigingen toegekend aan elke rol. Het kan hierbij gaan **acties**, die machtigingen geven en **NotActions**, die ze beperken.

### <a name="api-management-service-contributor"></a>API Management-Service Inzender
API Management-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/* |Maken en beheren van API Management-service |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van rollen en roltoewijzingen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="api-management-service-operator-role"></a>Rol Operator API Management-Service
API Management-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Lees API Management-Service-exemplaren |
| Microsoft.ApiManagement/Service/backup/action | Back-up API Management-Service op de opgegeven container in een opslagaccount opgegeven gebruiker |
| Microsoft.ApiManagement/Service/delete | Verwijderen van een exemplaar van API Management-Service |
| Microsoft.ApiManagement/Service/managedeployments/action | Wijziging SKU/eenheden; toevoegen of verwijderen van regionale implementaties van API Management-Service |
| Microsoft.ApiManagement/Service/read | Lezen van metagegevens voor een exemplaar van API Management-Service |
| Microsoft.ApiManagement/Service/restore/action | API Management-Service van de opgegeven container in het opgegeven opslagaccount van een gebruiker herstellen |
| Microsoft.ApiManagement/Service/updatehostname/action | Instellen, bijwerken of verwijderen van aangepaste domeinnamen voor een API Management-Service |
| Microsoft.ApiManagement/Service/write | Maak een nieuw exemplaar van API Management-Service |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van rollen en roltoewijzingen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="api-management-service-reader-role"></a>Rol van API Management-Service lezer
API Management-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.ApiManagement/Service/*/read | Lees API Management-Service-exemplaren |
| Microsoft.ApiManagement/Service/read | Lezen van metagegevens voor een exemplaar van API Management-Service |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van rollen en roltoewijzingen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="application-insights-component-contributor"></a>Application Insights-Onderdeelinzender
Application Insights-onderdelen kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.Insights/components/* |Maken en beheren van Insights-onderdelen |
| Microsoft.Insights/webtests/* |Maken en beheren van webtests |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="automation-operator"></a>Automation-operator
Kunnen starten, stoppen, onderbreken en hervatten van taken

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en roltoewijzingen |
| Microsoft.Automation/automationAccounts/jobs/read |Automation-account taken lezen |
| Microsoft.Automation/automationAccounts/jobs/resume/action |Een automation-account-taak hervatten |
| Microsoft.Automation/automationAccounts/jobs/stop/action |Een automation-account-taak stoppen |
| Microsoft.Automation/automationAccounts/jobs/streams/read |Automation-account taak streams lezen |
| Microsoft.Automation/automationAccounts/jobs/suspend/action |Een automation-account-taak onderbreken |
| Microsoft.Automation/automationAccounts/jobs/write |Automation-account taken schrijven |
| Microsoft.Automation/automationAccounts/jobSchedules/read |Een automation-account-taakschema lezen |
| Microsoft.Automation/automationAccounts/jobSchedules/write |Een automation-account-taakschema lezen |
| Microsoft.Automation/automationAccounts/read |Automation-accounts lezen |
| Microsoft.Automation/automationAccounts/runbooks/read |Automation-runbooks lezen |
| Microsoft.Automation/automationAccounts/schedules/read |Automation-account planningen lezen |
| Microsoft.Automation/automationAccounts/schedules/write |Automation-account schema's schrijven |
| Microsoft.Insights/components/* |Maken en beheren van Insights-onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="backup-contributor"></a>Back-up Inzender
Alle back-management-acties, met uitzondering van de Recovery Services-kluis maken en toegang geven aan anderen kunt beheren

| **Acties** | |
| --- | --- |
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
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="backup-operator"></a>Back-upoperator
Alle back-acties, behalve voor het maken van kluizen, back-up en geven toegang tot andere verwijderen kunt beheren

| **Acties** | |
| --- | --- |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | Lezen van de resultaten van de bewerking op de back-management |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Resultaat van de bewerking op de beveiliging containers lezen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Back-upbewerking op een item voor back-ups op aanvraag uitvoeren |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Lees-resultaat van bewerking uitgevoerd op een back-up item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read | Leesstatus van bewerking uitgevoerd op een back-up item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Lezen van het herstelpunt van een back-up-item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Een herstelbewerking met behulp van een herstelpunt wordt gemaakt van een back-up item uitvoeren |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Een back-item maken |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Back-upitems houden-containers gelezen |
| Microsoft.RecoveryServices/Vaults/backupJobs/* | Maken en beheren van back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Back-uptaken exporteren naar een excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read | Lees metagegevens met betrekking tot back-management |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Maken en beheren van de resultaten van de Backup-beheertaken uit te voeren |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | Resultaten van de bewerkingen die worden uitgevoerd op de back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read | Back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Maken en beheren van items die u kunnen een back-up |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Alleen een back-up containers bewaren van back-artikelen |
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
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="backup-reader"></a>Back-lezer
Back-beheer in de Recovery Services-kluis kunt bewaken

| **Acties** | |
| --- | --- |
| Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read  | Lezen van de resultaten van de bewerking op de back-management |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read  | Resultaat van de bewerking op de beveiliging containers lezen |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read  | Lees-resultaat van bewerking uitgevoerd op een back-up item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationStatus/read  | Leesstatus van bewerking uitgevoerd op een back-up item |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read  | Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read  | Back-upitems houden-containers gelezen |
| Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read  | Lezen van de resultaten van de back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobs/read  | Lezen van de back-uptaken |
| Microsoft.RecoveryServices/Vaults/backupJobsExport/action | Back-uptaken exporteren naar een excel |
| Microsoft.RecoveryServices/Vaults/backupManagementMetaData/read  | Lees metagegevens met betrekking tot back-management |
| Microsoft.RecoveryServices/Vaults/backupOperationResults/read  | Lees de back-beheer Bewerkingsresultaten |
| Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read  | Resultaten van de bewerkingen die worden uitgevoerd op de back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupPolicies/read  | Back-upbeleid lezen |
| Microsoft.RecoveryServices/Vaults/backupProtectedItems/read  |  Alleen een back-up items |
| Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read  | Alleen een back-up containers bewaren van back-artikelen |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Lezen van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/read  | Recovery services-kluizen lezen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Resultaat van de detectiebewerking aan voor het ophalen van een nieuw gemaakt containers lezen |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | De resultaten van het lezen van bewerking uitgevoerd op de geregistreerde items van de kluis |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Geregistreerde items van de kluis lezen |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Lees de informatie over het gebruik van de Recovery Services-kluis |

### <a name="billing-reader"></a>Lezer facturering
Alle factureringsgegevens kunt weergeven

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en roltoewijzingen |
| Microsoft.Billing/*/read |Factureringsgegevens lezen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="biztalk-contributor"></a>BizTalk Inzender
BizTalk services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en roltoewijzingen |
| Microsoft.BizTalkServices/BizTalk/* |Maken en beheren van BizTalk services |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="cleardb-mysql-db-contributor"></a>ClearDB MySQL DB Contributor
ClearDB MySQL-databases kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |
| successbricks.cleardb/databases/* |Maken en beheren van ClearDB MySQL-databases |

### <a name="contributor"></a>Inzender
Kunnen alles beheren behalve toegang

| **Acties** |  |
| --- | --- |
| * |Maken en beheren van bronnen van alle typen |

| **NotActions** |  |
| --- | --- |
| Microsoft.Authorization/*/Delete |Rollen en roltoewijzingen verwijderen niet |
| Microsoft.Authorization/*/Write |Kan de functies en roltoewijzingen niet maken |

### <a name="data-factory-contributor"></a>Data Factory Inzender
Maken en beheren van de gegevensfactory en de onderliggende resources binnen deze.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.DataFactory/dataFactories/* |Maken en beheren van de gegevensfactory en de onderliggende resources binnen deze. |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="devtest-labs-user"></a>DevTest Labs gebruiker
Kunnen alles weergeven en verbinding maken, starten, opnieuw opstarten en afsluiten van de virtuele machines

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Compute/availabilitySets/read |Lezen van de eigenschappen van beschikbaarheidssets |
| Microsoft.Compute/virtualMachines/*/read |Lezen van de eigenschappen van een virtuele machine (VM-grootten, runtimestatus, VM-extensies, enz.) |
| Microsoft.Compute/virtualMachines/deallocate/action |Toewijzing van virtuele machines |
| Microsoft.Compute/virtualMachines/read |De eigenschappen van een virtuele machine lezen |
| Microsoft.Compute/virtualMachines/restart/action |Virtuele machines opnieuw opstarten |
| Microsoft.Compute/virtualMachines/start/action |Virtuele machines starten |
| Microsoft.DevTestLab/*/read |Lezen van de eigenschappen van een testlab |
| Microsoft.DevTestLab/labs/createEnvironment/action |Een testomgeving maken |
| Microsoft.DevTestLab/labs/formulas/delete |Formules verwijderen |
| Microsoft.DevTestLab/labs/formulas/read |Formules lezen |
| Microsoft.DevTestLab/labs/formulas/write |Toevoegen of wijzigen van formules |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action |Labbeleidsregels evalueren |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Deelnemen aan een load balancer back-end-adresgroep |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Deelnemen aan een load balancer binnenkomende NAT-regel |
| Microsoft.Network/networkInterfaces/*/read |Lezen van de eigenschappen van een netwerkinterface (bijvoorbeeld alle load balancers die de netwerkinterface deel van uitmaakt) |
| Microsoft.Network/networkInterfaces/join/action |Een virtuele Machine toevoegen aan een netwerkinterface |
| Microsoft.Network/networkInterfaces/read |Netwerkinterfaces lezen |
| Microsoft.Network/networkInterfaces/write |Netwerkinterfaces schrijven |
| Microsoft.Network/publicIPAddresses/*/read |Lezen van de eigenschappen van een openbaar IP-adres |
| Microsoft.Network/publicIPAddresses/join/action |Lid worden van een openbaar IP-adres |
| Microsoft.Network/publicIPAddresses/read |Lezen van het openbare IP-netwerkadressen |
| Microsoft.Network/virtualNetworks/subnets/join/action |Een virtueel netwerk koppelen |
| Microsoft.Resources/deployments/operations/read |Implementatie leesbewerkingen |
| Microsoft.Resources/deployments/read |Lezen van implementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/listKeys/action |Lijst met sleutels voor opslagaccount |

### <a name="dns-zone-contributor"></a>DNS-Zone Inzender
Kunnen DNS-zones en -records beheren.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/ \* /lezen |Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/\* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/dnsZones/\* |DNS-zones en records maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read |Lezen van de status van de resources |
| Microsoft.Resources/deployments/\* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/\* |Maken en ondersteuningstickets beheren |

### <a name="azure-cosmos-db-account-contributor"></a>Azure Cosmos DB Account Inzender
Azure DB die Cosmos-accounts kunnen beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.DocumentDb/databaseAccounts/* |Maken en beheren van de DocumentDB-accounts |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="intelligent-systems-account-contributor"></a>Intelligente systemen Account Inzender
Intelligente systemen accounts kunnen beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.IntelligentSystems/accounts/* |Intelligente systemen rekeningen maken en beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="monitoring-reader"></a>Bewaking van de lezer
Alle bewakingsgegevens (metrische gegevens, Logboeken, enz.) kunnen worden gelezen. Zie ook [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Acties** |  |
| --- | --- |
| * / lezen |Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.OperationalInsights/workspaces/search/action |Log Analytics zoekgegevens |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="monitoring-contributor"></a>Inzender bewaking
Kan alle bewakingsgegevens lezen en controle-instellingen bewerken. Zie ook [aan de slag met rollen, machtigingen en -beveiliging met Azure Monitor](/monitoring-and-diagnostics/monitoring-roles-permissions-security.md#built-in-monitoring-roles).

| **Acties** |  |
| --- | --- |
| * / lezen |Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.Insights/AlertRules/* |Waarschuwingsregels voor lezen, schrijven, verwijderen. |
| Microsoft.Insights/components/* |Onderdelen van de Application Insights lezen, schrijven, verwijderen. |
| Microsoft.Insights/DiagnosticSettings/* |Diagnostische instellingen voor lezen, schrijven, verwijderen. |
| Microsoft.Insights/eventtypes/* |Activiteitenlogboek gebeurtenissen (management gebeurtenissen) in een abonnement weergeven. Deze machtiging is van toepassing op de portal zowel programmatische toegang tot het activiteitenlogboek. |
| Microsoft.Insights/LogDefinitions/* |Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Lijst met logboekbestanden categorieën in het activiteitenlogboek. |
| Microsoft.Insights/MetricDefinitions/* |Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen. |
| Microsoft.Insights/Metrics/* |Metrische gegevens voor een resource lezen. |
| Microsoft.Insights/Register/Action |Registreer de provider Microsoft.Insights. |
| Microsoft.Insights/webtests/* |Webtests Application Insights voor lezen, schrijven, verwijderen. |
| Microsoft.OperationalInsights/workspaces/intelligencepacks/* |Lezen/schrijven/verwijderen Log Analytics-oplossing packs. |
| Microsoft.OperationalInsights/workspaces/savedSearches/* |Lezen/schrijven/verwijderen Log Analytics opgeslagen zoekopdrachten. |
| Microsoft.OperationalInsights/workspaces/search/action |Log Analytics-werkruimten zoeken. |
| Microsoft.OperationalInsights/workspaces/sharedKeys/action |Lijst met sleutels voor een werkruimte voor logboekanalyse. |
| Microsoft.OperationalInsights/workspaces/storageinsightconfigs/* |Lezen/schrijven/verwijderen logboekanalyse inzicht opslagconfiguraties. |

### <a name="network-contributor"></a>Network Contributor
Alle netwerkbronnen kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/* |Maken en beheren van netwerken |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="new-relic-apm-account-contributor"></a>Inzender voor het New Relic APM-account
Nieuwe Relic prestaties Toepassingsbeheer accounts en -toepassingen kunnen beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |
| NewRelic.APM/accounts/* |New Relic toepassingsprestaties management rekeningen maken en beheren |

### <a name="owner"></a>Eigenaar
Kunnen alles beheren, inclusief toegang

| **Acties** |  |
| --- | --- |
| * |Maken en beheren van bronnen van alle typen |

### <a name="reader"></a>Lezer
Alles weergeven, maar u kunt geen wijzigingen aanbrengen

| **Acties** |  |
| --- | --- |
| * / lezen |Bronnen van alle typen, met uitzondering van geheimen lezen. |

### <a name="redis-cache-contributor"></a>Redis-Cache Inzender
Redis-caches kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Cache/redis/* |Maken en beheren van Redis-caches |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="scheduler-job-collections-contributor"></a>Scheduler-taak verzamelingen Inzender
Scheduler-taakverzamelingen kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Scheduler/jobcollections/* |Maken en beheren van jobverzamelingen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="search-service-contributor"></a>Search Service Inzender
Search-services kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Search/searchServices/* |Maken en beheren van de search-services |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="security-manager"></a>Beveiligingsbeheer
Beveiligingsonderdelen, beveiligingsbeleid en virtuele machines beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.ClassicCompute/*/read |Lezen van configuratiegegevens klassieke virtuele machines berekenen |
| Microsoft.ClassicCompute/virtualMachines/*/write |Configuratie voor virtuele machines schrijven |
| Microsoft.ClassicNetwork/*/read |Configuratie-informatie lezen over klassiek netwerk |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Security/* |Beveiligingsonderdelen en -beleid maken en beheren |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="site-recovery-contributor"></a>Site Recovery Inzender
Alle beheeracties van Site Recovery, met uitzondering van de Recovery Services-kluis maken en toewijzen van rechten aan andere gebruikers kunnen beheren

| **Acties** | |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.RecoveryServices/Vaults/certificates/write | Het referentiecertificaat van de kluis bijgewerkt |
| Microsoft.RecoveryServices/Vaults/extendedInformation/* | Maken en beheren van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd  | Meldingsconfiguratie voor lezen Recovery services-kluis |
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
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="site-recovery-operator"></a>Site Recovery-Operator
Kan Failover en Failback kan maar niet andere Site Recovery management acties worden uitgevoerd of toegang toewijzen aan andere gebruikers

| **Acties** | |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.Insights/alertRules/* | Maken en beheren van regels voor waarschuwingen |
| Microsoft.Network/virtualNetworks/read | Virtuele netwerken gelezen |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read | Lezen van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/*  | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd  | Meldingsconfiguratie voor lezen Recovery services-kluis |
| Microsoft.RecoveryServices/Vaults/read | Lees Recovery Services-kluizen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | De bewerkingsstatus en resultaat voor het uitvoeren van een ingediende lezen |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Geregistreerd voor een resource-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Replicatie-instellingen voor waarschuwingen lezen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read | Replicatiegebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Consistentie van de fabrics controleren |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read | Lees replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ reassociateGateway/actie | Replicatie-gateway opnieuw koppelen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Replicatie fabric certificaat vernieuwen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Lezen van replicatie fabric-netwerken |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks replicationNetworkMappings/leestijd | Lees replicatie fabric netwerktoewijzing |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers leestijd | Beveiliging-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectableItems/leestijd | Lijst met alle beveiligbare objecten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/actie | Toepassen van een specifiek herstelpunt |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/failoverCommit/actie | Failover voor een mislukte doorvoeren boven item |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/plannedFailover/actie | De geplande failover voor een beveiligde item starten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectedItems/leestijd | Lijst met alle beveiligde items |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectedItems/recoveryPoints/leestijd | Lijst met beschikbare herstelpunten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/repairReplication/actie | Replicatie voor een beveiligde item herstellen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/beveiligt/actie | Start voor een beveiligde item opnieuw te beveiligen|
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailover, actie | Testfailover van een beveiligde item starten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/actie | Het opruimen van een testfailover starten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/unplannedFailover/actie | Niet-geplande failover van een beveiligde item starten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers/replicationProtectedItems/updateMobilityService/actie | De mobility-service bijwerken |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectionContainerMappings/leestijd | Beveiliging lezen container toewijzingen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders leestijd | Lees Recovery Services-providers |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ refreshProvider-replicationRecoveryServicesProviders/actie | Vernieuw Recovery Services-provider |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications leestijd | Alleen opslagclassificaties voor replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications replicationStorageClassificationMappings/leestijd | Toewijzingen voor classificatie van bestanden lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Lees de vCenter-gegevens geregistreerd |
| Microsoft.RecoveryServices/vaults/replicationJobs/* | Maken en beheren van replicatietaken |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read | Lezen van replicatiebeleid |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ failoverCommit/actie | Failover voor een herstelplan-failover doorvoeren |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ plannedFailover/actie | Failover van een herstelplan starten |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Plannen voor herstel te lezen |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Start opnieuw beveiligen van een herstelplan |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Testfailover van een herstelplan starten |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ testFailoverCleanup/actie | Start het opruimen van een herstelplan-testfailover |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/ unplannedFailover/actie | Niet-geplande failover van een herstelplan starten |
| Microsoft.RecoveryServices/Vaults/storageConfig/read | Lezen van configuratie van de opslag van een Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read | Lees Recovery Services-kluis token-informatie |
| Microsoft.RecoveryServices/Vaults/usages/read | Details van het gebruik van een Recovery Services-kluis lezen |
| Microsoft.ResourceHealth/availabilityStatuses/read | Status van de bronnen lezen |
| Microsoft.Resources/deployments/* | Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/read | Lezen van de storage-accounts |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |

### <a name="site-recovery-reader"></a>Site Recovery lezer
Kan de status van de Site Recovery in de Recovery Services-kluis bewaken en ondersteuningstickets genereren

| **Acties** | |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van rollen en roltoewijzingen |
| Microsoft.RecoveryServices/Vaults/extendedInformation/read  | Lezen van uitgebreide informatie die betrekking hebben op de kluis |
| Microsoft.RecoveryServices/Vaults/monitoringAlerts/read  | Waarschuwingen voor de Recovery services-kluis lezen |
| Microsoft.RecoveryServices/Vaults/monitoringConfigurations/ notificationConfiguration leestijd  | Meldingsconfiguratie voor lezen Recovery services-kluis |
| Microsoft.RecoveryServices/Vaults/read  | Lees Recovery Services-kluizen |
| Microsoft.RecoveryServices/Vaults/refreshContainers/read  | Beheren detectiebewerking aan voor het ophalen van een nieuw gemaakt containers |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read  | De bewerkingsstatus en resultaat voor het uitvoeren van een ingediende lezen |
| Microsoft.RecoveryServices/Vaults/registeredIdentities/read  | Geregistreerd voor een resource-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Replicatie-instellingen voor waarschuwingen lezen |
| Microsoft.RecoveryServices/vaults/replicationEvents/read  | Replicatiegebeurtenissen lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/read  | Lees replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read  | Lezen van replicatie fabric-netwerken |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationNetworks replicationNetworkMappings/leestijd  | Lees replicatie fabric netwerktoewijzing |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers leestijd  |  Beveiliging-containers gelezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectableItems/leestijd  | Lijst met alle beveiligbare objecten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectedItems/leestijd  | Lijst met alle beveiligde items |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectedItems/recoveryPoints/leestijd  | Lijst met beschikbare herstelpunten |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationProtectionContainers replicationProtectionContainerMappings/leestijd  | Beveiliging lezen container toewijzingen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationRecoveryServicesProviders leestijd  | Lees Recovery Services-providers |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications leestijd  | Alleen opslagclassificaties voor replicatie-fabrics |
| Microsoft.RecoveryServices/vaults/replicationFabrics/ replicationStorageClassifications replicationStorageClassificationMappings/leestijd  |  Toewijzingen voor classificatie van bestanden lezen |
| Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read  |  Lees de vCenter-gegevens geregistreerd |
| Microsoft.RecoveryServices/vaults/replicationJobs/read  |  Status van replicatietaken lezen |
| Microsoft.RecoveryServices/vaults/replicationPolicies/read  |  Lezen van replicatiebeleid |
| Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read  |  Plannen voor herstel te lezen |
| Microsoft.RecoveryServices/Vaults/storageConfig/read  |  Lezen van configuratie van de opslag van een Recovery Services-kluis |
| Microsoft.RecoveryServices/Vaults/tokenInfo/read  |  Lees Recovery Services-kluis token-informatie |
| Microsoft.RecoveryServices/Vaults/usages/read  |  Details van het gebruik van een Recovery Services-kluis lezen |
| Microsoft.Support/*  |  Maken en ondersteuningstickets beheren |

### <a name="sql-db-contributor"></a>SQL DB Contributor
Kan SQL-databases, maar niet hun beleid betrekking hebben op beveiliging beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van rollen en functie toewijzingen |
| Microsoft.Insights/alertRules/* |Maken en beheren van regels voor waarschuwingen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Sql/servers/databases/* |Maken en beheren van de SQL-databases |
| Microsoft.Sql/servers/read |Lezen van de SQL-Servers |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Controlebeleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditingSettings/* |Controle-instellingen kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditRecords/read |Kan de controlerecords niet lezen. |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Beleid voor de verbinding kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Gegevensmaskering beleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Waarschuwing beveiligingsbeleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityMetrics/* |Metrische gegevens voor beveiliging kan niet worden bewerkt. |

### <a name="sql-security-manager"></a>SQL Security Manager
Het beleid betrekking hebben op de beveiliging van SQL-servers en databases kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lees Microsoft autorisatie |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Sql/servers/auditingPolicies/* |SQL server audit beleidsregels maken en beheren |
| Microsoft.Sql/servers/auditingSettings/* |Maken en beheren van controle instelling van SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Maken en te beheren controlebeleid van de SQL server-database |
| Microsoft.Sql/servers/databases/auditingSettings/* |Maken en beheren van SQL server-database controle-instellingen |
| Microsoft.Sql/servers/databases/auditRecords/read |Lees AuditRecords |
| Microsoft.Sql/servers/databases/connectionPolicies/* |SQL server-database verbinding beleidsregels maken en beheren |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |Maken en beheren van SQL server-databasegegevens maskeren van beleid |
| Microsoft.Sql/servers/databases/read |Lees SQL-databases |
| Microsoft.Sql/servers/databases/schemas/read |Schema's lezen SQL server-database |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read |Tabelkolommen voor lezen SQL server-database |
| Microsoft.Sql/servers/databases/schemas/tables/read |Lees SQL server-databasetabellen |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Maken en beheren van de waarschuwing beveiligingsbeleid van SQL server-database |
| Microsoft.Sql/servers/databases/securityMetrics/* |Maken en beheren van SQL server-database beveiliging metrische gegevens |
| Microsoft.Sql/servers/read |Lezen van de SQL-Servers |
| Microsoft.Sql/servers/securityAlertPolicies/* |Maken en beheren van de waarschuwing beleidsregels voor de beveiliging van SQL server |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="sql-server-contributor"></a>SQL Server Inzender
Kan SQL-servers en databases, maar niet hun beleid betrekking hebben op beveiliging beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Sql/servers/* |Maken en beheren van de SQL-servers |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

| **NotActions** |  |
| --- | --- |
| Microsoft.Sql/servers/auditingPolicies/* |Controlebeleid van SQL server kan niet worden bewerkt. |
| Microsoft.Sql/servers/auditingSettings/* |Kan geen SQL server-controle-instellingen bewerken |
| Microsoft.Sql/servers/databases/auditingPolicies/* |Controlebeleid van de SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/auditingSettings/* |Kan geen SQL server-database controle-instellingen bewerken |
| Microsoft.Sql/servers/databases/auditRecords/read |Kan de controlerecords niet lezen. |
| Microsoft.Sql/servers/databases/connectionPolicies/* |Beleidsregels voor SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* |SQL server-database gegevensmaskering beleid kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* |Waarschuwing beveiligingsbeleid van SQL server-database kan niet worden bewerkt. |
| Microsoft.Sql/servers/databases/securityMetrics/* |Metrische gegevens van SQL server-database beveiliging kan niet worden bewerkt. |
| Microsoft.Sql/servers/securityAlertPolicies/* |Waarschuwing beleidsregels voor de beveiliging van SQL server kan niet worden bewerkt. |

### <a name="classic-storage-account-contributor"></a>Klassieke Storage Account Inzender
Klassieke opslagaccounts kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.ClassicStorage/storageAccounts/* |Maken en beheren van de storage-accounts |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="storage-account-contributor"></a>Storage-Account Inzender
Kan storage-accounts beheren, maar geen toegang tot deze.

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Alle autorisatie lezen |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/diagnosticSettings/* |Diagnostische instellingen beheren |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/* |Maken en beheren van de storage-accounts |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="support-request-contributor"></a>Ondersteuning voor aanvraag Inzender
Kunt maken en beheren van ondersteuningstickets bij het abonnementsbereik

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read | Lezen van autorisatie |
| Microsoft.Support/* | Maken en ondersteuningstickets beheren |
| Microsoft.Resources/subscriptions/resourceGroups/read | Lezen van rollen en roltoewijzingen |

### <a name="user-access-administrator"></a>Beheerder van gebruikerstoegang
Gebruikerstoegang tot Azure-resources kunt beheren

| **Acties** |  |
| --- | --- |
| * / lezen |Bronnen van alle typen, met uitzondering van geheimen lezen. |
| Microsoft.Authorization/* |Machtigingen beheren |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="classic-virtual-machine-contributor"></a>Klassieke Virtual Machine Contributor
Klassieke virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.ClassicCompute/domainNames/* |Maken en beheren van domeinnamen klassieke compute |
| Microsoft.ClassicCompute/virtualMachines/* |Maken en beheren van virtuele machines |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action |Deelnemen aan netwerkbeveiligingsgroepen |
| Microsoft.ClassicNetwork/reservedIps/link/action |Gereserveerde IP's koppelen |
| Microsoft.ClassicNetwork/reservedIps/read |Lees gereserveerde IP-adressen |
| Microsoft.ClassicNetwork/virtualNetworks/join/action |Virtuele netwerken koppelen |
| Microsoft.ClassicNetwork/virtualNetworks/read |Virtuele netwerken gelezen |
| Microsoft.ClassicStorage/storageAccounts/disks/read |Schijven met opslagruimte lezen |
| Microsoft.ClassicStorage/storageAccounts/images/read |Installatiekopieën van de storage-account lezen |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action |Lijst met sleutels voor opslagaccount |
| Microsoft.ClassicStorage/storageAccounts/read |Klassieke opslagaccounts lezen |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="virtual-machine-contributor"></a>Virtual Machine Contributor
Beheren van virtuele machines, maar niet het virtuele netwerk of opslag account waaraan ze zijn verbonden

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.Compute/availabilitySets/* |Maken en beheren van de compute-beschikbaarheidssets |
| Microsoft.Compute/locations/* |Maken en beheren van de compute-locaties |
| Microsoft.Compute/virtualMachines/* |Maken en beheren van virtuele machines |
| Microsoft.Compute/virtualMachineScaleSets/* |Maken en beheren van virtuele-machineschaalsets |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action |Deelnemen aan netwerk application gateway back-end-adresgroepen |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action |Lid load balancer back-end-adresgroepen |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action |Lid load balancer inkomende NAT-pools |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action |Lid load balancer NAT-regels voor binnenkomende verbindingen |
| Microsoft.Network/loadBalancers/read |Taakverdelers lezen |
| Microsoft.Network/locations/* |Maken en beheren van netwerklocaties |
| Microsoft.Network/networkInterfaces/* |Maken en beheren van de netwerkinterfaces |
| Microsoft.Network/networkSecurityGroups/join/action |Deelnemen aan netwerkbeveiligingsgroepen |
| Microsoft.Network/networkSecurityGroups/read |Lezen van netwerkbeveiligingsgroepen |
| Microsoft.Network/publicIPAddresses/join/action |Deelnemen aan het openbare IP-netwerkadressen |
| Microsoft.Network/publicIPAddresses/read |Lezen van het openbare IP-netwerkadressen |
| Microsoft.Network/virtualNetworks/read |Virtuele netwerken gelezen |
| Microsoft.Network/virtualNetworks/subnets/join/action |Virtueel netwerksubnetten toevoegen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Storage/storageAccounts/listKeys/action |Lijst met sleutels voor opslagaccount |
| Microsoft.Storage/storageAccounts/read |Lezen van de storage-accounts |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="classic-network-contributor"></a>Klassieke Network Contributor
Klassieke virtuele netwerken en gereserveerde IP's kunt beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.ClassicNetwork/* |Maken en beheren van klassieke netwerken |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |

### <a name="web-plan-contributor"></a>Web Plan Inzender
Kunt u web-abonnementen beheren

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |
| Microsoft.Web/serverFarms/* |Maken en beheren van serverfarms |

### <a name="website-contributor"></a>Website Inzender
Kunt beheren, websites, maar niet de web-abonnementen waaraan ze zijn verbonden

| **Acties** |  |
| --- | --- |
| Microsoft.Authorization/*/read |Lezen van autorisatie |
| Microsoft.Insights/alertRules/* |Maken en beheren van Insights waarschuwingsregels |
| Microsoft.Insights/components/* |Maken en beheren van Insights-onderdelen |
| Microsoft.ResourceHealth/availabilityStatuses/read |Status van de bronnen lezen |
| Microsoft.Resources/deployments/* |Maken en beheren van resourcegroepimplementaties |
| Microsoft.Resources/subscriptions/resourceGroups/read |Lezen van resourcegroepen |
| Microsoft.Support/* |Maken en ondersteuningstickets beheren |
| Microsoft.Web/certificates/* |Maken en beheren van Websitecertificaten |
| Microsoft.Web/listSitesAssignedToHostName/read |Lezen van de sites die zijn toegewezen aan een host-naam |
| Microsoft.Web/serverFarms/join/action |Deelnemen aan serverfarms |
| Microsoft.Web/serverFarms/read |Serverfarms lezen |
| Microsoft.Web/sites/* |Maken en beheren van websites (maken van de site vereist ook schrijfmachtigingen in voor de bijbehorende App Service-Plan) |

## <a name="see-also"></a>Zie ook
* [Toegangsbeheer op basis van rollen](role-based-access-control-configure.md): aan de slag met RBAC in Azure portal.
* [Aangepaste rollen in Azure RBAC](role-based-access-control-custom-roles.md): informatie over het maken van aangepaste rollen aan uw behoeften toegang.
* [Maken van een geschiedenisrapport voor gewijzigde van toegang](role-based-access-control-access-change-history-report.md): bijhouden van wijzigen van roltoewijzingen in RBAC.
* [Probleemoplossing voor toegangsbeheer op basis van rollen](role-based-access-control-troubleshooting.md): Profiteer van tips voor het oplossen van veelvoorkomende problemen.
