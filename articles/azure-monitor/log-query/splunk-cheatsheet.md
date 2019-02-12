---
title: Splunk naar Azure Monitor log-query | Microsoft Docs
description: Help voor gebruikers die bekend met Splunk zijn in het leren werken met Azure Monitor logboeken-query's.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: d6065420b9081333fc257501ace868f64220ca63
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005247"
---
# <a name="splunk-to-azure-monitor-log-query"></a>Splunk naar Azure Monitor log-query

In dit artikel is bedoeld ter ondersteuning van gebruikers die bekend met Splunk zijn voor meer informatie over de Data Explorer-querytaal voor het schrijven van Logboeken-query's in Azure Monitor. Directe vergelijking worden gemaakt tussen de twee om te begrijpen van de belangrijkste verschillen en overeenkomsten waar u kunt gebruikmaken van uw bestaande kennis.

## <a name="structure-and-concepts"></a>Structuur en concepten

De volgende tabel vergelijkt de concepten en gegevensstructuren tussen Splunk en Azure Monitor-Logboeken.

 | Concept  | Splunk | Azure Monitor |  Opmerking
 | --- | --- | --- | ---
 | Implementatie-eenheid  | cluster |  cluster |  Azure Monitor kunt willekeurige cross-cluster query's. Splunk bestaat niet. |
 | Gegevens-caches |  buckets  |  Caching-en bewaarbeleid |  Hiermee bepaalt u de periode en het niveau van de gegevens opslaan in cache. Deze instelling rechtstreeks van invloed is op de prestaties van query's en kosten van de implementatie. |
 | Logische partitie van de gegevens  |  index  |  database  |  Hiermee kunt logische scheiding van de gegevens. Beide implementaties kunt samenvoegingen en deze koppelen aan over deze partities. |
 | De metagegevens van gestructureerde gebeurtenis | N/A | tabel |  Splunk beschikt niet over het concept blootgesteld aan de zoektaal van metagegevens van de gebeurtenis. Logboeken in Azure Monitor heeft het concept van een tabel, die kolommen bevat. Elk exemplaar van de gebeurtenis is toegewezen aan een rij. |
 | Gegevensrecord | gebeurtenis | rij |  Terminologie wijzigen. |
 | Kenmerk van de record gegevens | Veld |  Kolom |  In Azure Monitor, is dit een vooraf gedefinieerd als onderdeel van de structuur van de tabel. In Splunk heeft elke gebeurtenis een eigen set velden. |
 | Typen | datatype |  datatype |  Azure Monitor-gegevenstypen zijn meer expliciete zoals ze zijn ingesteld op de kolommen. Hebben de mogelijkheid om te werken dynamisch met gegevenstypen en ruwweg set gegevenstypen, inclusief ondersteuning voor JSON. |
 | Query's en zoeken  | zoeken | query |  Concepten zijn in wezen hetzelfde tussen zowel Azure Monitor en Splunk. |
 | Tijd van de gebeurtenis opname | Wanneer het systeem | ingestion_time() |  Elke gebeurtenis wordt in Splunk, de tijdstempel van een systeem van de tijd die de gebeurtenis is geïndexeerd. U kunt een beleid met de naam ingestion_time waarmee wordt aangegeven dat een systeemkolom die kan worden verwezen door de functie ingestion_time() definiëren in Azure Monitor. |

## <a name="functions"></a>Functions

De volgende tabel worden de functies in Azure Monitor die gelijk zijn aan Splunk functies.

|Splunk | Azure Monitor |Opmerking
|---|---|---
|strcat | strcat()| (1) |
|split  | split() | (1) |
|if     | iff()   | (1) |
|tonumber | todouble()<br>tolong()<br>toint() | (1) |
|bovenste<br>Lagere |toupper()<br>tolower()|(1) |
| vervangen | replace() | (1)<br> Let ook op dat hoewel `replace()` neemt drie parameters in beide producten, de parameters zijn verschillend. |
| substr | substring() | (1)<br>Houd er ook rekening mee dat Splunk maakt gebruik van indexen op basis van een. Azure Monitor opmerkingen bij de op nul gebaseerde index. |
| tolower |  tolower() | (1) |
| ToUpper | toupper() | (1) |
| overeenkomst | komt overeen met reguliere expressie |  (2)  |
| regex | komt overeen met reguliere expressie | In Splunk, `regex` een operator. In Azure Monitor is een relationele operator. |
| searchmatch | == | In Splunk, `searchmatch` kunt u zoeken naar de exacte tekenreeks.
| willekeurig | ASELECT()<br>rand(n) | Splunk van functie retourneert een getal van 0 tot 2<sup>31</sup>-1. Azure Monitor' retourneert een getal tussen 0,0 en 1,0, of als een parameter die is opgegeven, tussen 0 en n-1.
| nu | now() | (1)
| relative_time | totimespan() | (1)<br>In Azure Monitor is het equivalent van Splunk van relative_time (datetimeVal, offsetVal) datetimeVal + totimespan(offsetVal).<br>Bijvoorbeeld, <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> wordt <code>...  &#124; extend myTime = now() - totimespan("1d")</code>.

(1) in Splunk, de functie is aangeroepen met de `eval` operator. In Azure Monitor, deze wordt gebruikt als onderdeel van `extend` of `project`.<br>(2) in Splunk, de functie is aangeroepen met de `eval` operator. In Azure Monitor kan worden gebruikt met de `where` operator.


## <a name="operators"></a>Operators

De volgende secties bevatten voorbeelden van het gebruik van verschillende operators tussen Splunk en Azure Monitor.

> [!NOTE]
> Ten behoeve van het volgende voorbeeld wordt het veld Splunk _regel_ kaarten naar een tabel in Azure Monitor en Splunk van standaard tijdstempel is toegewezen aan de Analytics-logboeken _ingestion_time()_ kolom.

### <a name="search"></a>Search
In Splunk, kunt u weglaten de `search` sleutelwoord en geef een tekenreeks op zonder aanhalingstekens. In Azure Monitor begint u elke query met `find`, een tekenreeks zonder aanhalingstekens is de kolomnaam van een en de lookup-waarde moet een tekenreeks tussen aanhalingstekens. 

| |  | |
|:---|:---|:---|
| Splunk | **search** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| Azure Monitor | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |
| | |

### <a name="filter"></a>Filteren
Azure Monitor log-query's beginnen met een tabelresultaat instellen waar u het filter. In Splunk is filteren de standaardbewerking op de huidige index. U kunt ook `where` operator in Splunk, maar wordt niet aanbevolen.

| |  | |
|:---|:---|:---|
| Splunk | **search** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| Azure Monitor | **waar** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |
| | |


### <a name="getting-n-eventsrows-for-inspection"></a>N gebeurtenissen/rijen ophalen voor controle 
Azure Monitor log query's ook ondersteuning voor `take` als een alias voor `limit`. In Splunk, als de resultaten worden gesorteerd, `head` worden de eerste n resultaten geretourneerd. In Azure Monitor, limiet niet is besteld, maar retourneert de eerste n rijen die zijn gevonden.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| Azure Monitor | **Limiet** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |
| | |



### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>De eerste n gebeurtenissen/rijen gerangschikt op een veld/kolom ophalen
Voor de onder resultaten in Splunk u `tail`. In Azure Monitor kunt u de richting van schrijfvolgorde wordt toegepast met `asc`.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| Azure Monitor | **Boven** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |
| | |




### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Uitbreiding van het resultaat ingesteld met nieuwe velden/kolommen
Splunk heeft ook een `eval` functie, die niet moet worden vergeleken met de `eval` operator. Zowel de `eval` operator in Splunk en de `extend` operator in Azure Monitor alleen ondersteuning voor scalaire functies en rekenkundige operatoren.

| |  | |
|:---|:---|:---|
| Splunk | **Evaluatie** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| Azure Monitor | **Uitbreiden** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |
| | |


### <a name="rename"></a>Naam wijzigen 
Azure Monitor maakt gebruik van de operator voor dezelfde naam en een nieuw veld te maken. Splunk bevat twee afzonderlijke operators, `eval` en `rename`.

| |  | |
|:---|:---|:---|
| Splunk | **Wijzig de naam** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| Azure Monitor | **Uitbreiden** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend exception = Date_Exception</code> |
| | |




### <a name="format-resultsprojection"></a>Indeling resultaten/projectie
Splunk lijkt niet te zijn van een operator die vergelijkbaar is met `project-away`. De gebruikersinterface kunt u opslag velden filteren.

| |  | |
|:---|:---|:---|
| Splunk | **table** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| Azure Monitor | **project**<br>**project-away** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |
| | |



### <a name="aggregation"></a>Aggregatie
Zie de [aggregaties in Azure Monitor query's bijgehouden](aggregations.md) voor de verschillende aggregatiefuncties.

| |  | |
|:---|:---|:---|
| Splunk | **Statistieken** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| Azure Monitor | **samenvatten** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |
| | |



### <a name="join"></a>Koppelen
Join in Splunk heeft belangrijke beperkingen. De subquery heeft een limiet van 10000 resultaten (ingesteld in het configuratiebestand voor implementatie), en er een beperkt aantal join-versies.

| |  | |
|:---|:---|:---|
| Splunk | **join** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias | deelnemen aan Client.Id max = 0 [vroegste = 24 uur Event.Rule="150310.0 zoeken" Data.Hresult= 2147221040]</code> |
| Azure Monitor | **join** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |
| | |



### <a name="sort"></a>Sorteren
In Splunk, om te sorteren in oplopende volgorde moet u de `reverse` operator. Azure controleren ook ondersteuning voor de opslag van null-waarden, aan het begin of aan het einde te definiëren.

| |  | |
|:---|:---|:---|
| Splunk | **Sorteren** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| Azure Monitor | **order by-** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |
| | |



### <a name="multivalue-expand"></a>Meerdere waarden uitvouwen
Dit is een vergelijkbare operator in Splunk en Azure Monitor.

| |  | |
|:---|:---|:---|
| Splunk | **mvexpand** |  `mvexpand foo` |
| Azure Monitor | **mvexpand** | `mvexpand foo` |
| | |




### <a name="results-facets-interesting-fields"></a>Resultaten facetten, interessante velden
In Log Analytics in Azure portal, wordt alleen de eerste kolom weergegeven. Alle kolommen zijn beschikbaar via de API.

| |  | |
|:---|:---|:---|
| Splunk | **Velden** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| Azure Monitor | **facets** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |
| | |




### <a name="de-duplicate"></a>Dubbele ongedaan maken
U kunt `summarize arg_min()` in plaats daarvan de volgorde van die record wordt gekozen.

| |  | |
|:---|:---|:---|
| Splunk | **Ontdubbeling** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| Azure Monitor | **summarize arg_max()** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |
| | |




## <a name="next-steps"></a>Volgende stappen

- Ga via een les op de [logboeken-query's schrijven in Azure Monitor](get-started-queries.md).