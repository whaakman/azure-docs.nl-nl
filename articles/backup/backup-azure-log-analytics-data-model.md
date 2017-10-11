---
title: Log Analytics-gegevensmodel voor Azure Backup
description: In dit artikel wordt gesproken over logboekanalyse Gegevensdetails model voor Azure backup-gegevens.
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: dfd5c73d-0d34-4d48-959e-1936986f9fc0
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 041a8835a1dd185739b23d4073fd5811bb4490b5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2017
---
# <a name="log-analytics-data-model-for-azure-backup-data"></a>Log Analytics-gegevensmodel voor Azure backup-gegevens
In dit artikel beschrijft het gegevensmodel dat wordt gebruikt voor logboekanalyse reporting gegevens worden gepusht. Met dit gegevensmodel, kunt u aangepaste query's, dashboards maken en gebruiken in OMS. 

## <a name="using-azure-backup-data-model"></a>Met behulp van Azure Backup-gegevensmodel
U kunt de volgende velden geleverd als onderdeel van het gegevensmodel gebruiken voor het maken van visuele elementen, aangepaste query's en dashboard volgens uw vereisten.

### <a name="alert"></a>Waarschuwing
Deze tabel bevat informatie over verwante waarschuwingsvelden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| AlertUniqueId_s |Tekst |Unieke Id van de gegenereerde waarschuwing |
| AlertType_s |Tekst |Type van de gegenereerde waarschuwing, bijvoorbeeld: back-up |
| AlertStatus_s |Tekst |Status van de waarschuwing, bijvoorbeeld, actief |
| AlertOccurenceDateTime_s |Datum/tijd |Datum en tijd waarop de waarschuwing is gemaakt |
| AlertSeverity_s |Tekst |Ernst van de waarschuwing, bijvoorbeeld: kritiek |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| BackupItemUniqueId_s |Tekst |Unieke Id van het back-item op te geven waarop deze waarschuwing behoort |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van de waarschuwing object, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waarvan deze waarschuwing deel uitmaakt |
| OperationName |Tekst |Dit veld wordt de naam van de huidige bewerking - waarschuwing |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |Unieke Id van de beveiligde waarvan deze waarschuwing deel uitmaakt |
| VaultUniqueId_s |Tekst |Unieke Id van de beveiligde waarvan deze waarschuwing deel uitmaakt |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="backupitem"></a>BackupItem
Deze tabel bevat informatie over back-item-gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |  
| BackupItemUniqueId_s |Tekst |Unieke Id van het back-item |
| BackupItemId_s |Tekst |Id van de back-item |
| BackupItemName_s |Tekst |Naam van back-item |
| BackupItemFriendlyName_s |Tekst |Beschrijvende naam van back-item |
| BackupItemType_s |Tekst |Type back-item, bijvoorbeeld VM FileFolder |
| ProtectedServerName_s |Tekst |Naam van de beveiligde server naar welke back-item behoort tot |
| ProtectionState_s |Tekst |Huidige beveiligingsstatus van de back-item, bijvoorbeeld beveiligde, ProtectionStopped |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van het object van de back-item, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waarnaar het back-artikel behoort |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - BackupItem |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="backupitemassociation"></a>BackupItemAssociation
Deze tabel bevat informatie over back-upitems koppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |  
| BackupItemUniqueId_s |Tekst |Unieke Id van het back-item |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van het back-koppelingsobject item, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waarnaar het back-artikel behoort |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - BackupItemAssociation |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Tekst |De unieke Id voor het identificeren van het beleid, welke back-item is gekoppeld aan |
| ProtectedServerUniqueId_s |Tekst |Unieke Id van de beveiligde server waarnaar het back-artikel behoort |
| VaultUniqueId_s |Tekst |Unieke Id van de kluis op te geven dat dit back-item behoort |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="job"></a>Job
Deze tabel bevat informatie over velden met betrekking tot de taak.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| BackupItemUniqueId_s |Tekst |Unieke Id van het back-item op te geven waarvan deze taak behoort |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van het taakobject, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder op te geven waarvan deze taak behoort |
| OperationName |Tekst |Dit veld wordt de naam van de huidige bewerking - taak |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |Unieke Id van de beveiligde op te geven waarvan deze taak behoort |
| VaultUniqueId_s |Tekst |Unieke Id van de beveiligde op te geven waarvan deze taak behoort |
| JobOperation_s |Tekst |Bewerking voor die taak wordt uitgevoerd bijvoorbeeld back-up, herstel, Configureer back-up |
| JobStatus_s |Tekst |Status van de voltooide taak, bijvoorbeeld voltooid, is mislukt |
| JobFailureCode_s |Tekst |Code string voor mislukt vanwege die taakfout hebben plaatsgevonden |
| JobStartDateTime_s |Datum/tijd |Datum en tijd wanneer taak gestart die wordt uitgevoerd |
| BackupStorageDestination_s |Tekst |Bestemming van back-upopslag, bijvoorbeeld Cloud, schijf  |
| JobDurationInSecs_s | Aantal |Totaal aantal taak duur in seconden |
| DataTransferredInMB_s | Aantal |Gegevens die voor deze taak worden overgebracht in MB|
| JobUniqueId_g |Tekst |De unieke Id voor het identificeren van de taak |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="policy"></a>Beleid
Deze tabel bevat informatie over beleid gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van het beleidsobject, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder waarop dit beleid behoort |
| OperationName |Tekst |Dit veld wordt de naam van de huidige bewerking - beleid |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Tekst |De unieke Id voor het identificeren van het beleid |
| PolicyName_s |Tekst |Naam van het beleid gedefinieerd |
| BackupFrequency_s |Tekst |Frequentie waarmee de back-ups worden uitgevoerd, bijvoorbeeld dagelijks, wekelijks |
| BackupTimes_s |Tekst |Datum en tijd wanneer de back-ups zijn gepland |
| BackupDaysOfTheWeek_s |Tekst |Dagen van de week wanneer back-ups zijn gepland |
| RetentionDuration_s |Geheel getal |Duur van de bewaarperiode voor geconfigureerde back-ups |
| DailyRetentionDuration_s |Geheel getal |Duur van de totale bewaartermijn in dagen voor geconfigureerde back-ups |
| DailyRetentionTimes_s |Tekst |Datum en tijd wanneer de dagelijkse bewaarperiode is geconfigureerd |
| WeeklyRetentionDuration_s |Decimaal getal zijn |Totale duur van de wekelijkse bewaren weken voor geconfigureerde back-ups |
| WeeklyRetentionTimes_s |Tekst |Datum en tijd wanneer de wekelijkse bewaarperiode is geconfigureerd |
| WeeklyRetentionDaysOfTheWeek_s |Tekst |Dagen van de week geselecteerd voor het bewaren van wekelijks |
| MonthlyRetentionDuration_s |Decimaal getal zijn |Duur van de totale bewaarperiode in maanden voor geconfigureerde back-ups |
| MonthlyRetentionTimes_s |Tekst |Datum en tijd wanneer maandelijkse bewaarperiode is geconfigureerd |
| MonthlyRetentionFormat_s |Tekst |Configuratie voor het maandelijkse bewaren, bijvoorbeeld dagelijks voor de dag gebaseerd wekelijks voor de week op basis van type |
| MonthlyRetentionDaysOfTheWeek_s |Tekst |Dagen van de week geselecteerd voor het maandelijkse bewaren |
| MonthlyRetentionWeeksOfTheMonth_s |Tekst |Weken van de maand wanneer maandelijkse bewaarperiode is geconfigureerd, bijvoorbeeld de eerste, laatste enzovoort. |
| YearlyRetentionDuration_s |Decimaal getal zijn |Duur van de totale bewaartermijn in jaren voor geconfigureerde back-ups |
| YearlyRetentionTimes_s |Tekst |Datum en tijd wanneer jaarlijks bewaarbeleid is geconfigureerd |
| YearlyRetentionMonthsOfTheYear_s |Tekst |Maanden van het jaar geselecteerd voor het bewaren van jaarlijkse |
| YearlyRetentionFormat_s |Tekst |Configuratie voor jaarlijkse bewaren, bijvoorbeeld dagelijks voor de dag gebaseerd wekelijks voor de week op basis van type |
| YearlyRetentionDaysOfTheMonth_s |Tekst |Datums van de maand voor het bewaren van jaarlijkse geselecteerd |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="policyassociation"></a>PolicyAssociation
Deze tabel bevat informatie over beleid koppelingen met verschillende entiteiten.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van het beleidsobject, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up bijvoorbeeld IaaSVM FileFolder waarop dit beleid behoort |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - PolicyAssociation |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| PolicyUniqueId_g |Tekst |De unieke Id voor het identificeren van het beleid |
| VaultUniqueId_s |Tekst |Unieke Id van de kluis op te geven waarop dit beleid behoort |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="protectedserver"></a>ProtectedServer
Deze tabel bevat informatie over beveiligde velden met betrekking tot de server.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| ProtectedServerName_s |Tekst |Naam van de beveiligde server |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van de beveiligde server-object, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up bijvoorbeeld IaaSVM FileFolder op te geven die deze beveiligde server behoort |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - ProtectedServer |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |Unieke Id van de beveiligde server |
| RegisteredContainerId_s |Tekst |Id van de container die is geregistreerd voor back-up |
| ProtectedServerType_s |Tekst |Type van de beveiligde server een back-up bijvoorbeeld Windows |
| ProtectedServerFriendlyName_s |Tekst |Beschrijvende naam van de beveiligde server |
| AzureBackupAgentVersion_s |Tekst |Versienummer van Agent back-up-versie |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="protectedserverassociation"></a>ProtectedServerAssociation
Deze tabel bevat informatie over de beveiligde server koppelingen met andere entiteiten.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van de beveiligde server koppelingsobject, bijvoorbeeld Active, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder op te geven die deze beveiligde server behoort |
| OperationName |Tekst |Dit veld vertegenwoordigt naam van de huidige bewerking - ProtectedServerAssociation |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |Unieke Id van de beveiligde server |
| VaultUniqueId_s |Tekst |Unieke Id van de kluis op te geven die deze beveiligde server behoort |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="storage"></a>Storage
Deze tabel bevat informatie over velden met betrekking tot opslag.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| CloudStorageInBytes_s |Decimaal getal zijn |Back-cloudopslag die wordt gebruikt door de back-ups, berekend op basis van de laatste waarde |
| ProtectedInstances_s |Decimaal getal zijn |Het aantal beveiligde exemplaren dat wordt gebruikt voor het berekenen van de frontend-opslag in facturering, berekend op basis van de laatste waarde |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van het opslagobject bijvoorbeeld actief, verwijderd |
| BackupManagementType_s |Tekst |Providertype voor het uitvoeren van back-up, bijvoorbeeld IaaSVM FileFolder op te geven die deze opslag behoort |
| OperationName |Tekst |Dit veld wordt de naam van de huidige bewerking - opslag |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| ProtectedServerUniqueId_s |Tekst |Unieke Id van de beveiligde server waarvoor de opslag wordt berekend |
| VaultUniqueId_s |Tekst |Unieke Id van de kluis voor de opslag wordt berekend |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld representse type van de resource waarvan gegevens worden verzameld - kluizen |

### <a name="vault"></a>Kluis
Deze tabel bevat informatie over velden met betrekking tot de kluis.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| EventName_s |Tekst |Dit veld naam van deze gebeurtenis vertegenwoordigt, is altijd AzureBackupCentralReport |
| SchemaVersion_s |Tekst |Dit veld wordt de huidige versie van het schema geeft, wordt deze **V1** |
| State_s |Tekst |Huidige status van het object kluis, bijvoorbeeld Active, verwijderd |
| OperationName |Tekst |Dit veld wordt de naam van de huidige bewerking - kluis |
| Category |Tekst |Dit veld wordt de categorie van diagnostische gegevens naar het Log Analytics gepusht, is het AzureBackupReport |
| Resource |Tekst |Dit is de resource waarvan gegevens worden verzameld, wordt de naam van de Recovery Services-kluis |
| VaultUniqueId_s |Tekst |Unieke Id van de kluis |
| VaultName_s |Tekst |Naam van de kluis |
| AzureDataCenter_s |Tekst |Datacenter waar kluis zich bevindt |
| StorageReplicationType_s |Tekst |Type van de storage-replicatie voor de kluis, bijvoorbeeld GeoRedundant |
| SourceSystem |Tekst |Bronsysteem van de huidige gegevensarchiefopslag - Azure |
| ResourceId |Tekst |Dit veld geeft resource-id waarvoor gegevens worden verzameld, wordt een melding weergegeven Recovery Services-kluis resource-id |
| SubscriptionId |Tekst |Dit veld wordt de abonnement-id van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceGroup |Tekst |Dit veld wordt de resourcegroep van de resource (RS kluis) waarvoor gegevens worden verzameld |
| ResourceProvider |Tekst |Dit veld wordt de resourceprovider waarvoor gegevens worden verzameld - Microsoft.RecoveryServices |
| ResourceType |Tekst |Dit veld vertegenwoordigt type van de resource waarvan gegevens worden verzameld - kluizen |

## <a name="next-steps"></a>Volgende stappen
Wanneer u het gegevensmodel voor het maken van Azure Backup rapporten bekijkt, kunt u beginnen [dashboard maken](../log-analytics/log-analytics-dashboards.md) in Log Analytics en OMS.
