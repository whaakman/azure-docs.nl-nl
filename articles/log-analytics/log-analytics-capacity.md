---
title: Oplossing voor capaciteit en prestaties in Azure Log Analytics | Microsoft Docs
description: De oplossing capaciteit en prestaties in Log Analytics gebruiken om te begrijpen van de capaciteit van de Hyper-V-servers.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 51617a6f-ffdd-4ed2-8b74-1257149ce3d4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.author: magoedte
ms.component: na
ms.openlocfilehash: e9163b3c29fd304c80eff46426a30c4fa0ce3b15
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130340"
---
# <a name="plan-hyper-v-virtual-machine-capacity-with-the-capacity-and-performance-solution-preview"></a>Plannen van capaciteit voor Hyper-V-virtuele machine met de capaciteit en prestaties-oplossing (Preview)

![Symbool van capaciteit en prestaties](./media/log-analytics-capacity/capacity-solution.png)

> [!NOTE]
> De capaciteit en prestaties oplossing is afgeschaft.  Klanten die de oplossing al hebt geïnstalleerd kunnen blijven gebruiken, maar de capaciteit en prestaties kunnen niet worden toegevoegd aan een nieuwe werkruimten.

U kunt de oplossing capaciteit en prestaties in Log Analytics gebruiken om te begrijpen van de capaciteit van de Hyper-V-servers. De oplossing biedt inzicht in uw Hyper-V-omgeving waarin u het totale gebruik (CPU, geheugen en schijfruimte) van de hosts en de virtuele machines op deze hosts Hyper-V wordt uitgevoerd. Metrische gegevens worden verzameld voor CPU, geheugen en schijven op alle hosts en de VM's erop worden uitgevoerd.

De oplossing:

-   Bevat hosts met de hoogste en laagste gebruik van CPU en geheugen
-   Virtuele machines bevat met de hoogste en laagste gebruik van CPU en geheugen
-   Virtuele machines bevat met de hoogste en laagste IOPS en doorvoerlimieten gebruik
-   Geeft aan welke VM's worden uitgevoerd op welke hosts
-   Geeft de bovenste schijven met hoge doorvoer, IOPS en latentie in gedeelde clustervolumes
- Hiermee kunt u filteren op basis van groepen en aanpassen

> [!NOTE]
> De vorige versie van de capaciteit en prestaties oplossing aangeroepen capaciteit Management vereist zowel System Center Operations Manager en System Center Virtual Machine Manager. Deze bijgewerkte oplossing beschikt niet over die afhankelijkheden.


## <a name="connected-sources"></a>Verbonden bronnen

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteuning | Beschrijving |
|---|---|---|
| [Windows-agents](log-analytics-windows-agent.md) | Ja | De oplossing verzamelt gegevens van de capaciteit en prestaties van Windows-agents. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee    | De oplossing verzamelt geen gegevens van de capaciteit en prestaties van rechtstreekse Linux-agents.|
| [SCOM-beheergroep](log-analytics-om-agents.md) | Ja |De oplossing verzamelt gegevens van capaciteit en prestaties van agents in een verbonden SCOM-beheergroep. Een rechtstreekse verbinding tussen de SCOM-agents met logboekanalyse is niet vereist.|
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | Azure-opslag bevat geen gegevens voor capaciteit en prestaties.|

## <a name="prerequisites"></a>Vereisten

- Windows- of Operations Manager-agents moeten worden geïnstalleerd op Windows Server 2012 Hyper-V-hosts of hoger, niet voor virtuele machines.


## <a name="configuration"></a>Configuratie

Voer de volgende stap om de oplossing capaciteit en prestaties toevoegen aan uw werkruimte.

- De oplossing capaciteit en prestaties toevoegen aan uw werkruimte voor logboekanalyse met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).

## <a name="management-packs"></a>Management packs

Als uw SCOM-beheergroep met de werkruimte voor logboekanalyse is verbonden, worden wordt de volgende management packs geïnstalleerd in SCOM wanneer u deze oplossing toevoegt. Er is geen configuratie of onderhoud van deze management packs vereist.

- Microsoft.IntelligencePacks.CapacityPerformance

De gebeurtenis 1201 er ongeveer als volgt:


```
New Management Pack with id:"Microsoft.IntelligencePacks.CapacityPerformance", version:"1.10.3190.0" received.
```

Wanneer de capaciteit en prestaties oplossing wordt bijgewerkt, wordt het versienummer wordt gewijzigd.

Zie [Operations Manager koppelen aan Log Analytics](log-analytics-om-agents.md) voor meer informatie over de manier waarop uw management packs voor oplossingen worden bijgewerkt.

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u de oplossing capaciteit en prestaties toevoegen aan uw werkruimte, wordt de capaciteit en prestaties toegevoegd aan het Overzichtsdashboard. Deze tegel geeft een telling van het aantal actieve Hyper-V-hosts en het aantal actieve virtuele machines die zijn gecontroleerd voor de tijdsperiode geselecteerd.

![Tegel capaciteit en prestaties](./media/log-analytics-capacity/capacity-tile.png)


### <a name="review-utilization"></a>Gebruik controleren

Klik op de capaciteit en prestaties tegel om de capaciteit en prestaties dashboard te openen. Het dashboard bevat de kolommen in de volgende tabel. Elke kolom bevat maximaal tien items die overeenkomen de criteria voor het opgegeven bereik en het tijdsbereik van die kolom. U kunt een zoekactie die alle records retourneert, uitvoeren op het logboek door onderaan in de kolom te klikken op **Alles bekijken** of door te klikken op de kolomkoptekst.

- **Hosts**
    - **Host-CPU-gebruik** ziet u een grafische trend weergegeven van de CPU-gebruik van de host-computers en een lijst met hosts, op basis van de geselecteerde tijdsperiode. Beweeg de muisaanwijzer over het lijndiagram om details te bekijken voor een bepaald punt in tijd. Klik in de grafiek om meer details weergeven in het logboek zoeken. Klik op de hostnaam van elke logboek zoeken openen en weergeven van CPU-details voor prestatiemeteritems voor gehoste virtuele machines.
    - **Geheugengebruik host** ziet u een grafische trend weergegeven van het geheugengebruik van hostcomputers en een lijst met hosts, op basis van de geselecteerde tijdsperiode. Beweeg de muisaanwijzer over het lijndiagram om details te bekijken voor een bepaald punt in tijd. Klik in de grafiek om meer details weergeven in het logboek zoeken. Klik op de hostnaam van elke logboek zoeken openen en weergeven van geheugen-details voor prestatiemeteritems voor gehoste virtuele machines.
- **Virtuele machines**
    - **VM CPU-gebruik** ziet u een grafische trend weergegeven van het CPU-gebruik van virtuele machines en een lijst van virtuele machines, op basis van de geselecteerde tijdsperiode. Beweeg de muisaanwijzer over het lijndiagram om details te bekijken voor een bepaald punt in tijd voor de bovenste 3 virtuele machines. Klik in de grafiek om meer details weergeven in het logboek zoeken. Klik op de naam van elke VM logboek zoeken openen en weergeven van cumulatieve CPU-details voor prestatiemeteritems voor de virtuele machine.
    - **VM-geheugengebruik** ziet u een grafische trend weergegeven van het geheugengebruik van virtuele machines en een lijst van virtuele machines, op basis van de geselecteerde tijdsperiode. Beweeg de muisaanwijzer over het lijndiagram om details te bekijken voor een bepaald punt in tijd voor de bovenste 3 virtuele machines. Klik in de grafiek om meer details weergeven in het logboek zoeken. Klik op de naam van elke VM logboek zoeken openen en weergeven van details voor prestatiemeteritems geaggregeerde geheugen voor de virtuele machine.
    - **Totaal aantal schijf-IOPS VM** toont een grafische trend weergegeven van de totale schijf-IOPS voor virtuele machines en een lijst met virtuele machines met het aantal IOPS dat voor elk, op basis van de geselecteerde tijdsperiode. Beweeg de muisaanwijzer over het lijndiagram om details te bekijken voor een bepaald punt in tijd voor de bovenste 3 virtuele machines. Klik in de grafiek om meer details weergeven in het logboek zoeken. Klik op de naam van een virtuele machine openen logboek Zoek- en samengevoegde schijf-IOPS teller details voor de virtuele machine.
    - **De totale schijfruimte doorvoer VM** toont een grafische trend weergegeven van de totale schijfruimte doorvoer voor virtuele machines en een lijst met virtuele machines met de totale schijfruimte doorvoer voor elk, op basis van de geselecteerde tijdsperiode. Beweeg de muisaanwijzer over het lijndiagram om details te bekijken voor een bepaald punt in tijd voor de bovenste 3 virtuele machines. Klik in de grafiek om meer details weergeven in het logboek zoeken. Klik op de naam van elke VM logboek zoeken openen en weergeven van totale schijfgrootte geaggregeerde doorvoer teller details voor de virtuele machine.
- **Geclusterde gedeelde Volumes**
    - **Totale doorvoer** bevat de som van beide leest en schrijft op geclusterde gedeelde volumes.
    - **Totaal aantal IOPS** ziet u de som van i/o-bewerkingen per seconde op geclusterde gedeelde volumes.
    - **Totale latentie** ziet u de totale latentie op geclusterde gedeelde volumes.
- **Host dichtheid** de eerste tegel toont het totale aantal hosts en virtuele machines beschikbaar zijn voor de oplossing. Klik op de bovenste tegel aanvullende om details te bekijken in het logboek zoeken. Een lijst met alle hosts en het aantal virtuele machines die worden gehost. Klik op een host om te zoomen op de resultaten van de virtuele machine in een logboek zoekopdracht.


![dashboard Hosts blade](./media/log-analytics-capacity/dashboard-hosts.png)

![blade van de virtuele machines dashboard](./media/log-analytics-capacity/dashboard-vms.png)


### <a name="evaluate-performance"></a>Prestaties evalueren

Computeromgevingen met productie verschillen aanzienlijk van een organisatie. Bovendien capaciteit en prestaties werkbelastingen is afhankelijk van hoe uw virtuele machines worden uitgevoerd, en wat u rekening houden met normale. Specifieke procedures om u te helpen de prestaties van de meting waarschijnlijk niet van toepassing op uw omgeving. Zodat meer gegeneraliseerd prescriptieve richtlijnen is beter geschikt is voor help. Microsoft publiceert tal van artikelen richtlijnen om te meten van prestaties.

Om samen te vatten, verzamelt de oplossing capaciteit en prestaties gegevens uit diverse bronnen, met inbegrip van prestatiemeteritems. Gebruik van capaciteit en prestaties gegevens die die zijn gepresenteerd in verschillende vlakken in de oplossing en vergelijkt u de resultaten op die op de [meten van de prestaties van Hyper-V](https://msdn.microsoft.com/library/cc768535.aspx) artikel. Hoewel het artikel kan enige tijd geleden is gepubliceerd, zijn de metrische gegevens, overwegingen en richtlijnen nog geldig. Het artikel bevat koppelingen naar andere nuttige informatiebronnen.


## <a name="sample-log-searches"></a>Voorbeeldzoekopdrachten in logboeken

De volgende tabel bevat een voorbeeld-logboek zoekt capaciteit en prestaties gegevens verzameld en wordt berekend door deze oplossing.


| Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| Alle configuraties van de host geheugen | Perf &#124; waar ObjectName == 'Capaciteit en prestaties' en CounterName == "Host toegewezen geheugen (MB)" &#124; samenvatten MB avg(CounterValue) door InstanceName = |
| Alle configuraties voor VM-geheugen | Perf &#124; waar ObjectName == 'Capaciteit en prestaties' en CounterName == "VM toegewezen geheugen (MB)" &#124; samenvatten MB avg(CounterValue) door InstanceName = |
| Overzicht van de totale schijf-IOPS over alle VM 's | Perf &#124; waar ObjectName == 'Capaciteit en prestaties' en (CounterName == "VHD leesbewerkingen/s" of CounterName == "VHD-schrijfbewerkingen/s") &#124; AggregatedValue samenvatten = avg(CounterValue) door bin (TimeGenerated, 1 uur), CounterName, InstanceName |
| Overzicht van de totale doorvoer van de schijf over alle VM 's | Perf &#124; waar ObjectName == 'Capaciteit en prestaties' en (CounterName == "VHD lezen MB/s" of CounterName == "VHD schrijven MB/s") &#124; AggregatedValue samenvatten = avg(CounterValue) door bin (TimeGenerated, 1 uur), CounterName, InstanceName |
| Overzicht van de totale IOP's over alle CSV 's | Perf &#124; waar ObjectName == 'Capaciteit en prestaties' en (CounterName == "CSV leesbewerkingen/s" of CounterName == "CSV-schrijfbewerkingen/s") &#124; AggregatedValue samenvatten = avg(CounterValue) door bin (TimeGenerated, 1 uur), CounterName, InstanceName |
| Overzicht van de totale doorvoer over alle CSV 's | Perf &#124; waar ObjectName == 'Capaciteit en prestaties' en (CounterName == "CSV leesbewerkingen/s" of CounterName == "CSV-schrijfbewerkingen/s") &#124; AggregatedValue samenvatten = avg(CounterValue) door bin (TimeGenerated, 1 uur), CounterName, InstanceName |
| Overzicht van de totale latentie over alle CSV 's | Perf &#124; waar ObjectName == 'Capaciteit en prestaties' en (CounterName == 'CSV lezen latentie' of CounterName == 'CSV schrijven latentie') &#124; AggregatedValue samenvatten = avg(CounterValue) door bin (TimeGenerated, 1 uur), CounterName, InstanceName |


## <a name="next-steps"></a>Volgende stappen
* Gebruik [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-search.md) om gedetailleerde gegevens voor capaciteit en prestaties te bekijken.
