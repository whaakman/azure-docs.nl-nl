---
title: Azure Monitor logboeken-gegevensmodel voor Azure Backup
description: In dit artikel praat over Azure Monitor registreert details voor het model van gegevens voor Azure backup-gegevens.
services: backup
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: adigan
ms.openlocfilehash: 00bdc5ff63e78b0f96b794ca907bc28158e62c62
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56883637"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Log Analytics-gegevensmodel voor Azure backup-gegevens

Het gegevensmodel van Log Analytics gebruiken om te maken van aangepaste waarschuwingen van Log Analytics.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="using-azure-backup-data-model"></a>Met behulp van Azure Backup-gegevensmodel

U kunt de volgende velden geleverd als onderdeel van het gegevensmodel gebruiken voor het maken van visuele elementen, aangepaste query's en dashboard aan de hand van uw vereisten.

### <a name="alert"></a>Waarschuwing

Deze tabel bevat informatie over waarschuwingen gerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| AlertUniqueId_s |Tekst |De unieke id van de gegenereerde waarschuwing |
| AlertType_s |Tekst |Type van de waarschuwing, bijvoorbeeld: back-up |
| AlertStatus_s |Tekst |Status van de waarschuwing, bijvoorbeeld: actief |
| AlertOccurrenceDateTime_s |Datum/tijd |De datum en tijd waarop waarschuwing is gemaakt |
| AlertSeverity_s |Tekst |Ernst van de waarschuwing, bijvoorbeeld: kritiek |
|AlertTimeToResolveInMinutes_s    | Aantal        |Gebruikte tijd voor het oplossen van een waarschuwing. Leeg voor actieve waarschuwingen.         |
|AlertConsolidationStatus_s   |Tekst         |Als de waarschuwing een geconsolideerde waarschuwing of niet is identificeren         |
|CountOfAlertsConsolidated_s     |Aantal         |Aantal waarschuwingen samengevoegd als deze een geconsolideerde waarschuwing is          |
|AlertRaisedOn_s     |Tekst         |Type entiteit die de waarschuwing is gegenereerd op         |
|AlertCode_s     |Tekst         |Code voor het aanduiden van een Waarschuwingstype         |
|RecommendedAction_s   |Tekst         |De waarschuwing te verhelpen aanbevolen actie         |
| EventName_s |Tekst |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Tekst |De unieke id van het back-upitem dat is gekoppeld aan de waarschuwing |
| SchemaVersion_s |Tekst |Huidige versie van het schema, bijvoorbeeld **V2** |
| State_s |Tekst |Huidige status van de waarschuwing object, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waarop deze waarschuwing hoort bij |
| OperationName |Tekst |Naam van de huidige bewerking, bijvoorbeeld: waarschuwing |
| Categorie |Tekst |De categorie van diagnostische gegevens te pushen naar Azure Monitor-Logboeken. Always AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |De unieke id van de beveiligde server die is gekoppeld aan de waarschuwing |
| VaultUniqueId_s |Tekst |De unieke id van de beveiligde kluis die zijn gekoppeld aan de waarschuwing |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |De unieke id voor de resource over welke gegevens worden verzameld. Bijvoorbeeld, een Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="backupitem"></a>BackupItem

Deze tabel bevat informatie over back-item-gerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |De naam van de gebeurtenis. Always AzureBackupCentralReport |  
| BackupItemUniqueId_s |Tekst |De unieke id van het back-upitem |
| BackupItemId_s |Tekst |Id van de back-upitem |
| BackupItemName_s |Tekst |Naam van de back-upitem |
| BackupItemFriendlyName_s |Tekst |Beschrijvende naam van de back-upitem |
| BackupItemType_s |Tekst |Type back-upitem, bijvoorbeeld VM FileFolder |
| ProtectionState_s |Tekst |Huidige beveiligingsstatus van de back-upitem, bijvoorbeeld, beveiligde, ProtectionStopped |
| ProtectionGroupName_s |Tekst | Naam van de beveiligingsgroep de back-upitem is beveiligd, voor SC DPM en MABS, indien van toepassing|
| SecondaryBackupProtectionState_s |Tekst |Of secundaire beveiliging is ingeschakeld voor de back-upitem|
| SchemaVersion_s |Tekst |Versie van het schema, bijvoorbeeld **V2** |
| State_s |Tekst |Status van de back-upitem-object, bijvoorbeeld, actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waaraan dit back-upitem behoort |
| OperationName |Tekst |Naam van de bewerking, bijvoorbeeld BackupItem |
| Categorie |Tekst |De categorie van diagnostische gegevens te pushen naar Azure Monitor-Logboeken. Always AzureBackupReport |
| Resource |Tekst |Resource voor welke gegevens worden verzameld, bijvoorbeeld de naam voor Recovery Services-kluis |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld, bijvoorbeeld: Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Abonnements-id van de resource (voor bijvoorbeeld. Recovery Services-kluis) voor gegevens die worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (voor bijvoorbeeld. Recovery Services-kluis) voor gegevens die worden verzameld |
| ResourceProvider |Tekst |Resourceprovider voor gegevens die worden verzameld, bijvoorbeeld Microsoft.RecoveryServices |
| ResourceType |Tekst |Type van de resource voor gegevens die worden verzameld, bijvoorbeeld-kluizen |

### <a name="backupitemassociation"></a>BackupItemAssociation

Deze tabel bevat informatie over back-upitem koppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |  
| BackupItemUniqueId_s |Tekst |De unieke Id van het back-upitem |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Tekst |Huidige status van het back-upitem koppeling-object, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| BackupItemSourceSize_s |Tekst | Grootte van de front-end van de back-upitem |
| BackupManagementServerUniqueId_s |Tekst | Veld voor het aanduiden van de back-up-Management-Server de back-upitem is beschermd met, indien van toepassing |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - BackupItemAssociation |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Tekst |De unieke id voor het beleid dat is gekoppeld aan het back-upitem |
| ProtectedServerUniqueId_s |Tekst |De unieke id van de beveiligde server die is gekoppeld aan het back-upitem |
| VaultUniqueId_s |Tekst |De unieke id van de kluis waarin de back-upitem |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Resourceprovider voor gegevens die worden verzameld, bijvoorbeeld Microsoft.RecoveryServices |
| ResourceType |Tekst |Type van de resource voor de gegevens worden verzameld, bijvoorbeeld kluizen |

### <a name="backupmanagementserver"></a>BackupManagementServer

Deze tabel bevat informatie over back-upitem koppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
|BackupManagementServerName_s     |Tekst         |Naam van de Server back-upbeheer        |
|AzureBackupAgentVersion_s     |Tekst         |Versie van de Azure Backup-Agent op de Server back-upbeheer          |
|BackupManagementServerVersion_s     |Tekst         |Versie van de Server back-upbeheer|
|BackupManagementServerOSVersion_s    |Tekst            |Versie van het besturingssysteem van de beheerserver voor back-up|
|BackupManagementServerType_s     |Tekst         |Type van de back-upbeheer-Server, als MABS, SC-DPM|
|BackupManagementServerUniqueId_s     |Tekst         |Veld voor het aanduiden van de beheerserver voor back-up       |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Resourceprovider voor gegevens die worden verzameld, bijvoorbeeld Microsoft.RecoveryServices |
| ResourceType |Tekst |Type van de resource voor de gegevens worden verzameld, bijvoorbeeld kluizen |

### <a name="job"></a>Taak

Deze tabel bevat details over velden met betrekking tot de taak.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Tekst |De unieke id van het back-upitem |
| SchemaVersion_s |Tekst |Versie van het schema, bijvoorbeeld **V2** |
| State_s |Tekst |Huidige status van het taakobject, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - taak |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |De unieke id van de beveiligde server die de taak |
| ProtectedContainerUniqueId_s |Tekst | De unieke Id voor het identificeren van de beveiligde container de taak wordt uitgevoerd op |
| VaultUniqueId_s |Tekst |De unieke id van de beveiligde kluis |
| JobOperation_s |Tekst |Bewerking voor die taak wordt uitgevoerd, bijvoorbeeld back-up, herstel, configureren van back-up |
| JobStatus_s |Tekst |Status van de voltooide taak, bijvoorbeeld, voltooid, is mislukt |
| JobFailureCode_s |Tekst |Mislukt codetekenreeks vanwege die taakfout opgetreden |
| JobStartDateTime_s |Datum/tijd |Datum en tijd wanneer de taak gestart die wordt uitgevoerd |
| BackupStorageDestination_s |Tekst |Bestemming van back-upopslag, bijvoorbeeld Cloud, schijf  |
| AdHocOrScheduledJob_s |Tekst | Om aan te geven als de taak gepland of Ad-Hoc is |
| JobDurationInSecs_s | Aantal |Totaal aantal taak duur in seconden |
| DataTransferredInMB_s | Aantal |Gegevens die worden overgedragen in MB voor deze taak|
| JobUniqueId_g |Tekst |De unieke Id voor het identificeren van de taak |
| RecoveryJobDestination_s |Tekst | Doel van een hersteltaak wilt weergeven, waar de gegevens is hersteld |
| RecoveryJobRPDateTime_s |DateTime | De datum, tijd waarop het herstelpunt dat wordt hersteld is gemaakt |
| RecoveryJobRPLocation_s |Tekst | De locatie waar het herstelpunt dat wordt hersteld is opgeslagen|
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis|
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="policy"></a>Beleid

Deze tabel bevat informatie over beleid-gerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Tekst |Huidige status van het Groepsbeleid-object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - beleid |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Tekst |De unieke Id voor het identificeren van het beleid |
| PolicyName_s |Tekst |Naam van het beleid is gedefinieerd |
| BackupFrequency_s |Tekst |Frequentie waarmee de back-ups worden uitgevoerd, bijvoorbeeld dagelijks, wekelijks |
| BackupTimes_s |Tekst |Datum en tijd wanneer de back-ups zijn gepland |
| BackupDaysOfTheWeek_s |Tekst |Dagen van de week wanneer back-ups zijn gepland |
| RetentionDuration_s |Geheel getal |Bewaartermijn voor geconfigureerde back-ups |
| DailyRetentionDuration_s |Geheel getal |Totale bewaartermijn in dagen voor de geconfigureerde back-ups |
| DailyRetentionTimes_s |Tekst |Datum en tijd wanneer de dagelijkse bewaarperiode is geconfigureerd |
| WeeklyRetentionDuration_s |Decimaal getal |Totale duur van de wekelijkse bewaren in weken voor de geconfigureerde back-ups |
| WeeklyRetentionTimes_s |Tekst |Datum en tijd wanneer de wekelijkse bewaarperiode is geconfigureerd |
| WeeklyRetentionDaysOfTheWeek_s |Tekst |Dagen van de week geselecteerd voor het bewaren van week |
| MonthlyRetentionDuration_s |Decimaal getal |Totale bewaartermijn in maanden voor geconfigureerde back-ups |
| MonthlyRetentionTimes_s |Tekst |Datum en tijd wanneer de maandelijkse bewaarperiode is geconfigureerd |
| MonthlyRetentionFormat_s |Tekst |Type van de configuratie voor maandelijkse bewaren, bijvoorbeeld dagelijks voor per dag, per week voor op basis van de week |
| MonthlyRetentionDaysOfTheWeek_s |Tekst |Dagen van de week geselecteerd voor het bewaren van maandelijkse |
| MonthlyRetentionWeeksOfTheMonth_s |Tekst |Weken van de maand wanneer de maandelijkse bewaarperiode is geconfigureerd, bijvoorbeeld de eerste, laatste enzovoort. |
| YearlyRetentionDuration_s |Decimaal getal |Totale bewaartermijn in de jaren voor geconfigureerde back-ups |
| YearlyRetentionTimes_s |Tekst |Datum en tijd wanneer de jaarlijkse bewaarperiode is geconfigureerd |
| YearlyRetentionMonthsOfTheYear_s |Tekst |Maanden van het jaar voor jaarlijkse retentie geselecteerd |
| YearlyRetentionFormat_s |Tekst |Type van de configuratie voor jaarlijkse bewaren, bijvoorbeeld dagelijks voor per dag, per week voor op basis van de week |
| YearlyRetentionDaysOfTheMonth_s |Tekst |Datums van de maand geselecteerd voor het bewaren van jaar |
| SynchronisationFrequencyPerDay_s |Geheel getal |v2|Aantal keren per dag die een back-up is gesynchroniseerd voor SC DPM en MABS |
| DiffBackupFormat_s |Tekst |v2|Indeling voor differentiële back-ups voor SQL in Azure VM backup |
| DiffBackupTime_s |Time |v2|Tijd voor differentiële back-ups voor SQL in Azure VM Backup|
| DiffBackupRetentionDuration_s |Decimaal getal |v2|Bewaartermijn voor differentiële back-ups voor SQL in Azure VM Backup|
| LogBackupFrequency_s |Decimaal getal |v2|Frequentie van logboekback-ups voor SQL|
| LogBackupRetentionDuration_s |Decimaal getal |v2|Bewaartermijn voor logboekback-ups voor SQL in Azure VM Backup|
| DiffBackupDaysofTheWeek_s |Tekst |v2|Dagen van de week voor differentiële back-ups voor SQL in Azure VM Backup|
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="policyassociation"></a>PolicyAssociation

Deze tabel bevat informatie over beleidskoppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Tekst |Huidige status van het Groepsbeleid-object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - PolicyAssociation |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Tekst |De unieke Id voor het identificeren van het beleid |
| VaultUniqueId_s |Tekst |De unieke Id van de kluis waarop dit beleid behoort |
| BackupManagementServerUniqueId_s |Tekst |v2 |Veld voor het aanduiden van de back-up-Management-Server de back-upitem is beschermd met, indien van toepassing        |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="protected-container"></a>Beveiligde Container

Deze tabel bevat de algemene velden over beveiligde Containers. (ProtectedServer was in v1)

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Tekst | Veld voor het aanduiden van een beveiligde Container |
| ProtectedContainerOSType_s |Tekst |Type besturingssysteem van de beveiligde Container |
| ProtectedContainerOSVersion_s |Tekst |Versie van het besturingssysteem van de beveiligde Container |
| AgentVersion_s |Tekst |Versienummer van Agent back-up of de beveiligingsagent (in het geval van SC DPM en MABS) |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up bijvoorbeeld IaaSVM FileFolder |
| EntityState_s |Tekst |Huidige status van het object van de beveiligde server bijvoorbeeld actief, verwijderd |
| ProtectedContainerFriendlyName_s |Tekst |Beschrijvende naam van de beveiligde server |
| ProtectedContainerName_s |Tekst |Naam van de beveiligde Container |
| ProtectedContainerWorkloadType_s |Tekst |Type van de Container beveiligd back-up bijvoorbeeld IaaSVMContainer |
| ProtectedContainerLocation_s |Tekst |De Container beveiligd kan zich On-premises of in Azure |
| ProtectedContainerType_s |Tekst |Of de beveiligde Container is een server of een container |

### <a name="storage"></a>Opslag

Deze tabel bevat details over velden met betrekking tot opslag.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| CloudStorageInBytes_s |Decimaal getal |Back-cloudopslag die wordt gebruikt door back-ups, berekend op basis van de meest recente |
| ProtectedInstances_s |Decimaal getal |Aantal beveiligde exemplaren die worden gebruikt voor het berekenen van de frontend-opslag in facturering, berekend op basis van de laatste waarde |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Tekst |Huidige status van het opslagobject, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - opslag |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |De unieke Id van de beveiligde server waarvoor de opslag wordt berekend |
| VaultUniqueId_s |Tekst |De unieke Id van de kluis voor de opslag wordt berekend |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="storageassociation"></a>StorageAssociation

Deze tabel bevat de basic-opslag-gerelateerde velden storage verbinden met andere entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- |  --- |
| StorageUniqueId_s |Tekst |Unieke Id gebruikt voor het identificeren van de opslagentiteit |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V2** |
| BackupItemUniqueId_s |Tekst |Unieke Id gebruikt voor het identificeren van de back-upitem met betrekking tot de opslagentiteit |
| BackupManagementServerUniqueId_s |Tekst |Unieke Id gebruikt voor het identificeren van de back-upbeheer-server met betrekking tot de opslagentiteit|
| VaultUniqueId_s |Tekst |Unieke Id gebruikt voor het identificeren van de kluis die betrekking hebben op de opslagentiteit|
| StorageConsumedInMBs_s |Aantal|Grootte van de opslag die door het bijbehorende back-upitem in de bijbehorende opslag |
| StorageAllocatedInMBs_s |Aantal |Grootte van opslag toegewezen door het bijbehorende back-upitem in de bijbehorende opslag van het type schijf|

### <a name="vault"></a>Kluis

Deze tabel bevat details over velden met betrekking tot de kluis.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Tekst |Huidige status van het object van de kluis, bijvoorbeeld: actief, verwijderd |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - kluis |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| VaultUniqueId_s |Tekst |De unieke Id van de kluis |
| VaultName_s |Tekst |Naam van de kluis |
| AzureDataCenter_s |Tekst |Datacenter waarin kluis zich bevindt |
| StorageReplicationType_s |Tekst |Type van de storage-replicatie voor de kluis, bijvoorbeeld GeoRedundant |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="backup-management-server"></a>Back-upbeheer-Server

Deze tabel bevat de algemene velden over back-up-beheerservers.

|Veld  |Gegevenstype  | Description  |
|---------|---------|----------|
|BackupManagmentServerName_s     |Tekst         |Naam van de Server back-upbeheer        |
|AzureBackupAgentVersion_s     |Tekst         |Versie van de Azure Backup-Agent op de Server back-upbeheer          |
|BackupManagmentServerVersion_s     |Tekst         |Versie van de Server back-upbeheer|
|BackupManagmentServerOSVersion_s     |Tekst            |Versie van het besturingssysteem van de beheerserver voor back-up|
|BackupManagementServerType_s     |Tekst         |Type van de back-upbeheer-Server, als MABS, SC-DPM|
|BackupManagmentServerUniqueId_s     |Tekst         |Veld voor het aanduiden van de beheerserver voor back-up       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

Deze tabel bevat de workload(s) die een Volume gekoppeld is.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| StorageUniqueId_s |Tekst |Unieke Id gebruikt voor het identificeren van de opslagentiteit |
| BackupItemType_s |Tekst |De werkbelastingen waarvoor dit volume de gewenste opslag is|

### <a name="protectedinstance"></a>ProtectedInstance

Deze tabel bevat algemene beveiligde exemplaren gerelateerde velden.

| Veld | Gegevenstype |Versies van toepassing | Description |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Tekst |v2|Unieke Id gebruikt voor het identificeren van de back-upitem voor virtuele machines een back-up met behulp van DPM, MABS|
| ProtectedContainerUniqueId_s |Tekst |v2|Unieke Id gebruikt voor het identificeren van de beveiligde container voor alles, behalve de virtuele machines een back-up met behulp van DPM, MABS|
| ProtectedInstanceCount_s |Tekst |v2|Telling van beveiligde instanties voor de gekoppelde back-upitem of beveiligde container op die datum / tijd|

### <a name="recoverypoint"></a>RecoveryPoint

Deze tabel bevat algemene herstel gerelateerde velden verwijzen.

| Veld | Gegevenstype | Description |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Tekst |Unieke Id gebruikt voor het identificeren van de back-upitem voor virtuele machines een back-up met behulp van DPM, MABS|
| OldestRecoveryPointTime_s |Tekst |Datum-tijd van het oudste herstelpunt van de back-upitem|
| OldestRecoveryPointLocation_s |Tekst |Locatie van het oudste herstelpunt van de back-upitem|
| LatestRecoveryPointTime_s |Tekst |Datum-tijd van de meest recente herstelpunt van de back-upitem|
| LatestRecoveryPointLocation_s |Tekst |Locatie van de meest recente herstelpunt van de back-upitem|

## <a name="next-steps"></a>Volgende stappen

Als u het gegevensmodel bekijkt, kunt u starten [het maken van aangepaste query's](../azure-monitor/learn/tutorial-logs-dashboards.md) in Azure Monitor-logboeken naar uw eigen dashboard maken.
