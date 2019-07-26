---
title: Azure Monitor logboek gegevens model voor Azure Backup
description: In dit artikel vindt u informatie over het gegevens model van Azure Monitor logboeken voor Azure Backup gegevens.
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: adigan
ms.openlocfilehash: 6563eefffee0ed8d9ce94c3e0a1e24b0d32314f0
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68466161"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Log Analytics gegevens model voor Azure Backup gegevens

Gebruik het Log Analytics gegevens model om aangepaste waarschuwingen van Log Analytics te maken.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="using-azure-backup-data-model"></a>Azure Backup gegevens model gebruiken

U kunt de volgende velden als onderdeel van het gegevens model gebruiken om visuele elementen, aangepaste query's en dash boards te maken conform uw vereisten.

### <a name="alert"></a>Waarschuwing

Deze tabel bevat details over velden die betrekking hebben op waarschuwingen.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| AlertUniqueId_s |Text |De unieke id van de gegenereerde waarschuwing |
| AlertType_s |Text |Type waarschuwing, bijvoorbeeld back-up |
| AlertStatus_s |Text |De status van de waarschuwing, bijvoorbeeld actief |
| AlertOccurrenceDateTime_s |Date/Time |Datum en tijd waarop de waarschuwing is gemaakt |
| AlertSeverity_s |Text |Ernst van de waarschuwing, bijvoorbeeld kritiek |
|AlertTimeToResolveInMinutes_s    | Number        |De tijd die nodig is om een waarschuwing op te lossen. Leeg voor actieve waarschuwingen.         |
|AlertConsolidationStatus_s   |Text         |Vaststellen of de waarschuwing een geconsolideerde waarschuwing is         |
|CountOfAlertsConsolidated_s     |Number         |Aantal geconsolidatiete waarschuwingen als het een geconsolideerde waarschuwing is          |
|AlertRaisedOn_s     |Text         |Het type entiteit waarop de waarschuwing is opgetreden         |
|AlertCode_s     |Text         |Code voor een unieke identificatie van een waarschuwings type         |
|RecommendedAction_s   |Text         |Aanbevolen actie om de waarschuwing op te lossen         |
| EventName_s |Text |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |De unieke id van het back-upitem dat aan de waarschuwing is gekoppeld |
| SchemaVersion_s |Text |Huidige versie van het schema, bijvoorbeeld **v2** |
| State_s |Text |Huidige status van het waarschuwings object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text |Provider type voor het uitvoeren van back-ups, bijvoorbeeld IaaSVM, FileFolder waartoe deze waarschuwing behoort |
| OperationName |Text |De naam van de huidige bewerking, bijvoorbeeld waarschuwing |
| Categorie |Text |Categorie diagnostische gegevens die naar Azure Monitor logboeken worden gepusht. Always AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, de naam van Recovery Services kluis wordt weer gegeven |
| ProtectedContainerUniqueId_s |Text |De unieke id van de beveiligde server die is gekoppeld aan de waarschuwing (was ProtectedServerUniqueId_s in v1)|
| VaultUniqueId_s |Text |De unieke id van de beveiligde kluis die is gekoppeld aan de waarschuwing |
| SourceSystem |Text |Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text |De unieke id voor de resource waarover gegevens worden verzameld. Bijvoorbeeld een Recovery Services kluis resource-id |
| SubscriptionId |Text |De abonnements-id van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |De resource groep van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resource provider waarvoor gegevens worden verzameld. Bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text |Het resource type waarvoor gegevens worden verzameld. Bijvoorbeeld kluizen |

### <a name="backupitem"></a>BackupItem

Deze tabel bevat details over velden die betrekking hebben op Back-upitems.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |De naam van de gebeurtenis. Always AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |De unieke id van het back-upitem |
| BackupItemId_s |Text |Id van het back-upitem (dit veld is alleen voor v1-schema) |
| BackupItemName_s |Text |Naam van back-upitem |
| BackupItemFriendlyName_s |Text |Beschrijvende naam van het back-upitem |
| BackupItemType_s |Text |Type back-upitem, bijvoorbeeld VM, FileFolder |
| BackupItemProtectionState_s |Text |De beveiligings status van het back-upitem |
| BackupItemAppVersion_s |Text |Toepassings versie van het back-upitem |
| ProtectionState_s |Text |Huidige beveiligings status van het back-upitem, bijvoorbeeld beveiligd, ProtectionStopped |
| ProtectionGroupName_s |Text | Naam van de beveiligings groep waarop het back-upitem is beveiligd, voor SC DPM en MABS, indien van toepassing|
| SecondaryBackupProtectionState_s |Text |Of secundaire beveiliging is ingeschakeld voor het back-upitem|
| SchemaVersion_s |Text |De versie van het schema, bijvoorbeeld **v2** |
| State_s |Text |Status van het object voor het back-upitem, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text |Provider type voor het uitvoeren van back-ups, bijvoorbeeld IaaSVM, FileFolder waaraan dit back-upitem behoort |
| OperationName |Text |De naam van de bewerking, bijvoorbeeld BackupItem |
| Categorie |Text |Categorie diagnostische gegevens die naar Azure Monitor logboeken worden gepusht. Always AzureBackupReport |
| Resource |Text |Resource waarvoor gegevens worden verzameld, bijvoorbeeld Recovery Services kluis naam |
| SourceSystem |Text |Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text |De resource-id voor de gegevens die worden verzameld, bijvoorbeeld Recovery Services kluis resource-id |
| SubscriptionId |Text |De abonnements-id van de resource (voor ex. Recovery Services kluis) voor gegevens die worden verzameld |
| ResourceGroup |Text |Resource groep van de resource (voor bijvoorbeeld Recovery Services kluis) voor gegevens die worden verzameld |
| ResourceProvider |Text |Resource provider voor gegevens die worden verzameld, bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text |Het type resource voor de gegevens die worden verzameld, bijvoorbeeld kluizen |

### <a name="backupitemassociation"></a>BackupItemAssociation

Deze tabel bevat details over koppelingen van back-upitems met verschillende entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |Dit veld vertegenwoordigt de naam van deze gebeurtenis, maar is altijd AzureBackupCentralReport |  
| BackupItemUniqueId_s |Text |De unieke id van het back-upitem |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema aan. Dit is **v2** |
| State_s |Text |Huidige status van het object koppeling van het back-upitem, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text |Provider type voor het uitvoeren van een back-uptaak van de server, bijvoorbeeld IaaSVM, FileFolder |
| BackupItemSourceSize_s |Text | Front-end grootte van het back-upitem |
| BackupManagementServerUniqueId_s |Text | Veld voor het identificeren van de back-upbeheerserver waarop het back-upitem is beveiligd, indien van toepassing |
| Categorie |Text |Dit veld vertegenwoordigt een categorie met diagnostische gegevens die naar Log Analytics zijn gepusht, en is AzureBackupReport |
| OperationName |Text |Dit veld vertegenwoordigt de naam van de huidige bewerking-BackupItemAssociation |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, de naam van Recovery Services kluis wordt weer gegeven |
| ProtectedContainerUniqueId_s |Text |De unieke id van de beveiligde server die is gekoppeld aan het back-upitem (was ProtectedServerUniqueId_s in v1) |
| VaultUniqueId_s |Text |De unieke id van de kluis die het back-upitem bevat |
| SourceSystem |Text |Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text |De resource-id voor de gegevens die worden verzameld. Bijvoorbeeld Recovery Services kluis resource-id |
| SubscriptionId |Text |De abonnements-id van de resource (voor ex. Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resource groep van de resource (voor bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resource provider voor gegevens die worden verzameld, bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text |Het type resource voor de gegevens die worden verzameld, bijvoorbeeld kluizen |

### <a name="backupmanagementserver"></a>BackupManagementServer

Deze tabel bevat details over koppelingen van back-upitems met verschillende entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
|BackupManagementServerName_s     |Text         |Naam van de server voor back-upbeheer        |
|AzureBackupAgentVersion_s     |Text         |Versie van de Azure Backup-Agent op de server voor back-upbeheer          |
|BackupManagementServerVersion_s     |Text         |Versie van de server voor back-upbeheer|
|BackupManagementServerOSVersion_s    |Text            |Versie van het besturings systeem van de server voor back-upbeheer|
|BackupManagementServerType_s     |Text         |Type van de server voor back-upbeheer, zoals MABS, SC DPM|
|BackupManagementServerUniqueId_s     |Text         |Veld om de back-upbeheerserver op unieke wijze te identificeren       |
| SourceSystem |Text |Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text |De resource-id voor de gegevens die worden verzameld. Bijvoorbeeld Recovery Services kluis resource-id |
| SubscriptionId |Text |De abonnements-id van de resource (voor ex. Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |Resource groep van de resource (voor bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resource provider voor gegevens die worden verzameld, bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text |Het type resource voor de gegevens die worden verzameld, bijvoorbeeld kluizen |

### <a name="job"></a>Taak

Deze tabel bevat details over projectgerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Text |De unieke id van het back-upitem |
| SchemaVersion_s |Text |De versie van het schema, bijvoorbeeld **v2** |
| State_s |Text |Huidige status van het taak object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text |Provider type voor het uitvoeren van een back-uptaak van de server, bijvoorbeeld IaaSVM, FileFolder |
| OperationName |Text |Dit veld bevat de naam van de huidige bewerking-taak |
| Categorie |Text |Dit veld vertegenwoordigt een categorie met diagnostische gegevens die naar Azure Monitor logboeken zijn gepusht, en is AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, de naam van Recovery Services kluis wordt weer gegeven |
| ProtectedServerUniqueId_s |Text |De unieke id van de beveiligde server die aan de taak is gekoppeld |
| ProtectedContainerUniqueId_s |Text | Unieke id voor het identificeren van de beveiligde container waarop de taak wordt uitgevoerd |
| VaultUniqueId_s |Text |De unieke id van de beveiligde kluis |
| JobOperation_s |Text |Bewerking waarvoor de taak wordt uitgevoerd bijvoorbeeld back-ups maken, herstellen, back-up configureren |
| JobStatus_s |Text |De status van de voltooide taak, bijvoorbeeld voltooid, is mislukt |
| JobFailureCode_s |Text |De teken reeks voor de fout code omdat de taak fout is opgetreden |
| JobStartDateTime_s |Date/Time |Datum en tijd waarop de taak is gestart |
| BackupStorageDestination_s |Text |Doel van back-upopslag, bijvoorbeeld Cloud, schijf  |
| AdHocOrScheduledJob_s |Text | Veld om op te geven of de taak ad-hoc of gepland is |
| JobDurationInSecs_s | Number |Totale taak duur in seconden |
| DataTransferredInMB_s | Number |Gegevens die worden overgebracht in MB voor deze taak|
| JobUniqueId_g |Text |Unieke id voor het identificeren van de taak |
| RecoveryJobDestination_s |Text | Doel van een herstel taak, waarbij de gegevens worden hersteld |
| RecoveryJobRPDateTime_s |Datetime | De datum, het tijdstip waarop het herstel punt dat wordt hersteld, is gemaakt |
| RecoveryJobRPLocation_s |Text | De locatie waar het herstel punt dat wordt hersteld is opgeslagen|
| SourceSystem |Text |Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text |De resource-id voor de gegevens die worden verzameld. Bijvoorbeeld Recovery Services kluis resource-id|
| SubscriptionId |Text |De abonnements-id van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |De resource groep van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resource provider waarvoor gegevens worden verzameld. Bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text |Het resource type waarvoor gegevens worden verzameld. Bijvoorbeeld kluizen |

### <a name="policy"></a>Beleid

Deze tabel bevat details over velden die betrekking hebben op het beleid.

| Veld | Gegevenstype | Versies van toepassing | Description |
| --- | --- | --- | --- |
| EventName_s |Text ||Dit veld vertegenwoordigt de naam van deze gebeurtenis, maar is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text ||Dit veld geeft de huidige versie van het schema aan. Dit is **v2** |
| State_s |Text ||Huidige status van het beleids object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text ||Provider type voor het uitvoeren van een back-uptaak van de server, bijvoorbeeld IaaSVM, FileFolder |
| OperationName |Text ||Dit veld bevat de naam van de huidige bewerking-beleid |
| Categorie |Text ||Dit veld vertegenwoordigt een categorie met diagnostische gegevens die naar Azure Monitor logboeken zijn gepusht, en is AzureBackupReport |
| Resource |Text ||Dit is de resource waarvoor gegevens worden verzameld, de naam van Recovery Services kluis wordt weer gegeven |
| PolicyUniqueId_g |Text ||Unieke id voor het identificeren van het beleid |
| PolicyName_s |Text ||De naam van het beleid dat is gedefinieerd |
| BackupFrequency_s |Text ||De frequentie waarmee back-ups worden uitgevoerd, bijvoorbeeld dagelijks, wekelijks |
| BackupTimes_s |Text ||De datum en tijd waarop back-ups zijn gepland |
| BackupDaysOfTheWeek_s |Text ||Dagen van de week waarop back-ups zijn gepland |
| RetentionDuration_s |Geheel getal ||Bewaar periode voor geconfigureerde back-ups |
| DailyRetentionDuration_s |Geheel getal ||Totale retentie duur in dagen voor geconfigureerde back-ups |
| DailyRetentionTimes_s |Text ||De datum en tijd waarop een dagelijkse Bewaar periode is geconfigureerd |
| WeeklyRetentionDuration_s |Decimaal getal ||Totale wekelijkse Bewaar periode in weken voor geconfigureerde back-ups |
| WeeklyRetentionTimes_s |Text ||De datum en tijd waarop een wekelijkse Bewaar periode is geconfigureerd |
| WeeklyRetentionDaysOfTheWeek_s |Text ||Dagen van de week geselecteerd voor een wekelijkse Bewaar periode |
| MonthlyRetentionDuration_s |Decimaal getal ||Totale Bewaar periode in maanden voor geconfigureerde back-ups |
| MonthlyRetentionTimes_s |Text ||De datum en tijd waarop de maandelijkse retentie is geconfigureerd |
| MonthlyRetentionFormat_s |Text ||Type configuratie voor de maandelijkse Bewaar periode, bijvoorbeeld dagelijks voor dag, wekelijks voor op basis van de week |
| MonthlyRetentionDaysOfTheWeek_s |Text ||Dagen van de week geselecteerd voor een maandelijkse Bewaar periode |
| MonthlyRetentionWeeksOfTheMonth_s |Text ||Weken van de maand waarin de maandelijkse retentie is geconfigureerd, bijvoorbeeld eerst, laatste, enzovoort. |
| YearlyRetentionDuration_s |Decimaal getal ||Totale Bewaar duur in jaren voor geconfigureerde back-ups |
| YearlyRetentionTimes_s |Text ||De datum en tijd waarop de jaarlijkse Bewaar periode is geconfigureerd |
| YearlyRetentionMonthsOfTheYear_s |Text ||Maanden van het jaar dat is geselecteerd voor een jaarlijkse Bewaar periode |
| YearlyRetentionFormat_s |Text ||Type configuratie voor jaarlijks bewaren, bijvoorbeeld dagelijks voor dag, wekelijks voor op basis van een week | |
| YearlyRetentionDaysOfTheMonth_s |Text ||De datums van de maand die zijn geselecteerd voor een jaarlijkse Bewaar periode |
| SynchronisationFrequencyPerDay_s |Geheel getal |v2|Aantal keren per dag dat een back-up van een bestand wordt gesynchroniseerd voor SC DPM en MABS |
| DiffBackupFormat_s |Text |v2|Indeling voor differentiële back-ups voor SQL in azure VM-back-up |
| DiffBackupTime_s |Time |v2|Tijd voor differentiële back-ups voor SQL in azure VM-back-up|
| DiffBackupRetentionDuration_s |Decimaal getal |v2|Bewaar periode voor differentiële back-ups voor SQL in azure VM-back-up|
| LogBackupFrequency_s |Decimaal getal |v2|Frequentie voor logboek back-ups voor SQL|
| LogBackupRetentionDuration_s |Decimaal getal |v2|Bewaar periode voor back-ups van Logboeken voor SQL in azure VM-back-up|
| DiffBackupDaysofTheWeek_s |Text |v2|Dagen van de week voor differentiële back-ups voor SQL in azure VM-back-up|
| SourceSystem |Text ||Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text ||De resource-id voor de gegevens die worden verzameld. Bijvoorbeeld Recovery Services kluis resource-id |
| SubscriptionId |Text ||De abonnements-id van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text ||De resource groep van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text ||Resource provider waarvoor gegevens worden verzameld. Bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text ||Het resource type waarvoor gegevens worden verzameld. Bijvoorbeeld kluizen |

### <a name="policyassociation"></a>PolicyAssociation

Deze tabel bevat details over beleids koppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Versies van toepassing | Description |
| --- | --- | --- | --- |
| EventName_s |Text ||Dit veld vertegenwoordigt de naam van deze gebeurtenis, maar is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text ||Dit veld geeft de huidige versie van het schema aan. Dit is **v2** |
| State_s |Text ||Huidige status van het beleids object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text ||Provider type voor het uitvoeren van een back-uptaak van de server, bijvoorbeeld IaaSVM, FileFolder |
| OperationName |Text ||Dit veld vertegenwoordigt de naam van de huidige bewerking-PolicyAssociation |
| Categorie |Text ||Dit veld vertegenwoordigt een categorie met diagnostische gegevens die naar Azure Monitor logboeken zijn gepusht, en is AzureBackupReport |
| Resource |Text ||Dit is de resource waarvoor gegevens worden verzameld, de naam van Recovery Services kluis wordt weer gegeven |
| PolicyUniqueId_g |Text ||Unieke id voor het identificeren van het beleid |
| VaultUniqueId_s |Text ||De unieke id van de kluis waartoe dit beleid behoort |
| BackupManagementServerUniqueId_s |Text |v2 |Veld voor het identificeren van de back-upbeheerserver waarop het back-upitem is beveiligd, indien van toepassing        |
| SourceSystem |Text ||Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text ||De resource-id voor de gegevens die worden verzameld. Bijvoorbeeld Recovery Services kluis resource-id |
| SubscriptionId |Text ||De abonnements-id van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text ||De resource groep van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text ||Resource provider waarvoor gegevens worden verzameld. Bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text ||Het resource type waarvoor gegevens worden verzameld. Bijvoorbeeld kluizen |

### <a name="protected-container"></a>Beveiligde container

Deze tabel bevat basis velden over beveiligde containers. (Was ProtectedServer in v1)

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| ProtectedContainerUniqueId_s |Text | Veld om een beveiligde container op unieke wijze te identificeren |
| ProtectedContainerOSType_s |Text |Type besturings systeem van de beveiligde container |
| ProtectedContainerOSVersion_s |Text |Versie van het besturings systeem van de beveiligde container |
| AgentVersion_s |Text |Het versie nummer van de back-up van de agent of de beveiligings agent (in het geval van SC DPM en MABS) |
| BackupManagementType_s |Text |Provider type voor het uitvoeren van back-ups. Bijvoorbeeld, IaaSVM, FileFolder |
| EntityState_s |Text |Huidige status van het beveiligde server object. Bijvoorbeeld actief, verwijderd |
| ProtectedContainerFriendlyName_s |Text |Beschrijvende naam van beveiligde server |
| ProtectedContainerName_s |Text |De naam van de beveiligde container |
| ProtectedContainerWorkloadType_s |Text |Type van de beveiligde container waarvan een back-up is gemaakt. Bijvoorbeeld IaaSVMContainer |
| ProtectedContainerLocation_s |Text |Of de beveiligde container zich lokaal of in azure bevindt |
| ProtectedContainerType_s |Text |Of de beveiligde container een server of een container is |
| ProtectedContainerProtectionState_s’  |Text |Beveiligings status van de beveiligde container |

### <a name="storage"></a>Storage

Deze tabel bevat details over velden die betrekking hebben op opslag.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| CloudStorageInBytes_s |Decimaal getal |Back-upopslag voor de cloud die wordt gebruikt voor back-ups, berekend op basis van de meest recente waarde (dit veld is alleen voor v1-schema)|
| ProtectedInstances_s |Decimaal getal |Aantal beveiligde instanties dat wordt gebruikt voor het berekenen van de front-end opslag in de facturering, berekend op basis van de nieuwste waarde |
| EventName_s |Text |Dit veld vertegenwoordigt de naam van deze gebeurtenis, maar is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema aan. Dit is **v2** |
| State_s |Text |Huidige status van het opslag object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Text |Provider type voor het uitvoeren van een back-uptaak van de server, bijvoorbeeld IaaSVM, FileFolder |
| OperationName |Text |Dit veld bevat de naam van de huidige bewerking-opslag |
| Categorie |Text |Dit veld vertegenwoordigt een categorie met diagnostische gegevens die naar Azure Monitor logboeken zijn gepusht, en is AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, de naam van Recovery Services kluis wordt weer gegeven |
| ProtectedServerUniqueId_s |Text |De unieke id van de beveiligde server waarvoor opslag wordt berekend |
| VaultUniqueId_s |Text |De unieke id van de kluis voor opslag wordt berekend |
| SourceSystem |Text |Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text |De resource-id voor de gegevens die worden verzameld. Bijvoorbeeld Recovery Services kluis resource-id |
| SubscriptionId |Text |De abonnements-id van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |De resource groep van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resource provider waarvoor gegevens worden verzameld. Bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text |Het resource type waarvoor gegevens worden verzameld. Bijvoorbeeld kluizen |
| StorageUniqueId_s |Text |Unieke id die wordt gebruikt voor het identificeren van de opslag entiteit |
| StorageType_s |Text |Type opslag, bijvoorbeeld Cloud, volume, schijf |
| StorageName_s |Text |De naam van de opslag entiteit, bijvoorbeeld E:\ |
| StorageTotalSizeInGBs_s |Text |Totale grootte van de opslag, in GB, die wordt verbruikt door de opslag entiteit|

### <a name="storageassociation"></a>StorageAssociation

Deze tabel bevat basis velden die betrekking hebben op opslag en die opslag aan andere entiteiten koppelen.

| Veld | Gegevenstype | Description |
| --- | --- |  --- |
| StorageUniqueId_s |Text |Unieke id die wordt gebruikt voor het identificeren van de opslag entiteit |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema aan. Dit is **v2** |
| BackupItemUniqueId_s |Text |Unieke id die wordt gebruikt om het back-upitem te identificeren dat is gerelateerd aan de opslag entiteit |
| BackupManagementServerUniqueId_s |Text |Unieke id die wordt gebruikt om de back-upbeheerserver te identificeren die betrekking heeft op de opslag entiteit|
| VaultUniqueId_s |Text |Unieke id die wordt gebruikt om de kluis te identificeren die is gerelateerd aan de opslag entiteit|
| StorageConsumedInMBs_s |Number|Grootte van de opslag die wordt gebruikt door het bijbehorende back-upitem in de bijbehorende opslag |
| StorageAllocatedInMBs_s |Number |Grootte van de opslag die wordt toegewezen door het bijbehorende back-upitem in de bijbehorende opslag van het type schijf|

### <a name="vault"></a>Kluis

Deze tabel bevat details over aan de kluis gerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Text |Dit veld vertegenwoordigt de naam van deze gebeurtenis, maar is altijd AzureBackupCentralReport |
| SchemaVersion_s |Text |Dit veld geeft de huidige versie van het schema aan. Dit is **v2** |
| State_s |Text |Huidige status van het kluis object, bijvoorbeeld actief, verwijderd |
| OperationName |Text |Dit veld bevat de naam van de huidige bewerking-kluis |
| Categorie |Text |Dit veld vertegenwoordigt een categorie met diagnostische gegevens die naar Azure Monitor logboeken zijn gepusht, en is AzureBackupReport |
| Resource |Text |Dit is de resource waarvoor gegevens worden verzameld, de naam van Recovery Services kluis wordt weer gegeven |
| VaultUniqueId_s |Text |De unieke id van de kluis |
| VaultName_s |Text |De naam van de kluis |
| AzureDataCenter_s |Text |Data Center waar de kluis zich bevindt |
| StorageReplicationType_s |Text |Type opslag replicatie voor de kluis, bijvoorbeeld georedundant |
| SourceSystem |Text |Bron systeem van de huidige gegevens-Azure |
| ResourceId |Text |De resource-id voor de gegevens die worden verzameld. Bijvoorbeeld Recovery Services kluis resource-id |
| SubscriptionId |Text |De abonnements-id van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Text |De resource groep van de resource (bijvoorbeeld Recovery Services kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Text |Resource provider waarvoor gegevens worden verzameld. Bijvoorbeeld micro soft. Recovery Services |
| ResourceType |Text |Het resource type waarvoor gegevens worden verzameld. Bijvoorbeeld kluizen |

### <a name="backup-management-server"></a>Beheer server voor back-up

Deze tabel bevat basis velden over back-upbeheerser vers.

|Veld  |Gegevenstype  | Description  |
|---------|---------|----------|
|BackupManagmentServerName_s     |Text         |Naam van de server voor back-upbeheer        |
|AzureBackupAgentVersion_s     |Text         |Versie van de Azure Backup-Agent op de server voor back-upbeheer          |
|BackupManagmentServerVersion_s     |Text         |Versie van de server voor back-upbeheer|
|BackupManagmentServerOSVersion_s     |Text            |Versie van het besturings systeem van de server voor back-upbeheer|
|BackupManagementServerType_s     |Text         |Type van de server voor back-upbeheer, zoals MABS, SC DPM|
|BackupManagmentServerUniqueId_s     |Text         |Veld om de back-upbeheerserver op unieke wijze te identificeren       |

### <a name="preferredworkloadonvolume"></a>PreferredWorkloadOnVolume

In deze tabel worden de werk belasting (s) aangegeven waaraan een volume is gekoppeld.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| StorageUniqueId_s |Text |Unieke id die wordt gebruikt voor het identificeren van de opslag entiteit |
| BackupItemType_s |Text |De werk belastingen waarvoor dit volume de voorkeurs opslag is|

### <a name="protectedinstance"></a>ProtectedInstance

Deze tabel bevat basis velden die betrekking hebben op een beveiligd exemplaar.

| Veld | Gegevenstype |Versies van toepassing | Description |
| --- | --- | --- | --- |
| BackupItemUniqueId_s |Text |v2|Unieke id die wordt gebruikt voor het identificeren van het back-upitem voor Vm's waarvan een back-up is gemaakt met DPM, MABS|
| ProtectedContainerUniqueId_s |Text |v2|Unieke id die wordt gebruikt om de beveiligde container te identificeren voor alles behalve Vm's waarvoor een back-up is gemaakt met behulp van DPM, MABS|
| ProtectedInstanceCount_s |Text |v2|Aantal beveiligde instanties voor het bijbehorende back-upitem of de beveiligde container op die datum-tijd|

### <a name="recoverypoint"></a>RecoveryPoint

Deze tabel bevat algemene velden voor herstel punten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| BackupItemUniqueId_s |Text |Unieke id die wordt gebruikt voor het identificeren van het back-upitem voor Vm's waarvan een back-up is gemaakt met DPM, MABS|
| OldestRecoveryPointTime_s |Text |Datum en tijd van het oudste herstel punt voor het back-upitem|
| OldestRecoveryPointLocation_s |Text |Locatie van het oudste herstel punt voor het back-upitem|
| LatestRecoveryPointTime_s |Text |Datum en tijd van het laatste herstel punt voor het back-upitem|
| LatestRecoveryPointLocation_s |Text |Locatie van het meest recente herstel punt voor het back-upitem|

## <a name="next-steps"></a>Volgende stappen

Wanneer u het gegevens model hebt gecontroleerd, kunt u beginnen met het [maken van aangepaste query's](../azure-monitor/learn/tutorial-logs-dashboards.md) in azure monitor Logboeken om uw eigen dash board te bouwen.
