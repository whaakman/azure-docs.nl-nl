---
title: Log Analytics-gegevensmodel voor Azure Backup
description: In dit artikel wordt besproken details van Log Analytics data model voor Azure backup-gegevens.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 07/24/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09f7d4c5e76d4f74d447f8e8760e1f348462c769
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887016"
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Log Analytics-gegevensmodel voor Azure backup-gegevens
De Log Analytics-gegevensmodel gebruiken om rapporten te maken. U kunt met het gegevensmodel, aangepaste query's en dashboards maken of aanpassen van Azure backup-gegevens, maar u wilt.

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
| EventName_s |Tekst |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Tekst |De unieke id van het back-upitem dat is gekoppeld aan de waarschuwing |
| SchemaVersion_s |Tekst |Huidige versie van het schema, bijvoorbeeld **V1** |
| State_s |Tekst |Huidige status van de waarschuwing object, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waarop deze waarschuwing hoort bij |
| OperationName |Tekst |Naam van de huidige bewerking, bijvoorbeeld: waarschuwing |
| Categorie |Tekst |De categorie van diagnostische gegevens te pushen naar Log Analytics. Always AzureBackupReport |
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
| ProtectedServerName_s |Tekst |Naam van de beveiligde server naar welke back-upitem behoort |
| ProtectionState_s |Tekst |Huidige beveiligingsstatus van de back-upitem, bijvoorbeeld, beveiligde, ProtectionStopped |
| SchemaVersion_s |Tekst |Versie van het schema, bijvoorbeeld **V1** |
| State_s |Tekst |Status van de back-upitem-object, bijvoorbeeld, actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waaraan dit back-upitem behoort |
| OperationName |Tekst |Naam van de bewerking, bijvoorbeeld BackupItem |
| Categorie |Tekst |De categorie van diagnostische gegevens te pushen naar Log Analytics. Always AzureBackupReport |
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
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V1** |
| State_s |Tekst |Huidige status van het back-upitem koppeling-object, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - BackupItemAssociation |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
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

### <a name="job"></a>Taak
Deze tabel bevat details over velden met betrekking tot de taak.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |De naam van de gebeurtenis. Always AzureBackupCentralReport |
| BackupItemUniqueId_s |Tekst |De unieke id van het back-upitem |
| SchemaVersion_s |Tekst |Versie van het schema, bijvoorbeeld **V1** |
| State_s |Tekst |Huidige status van het taakobject, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - taak |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |De unieke id van de beveiligde server die de taak |
| VaultUniqueId_s |Tekst |De unieke id van de beveiligde kluis |
| JobOperation_s |Tekst |Bewerking voor die taak wordt uitgevoerd, bijvoorbeeld back-up, herstel, configureren van back-up |
| JobStatus_s |Tekst |Status van de voltooide taak, bijvoorbeeld, voltooid, is mislukt |
| JobFailureCode_s |Tekst |Mislukt codetekenreeks vanwege die taakfout opgetreden |
| JobStartDateTime_s |Datum/tijd |Datum en tijd wanneer de taak gestart die wordt uitgevoerd |
| BackupStorageDestination_s |Tekst |Bestemming van back-upopslag, bijvoorbeeld Cloud, schijf  |
| JobDurationInSecs_s | Aantal |Totaal aantal taak duur in seconden |
| DataTransferredInMB_s | Aantal |Gegevens die worden overgedragen in MB voor deze taak|
| JobUniqueId_g |Tekst |De unieke Id voor het identificeren van de taak |
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
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V1** |
| State_s |Tekst |Huidige status van het Groepsbeleid-object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - beleid |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
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
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V1** |
| State_s |Tekst |Huidige status van het Groepsbeleid-object, bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - PolicyAssociation |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Tekst |De unieke Id voor het identificeren van het beleid |
| VaultUniqueId_s |Tekst |De unieke Id van de kluis waarop dit beleid behoort |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="protectedserver"></a>ProtectedServer
Deze tabel bevat informatie over beveiligde velden met betrekking tot de server.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| ProtectedServerName_s |Tekst |Naam van de beveiligde server |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V1** |
| State_s |Tekst |Huidige status van de beveiligde server-object, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - ProtectedServer |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |De unieke Id van de beveiligde server |
| RegisteredContainerId_s |Tekst |Id van de container is geregistreerd voor back-up |
| ProtectedServerType_s |Tekst |Type van de beveiligde server, bijvoorbeeld Windows |
| ProtectedServerFriendlyName_s |Tekst |Beschrijvende naam van de beveiligde server |
| AzureBackupAgentVersion_s |Tekst |Versienummer van Agent back-up-versie |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Deze tabel bevat informatie over de beveiligde server koppelingen met andere entiteiten.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V1** |
| State_s |Tekst |Huidige status van de beveiligde server koppelingsobject, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - ProtectedServerAssociation |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |De unieke Id van de beveiligde server |
| VaultUniqueId_s |Tekst |De unieke Id van de kluis waarnaar deze beveiligde server behoort |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="storage"></a>Storage
Deze tabel bevat details over velden met betrekking tot opslag.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| CloudStorageInBytes_s |Decimaal getal |Back-cloudopslag die wordt gebruikt door back-ups, berekend op basis van de meest recente |
| ProtectedInstances_s |Decimaal getal |Aantal beveiligde exemplaren die worden gebruikt voor het berekenen van de frontend-opslag in facturering, berekend op basis van de laatste waarde |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V1** |
| State_s |Tekst |Huidige status van het opslagobject, bijvoorbeeld: actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor de server die back-uptaak, bijvoorbeeld IaaSVM, FileFolder doen |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - opslag |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvoor gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |De unieke Id van de beveiligde server waarvoor de opslag wordt berekend |
| VaultUniqueId_s |Tekst |De unieke Id van de kluis voor de opslag wordt berekend |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevens - Azure |
| ResourceId |Tekst |Resource-id voor gegevens die worden verzameld. Bijvoorbeeld: de resource-id voor Recovery Services-kluis |
| SubscriptionId |Tekst |Abonnements-id van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Resourcegroep van de resource (ex.) Recovery Services-kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |De resourceprovider waarvoor gegevens worden verzameld. Bijvoorbeeld, Microsoft.RecoveryServices |
| ResourceType |Tekst |Het type van de resource waarvoor gegevens worden verzameld. Bijvoorbeeld-kluizen |

### <a name="vault"></a>Kluis
Deze tabel bevat details over velden met betrekking tot de kluis.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, het is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld geeft de huidige versie van het schema, is het **V1** |
| State_s |Tekst |Huidige status van het object van de kluis, bijvoorbeeld: actief, verwijderd |
| OperationName |Tekst |Dit veld vertegenwoordigt de naam van de huidige bewerking - kluis |
| Categorie |Tekst |Dit veld categorie van diagnostische gegevens naar Log Analytics gepusht vertegenwoordigt, is het AzureBackupReport |
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

## <a name="next-steps"></a>Volgende stappen
Als u het gegevensmodel voor het maken van Azure Backup-rapporten bekijkt, kunt u starten [dashboard maken](../azure-monitor/learn/tutorial-logs-dashboards.md) in Log Analytics.
