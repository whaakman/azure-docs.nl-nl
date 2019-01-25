---
title: Azure Monitor Log Analytics-query voorbeelden | Microsoft Docs
description: Voorbeelden van query's in Log Analytics met behulp van de Kusto-taal.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/03/2018
ms.author: bwren
ms.openlocfilehash: d5cad3869e74f33a2d1a56352c658bb9c8f23db6
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52882715"
---
# <a name="log-analytics-query-examples"></a>Meld u voorbeelden van Analytics
In dit artikel bevat verschillende voorbeelden van [query's](../../azure-monitor/log-query/log-query-overview.md) met behulp van de [Kusto-taal](https://docs.microsoft.com/azure/kusto/query/) om op te halen van verschillende typen gegevens van Log Analytics. Verschillende methoden worden gebruikt om te consolideren en analyseren van gegevens, zodat u deze voorbeelden gebruiken om verschillende strategieën die u voor uw eigen vereisten gebruiken kunt te identificeren.  

Zie de [Kusto-Naslaggids](https://docs.microsoft.com/azure/kusto/query/) voor meer informatie over de verschillende trefwoorden die in deze voorbeelden worden gebruikt. Doorloop een [les over het maken van query's](get-started-queries.md) als u geen ervaring met Log Analytics.

## <a name="events"></a>Gebeurtenissen

### <a name="search-application-level-events-described-as-cryptographic"></a>Zoeken op toepassingsniveau gebeurtenissen die worden beschreven als "Cryptografische"
In dit voorbeeld wordt gezocht naar de **gebeurtenissen** tabel voor records waarin **EventLog** is _Application_ en **RenderedDescription** bevat _cryptographic_. Bevat records van de afgelopen 24 uur.

```Kusto
Event
| where EventLog == "Application" 
| where TimeGenerated > ago(24h) 
| where RenderedDescription == "cryptographic"
```

### <a name="search-events-related-to-unmarshaling"></a>Search-gebeurtenissen met betrekking tot unmarshaling
Tabellen zoeken **gebeurtenis** en **SecurityEvents** voor records die de vermelding _unmashaling_.

```Kusto
search in (Event, SecurityEvent) "unmarshaling"
```

## <a name="heartbeat"></a>Heartbeat

### <a name="chart-a-week-over-week-view-of-the-number-of-computers-sending-data"></a>Grafiek van een weergave van de week-over-week van het aantal computers die gegevens verzenden

Het volgende voorbeeld grafieken het aantal unieke computers dat heartbeats, elke week worden verzonden.

```Kusto
Heartbeat
| where TimeGenerated >= startofweek(ago(21d))
| summarize dcount(Computer) by endofweek(TimeGenerated) | render barchart kind=default
```

### <a name="find-stale-computers"></a>Inactieve computers zoeken

Het volgende voorbeeld zoeken naar computers die in de afgelopen dag actief zijn, maar heeft geen heartbeats verzonden in het afgelopen uur.

```Kusto
Heartbeat
| where TimeGenerated > ago(1d)
| summarize LastHeartbeat = max(TimeGenerated) by Computer
| where isnotempty(Computer)
| where LastHeartbeat < ago(1h)
```

### <a name="get-the-latest-heartbeat-record-per-computer-ip"></a>De meest recente record voor heartbeat per computer IP-adres ophalen

In dit voorbeeld retourneert de laatste heartbeat-record voor elk IP-adres voor de computer.
```Kusto
Heartbeat
| summarize arg_max(TimeGenerated, *) by ComputerIP
```

### <a name="match-protected-status-records-with-heartbeat-records"></a>Overeen met de status van de beveiligde gegevens met heartbeat-records

In dit voorbeeld vindt u verwante protection status records en heartbeat-records, afgestemd op zowel de Computer en de tijd.
Houd er rekening mee dat de tijd-veld wordt afgerond op de dichtstbijzijnde minuut. We runtime bin berekening gebruikt om dat te doen: `round_time=bin(TimeGenerated, 1m)`.

```Kusto
let protection_data = ProtectionStatus
    | project Computer, DetectionId, round_time=bin(TimeGenerated, 1m);
let heartbeat_data = Heartbeat
    | project Computer, Category, round_time=bin(TimeGenerated, 1m);
protection_data | join (heartbeat_data) on Computer, round_time
```

### <a name="server-availability-rate"></a>Server-tarief voor beschikbaarheid

Berekenen server beschikbaarheid-tarief op basis van heartbeat-records. Beschikbaarheid wordt gedefinieerd als 'ten minste 1 heartbeat per uur'.
Als een server beschikbaar 98 van 100 uur is, is de beschikbaarheidswaarde 98%.

```Kusto
let start_time=startofday(datetime("2018-03-01"));
let end_time=now();
Heartbeat
| where TimeGenerated > start_time and TimeGenerated < end_time
| summarize heartbeat_per_hour=count() by bin_at(TimeGenerated, 1h, start_time), Computer
| extend available_per_hour=iff(heartbeat_per_hour>0, true, false)
| summarize total_available_hours=countif(available_per_hour==true) by Computer 
| extend total_number_of_buckets=round((end_time-start_time)/1h)+1
| extend availability_rate=total_available_hours*100/total_number_of_buckets
```


## <a name="multiple-data-types"></a>Meerdere gegevenstypen

### <a name="chart-the-record-count-per-table"></a>Aantal records per tabel grafiek
Het volgende voorbeeld worden alle records van alle tabellen van de afgelopen vijf uur wordt verzameld en telt het aantal records in elke tabel zijn. De resultaten worden weergegeven in een tijdgrafiek.

```Kusto
union withsource=sourceTable *
| where TimeGenerated > ago(5h) 
| summarize count() by bin(TimeGenerated,10m), sourceTable
| render timechart
```

### <a name="count-all-logs-collected-over-the-last-hour-by-type"></a>Alle logboeken die worden verzameld via het afgelopen uur op type tellen
Het volgende voorbeeld wordt gezocht alles gerapporteerd in het afgelopen uur en de records in elke tabel door telt **Type**. De resultaten worden weergegeven in een staafdiagram.

```Kusto
search *
| where TimeGenerated > ago(1h) 
| summarize CountOfRecords = count() by Type
| render barchart
```

## <a name="azurediagnostics"></a>AzureDiagnostics

### <a name="count-azure-diagnostics-records-per-category"></a>Azure diagnostics records per categorie tellen
In dit voorbeeld telt alle Azure diagnostics-records voor elke unieke categorie.

```Kusto
AzureDiagnostics 
| where TimeGenerated > ago(1d)
| summarize count() by Category
```

### <a name="get-a-random-record-for-each-unique-category"></a>Een willekeurige record ophalen voor elke unieke categorie
In dit voorbeeld wordt een enkele willekeurige Azure diagnostics-record voor elke unieke categorie.

```Kusto
AzureDiagnostics
| where TimeGenerated > ago(1d) 
| summarize any(*) by Category
```

### <a name="get-the-latest-record-per-category"></a>Ophalen van de meest recente record per categorie
In dit voorbeeld haalt de meest recente Azure diagnostics-record in elke unieke categorie.

```Kusto
AzureDiagnostics
| where TimeGenerated > ago(1d) 
| summarize arg_max(TimeGenerated, *) by Category
```

## <a name="network-monitoring"></a>Netwerkbewaking

### <a name="computers-with-unhealthy-latency"></a>Computers met een slechte status latentie
In dit voorbeeld maakt een lijst van afzonderlijke computers met slechte latentie.

```Kusto
NetworkMonitoring 
| where LatencyHealthState <> "Healthy" 
| where Computer != "" 
| distinct Computer
```

## <a name="performance"></a>Prestaties

### <a name="join-computer-perf-records-to-correlate-memory-and-cpu"></a>Deelnemen aan perf computerrecords voor het correleren van geheugen en CPU
In dit voorbeeld overeenkomt met een bepaalde computer **perf** registreert en maakt u twee keer grafieken, de gemiddelde CPU en maximale hoeveelheid geheugen.

```Kusto
let StartTime = now()-5d;
let EndTime = now()-4d;
Perf
| where CounterName == "% Processor Time"  
| where TimeGenerated > StartTime and TimeGenerated < EndTime
and TimeGenerated < EndTime
| project TimeGenerated, Computer, cpu=CounterValue 
| join kind= inner (
   Perf
    | where CounterName == "% Used Memory"  
    | where TimeGenerated > StartTime and TimeGenerated < EndTime
    | project TimeGenerated , Computer, mem=CounterValue 
) on TimeGenerated, Computer
| summarize avgCpu=avg(cpu), maxMem=max(mem) by TimeGenerated bin=30m  
| render timechart
```

### <a name="perf-cpu-utilization-graph-per-computer"></a>De grafiek voor prestaties CPU-gebruik per computer
In dit voorbeeld wordt berekend en de CPU-gebruik van computers die met beginnen grafieken _Contoso_.

```Kusto
Perf
| where TimeGenerated > ago(4h)
| where Computer startswith "Contoso" 
| where CounterName == @"% Processor Time"
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 15m) 
| render timechart
```

## <a name="protection-status"></a>Beveiligingsstatus

### <a name="computers-with-non-reporting-protection-status-duration"></a>Computers met niet-rapportage protection status duur
In dit voorbeeld geeft een lijst van computers waarop de beveiligingsstatus van _Not Reporting_ en de duur die ze in deze status zijn.

```Kusto
ProtectionStatus
| where ProtectionStatus == "Not Reporting"
| summarize count(), startNotReporting = min(TimeGenerated), endNotReporting = max(TimeGenerated) by Computer, ProtectionStatusDetails
| join ProtectionStatus on Computer
| summarize lastReporting = max(TimeGenerated), startNotReporting = any(startNotReporting), endNotReporting = any(endNotReporting) by Computer
| extend durationNotReporting = endNotReporting - startNotReporting
```

### <a name="match-protected-status-records-with-heartbeat-records"></a>Overeen met de status van de beveiligde gegevens met heartbeat-records
In dit voorbeeld vindt u verwante protection status records en heartbeat-records die zijn afgestemd op zowel de Computer en de tijd.
Het tijdveld afgerond naar de dichtstbijzijnde minuut via **bin**.

```Kusto
let protection_data = ProtectionStatus
    | project Computer, DetectionId, round_time=bin(TimeGenerated, 1m);
let heartbeat_data = Heartbeat
    | project Computer, Category, round_time=bin(TimeGenerated, 1m);
protection_data | join (heartbeat_data) on Computer, round_time
```


## <a name="security-records"></a>Beveiligingsrecords in de

### <a name="count-security-events-by-activity-id"></a>Aantal beveiligingsgebeurtenissen door activiteits-ID


In dit voorbeeld is afhankelijk van de vaste structuur van de **activiteit** kolom: \<ID\>-\<naam\>.
Deze analyseert de **activiteit** waarde in twee nieuwe kolommen, en het exemplaar van elk telt **activityID**.

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m) 
| project Activity 
| parse Activity with activityID " - " activityDesc
| summarize count() by activityID
```

### <a name="count-security-events-related-to-permissions"></a>Aantal beveiligingsgebeurtenissen met betrekking tot machtigingen
In dit voorbeeld toont het aantal **securityEvent** records waarin de **activiteit** kolom bevat de gehele duur _Permissions_. De query is van toepassing op records gemaakt in de afgelopen 30 minuten.

```Kusto
SecurityEvent
| where TimeGenerated > ago(30m)
| summarize EventCount = countif(Activity has "Permissions")
```

### <a name="find-accounts-that-failed-to-log-in-from-computers-with-a-security-detection"></a>Accounts die zich aanmelden vanaf computers met een detectie kunnen niet vinden
In dit voorbeeld zoekt naar en telt accounts die niet zijn geslaagd om aan te melden vanaf computers waarop we een beveiligingsdetectie kunnen identificeren.

```Kusto
let detections = toscalar(SecurityDetection
| summarize makeset(Computer));
SecurityEvent
| where Computer in (detections) and EventID == 4624
| summarize count() by Account
```

### <a name="is-my-security-data-available"></a>Zijn mijn gegevens security beschikbaar?
Starten van de gegevens begint verkennen vaak met controle op beschikbaarheid van gegevens. In dit voorbeeld toont het aantal **SecurityEvent** records in de laatste 30 minuten.

```Kusto
SecurityEvent 
| where TimeGenerated  > ago(30m) 
| count
```

### <a name="parse-activity-name-and-id"></a>Naam van de activiteit en ID parseren
De onderstaande twee voorbeelden zijn afhankelijk van de vaste structuur van de **activiteit** kolom: \<ID\>-\<naam\>. Het eerste voorbeeld wordt de **parseren** operator op die waarden worden toegewezen aan twee nieuwe kolommen: **activityID** en **activityDesc**.

```Kusto
SecurityEvent
| take 100
| project Activity 
| parse Activity with activityID " - " activityDesc
```

In dit voorbeeld wordt de **splitsen** operator om te maken van een matrix van afzonderlijke waarden
```Kusto
SecurityEvent
| take 100
| project Activity 
| extend activityArr=split(Activity, " - ") 
| project Activity , activityArr, activityId=activityArr[0]
```

### <a name="explicit-credentials-processes"></a>Expliciete referentieprocessen
Het volgende voorbeeld ziet u een cirkeldiagram van processen die expliciete referenties in de afgelopen week gebruikt

```Kusto
SecurityEvent
| where TimeGenerated > ago(7d)
// filter by id 4648 ("A logon was attempted using explicit credentials")
| where EventID == 4648
| summarize count() by Process
| render piechart 
```

### <a name="top-running-processes"></a>Top-processen

Het volgende voorbeeld ziet een tijdlijn van de activiteit voor de vijf meest gebruikte processen in de afgelopen drie dagen.

```Kusto
// Find all processes that started in the last three days. ID 4688: A new process has been created.
let RunProcesses = 
    SecurityEvent
    | where TimeGenerated > ago(3d)
    | where EventID == "4688";
// Find the 5 processes that were run the most
let Top5Processes =
RunProcesses
| summarize count() by Process
| top 5 by count_;
// Create a time chart of these 5 processes - hour by hour
RunProcesses 
| where Process in (Top5Processes) 
| summarize count() by bin (TimeGenerated, 1h), Process
| render timechart
```


### <a name="find-repeating-failed-login-attempts-by-the-same-account-from-different-ips"></a>Herhaalde mislukte aanmeldingspogingen door hetzelfde account van verschillende IP-adressen zoeken

Het volgende voorbeeld zoekt mislukte aanmeldingspogingen door hetzelfde account van meer dan vijf verschillende IP-adressen in de afgelopen zes uur.

```Kusto
SecurityEvent 
| where AccountType == "User" and EventID == 4625 and TimeGenerated > ago(6h) 
| summarize IPCount = dcount(IpAddress), makeset(IpAddress)  by Account
| where IPCount > 5
| sort by IPCount desc
```

### <a name="find-user-accounts-that-failed-to-log-in"></a>Gebruikersaccounts die niet zijn geslaagd om aan te melden 
Het volgende voorbeeld identificeert de gebruikersaccounts die niet aan te melden in meer dan vijf keer in de laatste dag en wanneer ze van laatste synchronisatiepoging om aan te melden.

```Kusto
let timeframe = 1d;
SecurityEvent
| where TimeGenerated > ago(1d)
| where AccountType == 'User' and EventID == 4625 // 4625 - failed log in
| summarize failed_login_attempts=count(), latest_failed_login=arg_max(TimeGenerated, Account) by Account 
| where failed_login_attempts > 5
| project-away Account1
```

Met behulp van **join**, en **laten** instructies we uiteraard controleren kunnen als de dezelfde verdachte accounts later aanmelden met succes zijn.

```Kusto
let timeframe = 1d;
let suspicious_users = 
    SecurityEvent
    | where TimeGenerated > ago(timeframe)
    | where AccountType == 'User' and EventID == 4625 // 4625 - failed login
    | summarize failed_login_attempts=count(), latest_failed_login=arg_max(TimeGenerated, Account) by Account 
    | where failed_login_attempts > 5
    | project-away Account1;
let suspicious_users_that_later_logged_in = 
    suspicious_users 
    | join kind=innerunique (
        SecurityEvent
        | where TimeGenerated > ago(timeframe)
        | where AccountType == 'User' and EventID == 4624 // 4624 - successful login,
        | summarize latest_successful_login=arg_max(TimeGenerated, Account) by Account
    ) on Account
    | extend was_login_after_failures = iif(latest_successful_login>latest_failed_login, 1, 0)
    | where was_login_after_failures == 1
;
suspicious_users_that_later_logged_in
```

## <a name="usage"></a>Gebruik

### <a name="calculate-the-average-size-of-perf-usage-reports-per-computer"></a>Berekent de gemiddelde grootte van de prestaties gebruiksrapporten per computer

In dit voorbeeld berekent de gemiddelde grootte van rapporten over gebruik van prestaties per computer, in de afgelopen 3 uur.
De resultaten worden weergegeven in een staafdiagram.
```Kusto
Usage 
| where TimeGenerated > ago(3h)
| where DataType == "Perf" 
| where QuantityUnit == "MBytes" 
| summarize avg(Quantity) by Computer
| sort by avg_Quantity desc nulls last
| render barchart
```

### <a name="timechart-latency-percentiles-50-and-95"></a>Tijdgrafiek latentie percentielen 50 en 95 op

In dit voorbeeld berekent en grafieken de 50e en 95th percentielen van de gerapporteerde **avgLatency** per uur in de afgelopen 24 uur.

```Kusto
Usage
| where TimeGenerated > ago(24h)
| summarize percentiles(AvgLatencyInSeconds, 50, 95) by bin(TimeGenerated, 1h) 
| render timechart
```

### <a name="usage-of-specific-computers-today"></a>Gebruik van specifieke computers vandaag nog
In dit voorbeeld haalt **gebruik** gegevens uit de laatste dag voor de computernamen die de tekenreeks bevat _ContosoFile_. De resultaten worden gesorteerd op **TimeGenerated**.

```Kusto
Usage
| where TimeGenerated > ago(1d)
| where  Computer contains "ContosoFile" 
| sort by TimeGenerated desc nulls last
```

## <a name="updates"></a>Updates

### <a name="computers-still-missing-updates"></a>Nog steeds ontbrekende Updates computers
Dit voorbeeld toont een lijst van computers die ontbreken een of meer essentiële updates een paar dagen geleden en er nog steeds updates ontbreken.

```Kusto
let ComputersMissingUpdates3DaysAgo = Update
| where TimeGenerated between (ago(3d)..ago(2d))
| where  Classification == "Critical Updates" and UpdateState != "Not needed" and UpdateState != "NotNeeded"
| summarize makeset(Computer);

Update
| where TimeGenerated > ago(1d)
| where  Classification == "Critical Updates" and UpdateState != "Not needed" and UpdateState != "NotNeeded"
| where Computer in (ComputersMissingUpdates3DaysAgo)
| summarize UniqueUpdatesCount = dcount(Product) by Computer, OSType
```


## <a name="next-steps"></a>Volgende stappen

- Raadpleeg de [Kusto-Naslaggids](/azure/kusto/query) voor meer informatie over de taal.
- Doorloop een [les over het schrijven van query's in Log Analytics](get-started-queries.md).