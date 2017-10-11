---
title: Verzamelen van aangepaste logboeken in OMS Log Analytics | Microsoft Docs
description: "Log Analytics kunt verzamelen van gebeurtenissen uit tekstbestanden op Windows- en Linux-computers.  In dit artikel wordt beschreven hoe een nieuwe aangepaste logboek en details van de records die ze in de OMS-opslagplaats maken definiëren."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2017
ms.author: bwren
ms.openlocfilehash: b7f28868e3ffdf95dbe39872f382e7c97eae692c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="custom-logs-in-log-analytics"></a>Aangepaste logboeken in Log Analytics
De gegevensbron voor de aangepaste logboeken in Log Analytics kunt u het verzamelen van gebeurtenissen uit tekstbestanden op Windows- en Linux-computers. Veel toepassingen logboekgegevens naar tekstbestanden in plaats van standaard logboekregistratieservices zoals Windows-gebeurtenislogboek of Syslog.  Zodra verzameld, kunt u elke record in het logboek in afzonderlijke velden met parseren de [aangepaste velden](log-analytics-custom-fields.md) functie van logboekanalyse.

![Aangepaste logboekgegevens verzameld](media/log-analytics-data-sources-custom-logs/overview.png)

De logboekbestanden moeten worden verzameld, moeten overeenkomen met de volgende criteria.

- Het logboek moet hebben één vermelding per regel of een tijdstempel die overeenkomt met een van de volgende indelingen aan het begin van elke vermelding te gebruiken.

    JJJJ-MM-DD: MM: SS<br>M/D/JJJJ UU: MM: SS AM/PM <br>Ma DD JJJJ: mm: ss

- Het logboekbestand mag geen circulaire updates toestaan waar het bestand wordt overschreven met nieuwe gegevens.
- Het logboekbestand moet gebruiken ASCII- of UTF-8-codering.  Andere indelingen zoals UTF-16 worden niet ondersteund.

>[!NOTE]
>Als er dubbele vermeldingen in het logboekbestand, wordt ze logboekanalyse verzamelen.  De zoekresultaten wordt wel inconsistente waarop de filterresultaten meer gebeurtenissen dan het aantal resultaten weergeven.  Dit is belangrijk dat u het logboek om te bepalen of dit probleem wordt veroorzaakt door de toepassing die wordt deze gemaakt en los indien mogelijk het voordat u de definitie van de verzameling aangepaste logboek valideren.  
>
  
## <a name="defining-a-custom-log"></a>Een aangepast logboek definiëren
Gebruik de volgende procedure voor het definiëren van een aangepaste logboekbestand.  Ga naar het einde van dit artikel voor een overzicht van een steekproef van een aangepaste logboekbestanden toe te voegen.

### <a name="step-1-open-the-custom-log-wizard"></a>Step 1. Open de Wizard aangepaste logboek
De Wizard aangepaste logboek wordt uitgevoerd in de OMS-portal en kunt u een nieuwe aangepaste logboekgegevens verzamelen definiëren.

1. In de OMS-portal, gaat u naar **instellingen**.
2. Klik op **gegevens** en vervolgens **aangepaste logboeken**.
3. Standaard worden alle configuratiewijzigingen automatisch naar alle agents gepusht.  Voor Linux-agents wordt een configuratiebestand verzonden naar de gegevensverzamelaarset Fluentd.  Als u wijzigen van dit bestand handmatig op elke Linux-agent wilt, schakel het selectievakje *toepassen op de onderstaande configuratie op mijn Linux-machines*.
4. Klik op **toevoegen +** om het aangepaste logboek Wizard te openen.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Stap 2. Uploaden en een voorbeeldlogboek te parseren
Begint u met het uploaden van een steekproef van het aangepaste logboek.  De wizard parseren en de gegevens worden weergegeven in dit bestand om te valideren.  Log Analytics maakt gebruik van het scheidingsteken dat u opgeeft om te bepalen van elke record.

**Nieuwe regel** is het standaardscheidingsteken en wordt gebruikt voor logboekbestanden met één vermelding per regel.  Als de regel met een datum en tijd in een van de beschikbare indelingen begint en vervolgens kunt u een **tijdstempel** scheidingsteken die ondersteuning biedt voor vermeldingen die meer dan één regel omvatten.

Als een tijdstempel scheidingsteken wordt gebruikt, wordt de eigenschap TimeGenerated van elke record in OMS worden ingevuld met de datum/tijd opgegeven voor deze vermelding in het logboekbestand.  Als een nieuwe regel scheidingsteken wordt gebruikt, wordt TimeGenerated gevuld met de datum en tijd of Log Analytics de vermelding verzameld.

> [!NOTE]
> Log Analytics behandelt momenteel de datum/tijd verzameld van een logboek met behulp van een timestamp-scheidingsteken als UTC.  Dit zal binnenkort worden gewijzigd voor het gebruik van de tijdzone op de agent.
>
>

1. Klik op **Bladeren** en blader naar een voorbeeldbestand.  Let op deze knop kan mogelijk worden gelabeld **bestand kiezen** in sommige browsers.
2. Klik op **Volgende**.
3. De Wizard aangepaste logboek wordt upload het bestand en de records die wordt geïdentificeerd.
4. Het scheidingsteken dat wordt gebruikt om te bepalen van een nieuwe record en selecteer het scheidingsteken dat het beste de records in het logboekbestand identificeert wijzigen.
5. Klik op **Volgende**.

### <a name="step-3-add-log-collection-paths"></a>Stap 3. Paden van logboekverzamelingen toevoegen
U moet een of meer paden definiëren op de agent waar het aangepaste logboek kunnen worden gevonden.  U kunt ofwel een specifiek pad en naam voor het logboekbestand opgeven of kunt u een pad met een jokerteken voor de naam opgeven.  Dit biedt ondersteuning voor toepassingen die een nieuw bestand maken elke dag of wanneer een bestand een bepaalde grootte heeft bereikt.  U kunt ook meerdere paden opgeven voor één logboekbestand.

Bijvoorbeeld, een toepassing kan een logboekbestand elke dag maken met de datum die is opgenomen in de naam zoals log20100316.txt in. Een patroon voor een dergelijke logboek is mogelijk *logboek\*.txt* die wordt toegepast op een logboekbestand na de toepassing de naamgeving van schema.

De volgende tabel bevat voorbeelden van geldige patronen om op te geven van de verschillende logboekbestanden.

| Beschrijving | Pad |
|:--- |:--- |
| Alle bestanden in *C:\Logs* met .txt-extensie op Windows-agent |C:\Logs\\\*.txt |
| Alle bestanden in *C:\Logs* met een naam die begint met het logboek en een txt-extensie op Windows-agent |C:\Logs\log\*.txt |
| Alle bestanden in */var/log/audit* met .txt-extensie op Linux-agent |/var/log/audit/*.txt |
| Alle bestanden in */var/log/audit* met een naam die begint met het logboek en een txt-extensie op Linux-agent |/var/log/audit/log\*.txt |

1. Selecteer Windows of Linux welke padindeling van het opgeven die u wilt toevoegen.
2. Typ in het pad en de  **+**  knop.
3. Het proces herhalen voor elke extra paden.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Stap 4. Geef een naam en beschrijving voor het logboek
De naam die u opgeeft wordt gebruikt voor het logboektype zoals hierboven is beschreven.  Het wordt altijd eindigen met _CL om ze te onderscheiden als een aangepast logboek.

1. Typ een naam voor het logboek.  De  **\_CL** achtervoegsel automatisch wordt geleverd.
2. Voeg een optionele **beschrijving**.
3. Klik op **volgende** om op te slaan van het aangepaste logboek-definitie.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Stap 5. Valideren dat de aangepaste logboeken worden verzameld
Het kan een uur duren voor de initiële gegevens uit een nieuwe aangepaste logboek in Log Analytics wordt weergegeven.  Start het verzamelen van gegevens van de logboekbestanden gevonden in het pad dat u hebt opgegeven vanaf het punt dat u het aangepaste logboek gedefinieerd.  Het niet de vermeldingen die u tijdens het maken van aangepaste logboek geüpload behoudt, maar er al een bestaande vermeldingen in de logboekbestanden die wordt gezocht naar verzamelt.

Zodra logboekanalyse verzamelen van het aangepaste logboek start, is de records worden beschikbaar met een zoekopdracht logboek.  Gebruik de naam die u hebt opgegeven met het aangepaste logboek als de **Type** in uw query.

> [!NOTE]
> Als de eigenschap RawData ontbreekt in de zoekopdracht is, moet u wellicht sluit en Open uw browser.
>
>

### <a name="step-6-parse-the-custom-log-entries"></a>Stap 6. De aangepaste logboekvermeldingen parseren
De volledige logboekvermelding worden opgeslagen in één eigenschap aangeroepen **RawData**.  Waarschijnlijk wilt scheiden van de verschillende soorten informatie in elk item in afzonderlijke eigenschappen die zijn opgeslagen in de record.  U dit doen met de [aangepaste velden](log-analytics-custom-fields.md) functie van logboekanalyse.

Gedetailleerde stappen voor het parseren van de aangepaste logboekvermelding worden niet hier beschreven.  Raadpleeg de [aangepaste velden](log-analytics-custom-fields.md) documentatie voor deze informatie.

## <a name="disabling-a-custom-log"></a>Een aangepast logboek uitschakelen
U kunt de definitie van een aangepaste logboek niet verwijderen, zodra deze is gemaakt, maar u deze uitschakelen kunt door het verwijderen van alle de paden van de verzameling.

1. In de OMS-portal, gaat u naar **instellingen**.
2. Klik op **gegevens** en vervolgens **aangepaste logboeken**.
3. Klik op **Details** naast de definitie van het aangepaste logboek wilt uitschakelen.
4. Elk van de verzameling paden voor de definitie van de aangepaste logboekgegevens verwijderd.

## <a name="data-collection"></a>Gegevensverzameling
Log Analytics worden nieuwe gegevens worden verzameld uit elk aangepast logboek ongeveer elke 5 minuten.  De agent wordt plaats registreren in elk logboekbestand die worden verzameld uit.  Als de agent voor een bepaalde periode offline gaat, klikt u vervolgens verzamelt Log Analytics vermeldingen van waar het laatst werd afgebroken, zelfs als deze vermeldingen zijn gemaakt terwijl de agent offline was.

De volledige inhoud van de logboekvermelding worden geschreven naar een enkele eigenschap genaamd **RawData**.  U kunt dit parseren naar meerdere eigenschappen die kunnen worden geanalyseerd en afzonderlijk worden doorzocht door te definiëren [aangepaste velden](log-analytics-custom-fields.md) nadat u het aangepaste logboek hebt gemaakt.

## <a name="custom-log-record-properties"></a>Eigenschappen van aangepaste record
Een type met de naam van het logboek dat u opgeeft en de eigenschappen van hebben aangepaste logboekrecords in de volgende tabel.

| Eigenschap | Beschrijving |
|:--- |:--- |
| TimeGenerated |Datum en tijd waarop de record is verzameld door logboekanalyse.  Als het logboek een scheidingsteken wordt gebruikt op basis van tijd is dit de tijd verzameld van de vermelding. |
| SourceSystem |Type van de agent die de record is verzameld. <br> OpsManager – Windows-agent, ofwel direct verbinding maken of System Center Operations Manager <br> Linux – alle Linux-agents |
| RawData |Volledige tekst van de verzamelde vermelding. |
| ManagementGroupName |De naam van de beheergroep voor System Center Operations beheren agents.  Dit is voor andere agents AOI -\<werkruimte-ID\> |

## <a name="log-searches-with-custom-log-records"></a>Logboek van zoekopdrachten met aangepaste logboekrecords
Records van aangepaste logboeken worden opgeslagen in de OMS-opslagplaats net als records uit een andere gegevensbron.  Hebben ze een type dat overeenkomt met de naam die u opgeeft bij het definiëren van het logboek, zodat u de eigenschap Type in de zoekopdracht kunt records verzameld van een specifieke logboek op te halen.

De volgende tabel bevat voorbeelden van logboek-zoekopdrachten die records uit de aangepaste logboeken ophalen.

| Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| Type MyApp_CL = |Alle gebeurtenissen van een aangepaste benoemde MyApp_CL worden geregistreerd. |
| Type = MyApp_CL Severity_CF fout = |Alle gebeurtenissen van een aangepaste benoemde MyApp_CL worden geregistreerd met een waarde van *fout* in een aangepast veld met de naam *Severity_CF*. |

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe querytaal van Log Analytics](log-analytics-log-search-upgrade.md), worden bovenstaande query's gewijzigd in het volgende.

> | Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| MyApp_CL |Alle gebeurtenissen van een aangepaste benoemde MyApp_CL worden geregistreerd. |
| MyApp_CL &#124; waar Severity_CF == "error" |Alle gebeurtenissen van een aangepaste benoemde MyApp_CL worden geregistreerd met een waarde van *fout* in een aangepast veld met de naam *Severity_CF*. |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Overzicht van het toevoegen van een aangepaste logboek voorbeeld
De volgende sectie wordt uitgelegd een voorbeeld van een aangepaste logboek te maken.  De voorbeeldlogboek worden verzameld heeft één vermelding per regel beginnen met een datum en tijd en vervolgens door komma's gescheiden velden voor de code, status en het bericht.  Enkele voorbeelden van vermeldingen worden hieronder weergegeven.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Uploaden en een voorbeeldlogboek te parseren
We bieden een van de logboekbestanden en de gebeurtenissen die zullen worden verzameld kunnen zien.  In dit geval is de nieuwe regel een voldoende scheidingsteken.  Als een afzonderlijke vermelding in het logboek echter meerdere regels omvatten kan, zou een timestamp-scheidingsteken moet worden gebruikt.

![Uploaden en een voorbeeldlogboek te parseren](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Paden van logboekverzamelingen toevoegen
De logboekbestanden zich in de *C:\MyApp\Logs*.  Een nieuw bestand wordt gemaakt per dag met een naam die de datum in het patroon bevat *appYYYYMMDD.log*.  Een voldoende patroon voor dit logboek zou worden *C:\MyApp\Logs\\\*.log*.

![Verzameling logboekpad](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Geef een naam en beschrijving voor het logboek
We gebruiken een naam van *MyApp_CL* en typt u in een **beschrijving**.

![Logboeknaam](media/log-analytics-data-sources-custom-logs/log-name.png)

### <a name="validate-that-the-custom-logs-are-being-collected"></a>Valideren dat de aangepaste logboeken worden verzameld
We gebruiken een query voor *Type = MyApp_CL* voor alle records uit de verzamelde logboek.

![Logboek query met geen aangepaste velden](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>De aangepaste logboekvermeldingen parseren
We gebruiken van aangepaste velden voor het definiëren van de *EventTime*, *Code*, *Status*, en *bericht* velden en we ziet de verschillen in de records die worden geretourneerd door de query.

![Logboek query met aangepaste velden](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Volgende stappen
* Gebruik [aangepaste velden](log-analytics-custom-fields.md) parseren van de vermeldingen in het aangepaste logboek in afzonderlijke velden.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.
