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
ms.openlocfilehash: dd4dad2cc3e541d3b6866c02341161dc1d9e1e6c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61234911"
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
| AlertUniqueId_s |Text |De unieke id van de gegenereerde waarschuwing |
| AlertType_s |Text |Type van de waarschuwing, bijvoorbeeld: back-up |
| AlertStatus_s |Text |Status van de waarschuwing, bijvoorbeeld: actief |
| AlertOccurrenceDateTime_s |Datum/tijd |De datum en tijd waarop waarschuwing is gemaakt |
| AlertSeverity_s |Text |Ernst van de waarschuwing, bijvoorbeeld: kritiek |
|AlertTimeToResolveInMinutes_s    | Aantal        |Gebruikte tijd voor het oplossen van een waarschuwing. Leeg voor actieve waarschuwingen.         |
|AlertConsolidationStatus_s   |Text         |Als de waarschuwing een geconsolideerde waarschuwing of niet is identificeren         |
|CountOfAlertsConsolidated_s     |Aantal         |Aantal waarschuwingen samengevoegd als deze een geconsolideerde waarschuwing is          |
|AlertRaisedOn_s     |Text         |Type entiteit die de waarschuwing is gegenereerd op         |
|AlertCode_s     |Text         |Code voor het aanduiden van een Waarschuwingstype         |
|RecommendedAction_s   |Text         |De waarschuwing te verhelpen aanbevolen actie         |
| EventName_s |Text |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |De unieke id van het back-upitem dat is gekoppeld aan de waarschuwing |
| SchemaVersion_s |Text |Huidige versie van het schema, bijvoorbeeld **V2** |
| State_s |Text |Huidige status van de waarschuwing object, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Text |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waarop deze waarschuwing hoort bij |
| OperationName |Text |Naam van de huidige bewerking, bijvoorbeeld: waarschuwing |
| Category |Text |De categorie van diagnostische gegevens te pushen naar Azure Monitor-Logboeken. Always AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Text |De unieke id van de beveiligde server die is gekoppeld aan de waarschuwing |
| VaultUniqueId_s |Text |De unieke id van de beveiligde kluis die zijn gekoppeld aan de waarschuwing |
| SourceSystem |Text |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text |De unieke id voor de resource over welke gegevens worden verzameld. Bijvoorbeeld, een Recovery Services-kluis resource-id |
| SubscriptionId |Text |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Text |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="backupitem"></a>BackupItem

Deze tabel bevat informatie over back-item-gerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |De naam van de gebeurtenis. Always AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |De unieke id van het back-upitem |
| BackupItemId_s |Text |Id van de back-upitem |
| BackupItemName_s |Text |Naam van de back-upitem |
| BackupItemFriendlyName_s |Text |Beschrijvende naam van de back-upitem |
| BackupItemType_s |Text |Type back-upitem, bijvoorbeeld VM FileFolder |
| ProtectionState_s |Text |Huidige beveiligingsstatus van de back-upitem, bijvoorbeeld, beveiligde, ProtectionStopped |
| ProtectionGroupName_s |Text | Naam van de beveiligingsgroep de back-upitem is beveiligd, voor SC DPM en MABS, indien van toepassing|
| SecondaryBackupProtectionState_s |Text |Of secundaire beveiliging is ingeschakeld voor de back-upitem|
| SchemaVersion_s |Text |Versie van het schema, bijvoorbeeld **V2** |
| State_s |Text |Status van de back-upitem-object, bijvoorbeeld, actief, verwijderd |
| BackupManagementType_s |Text |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waaraan dit back-upitem behoort |
| OperationName |Text |Naam van de bewerking, bijvoorbeeld BackupItem |
| Category |Text |De categorie van diagnostische gegevens te pushen naar Azure Monitor-Logboeken. Always AzureBackupReport |
| Resource |Text |Resource voor welke gegevens worden verzameld, bijvoorbeeld de naam voor Recovery Services-kluis |
| SourceSystem |Text |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text |Resource-id voor gegevens die worden verzameld, bijvoorbeeld: Recovery Services-kluis resource-id |
| SubscriptionId |Text |Abonnements-id van de resource (voor bijvoorbeeld. Recovery Services-kluis) voor gegevens die worden verzameld |
| ResourceGroup |Text |Resourcegroep van de resource (voor bijvoorbeeld. Recovery Services-kluis) voor gegevens die worden verzameld |
| ResourceProvider |Text |Resourceprovider voor gegevens die worden verzameld, bijvoorbeeld Microsoft.RecoveryServices |
| ResourceType |Text |Type van de resource voor gegevens die worden verzameld, bijvoorbeeld-kluizen |

### <a name="backupitemassociation"></a>BackupItemAssociation

Deze tabel bevat informatie over back-upitem koppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |De unieke Id van het back-upitem |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Text |Huidige status van het back-upitem koppeling-object, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Text |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| BackupItemSourceSize_s |Text | Grootte van de front-end van de back-upitem |
| BackupManagementServerUniqueId_s |Text | Veld voor het aanduiden van de back-up-Management-Server de back-upitem is beschermd met, indien van toepassing |
| Category |Text |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
| OperationName |Text |Dit veld vertegenwoordigt naam van de huidige bewerking - BackupItemAssociation |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Text |De unieke id voor het beleid dat is gekoppeld aan het back-upitem |
| ProtectedServerUniqueId_s |Text |De unieke id van de beveiligde server die is gekoppeld aan het back-upitem |
| VaultUniqueId_s |Text |De unieke id van de kluis waarin de back-upitem |
| SourceSystem |Text |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Text |Abonnements-id van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resourcegroep van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resourceprovider voor gegevens die worden verzameld, bijvoorbeeld Microsoft.RecoveryServices |
| ResourceType |Text |Type van de resource voor de gegevens worden verzameld, bijvoorbeeld kluizen |

### <a name="backupmanagementserver"></a>BackupManagementServer

Deze tabel bevat informatie over back-upitem koppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
|BackupManagementServerName_s     |Text         |Naam van de Server back-upbeheer        |
|AzureBackupAgentVersion_s     |Text         |Versie van de Azure Backup-Agent op de Server back-upbeheer          |
|BackupManagementServerVersion_s     |Text         |Versie van de Server back-upbeheer|
|BackupManagementServerOSVersion_s    |Text            |Versie van het besturingssysteem van de beheerserver voor back-up|
|BackupManagementServerType_s     |Text         |Type van de back-upbeheer-Server, als MABS, SC-DPM|
|BackupManagementServerUniqueId_s     |Text         |Veld voor het aanduiden van de beheerserver voor back-up       |
| SourceSystem |Text |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Text |Abonnements-id van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resourcegroep van de resource (voor bijvoorbeeld. Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resourceprovider voor gegevens die worden verzameld, bijvoorbeeld Microsoft.RecoveryServices |
| ResourceType |Text |Type van de resource voor de gegevens worden verzameld, bijvoorbeeld kluizen |

### <a name="job"></a>Taak

Deze tabel bevat details over velden met betrekking tot de taak.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |De unieke id van het back-upitem |
| SchemaVersion_s |Text |Versie van het schema, bijvoorbeeld **V2** |
| State_s |Text |Huidige status van het taakobject, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Text |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Text |Dit veld vertegenwoordigt de naam van de huidige bewerking - taak |
| Category |Text |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Text |De unieke id van de beveiligde server die de taak |
| ProtectedContainerUniqueId_s |Text | De unieke Id voor het identificeren van de beveiligde container de taak wordt uitgevoerd op |
| VaultUniqueId_s |Text |De unieke id van de beveiligde kluis |
| JobOperation_s |Text |Bewerking voor die taak wordt uitgevoerd, bijvoorbeeld back-up, herstel, configureren van back-up |
| JobStatus_s |Text |Status van de voltooide taak, bijvoorbeeld, voltooid, is mislukt |
| JobFailureCode_s |Text |Mislukt codetekenreeks vanwege die taakfout opgetreden |
| JobStartDateTime_s |Datum/tijd |Datum en tijd wanneer de taak gestart die wordt uitgevoerd |
| BackupStorageDestination_s |Text |Bestemming van back-upopslag, bijvoorbeeld Cloud, schijf  |
| AdHocOrScheduledJob_s |Text | Om aan te geven als de taak gepland of Ad-Hoc is |
| JobDurationInSecs_s | Aantal |Totaal aantal taak duur in seconden |
| DataTransferredInMB_s | Aantal |Gegevens die worden overgedragen in MB voor deze taak|
| JobUniqueId_g |Text |De unieke Id voor het identificeren van de taak |
| RecoveryJobDestination_s |Text | Doel van een hersteltaak wilt weergeven, waar de gegevens is hersteld |
| RecoveryJobRPDateTime_s |DateTime | De datum, tijd waarop het herstelpunt dat wordt hersteld is gemaakt |
| RecoveryJobRPLocation_s |Text | De locatie waar het herstelpunt dat wordt hersteld is opgeslagen|
| SourceSystem |Text |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis|
| SubscriptionId |Text |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Text |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="policy"></a>Beleid

Deze tabel bevat informatie over beleid-gerelateerde velden.

| Veld | Gegevenstype | Versies van toepassing | Description |
| --- | --- | --- | --- |
| EventName_s |Text ||Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text ||Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Text ||Huidige status van het Groepsbeleid-object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text ||Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Text ||Dit veld vertegenwoordigt de naam van de huidige bewerking - beleid |
| Category |Text ||Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Text ||Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Text ||De unieke Id voor het identificeren van het beleid |
| PolicyName_s |Text ||Naam van het beleid is gedefinieerd |
| BackupFrequency_s |Text ||Frequentie waarmee de back-ups worden uitgevoerd, bijvoorbeeld dagelijks, wekelijks |
| BackupTimes_s |Text ||Datum en tijd wanneer de back-ups zijn gepland |
| BackupDaysOfTheWeek_s |Text ||Dagen van de week wanneer back-ups zijn gepland |
| RetentionDuration_s |Geheel getal ||Bewaartermijn voor geconfigureerde back-ups |
| DailyRetentionDuration_s |Geheel getal ||Totale bewaartermijn in dagen voor de geconfigureerde back-ups |
| DailyRetentionTimes_s |Text ||Datum en tijd wanneer de dagelijkse bewaarperiode is geconfigureerd |
| WeeklyRetentionDuration_s |Decimaal getal ||Totale duur van de wekelijkse bewaren in weken voor de geconfigureerde back-ups |
| WeeklyRetentionTimes_s |Text ||Datum en tijd wanneer de wekelijkse bewaarperiode is geconfigureerd |
| WeeklyRetentionDaysOfTheWeek_s |Text ||Dagen van de week geselecteerd voor het bewaren van week |
| MonthlyRetentionDuration_s |Decimaal getal ||Totale bewaartermijn in maanden voor geconfigureerde back-ups |
| MonthlyRetentionTimes_s |Text ||Datum en tijd wanneer de maandelijkse bewaarperiode is geconfigureerd |
| MonthlyRetentionFormat_s |Text ||Type van de configuratie voor maandelijkse bewaren, bijvoorbeeld dagelijks voor per dag, per week voor op basis van de week |
| MonthlyRetentionDaysOfTheWeek_s |Text ||Dagen van de week geselecteerd voor het bewaren van maandelijkse |
| MonthlyRetentionWeeksOfTheMonth_s |Text ||Weken van de maand wanneer de maandelijkse bewaarperiode is geconfigureerd, bijvoorbeeld de eerste, laatste enzovoort. |
| YearlyRetentionDuration_s |Decimaal getal ||Totale bewaartermijn in de jaren voor geconfigureerde back-ups |
| YearlyRetentionTimes_s |Text ||Datum en tijd wanneer de jaarlijkse bewaarperiode is geconfigureerd |
| YearlyRetentionMonthsOfTheYear_s |Text ||Maanden van het jaar voor jaarlijkse retentie geselecteerd |
| YearlyRetentionFormat_s |Text ||Type van de configuratie voor jaarlijkse bewaren, bijvoorbeeld dagelijks voor per dag, per week voor op basis van de week | |
| YearlyRetentionDaysOfTheMonth_s |Text ||Datums van de maand geselecteerd voor het bewaren van jaar |
| SynchronisationFrequencyPerDay_s |Geheel getal |v2|Aantal keren per dag die een back-up is gesynchroniseerd voor SC DPM en MABS |
| DiffBackupFormat_s |Text |v2|Indeling voor differentiële back-ups voor SQL in Azure VM backup |
| DiffBackupTime_s |Time |v2|Tijd voor differentiële back-ups voor SQL in Azure VM Backup|
| DiffBackupRetentionDuration_s |Decimaal getal |v2|Bewaartermijn voor differentiële back-ups voor SQL in Azure VM Backup|
| LogBackupFrequency_s |Decimaal getal |v2|Frequentie van logboekback-ups voor SQL|
| LogBackupRetentionDuration_s |Decimaal getal |v2|Bewaartermijn voor logboekback-ups voor SQL in Azure VM Backup|
| DiffBackupDaysofTheWeek_s |Text |v2|Dagen van de week voor differentiële back-ups voor SQL in Azure VM Backup|
| SourceSystem |Text ||Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text ||Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Text ||Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text ||Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text ||De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Text ||Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="policyassociation"></a>PolicyAssociation

Deze tabel bevat informatie over beleidskoppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Versies van toepassing | Description |
| --- | --- | --- | --- |
| EventName_s |Text ||Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text ||Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Text ||Huidige status van het Groepsbeleid-object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text ||Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Text ||Dit veld vertegenwoordigt naam van de huidige bewerking - PolicyAssociation |
| Category |Text ||Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Text ||Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Text ||De unieke Id voor het identificeren van het beleid |
| VaultUniqueId_s |Text ||De unieke Id van de kluis waarop dit beleid behoort |
| BackupManagementServerUniqueId_s |Text |v2 |Veld voor het aanduiden van de back-up-Management-Server de back-upitem is beschermd met, indien van toepassing        |
| SourceSystem |Text ||Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text ||Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Text ||Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text ||Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text ||De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Text ||Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="protected-container"></a>Beveiligde Container

Deze tabel bevat de algemene velden over beveiligde Containers. (ProtectedServer was in v1)

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Text | Veld voor het aanduiden van een beveiligde Container |
| ProtectedContainerOSType_s |Text |Type besturingssysteem van de beveiligde Container |
| ProtectedContainerOSVersion_s |Text |Versie van het besturingssysteem van de beveiligde Container |
| AgentVersion_s |Text |Versienummer van Agent back-up of de beveiligingsagent (in het geval van SC DPM en MABS) |
| BackupManagementType_s |Text |Providertype voor het uitvoeren van back-up bijvoorbeeld IaaSVM FileFolder |
| EntityState_s |Text |Huidige status van het object van de beveiligde server bijvoorbeeld actief, verwijderd |
| ProtectedContainerFriendlyName_s |Text |Beschrijvende naam van de beveiligde server |
| ProtectedContainerName_s |Text |Naam van de beveiligde Container |
| ProtectedContainerWorkloadType_s |Text |Type van de Container beveiligd back-up bijvoorbeeld IaaSVMContainer |
| ProtectedContainerLocation_s |Text |De Container beveiligd kan zich On-premises of in Azure |
| ProtectedContainerType_s |Text |Of de beveiligde Container is een server of een container |

### <a name="storage"></a>Opslag

Deze tabel bevat details over velden met betrekking tot opslag.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| CloudStorageInBytes_s |Decimaal getal |Back-cloudopslag die wordt gebruikt door back-ups, berekend op basis van de meest recente |
| ProtectedInstances_s |Decimaal getal |Aantal beveiligde exemplaren die worden gebruikt voor het berekenen van de frontend-opslag in facturering, berekend op basis van de laatste waarde |
| EventName_s |Text |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Text |Huidige status van het opslagobject, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Text |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Text |Dit veld vertegenwoordigt de naam van de huidige bewerking - opslag |
| Category |Text |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Text |De unieke Id van de beveiligde server waarvoor de opslag wordt berekend |
| VaultUniqueId_s |Text |De unieke Id van de kluis voor de opslag wordt berekend |
| SourceSystem |Text |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Text |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Text |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="storageassociation"></a>StorageAssociation

Deze tabel bevat de basic-opslag-gerelateerde velden storage verbinden met andere entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- |  --- |
| StorageUniqueId_s |Text |Unieke Id gebruikt voor het identificeren van de opslagentiteit |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema, is het **V2** |
| BackupItemUniqueId_s |Text |Unieke Id gebruikt voor het identificeren van de back-upitem met betrekking tot de opslagentiteit |
| BackupManagementServerUniqueId_s |Text |Unieke Id gebruikt voor het identificeren van de back-upbeheer-server met betrekking tot de opslagentiteit|
| VaultUniqueId_s |Text |Unieke Id gebruikt voor het identificeren van de kluis die betrekking hebben op de opslagentiteit|
| StorageConsumedInMBs_s |Aantal|Grootte van de opslag die door het bijbehorende back-upitem in de bijbehorende opslag |
| StorageAllocatedInMBs_s |Aantal |Grootte van opslag toegewezen door het bijbehorende back-upitem in de bijbehorende opslag van het type schijf|

### <a name="vault"></a>Kluis

Deze tabel bevat details over velden met betrekking tot de kluis.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema, is het **V2** |
| State_s |Text |Huidige status van het object van de kluis, bijvoorbeeld: actief, verwijderd |
| OperationName |Text |Dit veld vertegenwoordigt de naam van de huidige bewerking - kluis |
| Category |Text |Dit veld categorie van diagnostische gegevens te pushen naar Azure Monitor logboeken vertegenwoordigt, is het AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| VaultUniqueId_s |Text |De unieke Id van de kluis |
| VaultName_s |Text |Naam van de kluis |
| AzureDataCenter_s |Text |Datacenter waarin kluis zich bevindt |
| StorageReplicationType_s |Text |Type van de storage-replicatie voor de kluis, bijvoorbeeld GeoRedundant |
| SourceSystem |Text |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Text |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Text |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Text |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="backup-management-server"></a>Back-upbeheer-Server

Deze tabel bevat de algemene velden over back-up-beheerservers.

|Veld  |Gegevenstype  | Description  |
|---------|---------|----------|
|BackupManagmentServerName_s     |Text         |Naam van de Server back-upbeheer        |
|AzureBackupAgentVersion_s     |Text         |Versie van de Azure Backup-Agent op de Server back-upbeheer          |
|BackupManagmentServerVersion_s     |Text         |Versie van de Server back-upbeheer|
|BackupManagmentServerOSVersion_s     |Text            |Versie van het besturingssysteem van de beheerserver voor back-up|
|BackupManagementServerType_s     |Text         |Type van de back-upbeheer-Server, als MABS, SC-DPM|
|BackupManagmentServerUniqueId_s     |Text         |Veld voor het aanduiden van de beheerserver voor back-up       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

Deze tabel bevat de workload(s) die een Volume gekoppeld is.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| StorageUniqueId_s |Text |Unieke Id gebruikt voor het identificeren van de opslagentiteit |
| BackupItemType_s |Text |De werkbelastingen waarvoor dit volume de gewenste opslag is|

### <a name="protectedinstance"></a>ProtectedInstance

Deze tabel bevat algemene beveiligde exemplaren gerelateerde velden.

| Veld | Gegevenstype |Versies van toepassing | Description |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Text |v2|Unieke Id gebruikt voor het identificeren van de back-upitem voor virtuele machines een back-up met behulp van DPM, MABS|
| ProtectedContainerUniqueId_s |Text |v2|Unieke Id gebruikt voor het identificeren van de beveiligde container voor alles, behalve de virtuele machines een back-up met behulp van DPM, MABS|
| ProtectedInstanceCount_s |Text |v2|Telling van beveiligde instanties voor de gekoppelde back-upitem of beveiligde container op die datum / tijd|

### <a name="recoverypoint"></a>RecoveryPoint

Deze tabel bevat algemene herstel gerelateerde velden verwijzen.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| BackupItemUniqueId_s |Text |Unieke Id gebruikt voor het identificeren van de back-upitem voor virtuele machines een back-up met behulp van DPM, MABS|
| OldestRecoveryPointTime_s |Text |Datum-tijd van het oudste herstelpunt van de back-upitem|
| OldestRecoveryPointLocation_s |Text |Locatie van het oudste herstelpunt van de back-upitem|
| LatestRecoveryPointTime_s |Text |Datum-tijd van de meest recente herstelpunt van de back-upitem|
| LatestRecoveryPointLocation_s |Text |Locatie van de meest recente herstelpunt van de back-upitem|

## <a name="next-steps"></a>Volgende stappen

Als u het gegevensmodel bekijkt, kunt u starten [het maken van aangepaste query's](../azure-monitor/learn/tutorial-logs-dashboards.md) in Azure Monitor-logboeken naar uw eigen dashboard maken.
