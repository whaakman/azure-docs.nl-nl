---
title: Het gegevensmodel voor Azure Backup
description: In dit artikel wordt gesproken over Power BI Gegevensdetails model voor Azure Backup-rapporten.
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 0767c330-690d-474d-85a6-aa8ddc410bb2
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/26/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: efecbc9f1c410744f49795889c4ec3cc618f07e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="data-model-for-azure-backup-reports"></a>Gegevensmodel voor Azure Backup-rapporten
In dit artikel beschrijft de Power BI-gegevensmodel gebruikt voor het maken van Azure Backup-rapporten. Met deze gegevensmodel, kunt u bestaande rapporten op basis van de betreffende velden filteren en meer echter uw eigen rapporten maken met behulp van tabellen en velden in het model. 

## <a name="creating-new-reports-in-power-bi"></a>Maken van nieuwe rapporten in Power BI
Power BI biedt die u kunt met aanpassingsfuncties [rapporten maken met het gegevensmodel](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Met behulp van Azure Backup-gegevensmodel
U kunt de volgende velden geleverd als onderdeel van het gegevensmodel gebruiken voor het maken van rapporten en bestaande rapporten aanpassen.

### <a name="alert"></a>Waarschuwing
Deze tabel bevat basisvelden en aggregaties via verschillende gerelateerde waarschuwingsvelden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Geheel getal |Aantal waarschuwingen dat is gemaakt in de geselecteerde tijdsperiode |
| % ActiveAlertsCreatedInPeriod |Percentage |Percentage van actieve waarschuwingen in de geselecteerde tijdsperiode |
| % CriticalAlertsCreatedInPeriod |Percentage |Percentage van kritieke waarschuwingen in de geselecteerde tijdsperiode |
| AlertOccurenceDate |Date |Datum waarop de waarschuwing is gemaakt |
| AlertSeverity |Tekst |Ernst van waarschuwing bijvoorbeeld kritiek |
| AlertStatus |Tekst |Status van de waarschuwing bijvoorbeeld actief |
| AlertType |Tekst |Type van de gegenereerde waarschuwing bijvoorbeeld back-up |
| AlertUniqueId |Tekst |Unieke Id van de gegenereerde waarschuwing |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Decimaal getal zijn |Gemiddelde tijd (in minuten) om op te lossen waarschuwing voor geselecteerde tijdsperiode |
| EntityState |Tekst |Huidige status van de waarschuwing object bijvoorbeeld actief, verwijderd |

### <a name="backup-item"></a>Back-Item
Deze tabel bevat basisvelden en aggregaties gedurende verschillende back-item-gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #BackupItems |Geheel getal |Aantal back-items |
| #UnprotectedBackupItems |Geheel getal |Aantal back-items voor beveiliging is gestopt of geconfigureerd voor back-ups maar back-ups niet gestart|
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| BackupItemFriendlyName |Tekst |Beschrijvende naam van back-item |
| BackupItemId |Tekst |Id van de back-item |
| BackupItemName |Tekst |Naam van back-item |
| BackupItemType |Tekst |Type back-item bijvoorbeeld VM FileFolder |
| EntityState |Tekst |Huidige status van het object van het back-item bijvoorbeeld actief, verwijderd |
| LastBackupDateTime |Datum/tijd |Tijd van laatste back-up voor de geselecteerde back-item |
| LastBackupState |Tekst |Status van de laatste back-up voor de geselecteerde back-artikel bijvoorbeeld geslaagd, mislukt |
| LastSuccessfulBackupDateTime |Datum/tijd |Tijd van laatste geslaagde back-up voor de geselecteerde back-item |
| ProtectionState |Tekst |Huidige beveiligingsstatus van de back-item bijvoorbeeld beveiligde, ProtectionStopped |

### <a name="calendar"></a>Agenda
Deze tabel bevat informatie over velden met betrekking tot de agenda.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Date |Date |Datum geselecteerd voor het filteren van gegevens |
| DateKey |Tekst |Unieke sleutel voor elk item datum |
| DayDiff |Decimaal getal zijn |Verschil in dagen voor het filteren van gegevens, bijvoorbeeld, 0 geeft aan dat de gegevens van de huidige dag, -1 geeft aan dat de gegevens van de vorige één dag, 0 en 1 geven gegevens voor de huidige en vorige dag  |
| Maand |Tekst |Maand van het jaar geselecteerd voor het filteren van gegevens, maand op de eerste dag begint en eindigt op 31e dag |
| MonthDate | Date |Datum in de maand waarop maand eindigt, geselecteerd voor het filteren van gegevens |
| MonthDiff |Decimaal getal zijn |Verschil in maand voor het filteren van gegevens, bijvoorbeeld, 0 geeft aan dat gegevens van de huidige maand, -1 geeft aan dat gegevens van vorige maand, 0 en 1 geven gegevens voor de huidige en vorige maand |
| Week |Tekst |Week geselecteerd voor het filteren van gegevens, week op zondag begint en eindigt op zaterdag |
| WeekDate |Date |Datum in de week waarop week eindigt, geselecteerd voor het filteren van gegevens |
| WeekDiff |Decimaal getal zijn |Verschil in de week voor het filteren van gegevens, bijvoorbeeld, 0 geeft aan dat gegevens van de huidige week, -1 geeft aan dat gegevens van de vorige week, 0 en 1 geven gegevens voor de huidige en vorige week |
| jaar |Tekst |Kalenderjaar geselecteerd voor het filteren van gegevens |
| YearDate |Date |Datum in het jaar jaar wanneer eindigt, geselecteerd voor het filteren van gegevens |

### <a name="job"></a>Job
Deze tabel bevat basisvelden en aggregaties via verschillende velden met betrekking tot de taak.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #JobsCreatedInPeriod |Geheel getal |Aantal taken die zijn gemaakt in de geselecteerde tijdsperiode |
| % FailuresForJobsCreatedInPeriod |Percentage |Percentage taak algemene fouten in de geselecteerde tijdsperiode |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Decimaal getal zijn |80e percentielwaarde van de gegevens worden overgebracht in MB voor **back-** taken die zijn gemaakt in de geselecteerde tijdsperiode |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Decimaal getal zijn |Gemiddelde tijd in minuten voor **voltooide back-up** taken die zijn gemaakt in de geselecteerde tijdsperiode |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Decimaal getal zijn |Gemiddelde tijd in minuten voor **terugzetten voltooid** taken die zijn gemaakt in de geselecteerde tijdsperiode |
| BackupStorageDestination |Tekst |Bestemming van back-upopslag bijvoorbeeld Cloud schijf  |
| EntityState |Tekst |Huidige status van het taakobject bijvoorbeeld actief, verwijderd |
| JobFailureCode |Tekst |Code string voor mislukt vanwege die taakfout hebben plaatsgevonden |
| JobOperation |Tekst |Bewerking voor die taak wordt uitgevoerd bijvoorbeeld back-up, herstel, Configureer back-up |
| JobStartDate |Date |Datum waarop de taak is gestart |
| JobStartTime |Time |Tijd wanneer de taak is gestart |
| JobStatus |Tekst |Status van de voltooide taak bijvoorbeeld voltooid, is mislukt |
| JobUniqueId |Tekst |De unieke Id voor het identificeren van de taak |

### <a name="policy"></a>Beleid
Deze tabel bevat basisvelden en aggregaties via verschillende beleid gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #Policies |Geheel getal |Aantal back-upbeleid dat in het systeem bestaat |
| #PoliciesInUse |Geheel getal |Aantal beleidsregels die momenteel wordt gebruikt voor back-ups configureren |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| BackupDaysOfTheWeek |Tekst |Dagen van de week wanneer back-ups zijn gepland |
| BackupFrequency |Tekst |Frequentie waarmee back-ups worden uitgevoerd, bijvoorbeeld dagelijks, wekelijks |
| BackupTimes |Tekst |Datum en tijd wanneer de back-ups zijn gepland |
| DailyRetentionDuration |Geheel getal |Duur van de totale bewaartermijn in dagen voor geconfigureerde back-ups |
| DailyRetentionTimes |Tekst |Datum en tijd wanneer de dagelijkse bewaarperiode is geconfigureerd |
| EntityState |Tekst |Huidige status van het beleidsobject bijvoorbeeld actief, verwijderd |
| MonthlyRetentionDaysOfTheMonth |Tekst |Datums van de maand geselecteerd voor het maandelijkse bewaren |
| MonthlyRetentionDaysOfTheWeek |Tekst |Dagen van de week geselecteerd voor het maandelijkse bewaren |
| MonthlyRetentionDuration |Decimaal getal zijn |Duur van de totale bewaarperiode in maanden voor geconfigureerde back-ups |
| MonthlyRetentionFormat |Tekst |Typ van configuratie voor het bewaren van maandelijkse bijvoorbeeld dagelijks voor dag wekelijks is gebaseerd op basis van week |
| MonthlyRetentionTimes |Tekst |Datum en tijd wanneer maandelijkse bewaarperiode is geconfigureerd |
| MonthlyRetentionWeeksOfTheMonth |Tekst |Weken van de maand wanneer maandelijkse bewaartermijn is geconfigureerd, bijvoorbeeld First, Last, enzovoort. |
| PolicyName |Tekst |Naam van het beleid gedefinieerd |
| PolicyUniqueId |Tekst |De unieke Id voor het identificeren van het beleid |
| RetentionType |Tekst |Type bewaarbeleid bijvoorbeeld, dagelijks, wekelijks, maandelijks, jaarlijks |
| WeeklyRetentionDaysOfTheWeek |Tekst |Dagen van de week geselecteerd voor het bewaren van wekelijks |
| WeeklyRetentionDuration |Decimaal getal zijn |Totale duur van de wekelijkse bewaren weken voor geconfigureerde back-ups |
| WeeklyRetentionTimes |Tekst |Datum en tijd wanneer de wekelijkse bewaarperiode is geconfigureerd |
| YearlyRetentionDaysOfTheMonth |Tekst |Datums van de maand voor het bewaren van jaarlijkse geselecteerd |
| YearlyRetentionDaysOfTheWeek |Tekst |Dagen van de week geselecteerd voor het bewaren van jaarlijkse |
| YearlyRetentionDuration |Decimaal getal zijn |Duur van de totale bewaartermijn in jaren voor geconfigureerde back-ups |
| YearlyRetentionFormat |Tekst |Typ van configuratie voor het bewaren van jaarlijkse bijvoorbeeld dagelijks voor dag wekelijks is gebaseerd op basis van week |
| YearlyRetentionMonthsOfTheYear |Tekst |Maanden van het jaar geselecteerd voor het bewaren van jaarlijkse |
| YearlyRetentionTimes |Tekst |Datum en tijd wanneer jaarlijks bewaarbeleid is geconfigureerd |
| YearlyRetentionWeeksOfTheMonth |Tekst |Weken van de maand wanneer de jaarlijkse bewaartermijn is geconfigureerd, bijvoorbeeld First, Last, enzovoort. |

### <a name="protected-server"></a>Beveiligde Server
Deze tabel bevat basisvelden en aggregaties via verschillende beveiligde server-gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #ProtectedServers |Geheel getal |Het aantal beveiligde servers |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AzureBackupAgentOSType |Tekst |Type besturingssysteem van Azure Backup Agent |
| AzureBackupAgentOSVersion |Tekst |OS-versie van Azure Backup Agent |
| AzureBackupAgentUpdateDate |Tekst |Datum waarop de Agent Backup-Agent is bijgewerkt |
| AzureBackupAgentVersion |Tekst |Versienummer van Agent back-up-versie |
| BackupManagementType |Tekst |Providertype voor het uitvoeren van back-up bijvoorbeeld IaaSVM FileFolder |
| EntityState |Tekst |Huidige status van het object van de beveiligde server bijvoorbeeld actief, verwijderd |
| ProtectedServerFriendlyName |Tekst |Beschrijvende naam van de beveiligde server |
| ProtectedServerName |Tekst |Naam van de beveiligde server |
| ProtectedServerType |Tekst |Type van de beveiligde server een back-up bijvoorbeeld IaaSVMContainer |
| ProtectedServerName |Tekst |Naam van de beveiligde server naar welke back-item behoort |
| RegisteredContainerId |Tekst |Id van de container die is geregistreerd voor back-up |

### <a name="storage"></a>Storage
Deze tabel bevat basisvelden en aggregaties via verschillende velden met betrekking tot opslag.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #ProtectedInstances |Decimaal getal zijn |Het aantal beveiligde exemplaren dat wordt gebruikt voor het berekenen van frontend-opslag in facturering, berekend op basis van de laatste waarde in de geselecteerde tijd |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| CloudStorageInMB |Decimaal getal zijn |Back-cloudopslag die wordt gebruikt door de back-ups, berekend op basis van de laatste waarde in de geselecteerde tijd |
| EntityState |Tekst |Huidige status van het object bijvoorbeeld actief, verwijderd |
| LastUpdatedDate |Date |Datum waarop de geselecteerde rij voor het laatst is bijgewerkt |

### <a name="time"></a>Time
Deze tabel bevat informatie over velden met betrekking tot de tijd.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Uur |Time |Uur van de dag bijvoorbeeld 13:00:00 uur |
| HourNumber |Decimaal getal zijn |Het aantal uren van de dag bijvoorbeeld 13,00 |
| Minuut |Decimaal getal zijn |Minuut van het uur |
| PeriodOfTheDay |Tekst |Periode tijdsperiode van de dag bijvoorbeeld 12-3 uur |
| Time |Time |Tijd van de dag bijvoorbeeld 12:00:01 uur |
| TimeKey |Tekst |De sleutelwaarde te vertegenwoordigen tijd |

### <a name="vault"></a>Kluis
Deze tabel bevat basisvelden en aggregaties via verschillende velden met betrekking tot de kluis.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #Vaults |Geheel getal |Het aantal kluizen |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AzureDataCenter |Tekst |Datacenter waar kluis zich bevindt |
| EntityState |Tekst |Huidige status van het object kluis bijvoorbeeld actief, verwijderd |
| StorageReplicationType |Tekst |Type van de storage-replicatie voor de kluis bijvoorbeeld GeoRedundant |
| SubscriptionId |Tekst |Abonnement-Id van de klant geselecteerd voor het genereren van rapporten |
| VaultName |Tekst |Naam van de kluis |
| VaultTags |Tekst |Labels die is gekoppeld aan de kluis |

## <a name="next-steps"></a>Volgende stappen
Zodra u het gegevensmodel voor het maken van Azure Backup rapporten bekijken, raadpleegt u de volgende artikelen voor meer informatie over het maken en weergeven van rapporten in Power BI.

* [Maken van rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filteren van rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
