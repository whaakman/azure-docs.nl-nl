---
title: Gegevens model voor Azure Backup
description: In dit artikel vindt u informatie over Power BI gegevens model voor Azure Backup-rapporten.
author: adigan
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 06/26/2017
ms.author: adigan
ms.openlocfilehash: 66245a25af000d999ded87c58e3ba225b373b101
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465601"
---
# <a name="data-model-for-azure-backup-reports"></a>Gegevensmodel voor Azure Backup-rapporten
In dit artikel wordt het Power BI gegevens model beschreven dat wordt gebruikt voor het maken van Azure Backup-rapporten. Met dit gegevens model kunt u bestaande rapporten filteren op basis van relevante velden en belang rijker rapporten maken met behulp van tabellen en velden in het model. 

## <a name="creating-new-reports-in-power-bi"></a>Nieuwe rapporten maken in Power BI
Power BI biedt aanpassings functies die u kunt gebruiken om [rapporten te maken met behulp van het gegevens model](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/).

## <a name="using-azure-backup-data-model"></a>Azure Backup gegevens model gebruiken
U kunt de volgende velden gebruiken als onderdeel van het gegevens model voor het maken van rapporten en het aanpassen van bestaande rapporten.

### <a name="alert"></a>Waarschuwing
Deze tabel bevat basis velden en aggregaties over diverse velden die betrekking hebben op waarschuwingen.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| #AlertsCreatedInPeriod |Geheel getal |Aantal waarschuwingen dat is gemaakt in de geselecteerde tijds periode |
| %ActiveAlertsCreatedInPeriod |Percentage |Percentage actieve waarschuwingen in de geselecteerde tijds periode |
| %CriticalAlertsCreatedInPeriod |Percentage |Percentage kritieke waarschuwingen in de geselecteerde tijds periode |
| AlertOccurrenceDate |Date |Datum waarop de waarschuwing is gemaakt |
| AlertSeverity |Text |Ernst van de waarschuwing bijvoorbeeld kritiek |
| AlertStatus |Text |Status van de waarschuwing bijvoorbeeld actief |
| AlertType |Text |Type van de gegenereerde waarschuwing, bijvoorbeeld back-up |
| AlertUniqueId |Text |Unieke id van de gegenereerde waarschuwing |
| AsOnDateTime |Date/Time |Laatste vernieuwings tijd voor de geselecteerde rij |
| AvgResolutionTimeInMinsForAlertsCreatedInPeriod |Decimaal getal |Gemiddelde tijd (in minuten) voor het oplossen van de waarschuwing voor de geselecteerde tijds periode |
| EntityState |Text |Huidige status van het waarschuwings object, bijvoorbeeld actief, verwijderd |

### <a name="backup-item"></a>Back-upitem
Deze tabel bevat basis velden en aggregaties over diverse velden met betrekking tot back-upitems.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| #BackupItems |Geheel getal |Aantal back-upitems |
| #UnprotectedBackupItems |Geheel getal |Aantal back-upitems gestopt voor beveiliging of geconfigureerd voor back-ups, maar back-ups zijn niet gestart|
| AsOnDateTime |Date/Time |Laatste vernieuwings tijd voor de geselecteerde rij |
| BackupItemFriendlyName |Text |Beschrijvende naam van het back-upitem |
| BackupItemId |Text |Id van back-upitem |
| BackupItemName |Text |Naam van back-upitem |
| BackupItemType |Text |Type back-upitem bijvoorbeeld VM, FileFolder |
| EntityState |Text |Huidige status van het back-upitem-object bijvoorbeeld actief, verwijderd |
| LastBackupDateTime |Date/Time |Tijdstip van laatste back-up voor geselecteerd back-upitem |
| LastBackupState |Text |De status van de laatste back-up voor het geselecteerde back-upitem is bijvoorbeeld geslaagd, mislukt |
| LastSuccessfulBackupDateTime |Date/Time |Tijdstip van de laatste geslaagde back-up voor het geselecteerde back-upitem |
| ProtectionState |Text |Huidige beveiligings status van het back-upitem bijvoorbeeld beveiligd, ProtectionStopped |

### <a name="calendar"></a>Agenda
Deze tabel bevat details over kalender velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| Date |Date |Geselecteerde datum voor het filteren van gegevens |
| DateKey |Text |Unieke sleutel voor elk datum item |
| DayDiff |Decimaal getal |Verschil in dag voor het filteren van gegevens, 0 geeft de gegevens van de huidige dag aan,-1 geeft de gegevens van de vorige dag aan, 0 en-1 geven gegevens voor de huidige en vorige dag aan  |
| Maand |Text |Maand van het jaar dat is geselecteerd voor het filteren van gegevens, de maand begint op de eerste dag en eindigt op 31 dagen |
| MonthDate | Date |De datum in de maand waarin de maand eindigt, geselecteerd voor het filteren van gegevens |
| MonthDiff |Decimaal getal |Verschil in maand voor het filteren van gegevens, 0 geeft de gegevens van de huidige maand aan,-1 geeft de gegevens van de vorige maand aan, 0 en-1 geven gegevens voor de huidige en vorige maand aan |
| Week |Text |De week die is geselecteerd voor het filteren van gegevens, begint op zondag en eindigt op zaterdag |
| WeekDate |Date |De datum in de week waarop de week eindigt, geselecteerd voor het filteren van gegevens |
| WeekDiff |Decimaal getal |Verschil in de week voor het filteren van gegevens, 0 geeft de gegevens van de huidige week aan,-1 geeft de gegevens van de vorige week aan, 0 en-1 geven gegevens voor de huidige en vorige week aan |
| Jaar |Text |Kalender jaar geselecteerd voor het filteren van gegevens |
| YearDate |Date |De datum in het jaar waarin het jaar eindigt, geselecteerd voor het filteren van gegevens |

### <a name="job"></a>Taak
Deze tabel bevat basis velden en aggregaties ten opzichte van verschillende projectgerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| #JobsCreatedInPeriod |Geheel getal |Aantal taken dat in de geselecteerde tijds periode is gemaakt |
| %FailuresForJobsCreatedInPeriod |Percentage |Percentage algemene taak fouten in de geselecteerde tijds periode |
| 80thPercentileDataTransferredInMBForBackupJobsCreatedInPeriod |Decimaal getal |80th percentiel waarde van gegevens die zijn overgebracht in MB voor **back-** uptaken die zijn gemaakt in de geselecteerde tijds periode |
| AsOnDateTime |Date/Time |Laatste vernieuwings tijd voor de geselecteerde rij |
| AvgBackupDurationInMinsForJobsCreatedInPeriod |Decimaal getal |Gemiddelde tijd in minuten voor **voltooide back-** uptaken die zijn gemaakt in de geselecteerde tijds periode |
| AvgRestoreDurationInMinsForJobsCreatedInPeriod |Decimaal getal |Gemiddelde tijd in minuten voor **voltooide herstel** taken die zijn gemaakt in de geselecteerde tijds periode |
| BackupStorageDestination |Text |Doel van back-upopslag voor beeld van Cloud, schijf  |
| EntityState |Text |Huidige status van het taak object bijvoorbeeld actief, verwijderd |
| JobFailureCode |Text |De teken reeks voor de fout code omdat de taak fout is opgetreden |
| JobOperation |Text |Bewerking waarvoor de taak wordt uitgevoerd bijvoorbeeld back-ups maken, herstellen, back-up configureren |
| JobStartDate |Date |Datum waarop de taak is gestart |
| JobStartTime |Time |Tijdstip waarop de taak is gestart |
| JobStatus |Text |De status van de voltooide taak is bijvoorbeeld voltooid, mislukt |
| JobUniqueId |Text |Unieke id voor het identificeren van de taak |

### <a name="policy"></a>Beleid
Deze tabel bevat basis velden en aggregaties over diverse aan het beleid gerelateerde velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| #Policies |Geheel getal |Het aantal back-upbeleiden dat in het systeem bestaat |
| #PoliciesInUse |Geheel getal |Het aantal beleids regels dat momenteel wordt gebruikt voor het configureren van back-ups |
| AsOnDateTime |Date/Time |Laatste vernieuwings tijd voor de geselecteerde rij |
| BackupDaysOfTheWeek |Text |Dagen van de week waarop back-ups zijn gepland |
| BackupFrequency |Text |Frequentie waarmee back-ups bijvoorbeeld dagelijks en wekelijks worden uitgevoerd |
| BackupTimes |Text |De datum en tijd waarop back-ups zijn gepland |
| DailyRetentionDuration |Geheel getal |Totale retentie duur in dagen voor geconfigureerde back-ups |
| DailyRetentionTimes |Text |De datum en tijd waarop een dagelijkse Bewaar periode is geconfigureerd |
| EntityState |Text |Huidige status van het beleids object, bijvoorbeeld actief, verwijderd |
| MonthlyRetentionDaysOfTheMonth |Text |De datums van de maand die u hebt geselecteerd voor een maandelijkse retentie |
| MonthlyRetentionDaysOfTheWeek |Text |Dagen van de week geselecteerd voor een maandelijkse Bewaar periode |
| MonthlyRetentionDuration |Decimaal getal |Totale Bewaar periode in maanden voor geconfigureerde back-ups |
| MonthlyRetentionFormat |Text |Type configuratie voor een maandelijkse Bewaar periode, bijvoorbeeld dagelijks voor dag, wekelijks op basis van een week |
| MonthlyRetentionTimes |Text |De datum en tijd waarop de maandelijkse retentie is geconfigureerd |
| MonthlyRetentionWeeksOfTheMonth |Text |Weken van de maand waarin de maandelijkse retentie is geconfigureerd, bijvoorbeeld eerst, laatste, enzovoort. |
| PolicyName |Text |De naam van het beleid dat is gedefinieerd |
| PolicyUniqueId |Text |Unieke id voor het identificeren van het beleid |
| RetentionType |Text |Type Bewaar beleid voor bijvoorbeeld dagelijks, wekelijks, maandelijks, jaarlijks |
| WeeklyRetentionDaysOfTheWeek |Text |Dagen van de week geselecteerd voor een wekelijkse Bewaar periode |
| WeeklyRetentionDuration |Decimaal getal |Totale wekelijkse Bewaar periode in weken voor geconfigureerde back-ups |
| WeeklyRetentionTimes |Text |De datum en tijd waarop een wekelijkse Bewaar periode is geconfigureerd |
| YearlyRetentionDaysOfTheMonth |Text |De datums van de maand die zijn geselecteerd voor een jaarlijkse Bewaar periode |
| YearlyRetentionDaysOfTheWeek |Text |Dagen van de week geselecteerd voor een jaarlijkse Bewaar periode |
| YearlyRetentionDuration |Decimaal getal |Totale Bewaar duur in jaren voor geconfigureerde back-ups |
| YearlyRetentionFormat |Text |Type configuratie voor jaarlijkse Bewaar periode, bijvoorbeeld dagelijks voor dag, wekelijks op basis van een week |
| YearlyRetentionMonthsOfTheYear |Text |Maanden van het jaar dat is geselecteerd voor een jaarlijkse Bewaar periode |
| YearlyRetentionTimes |Text |De datum en tijd waarop de jaarlijkse Bewaar periode is geconfigureerd |
| YearlyRetentionWeeksOfTheMonth |Text |Weken van de maand waarin jaarlijks retentie is geconfigureerd, bijvoorbeeld eerst, laatste, enzovoort. |

### <a name="protected-server"></a>Beveiligde server
Deze tabel bevat basis velden en aggregaties over diverse beveiligde server velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| #ProtectedServers |Geheel getal |Aantal beveiligde servers |
| AsOnDateTime |Date/Time |Laatste vernieuwings tijd voor de geselecteerde rij |
| AzureBackupAgentOSType |Text |Type besturings systeem van Azure Backup Agent |
| AzureBackupAgentOSVersion |Text |Versie van het besturings systeem van Azure Backup Agent |
| AzureBackupAgentUpdateDate |Text |Datum waarop de agent back-upagent is bijgewerkt |
| AzureBackupAgentVersion |Text |Versie nummer van back-upversie van agent |
| BackupManagementType |Text |Provider type voor het uitvoeren van back-ups bijvoorbeeld IaaSVM, FileFolder |
| EntityState |Text |Huidige status van het beveiligde server object, bijvoorbeeld actief, verwijderd |
| ProtectedServerFriendlyName |Text |Beschrijvende naam van beveiligde server |
| ProtectedServerName |Text |Naam van beveiligde server |
| ProtectedServerType |Text |Type beveiligde server waarvan een back-up is gemaakt, bijvoorbeeld IaaSVMContainer |
| ProtectedServerName |Text |De naam van de beveiligde server waartoe het back-upitem behoort |
| RegisteredContainerId |Text |Id van de container die is geregistreerd voor back-up |

### <a name="storage"></a>Storage
Deze tabel bevat basis velden en aggregaties voor verschillende velden die betrekking hebben op opslag.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| #ProtectedInstances |Decimaal getal |Aantal beveiligde instanties dat wordt gebruikt voor het berekenen van de front-end opslag in de facturering, berekend op basis van de laatste waarde in de geselecteerde tijd |
| AsOnDateTime |Date/Time |Laatste vernieuwings tijd voor de geselecteerde rij |
| CloudStorageInMB |Decimaal getal |Cloud back-upopslag die wordt gebruikt door back-ups, berekend op basis van de nieuwste waarde in de geselecteerde tijd |
| EntityState |Text |Huidige status van het object bijvoorbeeld actief, verwijderd |
| LastUpdatedDate |Date |De datum waarop de geselecteerde rij voor het laatst is bijgewerkt |

### <a name="time"></a>Time
Deze tabel bevat informatie over tijdgebonden velden.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| Uur |Time |Uur van de dag bijvoorbeeld 1:00:00 uur |
| HourNumber |Decimaal getal |Het uur nummer op de dag bijvoorbeeld 13,00 |
| Minuut |Decimaal getal |Minuut van het uur |
| PeriodOfTheDay |Text |Tijds periode van de dag, bijvoorbeeld 12-3 uur |
| Time |Time |Tijdstip van de dag bijvoorbeeld 12:00:01 uur |
| TimeKey |Text |Sleutel waarde om tijd weer te geven |

### <a name="vault"></a>Kluis
Deze tabel bevat basis velden en aggregaties voor verschillende velden die betrekking hebben op de kluis.

| Veld | Gegevenstype | Description |
| --- | --- | --- |
| #Vaults |Geheel getal |Aantal kluizen |
| AsOnDateTime |Date/Time |Laatste vernieuwings tijd voor de geselecteerde rij |
| AzureDataCenter |Text |Data Center waar de kluis zich bevindt |
| EntityState |Text |Huidige status van het kluis object bijvoorbeeld actief, verwijderd |
| StorageReplicationType |Text |Type opslag replicatie voor de kluis, bijvoorbeeld georedundant |
| SubscriptionId |Text |Abonnements-id van de klant die is geselecteerd voor het genereren van rapporten |
| VaultName |Text |De naam van de kluis |
| VaultTags |Text |Labels die zijn gekoppeld aan de kluis |

## <a name="next-steps"></a>Volgende stappen
Als u het gegevens model voor het maken van Azure Backup rapporten hebt bekeken, raadpleegt u de volgende artikelen voor meer informatie over het maken en weer geven van rapporten in Power BI.

* [Rapporten maken in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-create-a-new-report/)
* [Rapporten filteren in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-about-filters-and-highlighting-in-reports/)
