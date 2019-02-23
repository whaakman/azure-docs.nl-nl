---
title: Verzamelen en analyseren van prestatiemeteritems in Azure Monitor | Microsoft Docs
description: Prestatiemeteritems worden verzameld door Azure Monitor voor het analyseren van prestaties van Windows en Linux-agents.  In dit artikel wordt beschreven hoe u het configureren van verzamelen van prestatiemeteritems voor zowel Windows en Linux-agents, details van deze zijn opgeslagen in de werkruimte, en over het analyseren van deze in Azure portal.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 93f47529e3be44ff1db4e089bdcdca3eb1b4dea3
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728341"
---
# <a name="windows-and-linux-performance-data-sources-in-azure-monitor"></a>Windows en Linux-gegevensbronnen van de prestaties die u in Azure Monitor
In Windows en Linux-prestatiemeteritems geven inzicht in de prestaties van de hardware-onderdelen, besturingssystemen en toepassingen.  Azure Monitor kunt verzamelen van prestatiemeteritems op regelmatige intervallen voor analyse in de buurt van real-time (NRT) naast het verzamelen van prestatiegegevens voor langere termijn analyse en rapportage.

![Prestatiemeteritems](media/data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Prestatiemeteritems configureren
Configureren van prestatiemeteritems van de [menu van de gegevens in de geavanceerde instellingen](agent-data-sources.md#configuring-data-sources).

Wanneer u prestatiemeteritems van Windows of Linux-prestaties voor een nieuwe werkruimte voor het eerst configureert, krijgt u de optie voor het snel verschillende algemene prestatiemeteritems te maken.  Ze worden weergegeven met een selectievakje ernaast.  Zorg ervoor dat alle items die u wilt maken in eerste instantie zijn ingeschakeld en klik vervolgens op **de geselecteerde prestatiemeteritems toevoegen**.

Voor Windows-prestatiemeteritems, kunt u een specifiek exemplaar voor elk prestatiemeteritem. Voor Linux-prestatiemeteritems, het exemplaar van elk item dat u kiest is van toepassing op alle onderliggende items van het bovenliggende item. De volgende tabel toont de algemene instanties beschikbaar voor zowel Windows als Linux-prestatiemeteritems.

| Exemplaarnaam | Description |
| --- | --- |
| \_Totaal |Totaal van alle instanties |
| \* |Alle instanties |
| (/&#124;/var) |Komt overeen met instanties met de naam: / of /var |

### <a name="windows-performance-counters"></a>Windows-prestatiemeteritems

![Configureren van Windows-prestatiemeteritems](media/data-sources-performance-counters/configure-windows.png)

Volg deze procedure om toe te voegen een nieuwe Windows-prestatiemeteritem verzamelen.

1. Typ de naam van het item in het tekstvak in de indeling *object (exemplaar) \counter*.  Wanneer u te typen begint, krijgt u een overeenkomende lijst met algemene prestatiemeteritems.  U kunt een item uit de lijst of typ in een van uw eigen selecteren.  U kunt ook voor alle exemplaren van een bepaald meteritem terugkeren door op te geven *object item*.  

    Wanneer SQL Server-prestatiemeteritems verzamelen van genoemde instanties, alle met de naam exemplaar prestatiemeteritems starten met *MSSQL$* en gevolgd door de naam van het exemplaar.  Geef bijvoorbeeld voor het verzamelen van de teller Log Cache Hit Ratio voor alle databases van de Database prestatie-object voor benoemde SQL INST2, `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`.

2. Klik op **+** of druk op **Enter** het item toevoegen aan de lijst.
3. Wanneer u een item toevoegen, gebruikt de standaardwaarde van 10 seconden voor de **controle-Interval**.  U kunt dit wijzigen naar een hogere waarde van maximaal 1800 seconden (30 minuten) als u wilt beperken van de vereisten van de verzamelde gegevens.
4. Wanneer u klaar bent met het tellers toe te voegen, klikt u op de **opslaan** knop aan de bovenkant van het scherm om de configuratie op te slaan.

### <a name="linux-performance-counters"></a>Linux-prestatiemeteritems

![Configureren van Linux-prestatiemeteritems](media/data-sources-performance-counters/configure-linux.png)

Volg deze procedure om toe te voegen een nieuwe Linux-prestatiemeteritem verzamelen.

1. Standaard worden alle wijzigingen in de configuratie automatisch doorgegeven naar alle agents.  Voor Linux-agents, wordt een configuratiebestand verzonden naar de gegevensverzamelaar Fluentd.  Als u dit bestand handmatig op elke Linux-agent wijzigen wilt, klikt u vervolgens het selectievakje *toepassen op de onderstaande configuratie op mijn Linux-machines* en volg de onderstaande instructies.
2. Typ de naam van het item in het tekstvak in de indeling *object (exemplaar) \counter*.  Wanneer u te typen begint, krijgt u een overeenkomende lijst met algemene prestatiemeteritems.  U kunt een item uit de lijst of typ in een van uw eigen selecteren.  
3. Klik op **+** of druk op **Enter** het item toevoegen aan de lijst met andere tellers voor het object.
4. Alle items voor een object gebruiken hetzelfde **controle-Interval**.  De standaardwaarde is 10 seconden.  U wijzigen dit naar een hogere waarde van maximaal 1800 seconden (30 minuten) als u wilt beperken van de vereisten van de verzamelde gegevens.
5. Wanneer u klaar bent met het tellers toe te voegen, klikt u op de **opslaan** knop aan de bovenkant van het scherm om de configuratie op te slaan.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Linux-prestatiemeteritems in configuratiebestand configureren
In plaats van het configureren van Linux-prestatiemeteritems met behulp van de Azure portal, hebt u de mogelijkheid van het bewerken van configuratiebestanden op de Linux-agent.  Voor het verzamelen van metrische gegevens voor prestaties worden beheerd door de configuratie in **/etc/opt/microsoft/omsagent/\<werkruimte-id\>/conf/omsagent.conf**.

Elk object of de categorie van metrische gegevens voor prestaties voor het verzamelen van moet worden gedefinieerd in het configuratiebestand als een enkel `<source>` element. De syntaxis van patroon het hieronder.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


De parameters in dit element worden in de volgende tabel beschreven.

| Parameters | Description |
|:--|:--|
| object\_naam | Objectnaam voor de verzameling. |
| instance\_regex |  Een *reguliere expressie* definiëren welke instanties te verzamelen. De waarde: `.*` Hiermee geeft u alle instanties. Voor het verzamelen van metrische gegevens van processor voor alleen de \_totale instantie die u kunt opgeven `_Total`. U kunt opgeven voor het verzamelen van metrische gegevens verwerken voor alleen de exemplaren crond of sshd: `(crond\|sshd)`. |
| teller\_naam\_reguliere expressie | Een *reguliere expressie* definiëren welke items (voor het object) te verzamelen. Geef voor het verzamelen van alle tellers voor het object: `.*`. Voor het verzamelen van alleen wisselen ruimte tellers voor het geheugenobject, bijvoorbeeld, kunt u opgeven: `.+Swap.+` |
| interval | De frequentie waarmee de prestatiemeteritems van het object die worden verzameld. |


De volgende tabel bevat de objecten en de items die u in het configuratiebestand opgeven kunt.  Er zijn extra items beschikbaar voor bepaalde toepassingen zoals beschreven in [verzamelen van prestatiemeteritems voor Linux-toepassingen in Azure Monitor](data-sources-linux-applications.md).

| Objectnaam | Naam van teller |
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
| Geheugen | Paginaschrijfbewerkingen per seconde |
| Geheugen | Pagina's per seconde |
| Geheugen | Gebruikte wisselruimte in megabytes |
| Geheugen | Gebruikt geheugen in megabytes |
| Netwerk | Totaal aantal verzonden Bytes |
| Netwerk | Totaal aantal ontvangen Bytes |
| Netwerk | Totaal aantal bytes |
| Netwerk | Totaal aantal verzonden pakketten |
| Netwerk | Totaal aantal ontvangen pakketten |
| Netwerk | Totaal aantal Rx-fouten |
| Netwerk | Totaal aantal Tx-fouten |
| Netwerk | Totaal aantal conflicten |
| Fysieke schijf | Gem. Schijf sec/lezen |
| Fysieke schijf | Gem. Schijfoverdrachten per seconde |
| Fysieke schijf | Gem. Schijf sec/schrijven |
| Fysieke schijf | Fysieke schijf Bytes per seconde |
| Verwerken | PCT gemachtigde tijd |
| Verwerken | PCT in gebruikersmodus |
| Verwerken | Gebruikt geheugen kB |
| Verwerken | Virtuele gedeeld geheugen |
| Processor | Percentage DPC-tijd |
| Processor | % Niet-actieve tijd |
| Processor | % Interrupt Time |
| Processor | Percentage wachttijd I/O |
| Processor | Percentage tijd in nice |
| Processor | % Gemachtigde tijd |
| Processor | % Processortijd |
| Processor | Percentage tijd in gebruikersmodus |
| Systeem | Vrij fysiek geheugen |
| Systeem | Vrije ruimte in Wisselgeheugenbestanden |
| Systeem | Vrij virtueel geheugen |
| Systeem | Processen |
| Systeem | Grootte opgeslagen In Pagineringbestanden |
| Systeem | Actieve tijdsduur |
| Systeem | Gebruikers |


Hieronder volgt de standaardconfiguratie voor metrische gegevens voor prestaties.

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
Azure Monitor verzamelt alle opgegeven prestatiemeteritems op de opgegeven controle-interval op alle agents die die meteritem geïnstalleerd hebben.  De gegevens worden niet samengevoegd en de onbewerkte gegevens is beschikbaar in alle log queryweergaven voor de duur die is opgegeven door uw abonnement.

## <a name="performance-record-properties"></a>Eigenschappen van de record Performance
Prestatierecords zijn een type **Perf** en hebben de eigenschappen in de volgende tabel.

| Eigenschap | Description |
|:--- |:--- |
| Computer |De computer waarop de gebeurtenis is verzameld. |
| CounterName |Naam van het prestatiemeteritem |
| CounterPath |Volledig pad van het item in de vorm \\ \\ \<Computer >\\object(exemplaar)\\teller. |
| CounterValue |Numerieke waarde van de teller. |
| InstanceName |De naam van de event-instantie.  Leeg zijn als er is geen exemplaar. |
| ObjectName |Naam van de prestatie-object |
| SourceSystem |Het type van de agent die de gegevens zijn verzameld uit. <br><br>OpsManager – Windows-agent, rechtstreeks verbinding maken of SCOM <br> Linux: alle Linux-agents  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated |Datum en tijd dat voorbeelden worden de gegevens is gemaakt. |

## <a name="sizing-estimates"></a>Maakt een schatting van grootte
 Een ruwe schatting voor de verzameling van een bepaald meteritem intervallen van 10 seconden is ongeveer 1 MB per dag per exemplaar.  U kunt een schatting maken van de vereisten van een bepaald prestatiemeteritem met de volgende formule.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-queries-with-performance-records"></a>Logboeken-query's met prestatiegegevens
De volgende tabel bevat voorbeelden van Logboeken-query's die prestatierecords ophalen.

| Query’s uitvoeren | Description |
|:--- |:--- |
| Prestaties |Alle prestatiegegevens |
| Perf &#124; where Computer == "MyComputer" |Alle prestatiegegevens van een bepaalde computer |
| Perf &#124; waar CounterName == "Huidige wachtrijlengte" |Alle prestatiegegevens voor een bepaald meteritem |
| Perf &#124; waarbij ObjectName == 'Processor' en CounterName == "% processortijd" en InstanceName == "_Totaal" &#124; AVGCPU samenvatten = avg(Average) door Computer |Gemiddeld CPU-gebruik op alle computers |
| Perf &#124; waar CounterName == "% processortijd" &#124; summarize AggregatedValue = max(Max) door Computer |Maximale CPU-gebruik op alle computers |
| Perf &#124; where ObjectName == "LogicalDisk" and CounterName == "Current Disk Queue Length" and Computer == "MyComputerName" &#124; summarize AggregatedValue = avg(Average) by InstanceName |Gemiddelde van huidige wachtrijlengte voor schijf over alle exemplaren van een bepaalde computer |
| Perf &#124; waar CounterName == "DiskTransfers per seconde" &#124; summarize AggregatedValue = percentiel (gemiddelde, 95) per Computer |95e percentiel van Schijfoverdrachten per seconde op alle computers |
| Perf &#124; waar CounterName == "% processortijd" en InstanceName == "_Totaal" &#124; summarize AggregatedValue = avg(CounterValue) door bin (TimeGenerated, 1 uur), Computer |Per uur gemiddelde CPU-gebruik op alle computers |
| Perf &#124; waarbij Computer == 'Computer' en CounterName startswith_cs '%' en InstanceName == "_Totaal" &#124; summarize AggregatedValue = percentiel (CounterValue, 70) door bin (TimeGenerated, 1 uur), CounterName | Per uur 70 percentiel van elke % percentage item voor een bepaalde computer |
| Perf &#124; waar CounterName == "% processortijd" en InstanceName == '_Totaal' en de Computer == 'Computer' &#124; samenvatten ["min(CounterValue)"] = min(CounterValue), ["avg(CounterValue)"] = avg(CounterValue), ["percentile75(CounterValue)"] percentiel (CounterValue, 75), = ["max(CounterValue)"] = max(CounterValue) door bin (TimeGenerated, 1 uur), Computer |Per uur gemiddelde, minimum, maximum en 75 percentiel CPU-gebruik voor een specifieke computer |
| Perf &#124; waarbij ObjectName == "MSSQL$ INST2: Databases ' en InstanceName == 'master' | Alle prestatiegegevens van de Database prestatie-object voor de hoofddatabase van het benoemde exemplaar van SQL Server INST2.  




## <a name="next-steps"></a>Volgende stappen
* [Verzamelen van prestatiemeteritems van Linux-toepassingen](data-sources-linux-applications.md) met inbegrip van MySQL en Apache HTTP-Server.
* Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.  
* Verzamelde gegevens te exporteren [Power BI](powerbi.md) voor extra visualisaties en analyse.
