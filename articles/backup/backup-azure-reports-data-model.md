---
title: Gegevensmodel voor Azure Backup
description: In dit artikel wordt besproken details van een Power BI-gegevens-model, voor Azure Backup-rapporten.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: adigan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b1531e23d0e5fd34eff59868055ccd855b423e4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37444300"
---
# <a name="data-model-for-azure-backup-reports"></a>Gegevensmodel voor Azure Backup-rapporten
Dit artikel beschrijft het gegevensmodel van Power BI gebruikt voor het maken van Azure Backup-rapporten. Met dit gegevensmodel, kunt u filteren op bestaande rapporten op basis van relevante velden en informatie bovendien uw eigen rapporten maken met behulp van tabellen en velden in het model. 

## <a name="creating-new-reports-in-power-bi"></a>Het maken van nieuwe rapporten in Power BI
Power BI biedt functies die u kunt met [rapporten maken met het gegevensmodel](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Met behulp van Azure Backup-gegevensmodel
Rapporten maken en aanpassen van bestaande rapporten kunt u de volgende velden geleverd als onderdeel van het gegevensmodel.

### <a name="alert"></a>Waarschuwing
Deze tabel bevat algemene velden en aggregaties die via verschillende gerelateerde waarschuwingsvelden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Geheel getal |Aantal waarschuwingen die zijn gemaakt in een geselecteerde tijdsperiode |
| % ActiveAlertsCreatedInPeriod |Percentage |Percentage van actieve waarschuwingen in de geselecteerde periode |
| % CriticalAlertsCreatedInPeriod |Percentage |Percentage van de kritieke waarschuwingen in de geselecteerde periode |
| AlertOccurenceDate |Date |Datum waarop de waarschuwing is gemaakt |
| AlertSeverity |Tekst |Ernst van de waarschuwing bijvoorbeeld: kritiek |
| AlertStatus |Tekst |Status van de waarschuwing bijvoorbeeld actief |
| AlertType |Tekst |Type van de gegenereerde waarschuwing bijvoorbeeld back-up |
| AlertUniqueId |Tekst |De unieke Id van de gegenereerde waarschuwing |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Decimaal getal |Gemiddelde tijd (in minuten) op te lossen waarschuwing voor geselecteerde tijdsperiode |
| EntityState |Tekst |Huidige status van de waarschuwing object bijvoorbeeld actief, verwijderd |

### <a name="backup-item"></a>Back-upitem
Deze tabel bevat de algemene velden en aggregaties die via verschillende back-item-gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #BackupItems |Geheel getal |Aantal back-upitems |
| #UnprotectedBackupItems |Geheel getal |Aantal back-upitems voor beveiliging is gestopt of geconfigureerd voor back-ups, maar back-ups niet gestart|
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| BackupItemFriendlyName |Tekst |Beschrijvende naam van de back-upitem |
| BackupItemId |Tekst |Id van de back-upitem |
| BackupItemName |Tekst |Naam van de back-upitem |
| BackupItemType |Tekst |Type back-upitem bijvoorbeeld VM FileFolder |
| EntityState |Tekst |Huidige status van het object back-upitem bijvoorbeeld actief, verwijderd |
| LastBackupDateTime |Datum/tijd |Tijd van laatste back-up voor de geselecteerde back-upitem |
| LastBackupState |Tekst |Status van de laatste back-up voor de geselecteerde back-upitem bijvoorbeeld geslaagd, mislukt |
| LastSuccessfulBackupDateTime |Datum/tijd |Tijd van laatste geslaagde back-up voor de geselecteerde back-upitem |
| ProtectionState |Tekst |Huidige beveiligingsstatus van de back-upitem bijvoorbeeld beveiligd, ProtectionStopped |

### <a name="calendar"></a>Agenda
Deze tabel bevat details over velden met betrekking tot de agenda.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Date |Date |Datum geselecteerd voor het filteren van gegevens |
| DateKey |Tekst |Unieke sleutel voor elk item datum |
| DayDiff |Decimaal getal |Bijvoorbeeld het verschil in dagen voor het filteren van gegevens, 0 geeft aan dat de gegevens van de huidige dag, -1 geeft aan dat de gegevens van de vorige één dag, 0 en 1 geven gegevens voor de huidige en vorige dag  |
| Maand |Tekst |Maand van het jaar dat is geselecteerd voor het filteren van gegevens, maand op de eerste dag begint en eindigt op 31 dagen |
| MonthDate | Date |Datum in de maand waarop maand eindigt, geselecteerd voor het filteren van gegevens |
| MonthDiff |Decimaal getal |Bijvoorbeeld het verschil in maanden voor het filteren van gegevens, 0 geeft aan dat de gegevens van de huidige maand, -1 geeft aan dat de vorige maand van de gegevens, 0 en 1 geven gegevens voor de huidige en vorige maand |
| Wekelijks |Tekst |Week geselecteerd voor het filteren van gegevens, week op zondag begint en eindigt op zaterdag |
| WeekDate |Date |Datum van de week waarop de week eindigt, geselecteerd voor het filteren van gegevens |
| WeekDiff |Decimaal getal |Bijvoorbeeld het verschil in week voor het filteren van gegevens, 0 geeft aan dat de gegevens van de huidige week, -1 geeft aan dat de gegevens van de vorige week, 0 en 1 geven gegevens voor de huidige en vorige week |
| Jaar |Tekst |Kalenderjaar geselecteerd voor het filteren van gegevens |
| YearDate |Date |Datum in het jaar als jaar wordt beëindigd, geselecteerd voor het filteren van gegevens |

### <a name="job"></a>Job
Deze tabel bevat de algemene velden en aggregaties die via verschillende velden met betrekking tot de taak.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #JobsCreatedInPeriod |Geheel getal |Het aantal taken die zijn gemaakt in de geselecteerde periode |
| % FailuresForJobsCreatedInPeriod |Percentage |Percentage algemene taak fouten in de geselecteerde periode |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Decimaal getal |80e percentielwaarde van de gegevens worden overgebracht in MB voor **back-up** taken die zijn gemaakt in de geselecteerde periode |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Decimaal getal |Gemiddelde tijd in minuten voor **voltooide back-up** taken die zijn gemaakt in een geselecteerde tijdsperiode |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Decimaal getal |Gemiddelde tijd in minuten voor **voltooid van herstellen** taken die zijn gemaakt in een geselecteerde tijdsperiode |
| BackupStorageDestination |Tekst |Bestemming van back-upopslag bijvoorbeeld Cloud schijf  |
| EntityState |Tekst |Huidige status van het taakobject bijvoorbeeld actief, verwijderd |
| JobFailureCode |Tekst |Mislukt codetekenreeks vanwege die taakfout opgetreden |
| JobOperation |Tekst |Bewerking voor die taak wordt uitgevoerd, bijvoorbeeld back-up, herstel, configureren van back-up |
| JobStartDate |Date |Datum waarop de taak is gestart |
| JobStartTime |Time |Tijd wanneer de taak is gestart |
| JobStatus |Tekst |Status van de voltooide taak bijvoorbeeld voltooid, is mislukt |
| JobUniqueId |Tekst |De unieke Id voor het identificeren van de taak |

### <a name="policy"></a>Beleid
Deze tabel bevat algemene velden en aggregaties die via verschillende beleid gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #Policies |Geheel getal |Aantal back-upbeleid die aanwezig zijn in het systeem |
| #PoliciesInUse |Geheel getal |Aantal beleidsregels die momenteel wordt gebruikt voor het configureren van back-ups |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| BackupDaysOfTheWeek |Tekst |Dagen van de week wanneer back-ups zijn gepland |
| BackupFrequency |Tekst |Frequentie waarmee back-ups worden uitgevoerd, bijvoorbeeld dagelijks, wekelijks |
| BackupTimes |Tekst |Datum en tijd wanneer de back-ups zijn gepland |
| DailyRetentionDuration |Geheel getal |Totale bewaartermijn in dagen voor de geconfigureerde back-ups |
| DailyRetentionTimes |Tekst |Datum en tijd wanneer de dagelijkse bewaarperiode is geconfigureerd |
| EntityState |Tekst |Huidige status van het beleidsobject bijvoorbeeld actief, verwijderd |
| MonthlyRetentionDaysOfTheMonth |Tekst |Datums van de maand geselecteerd voor het bewaren van maandelijkse |
| MonthlyRetentionDaysOfTheWeek |Tekst |Dagen van de week geselecteerd voor het bewaren van maandelijkse |
| MonthlyRetentionDuration |Decimaal getal |Totale bewaartermijn in maanden voor geconfigureerde back-ups |
| MonthlyRetentionFormat |Tekst |Typ van de configuratie voor het bewaren van maandelijkse bijvoorbeeld dagelijks voor per dag, per week voor op basis van de week |
| MonthlyRetentionTimes |Tekst |Datum en tijd wanneer de maandelijkse bewaarperiode is geconfigureerd |
| MonthlyRetentionWeeksOfTheMonth |Tekst |Weken van de maand wanneer de bewaartermijn voor maandelijkse is geconfigureerd, bijvoorbeeld eerste, laatste enzovoort. |
| PolicyName |Tekst |Naam van het beleid is gedefinieerd |
| PolicyUniqueId |Tekst |De unieke Id voor het identificeren van het beleid |
| RetentionType |Tekst |Type bewaarbeleid bijvoorbeeld, dagelijks, wekelijks, maandelijks, jaarlijks |
| WeeklyRetentionDaysOfTheWeek |Tekst |Dagen van de week geselecteerd voor het bewaren van week |
| WeeklyRetentionDuration |Decimaal getal |Totale duur van de wekelijkse bewaren in weken voor de geconfigureerde back-ups |
| WeeklyRetentionTimes |Tekst |Datum en tijd wanneer de wekelijkse bewaarperiode is geconfigureerd |
| YearlyRetentionDaysOfTheMonth |Tekst |Datums van de maand geselecteerd voor het bewaren van jaar |
| YearlyRetentionDaysOfTheWeek |Tekst |Dagen van de week geselecteerd voor het bewaren van jaar |
| YearlyRetentionDuration |Decimaal getal |Totale bewaartermijn in de jaren voor geconfigureerde back-ups |
| YearlyRetentionFormat |Tekst |Typ van de configuratie voor het bewaren van jaarlijkse bijvoorbeeld dagelijks voor per dag, per week voor op basis van de week |
| YearlyRetentionMonthsOfTheYear |Tekst |Maanden van het jaar voor jaarlijkse retentie geselecteerd |
| YearlyRetentionTimes |Tekst |Datum en tijd wanneer de jaarlijkse bewaarperiode is geconfigureerd |
| YearlyRetentionWeeksOfTheMonth |Tekst |Weken van de maand wanneer de bewaartermijn voor jaarlijkse is geconfigureerd, bijvoorbeeld eerste, laatste enzovoort. |

### <a name="protected-server"></a>Beveiligde Server
Deze tabel bevat de algemene velden en aggregaties die via verschillende beveiligde server-gerelateerde velden.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #ProtectedServers |Geheel getal |Aantal beveiligde servers |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AzureBackupAgentOSType |Tekst |Type besturingssysteem van de Azure Backup-Agent |
| AzureBackupAgentOSVersion |Tekst |Versie van het besturingssysteem van de Azure Backup-Agent |
| AzureBackupAgentUpdateDate |Tekst |Datum waarop de Agent Backup-Agent is bijgewerkt |
| AzureBackupAgentVersion |Tekst |Versienummer van Agent back-up-versie |
| BackupManagementType |Tekst |Providertype voor het uitvoeren van back-up bijvoorbeeld IaaSVM FileFolder |
| EntityState |Tekst |Huidige status van het object van de beveiligde server bijvoorbeeld actief, verwijderd |
| ProtectedServerFriendlyName |Tekst |Beschrijvende naam van de beveiligde server |
| ProtectedServerName |Tekst |Naam van de beveiligde server |
| ProtectedServerType |Tekst |Type van de beveiligde server een back-up bijvoorbeeld IaaSVMContainer |
| ProtectedServerName |Tekst |Naam van de beveiligde server naar welke back-upitem behoort |
| RegisteredContainerId |Tekst |Id van de container is geregistreerd voor back-up |

### <a name="storage"></a>Storage
Deze tabel bevat de algemene velden en aggregaties die via verschillende velden met betrekking tot opslag.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #ProtectedInstances |Decimaal getal |Aantal beveiligde exemplaren die worden gebruikt voor het berekenen van de frontend-opslag in facturering, berekend op basis van de laatste waarde in de geselecteerde tijd |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| CloudStorageInMB |Decimaal getal |Back-cloudopslag die wordt gebruikt door back-ups, berekend op basis van de laatste waarde in de geselecteerde tijd |
| EntityState |Tekst |Huidige status van het object bijvoorbeeld actief, verwijderd |
| LastUpdatedDate |Date |Wanneer de geselecteerde rij voor het laatst is bijgewerkt |

### <a name="time"></a>Time
Deze tabel bevat details over velden met betrekking tot de tijd.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Uur |Time |Uur van de dag bijvoorbeeld 13:00:00 uur |
| HourNumber |Decimaal getal |Het aantal uren op een dag bijvoorbeeld 13,00 |
| Minuut |Decimaal getal |Minuut van het uur |
| PeriodOfTheDay |Tekst |Periode sleuf van de tijd van de dag bijvoorbeeld 12-3 uur |
| Time |Time |Tijd van de dag bijvoorbeeld 12:00:01 AM |
| TimeKey |Tekst |Sleutelwaarde voor tijd |

### <a name="vault"></a>Kluis
Deze tabel bevat de algemene velden en aggregaties die via verschillende velden met betrekking tot de kluis.

| Veld | Gegevenstype | Beschrijving |
| --- | --- | --- |
| #Vaults |Geheel getal |Het aantal kluizen |
| AsOnDateTime |Datum/tijd |Vernieuwingstijd van laatste voor de geselecteerde rij |
| AzureDataCenter |Tekst |Datacenter waarin kluis zich bevindt |
| EntityState |Tekst |Huidige status van het object kluis bijvoorbeeld actief, verwijderd |
| StorageReplicationType |Tekst |Type van de storage-replicatie voor de kluis bijvoorbeeld GeoRedundant |
| SubscriptionId |Tekst |Abonnements-Id van de klant geselecteerd voor het genereren van rapporten |
| VaultName |Tekst |Naam van de kluis |
| VaultTags |Tekst |Tags die zijn gekoppeld aan de kluis |

## <a name="next-steps"></a>Volgende stappen
Nadat u het gegevensmodel voor het maken van Azure Backup-rapporten bekijken, raadpleegt u de volgende artikelen voor meer informatie over het maken en weergeven van rapporten in Power BI.

* [Het maken van rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Filters gebruiken om rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
