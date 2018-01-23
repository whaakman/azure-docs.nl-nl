---
title: Waarschuwing beheeroplossing in Azure Log Analytics | Microsoft Docs
description: De waarschuwing beheeroplossing in Log Analytics kunt u het analyseren van alle waarschuwingen in uw omgeving.  Naast het consolideren waarschuwingen gegenereerd binnen Log Analytics, importeert deze waarschuwingen van verbonden beheergroepen van System Center Operations Manager in logboekanalyse.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: c34916913915331020d9fc9789221f790b75a070
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="alert-management-solution-in-azure-log-analytics"></a>Waarschuwing beheeroplossing in Azure Log Analytics

![Pictogram voor het beheer van waarschuwing](media/log-analytics-solution-alert-management/icon.png)

De oplossing voor beheer van waarschuwingen kunt u analyseren van alle waarschuwingen in uw opslagplaats logboekanalyse.  Deze waarschuwingen hebben afkomstig zijn uit verschillende bronnen, met inbegrip van deze bronnen [gemaakt door logboekanalyse](log-analytics-alerts.md) of [geïmporteerd uit Nagios of Zabbix](log-analytics-linux-agents.md).  De oplossing ook waarschuwingen importeert uit een [System Center Operations Manager-beheergroepen verbonden](log-analytics-om-agents.md).

## <a name="prerequisites"></a>Vereisten
De oplossing werkt met alle records in de opslagplaats logboekanalyse met een type **waarschuwing**, dus moet u de configuratie is vereist voor het verzamelen van deze records uitvoeren.

- Voor logboekanalyse waarschuwingen, [waarschuwingsregels maken](log-analytics-alerts.md) waarschuwing records maken rechtstreeks in de opslagplaats.
- Voor Nagios en Zabbix waarschuwingen, [configureren die servers](log-analytics-linux-agents.md) om waarschuwingen te verzenden met logboekanalyse.
- Voor System Center Operations Manager-waarschuwingen [uw Operations Manager-beheergroep verbinden met uw werkruimte voor logboekanalyse](log-analytics-om-agents.md).  Alle waarschuwingen die zijn gemaakt in System Center Operations Manager worden geïmporteerd in logboekanalyse.  

## <a name="configuration"></a>Configuratie
De oplossing voor beheer van waarschuwingen toevoegen aan uw werkruimte voor logboekanalyse met behulp van de procedure beschreven in [oplossingen toevoegen](log-analytics-add-solutions.md).  Er is geen verdere configuratie nodig.

## <a name="management-packs"></a>Management packs
Als uw System Center Operations Manager-beheergroep met de werkruimte voor logboekanalyse is verbonden, wordt de volgende management packs geïnstalleerd in System Center Operations Manager als u deze oplossing toevoegt.  Er is geen configuratie of onderhoud van de management packs vereist.  

* Microsoft System Center Advisor waarschuwing Management (Microsoft.IntelligencePacks.AlertManagement)

Zie [Operations Manager koppelen aan Log Analytics](log-analytics-om-agents.md) voor meer informatie over de manier waarop uw management packs voor oplossingen worden bijgewerkt.

## <a name="data-collection"></a>Gegevensverzameling
### <a name="agents"></a>Agents
De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteuning | Beschrijving |
|:--- |:--- |:--- |
| [Windows-agents](log-analytics-windows-agent.md) | Nee |Directe Windows-agents genereren geen waarschuwingen.  Log Analytics waarschuwingen kunnen worden gemaakt van gebeurtenissen en prestatiegegevens verzameld via Windows-agents. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee |Directe Linux-agents genereren geen waarschuwingen.  Log Analytics waarschuwingen kunnen worden gemaakt van gebeurtenissen en prestatiegegevens verzameld van Linux-agents.  Nagios en Zabbix waarschuwingen worden verzameld van deze servers waarvoor de Linux-agent. |
| [System Center Operations Manager-beheergroep](log-analytics-om-agents.md) |Ja |Waarschuwingen die worden gegenereerd op de Operations Manager-agents zijn geleverd aan de beheergroep en vervolgens doorgestuurd naar logboekanalyse.<br><br>Een directe verbinding van Operations Manager-agents met logboekanalyse is niet vereist. Waarschuwingsgegevens wordt uit de beheergroep doorgestuurd naar de opslagplaats logboekanalyse. |


### <a name="collection-frequency"></a>Verzamelingsfrequentie
- Waarschuwing records zijn beschikbaar voor de oplossing zodra ze worden opgeslagen in de opslagplaats.
- Waarschuwing gegevens worden verzonden vanuit de Operations Manager-beheergroep met logboekanalyse elke drie minuten.  

## <a name="using-the-solution"></a>De oplossing gebruiken
Wanneer u het beheersysteem voor waarschuwing aan uw werkruimte voor logboekanalyse toevoegt de **waarschuwingenbeheer** tegel is toegevoegd aan uw dashboard.  Deze tegel wordt weergegeven voor een aantal en de grafische weergave van het aantal actieve waarschuwingen die zijn gegenereerd binnen de afgelopen 24 uur.  U kunt dit tijdsbereik niet wijzigen.

![Waarschuwing Management tegel](media/log-analytics-solution-alert-management/tile.png)

Klik op de **waarschuwingenbeheer** tegel openen de **waarschuwingenbeheer** dashboard.  Het dashboard bevat de kolommen in de volgende tabel.  Elke kolom bevat de bovenste 10 waarschuwingen op het aantal die overeenkomt met de criteria die kolom voor het opgegeven bereik en tijdsbereik.  U kunt een zoekopdracht logboek waarmee de volledige lijst door te klikken op uitvoeren **alle** aan de onderkant van de kolom of door te klikken op de kolomkop.

| Kolom | Beschrijving |
|:--- |:--- |
| Kritieke waarschuwingen |Alle waarschuwingen weergegeven met een ernst kritiek gegroepeerd op de naam van waarschuwing.  Klik op de naam van een waarschuwing te retourneren van alle records voor deze waarschuwing logboek zoekopdracht uitvoert. |
| Waarschuwingen |Alle waarschuwingen weergegeven met een ernst van waarschuwing gegroepeerd op de naam van waarschuwing.  Klik op de naam van een waarschuwing te retourneren van alle records voor deze waarschuwing logboek zoekopdracht uitvoert. |
| Actieve SCOM-waarschuwingen |Alle waarschuwingen die zijn verzameld uit Operations Manager met elke status anders dan *gesloten* gegroepeerd op de bron die de waarschuwing heeft gegenereerd. |
| Alle actieve waarschuwingen |Alle waarschuwingen weergegeven met alle ernst gegroepeerd op de naam van waarschuwing. Alleen dan bevat Operations Manager-waarschuwingen met elke status *gesloten*. |

Als u naar rechts schuiven, het dashboard bevat enkele algemene query's die u op klikken kunt om uit te voeren een [logboek zoeken](log-analytics-log-searches.md) voor waarschuwingsgegevens.

![Waarschuwing Management dashboard](media/log-analytics-solution-alert-management/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics-records
De waarschuwing beheeroplossing analyseert elke record met een type **waarschuwing**.  Waarschuwingen gemaakt met logboekanalyse of verzameld van Nagios of Zabbix worden niet rechtstreeks verzameld door de oplossing.

De oplossing waarschuwingen importeren uit System Center Operations Manager en maakt u een record voor elk met een type **waarschuwing** en een SourceSystem van **OpsManager**.  Deze records hebben de eigenschappen in de volgende tabel:  

| Eigenschap | Beschrijving |
|:--- |:--- |
| Type |*Een waarschuwing* |
| SourceSystem |*OpsManager* |
| AlertContext |Details van het gegevensitem waarvoor de waarschuwing wordt gegenereerd in XML-indeling. |
| AlertDescription |Gedetailleerde beschrijving van de waarschuwing. |
| AlertId |GUID van de waarschuwing. |
| AlertName |De naam van de waarschuwing. |
| AlertPriority |Het prioriteitsniveau van de waarschuwing. |
| AlertSeverity |Ernst van de waarschuwing. |
| AlertState |Meest recente toestand van de omzetting van de waarschuwing. |
| LastModifiedBy |De naam van de gebruiker die de waarschuwing het laatst is gewijzigd. |
| ManagementGroupName |Naam van de beheergroep waar de waarschuwing is gegenereerd. |
| RepeatCount |Aantal keren dat dezelfde waarschuwing is gegenereerd voor dezelfde bewaakt object sinds wordt omgezet. |
| ResolvedBy |De naam van de gebruiker die de waarschuwing is opgelost. Leeg zijn als de waarschuwing nog niet opgelost is. |
| SourceDisplayName |Weergavenaam van het controle-object dat de waarschuwing heeft gegenereerd. |
| SourceFullName |Volledige naam van het controle-object dat de waarschuwing heeft gegenereerd. |
| TicketId |Ticket-ID voor de waarschuwing als de System Center Operations Manager-omgeving is geïntegreerd met een proces voor het toewijzen van tickets voor waarschuwingen.  Lege van er is geen ticket-ID is toegewezen. |
| TimeGenerated |Datum en tijd waarop de waarschuwing is gemaakt. |
| TimeLastModified |Datum en tijd waarop de waarschuwing voor het laatst is gewijzigd. |
| TimeRaised |Datum en tijd waarop de waarschuwing is gegenereerd. |
| TimeResolved |Datum en tijd waarop de waarschuwing is opgelost. Leeg zijn als de waarschuwing nog niet opgelost is. |

## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken
De volgende tabel bevat een voorbeeld-logboek zoekt waarschuwing records die door deze oplossing worden verzameld: 

| Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| Type=Alert SourceSystem=OpsManager AlertSeverity=error TimeRaised>NOW-24HOUR |Kritieke waarschuwingen die in de afgelopen 24 uur zijn geactiveerd |
| Type waarschuwing AlertSeverity = waarschuwing TimeRaised = > nu 24 uur |Waarschuwingen die worden weergegeven gedurende de afgelopen 24 uur |
| Type waarschuwing SourceSystem = OpsManager AlertState =! gesloten TimeRaised = > nu - 24-UURS &#124; meting count() als tellen per SourceDisplayName |Bronnen met actieve waarschuwingen die worden weergegeven gedurende de afgelopen 24 uur |
| Type waarschuwing SourceSystem = OpsManager AlertSeverity = fout TimeRaised = > nu 24 uur AlertState! = gesloten |Kritieke waarschuwingen die worden weergegeven gedurende de afgelopen 24 uur die nog steeds actief zijn |
| Type waarschuwing SourceSystem = OpsManager TimeRaised = > nu 24 uur AlertState = gesloten |Deze gebeurtenis treedt op tijdens de afgelopen 24 uur die nu zijn gesloten waarschuwingen |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-1DAY &#124; measure count() as Count by AlertSeverity |Waarschuwingen die worden weergegeven gedurende de afgelopen dag gegroepeerd op hun ernst |
| Type=Alert SourceSystem=OpsManager TimeRaised>NOW-1DAY &#124; sort RepeatCount desc |Waarschuwingen die worden weergegeven gedurende de afgelopen dag gesorteerd op basis van hun waarde aantal herhalingen |


>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens de voorgaande query's in het volgende wijzigen wilt:
>
>| Query’s uitvoeren | Beschrijving |
|:---|:---|
| Waarschuwing &#124; waar SourceSystem == 'OpsManager' en AlertSeverity == "error" en TimeRaised > ago(24h) |Kritieke waarschuwingen die in de afgelopen 24 uur zijn geactiveerd |
| Waarschuwing &#124; waar AlertSeverity == 'waarschuwing' en TimeRaised > ago(24h) |Waarschuwingen die worden weergegeven gedurende de afgelopen 24 uur |
| Waarschuwing &#124; waar SourceSystem == 'OpsManager' en AlertState! = 'Gesloten' en TimeRaised > ago(24h) &#124; Aantal samenvatten count() door SourceDisplayName = |Bronnen met actieve waarschuwingen die worden weergegeven gedurende de afgelopen 24 uur |
| Waarschuwing &#124; waar SourceSystem == 'OpsManager' en AlertSeverity == "error" en TimeRaised > ago(24h) en AlertState! = 'Gesloten' |Kritieke waarschuwingen die worden weergegeven gedurende de afgelopen 24 uur die nog steeds actief zijn |
| Waarschuwing &#124; waar SourceSystem == 'OpsManager' en TimeRaised > ago(24h) en AlertState == 'Gesloten' |Deze gebeurtenis treedt op tijdens de afgelopen 24 uur die nu zijn gesloten waarschuwingen |
| Waarschuwing &#124; waar SourceSystem == 'OpsManager' en TimeRaised > ago(1d) &#124; Aantal samenvatten count() door AlertSeverity = |Waarschuwingen die worden weergegeven gedurende de afgelopen dag gegroepeerd op hun ernst |
| Waarschuwing &#124; waar SourceSystem == 'OpsManager' en TimeRaised > ago(1d) &#124; sorteren op RepeatCount desc |Waarschuwingen die worden weergegeven gedurende de afgelopen dag gesorteerd op basis van hun waarde aantal herhalingen |


## <a name="next-steps"></a>Volgende stappen
* Zie [Understanding alerts in Log Analytics](log-analytics-alerts.md) voor meer informatie over het genereren van waarschuwingen van Log Analytics.
