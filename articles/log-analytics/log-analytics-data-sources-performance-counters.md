---
title: Verzamelen en analyseren van de prestatiemeteritems in Azure Log Analytics | Microsoft Docs
description: Prestatiemeteritems worden verzameld door logboekanalyse voor het analyseren van de prestaties van Windows en Linux-agents.  In dit artikel wordt beschreven hoe u verzamelen van prestatiemeteritems voor zowel Windows en Linux-agents, details van deze worden opgeslagen in de werkruimte en hoe ze in de Azure portal te analyseren.
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2017
ms.author: magoedte
ms.openlocfilehash: 0f7119f280f2eb51222ade2ea7984b560a02f667
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows- en Linux prestaties gegevensbronnen in Log Analytics
Prestatiemeters in Windows en Linux bieden inzicht in de prestaties van hardwareonderdelen, besturingssystemen en toepassingen.  Log Analytics kunt verzamelen van prestatiemeteritems met regelmatige tussenpozen voor analyse van de bijna realtime (NRT) naast het aggregeren van prestatiegegevens voor langere termijn analyse en rapportage.

![Prestatiemeteritems](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Configureren van prestatiemeteritems
Configureren van de prestatiemeteritems van de [menu Data in logboekanalyse-instellingen](log-analytics-data-sources.md#configuring-data-sources).

Wanneer u Windows of Linux-prestatie-items voor een nieuwe werkruimte voor logboekanalyse voor het eerst configureert, krijgt u de optie snel maken verschillende algemene prestatiemeteritems.  Ze worden weergegeven met een selectievakje ernaast.  Zorg ervoor dat alle prestatiemeteritems die u wilt maken in eerste instantie zijn geselecteerd en klik vervolgens op **de geselecteerde prestatiemeteritems toevoegen**.

Voor Windows-prestatiemeteritems kunt u een specifiek exemplaar voor elk prestatiemeteritem. Voor Linux-prestatiemeteritems, het exemplaar van elk prestatiemeteritem dat u kiest is van toepassing op alle onderliggende items van het bovenliggende item. De volgende tabel toont de algemene exemplaren beschikbaar voor Linux- en Windows-prestatiemeteritems.

| Exemplaarnaam | Beschrijving |
| --- | --- |
| \_Totaal |Totaal van alle exemplaren |
| \* |Alle instanties |
| (/ &#124; / var) |Komt overeen met de exemplaren met de naam: / of /var |

### <a name="windows-performance-counters"></a>Windows-prestatiemeteritems

![Configureren van Windows-prestatiemeteritems](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Volg deze procedure om het toevoegen van een nieuwe Windows-prestatiemeteritem verzamelen.

1. Typ de naam van het item in het tekstvak in de indeling *object (exemplaar) \counter*.  Wanneer u te typen begint, krijgt u een overeenkomende lijst met algemene prestatiemeteritems.  U kunt een item uit de lijst of typ in een van uw eigen selecteren.  U kunt ook alle instanties voor een bepaald item terugkeren door op te geven *object item*.  

    Wanneer het verzamelen van SQL Server-prestatiemeters van genoemde instanties, alle exemplaar prestatiemeteritems starten met de naam *MSSQL$* en gevolgd door de naam van het exemplaar.  Geef bijvoorbeeld voor het verzamelen van de teller logboek Cache Hit Ratio voor alle databases uit de Database prestatie-object voor benoemde SQL-exemplaar INST2 `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`.

2. Klik op  **+**  of druk op **Enter** de teller toevoegen aan de lijst.
3. Wanneer u een item toevoegen, gebruikt de standaardwaarde van 10 seconden voor de **controle-Interval**.  U kunt dit wijzigen in een hogere waarde van 1800 seconden duren (30 minuten) als u wilt verminderen de opslagvereisten van de prestatiegegevens verzameld.
4. Wanneer u tellers toe te voegen bent klaar, klikt u op de **opslaan** knop aan de bovenkant van het scherm wordt de configuratie op te slaan.

### <a name="linux-performance-counters"></a>Linux-prestatiemeteritems

![Configureren van Linux-prestatiemeteritems](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Volg deze procedure om het toevoegen van een nieuwe Linux-prestatiemeteritem verzamelen.

1. Standaard worden alle configuratiewijzigingen automatisch naar alle agents gepusht.  Voor Linux-agents wordt een configuratiebestand verzonden naar de gegevensverzamelaarset Fluentd.  Als u wijzigen van dit bestand handmatig op elke Linux-agent wilt, schakel het selectievakje *toepassen op de onderstaande configuratie op mijn Linux-machines* en volg de onderstaande instructies.
2. Typ de naam van het item in het tekstvak in de indeling *object (exemplaar) \counter*.  Wanneer u te typen begint, krijgt u een overeenkomende lijst met algemene prestatiemeteritems.  U kunt een item uit de lijst of typ in een van uw eigen selecteren.  
3. Klik op  **+**  of druk op **Enter** de teller toevoegen aan de lijst met items van het object.
4. Alle items voor een object gebruiken hetzelfde **controle-Interval**.  De standaardwaarde is 10 seconden.  U wijzigen dit in een hogere waarde van 1800 seconden duren (30 minuten) als u wilt verminderen de opslagvereisten van de prestatiegegevens verzameld.
5. Wanneer u tellers toe te voegen bent klaar, klikt u op de **opslaan** knop aan de bovenkant van het scherm wordt de configuratie op te slaan.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Linux-prestatiemeteritems in het configuratiebestand configureren
In plaats van Linux-prestatiemeteritems met de Azure portal configureren, hebt u de optie voor het bewerken van de configuratiebestanden op de Linux-agent van.  Maatstaven voor prestaties voor het verzamelen van worden beheerd door de configuratie in **/etc/opt/microsoft/omsagent/\<werkruimte-id\>/conf/omsagent.conf**.

Elk object of categorie maatstaven voor prestaties voor het verzamelen van moet worden gedefinieerd in het configuratiebestand als een enkel `<source>` element. De syntaxis heeft het patroon hieronder.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


De parameters in dit element worden in de volgende tabel beschreven.

| Parameters | Beschrijving |
|:--|:--|
| object\_naam | Objectnaam voor de verzameling. |
| exemplaar\_regex |  Een *reguliere expressie* definiëren welke exemplaren te verzamelen. De waarde: `.*` bevat alle exemplaren. Voor het verzamelen van processor metrische gegevens voor alleen de \_totaalwaarde, kunt u opgeven `_Total`. U kunt opgeven voor het verzamelen van metrische procesgegevens voor alleen de instances crond of sshd: ' (crond\|sshd)'. |
| teller\_naam\_regex | Een *reguliere expressie* definiëren welke items (voor het object) moeten worden verzameld. Geef voor het verzamelen van alle meteritems voor het object: `.*`. Als u wilt verzamelen alleen wisselen ruimte tellers voor het geheugenobject, bijvoorbeeld: kunt u opgeven:`.+Swap.+` |
| interval | De frequentie waarmee de items van het object worden verzameld. |


De volgende tabel bevat de objecten en de items die u in het configuratiebestand opgeven kunt.  Er zijn extra items beschikbaar voor bepaalde toepassingen zoals beschreven in [verzamelen prestatiemeteritems voor Linux-toepassingen in logboekanalyse](log-analytics-data-sources-linux-applications.md).

| Objectnaam | Naam van het meteritem |
|:--|:--|
| Logische schijf | Percentage vrije Inodes |
| Logische schijf | Percentage vrije ruimte |
| Logische schijf | Percentage gebruikte Inodes |
| Logische schijf | Percentage gebruikte ruimte |
| Logische schijf | Bytes gelezen op schijf/sec |
| Logische schijf | Schijf lezen per seconde |
| Logische schijf | Schijfoverdrachten per seconde |
| Logische schijf | Bytes geschreven naar schijf/sec |
| Logische schijf | Schijf schrijven per seconde |
| Logische schijf | Beschikbare Megabytes |
| Logische schijf | Logische schijf Bytes per seconde |
| Geheugen | Percentage beschikbaar geheugen |
| Geheugen | Percentage beschikbare wisselruimte |
| Geheugen | Percentage gebruikt geheugen |
| Geheugen | Percentage gebruikte wisselruimte |
| Geheugen | Beschikbaar geheugen in megabytes |
| Geheugen | Beschikbare megabytes voor wisselen |
| Geheugen | Paginaleesbewerkingen per seconde |
| Geheugen | Pagina's per seconde |
| Geheugen | Pagina's per seconde |
| Geheugen | Gebruikte wisselruimte in megabytes |
| Geheugen | Gebruikt geheugen in megabytes |
| Netwerk | Totaal aantal verzonden Bytes |
| Netwerk | Totaal aantal ontvangen Bytes |
| Netwerk | Totaal aantal Bytes |
| Netwerk | Totaal aantal verzonden pakketten |
| Netwerk | Totaal aantal ontvangen pakketten |
| Netwerk | Totaal aantal Rx-fouten |
| Netwerk | Totaal aantal Tx-fouten |
| Netwerk | Totaal aantal conflicten |
| Fysieke schijf | Gem. Leestijd schijf |
| Fysieke schijf | Gem. Schijfoverdrachten per seconde |
| Fysieke schijf | Gem. Schrijftijd schijf |
| Fysieke schijf | Fysieke schijf Bytes per seconde |
| Proces | PCT beschermde tijd |
| Proces | Tijd in gebruikersmodus pct |
| Proces | Gebruikt geheugen kB |
| Proces | Virtuele gedeeld geheugen |
| Processor | Percentage DPC-tijd |
| Processor | Percentage niet-actieve tijd |
| Processor | Percentage interrupt-tijd |
| Processor | Percentage wachttijd I/O |
| Processor | Percentage tijd in nice |
| Processor | Percentage tijd in beschermde |
| Processor | Percentage processortijd |
| Processor | Percentage tijd in gebruikersmodus |
| Systeem | Beschikbaar fysiek geheugen |
| Systeem | Vrije ruimte in Wisselgeheugenbestanden |
| Systeem | Beschikbaar virtueel geheugen |
| Systeem | Processen |
| Systeem | Grootte opgeslagen In Pagineringbestanden |
| Systeem | Bedrijfstijd |
| Systeem | Gebruikers |


Hier volgt de standaardconfiguratie voor maatstaven voor prestaties.

    <source>
      type oms_omi
      object_name "Physical Disk"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Logical Disk"
      instance_regex ".*
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Processor"
      instance_regex ".*
      counter_name_regex ".*"
      interval 30s
    </source>

    <source>
      type oms_omi
      object_name "Memory"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>

## <a name="data-collection"></a>Gegevensverzameling
Log Analytics verzamelt alle opgegeven prestatiemeteritems op hun opgegeven controle-interval op alle agents die die meteritem geïnstalleerd hebben.  De gegevens wordt niet geaggregeerd en de onbewerkte gegevens is beschikbaar in alle weergaven van logboek zoeken voor de duur is opgegeven door uw abonnement.

## <a name="performance-record-properties"></a>Eigenschappen van de record Performance
Prestaties records hebben een soort **Perf** en de eigenschappen in de volgende tabel hebben.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Computer |Computer waarop de gebeurtenis is verzameld. |
| CounterName |Naam van het prestatiemeteritem |
| Itempad |Volledig pad van het item in de vorm \\ \\ \<Computer >\\object(exemplaar)\\teller. |
| Tegenwaarde |Numerieke waarde van de teller. |
| InstanceName |Naam van het exemplaar van de gebeurtenis.  Als er is geen exemplaar leeg. |
| Objectnaam |Naam van het prestatie-object |
| SourceSystem |Het type van de gegevens zijn verzameld van agent. <br><br>OpsManager – Windows-agent, ofwel direct verbinding maken of SCOM <br> Linux – alle Linux-agents  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated |Datum en tijd die de gegevens door actieve is. |

## <a name="sizing-estimates"></a>Maakt een schatting van schaling
 Een ruwe schatting voor de verzameling van een bepaald item met een interval van 10 seconden is ongeveer 1 MB per dag per exemplaar.  U kunt de opslagvereisten van een bepaald item met de volgende formule schatten.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Logboek van zoekopdrachten met prestatiegegevens
De volgende tabel bevat voorbeelden van logboek-zoekopdrachten die prestatiegegevens ophalen.

| Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| Perf |Alle prestatiegegevens |
| Perf &#124; waar Computer == 'Computer' |Alle prestatiegegevens van een bepaalde computer |
| Perf &#124; Indien CounterName == 'Huidige wachtrijlengte' |Alle prestatiegegevens voor een bepaald item |
| Perf &#124; waar ObjectName == 'Processor' en CounterName == '% processortijd' en InstanceName == "_Totaal" &#124; overzicht van AVGCPU = avg(Average) door Computer |Gemiddelde CPU-gebruik op alle computers |
| Perf &#124; Indien CounterName == "% processortijd" &#124; overzicht van AggregatedValue = max(Max) door Computer |Maximale CPU-gebruik op alle computers |
| Perf &#124; waar ObjectName == 'Logische schijf' en CounterName == 'Huidige wachtrijlengte' en Computer == "MijnComputernaam" &#124; overzicht van AggregatedValue avg(Average) door InstanceName = |Huidige wachtrijlengte voor schijf gemiddelde in alle exemplaren van een bepaalde computer |
| Perf &#124; Indien CounterName == "DiskTransfers per seconde" &#124; overzicht van AggregatedValue = percentiel (gemiddelde, 95) door Computer |95e percentiel van Schijfoverdrachten per seconde op alle computers |
| Perf &#124; Indien CounterName == '% processortijd' en InstanceName == "_Totaal" &#124; overzicht van AggregatedValue = avg(CounterValue) door bin (TimeGenerated, 1 uur), Computer |Elk uur gemiddelde CPU-gebruik op alle computers |
| Perf &#124; waar Computer == 'Computer' en CounterName startswith_cs '%' en InstanceName == "_Totaal" &#124; overzicht van AggregatedValue percentiel (tegenwaarde 70) door bin (TimeGenerated, 1 uur), CounterName = | Elk uur 70 percentiel van elke percentage prestatiemeteritem percentage voor een bepaalde computer |
| Perf &#124; Indien CounterName == '% processortijd' en InstanceName == '_Totaal' en Computer == 'Computer' &#124; samenvatten ['min(CounterValue)'] min(CounterValue), = ["avg(CounterValue)"] = avg(CounterValue), ['percentile75(CounterValue)'] percentiel (tegenwaarde, 75), = ["max(CounterValue)"] = max(CounterValue) door bin (TimeGenerated, 1 uur), Computer |Elk uur gemiddeld, minimum, maximum en 75 percentiel CPU-gebruik voor een specifieke computer |
| Perf &#124; waar ObjectName == "MSSQL$ INST2: Databases ' en InstanceName == 'master' | Alle prestatiegegevens van de prestatie-object van de Database voor de hoofddatabase van het benoemde exemplaar van SQL Server INST2.  




## <a name="next-steps"></a>Volgende stappen
* [Verzamelen van prestatiemeteritems van Linux-toepassingen](log-analytics-data-sources-linux-applications.md) met inbegrip van MySQL en Apache HTTP-Server.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.  
* Verzamelde gegevens exporteren naar [Power BI](log-analytics-powerbi.md) voor extra visualisaties en analyse.
