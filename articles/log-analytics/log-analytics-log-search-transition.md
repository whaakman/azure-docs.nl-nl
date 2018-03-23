---
title: Azure Log Analytics query language-referentieoverzicht | Microsoft Docs
description: In dit artikel biedt hulp bij het overstappen naar de nieuwe querytaal voor logboekanalyse als u al bekend met de verouderde taal bent.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: bwren
ms.openlocfilehash: 9c487ab33859ae453a0074ef0344f61de19c7b4d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="transitioning-to-azure-log-analytics-new-query-language"></a>Overstappen naar de nieuwe querytaal Azure Log Analytics
Log Analytics geïmplementeerd onlangs een nieuwe querytaal.  In dit artikel biedt hulp bij het overstappen naar deze taal voor logboekanalyse als u al bekend met de verouderde taal bent en nog steeds bepaalde hulp nodig hebt.

## <a name="resources"></a>Resources


## <a name="language-converter"></a>Taal converter

Als u bekend met de verouderde Log Analytics query language bent, is de eenvoudigste manier om dezelfde query maken in de nieuwe taal gebruiken de Converter taal die geïnstalleerd in de portal logboek zoeken wanneer uw werkruimte is geconverteerd.  Met het conversieprogramma is net zo eenvoudig als een verouderde query in het bovenste tekstvak typen en vervolgens te klikken op **converteren**.  U kunt ofwel op de zoekknop voor het uitvoeren van de query of kopieer en plak deze voor het gebruik van deze ergens anders klikt.

![Taal converter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="resources"></a>Resources
De [documentatiesite voor Log Analytics Query Language](https://docs.loganalytics.io) heeft alle resources die u moet over de nieuwe taal worden geleverd aan de slag.  Dit omvat zelfstudies, voorbeelden en complete naslaggids.


## <a name="cheat-sheet"></a>Overzichtskaart

De volgende tabel bevat een vergelijking tussen een aantal algemene query's naar gelijkwaardige opdrachten tussen de nieuwe en bestaande querytaal in Azure-logboekanalyse.

| Beschrijving | Verouderd | nieuw |
|:--|:--|:--|
| Alle tabellen zoeken      | error | Zoek "error" (niet hoofdlettergevoelig) |
| Gegevens uit tabel selecteren | Type=Event |  Gebeurtenis |
|                        | Type=Event &#124; select Source, EventLog, EventID | Gebeurtenis &#124; project bron, EventLog, gebeurtenis-id |
|                        | Type=Event &#124; top 100 | Gebeurtenis &#124; 100 duren |
| Vergelijking van tekenreeksen      | Type=Event Computer=srv01.contoso.com   | Gebeurtenis &#124; waar Computer == "srv01.contoso.com" |
|                        | Type=Event Computer=contains("contoso") | Gebeurtenis &#124; waar Computer bevat 'contoso' (niet hoofdlettergevoelig)<br>Gebeurtenis &#124; waar Computer contains_cs 'Contoso' (hoofdlettergevoelig) |
|                        | Type=Event Computer=RegEx("@contoso@")  | Gebeurtenis &#124; Computer overeenkomt met de reguliere expressie '. *contoso*' |
| Datumvergelijking        | Type=Event TimeGenerated > NOW-1DAYS | Gebeurtenis &#124; waar TimeGenerated > ago(1d) |
|                        | Type=Event TimeGenerated>2017-05-01 TimeGenerated<2017-05-31 | Gebeurtenis &#124; waar TimeGenerated tussen (datetime(2017-05-01)... datetime(2017-05-31)) |
| Boole-vergelijking     | Type=Heartbeat IsGatewayInstalled=false  | Heartbeat \| waar IsGatewayInstalled == false |
| Sorteren                   | Type gebeurtenis = &#124; Computer asc, EventLog desc, EventLevelName asc sorteren | Gebeurtenis \| sorteren op een Computer asc, EventLog desc, EventLevelName asc |
| Afzonderlijke               | Type gebeurtenis = &#124; Ontdubbeling Computer \| Computer selecteren | Gebeurtenis &#124; samenvatten EventLog-Computer |
| Kolommen uitbreiden         | Type=Perf CounterName="% Processor Time" &#124; EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION | Perf &#124; waar CounterName == "% processortijd" \| uitbreiden gebruik iff = (tegenwaarde > 50, 'Hoog', 'Laag') |
| Aggregatie            | Type=Event &#124; measure count() as Count by Computer | Gebeurtenis &#124; samenvatten Count = count() door Computer |
|                                | Type = Perf ObjectName = Processor CounterName = '% processortijd' &#124; avg(CounterValue) meten per Computer interval 5 minuten | Perf &#124; waar ObjectName == 'Processor' en CounterName == "% processortijd" &#124; samenvatten avg(CounterValue) door Computer bin (TimeGenerated, 5min) |
| Aggregatie met limiet | Type=Event &#124; measure count() by Computer &#124; top 10 | Gebeurtenis &#124; AggregatedValue samenvatten = count() door Computer &#124; 10 beperken |
| Union                  | Type gebeurtenis of Type = = Syslog | Union-gebeurtenis Syslog |
| Koppelen                   | Type=NetworkMonitoring &#124; join inner AgentIP (Type=Heartbeat) ComputerIP | NetworkMonitoring &#124; soort join interne = (Type Zoek == 'Heartbeat') op $left. AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Volgende stappen
- Bekijk een [zelfstudie over het schrijven van query's](https://go.microsoft.com/fwlink/?linkid=856078) met de nieuwe querytaal.
- Raadpleeg de [Query Language Reference](https://go.microsoft.com/fwlink/?linkid=856079) voor meer informatie over alle opdracht, operators en functies voor de nieuwe querytaal.  
