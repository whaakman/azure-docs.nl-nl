---
title: Verzamelen van aangepaste logboeken in Azure Monitor | Microsoft Docs
description: Azure Monitor kunt u gebeurtenissen verzamelen uit tekstbestanden op zowel Windows als Linux-computers.  In dit artikel wordt beschreven hoe u een nieuw aangepast logboek en details van de records die zij in Azure Monitor maken definiëren.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: aca7f6bb-6f53-4fd4-a45c-93f12ead4ae1
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2019
ms.author: bwren
ms.openlocfilehash: c80736dcd8be0c7ff3aae850aaaf9659f47daf36
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56234789"
---
# <a name="custom-logs-in-azure-monitor"></a>Aangepaste logboeken in Azure Monitor
Bron van de aangepaste logboeken in Azure Monitor kunt u voor het verzamelen van gebeurtenissen uit tekstbestanden op zowel Windows als Linux-computers. Veel toepassingen logboekgegevens naar tekstbestanden in plaats van standaard logboekregistratieservices zoals Windows-gebeurtenislogboek of Syslog. Zodra de verzameld, kunt u het parseren van de gegevens in afzonderlijke velden in uw query's of extraheer de gegevens die tijdens de verzameling die moet worden afzonderlijke velden.

![Aangepaste logboekverzameling](media/data-sources-custom-logs/overview.png)

De logboekbestanden te verzamelen, moeten overeenkomen met de volgende criteria voldoen.

- Het logboek moet hebben één vermelding per regel of een tijdstempel die overeenkomt met een van de volgende indelingen aan het begin van elke vermelding gebruiken.

    JJJJ-MM-DD UU: MM:<br>M/D/JJJJ UU: MM: SS AM/PM<br>Ma DD, jjjj uu: mm:<br />JJMMDD uu: mm:<br />ddMMyy uu: mm:<br />MMM d uu: mm:<br />DD MMM / / yyyy:HH:mm:ss zzz<br />jjjj-MM-ddTHH:mm:ssK

- Het logboekbestand moet niet toestaan circulair vastleggen of logboekrotatie, waar het bestand wordt overschreven met nieuwe gegevens.
- Het logboekbestand moet gebruiken ASCII- of UTF-8-codering.  Andere indelingen, zoals UTF-16 worden niet ondersteund.

>[!NOTE]
>Als er dubbele vermeldingen in het logboekbestand, worden ze door Azure Monitor verzameld.  Resultaten van de query wordt wel inconsistent waar de filterresultaten meer gebeurtenissen dan het aantal resultaten weergeven.  Het is belangrijk dat u in het logboek te bepalen of dit probleem wordt veroorzaakt door de toepassing die wordt deze gemaakt en los indien mogelijk het voordat u de definitie van de verzameling aangepaste logboek valideren.  
>
  
>[!NOTE]
> Als uw toepassing een nieuw logboekbestand maakt per dag of wanneer het een bepaalde grootte bereikt, detecteert de Log Analytics-agent voor Linux geen ze pas na het opnieuw is opgestart. Dit is omdat de agent alleen inventariseren en begint met de bewaking van patronen met de opgegeven zich aanmeldt bij het opstarten en als gevolg hiervan u van plan bent om het moet door het automatiseren van het opnieuw opstarten van de agent.  Deze beperking bestaat niet met de Log Analytics-agent voor Windows.  
>

>[!NOTE]
> Een Log Analytics-werkruimte ondersteunt de volgende beperkingen:
> 
> * Alleen 500 aangepaste logboeken kunnen worden gemaakt.
> * Een tabel biedt alleen ondersteuning voor maximaal 500 kolommen. 
> * Het maximum aantal tekens in voor de naam van de kolom is 500. 
>

## <a name="defining-a-custom-log"></a>Een aangepast logboek definiëren
Gebruik de volgende procedure voor het definiëren van een aangepaste logboekbestand.  Ga naar het einde van dit artikel voor een overzicht van een voorbeeld van het toevoegen van een aangepast logboek.

### <a name="step-1-open-the-custom-log-wizard"></a>Step 1. Open de Wizard aangepaste logboek
De Wizard voor aangepaste logboek wordt uitgevoerd in Azure portal en kunt u een nieuw aangepast logboek voor het verzamelen van definiëren.

1. Selecteer in de Azure portal, **Log Analytics-werkruimten** > uw werkruimte > **geavanceerde instellingen**.
2. Klik op **gegevens** > **aangepaste logboeken**.
3. Standaard worden alle wijzigingen in de configuratie automatisch doorgegeven naar alle agents.  Voor Linux-agents, wordt een configuratiebestand verzonden naar de gegevensverzamelaar Fluentd.  Als u dit bestand handmatig op elke Linux-agent wijzigen wilt, klikt u vervolgens het selectievakje *toepassen op de onderstaande configuratie op mijn Linux-machines*.
4. Klik op **toevoegen +** om het aangepaste logboek Wizard te openen.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Stap 2. Uploaden en een voorbeeldlogboek parseren
Begint u met het uploaden van een voorbeeld van het aangepaste logboek.  De wizard wordt parseren en de items worden weergegeven in dit bestand voor u om te valideren.  Azure Monitor maakt gebruik van het scheidingsteken dat u opgeeft om te bepalen van elke record.

**Nieuwe regel** is het scheidingsteken en wordt gebruikt voor de logboekbestanden met één vermelding per regel.  Als de regel met een datum en tijd in een van de beschikbare indelingen begint, dan kunt u een **Timestamp** scheidingsteken die ondersteuning biedt voor gegevens met betrekking meer dan één regel tot.

Als een timestamp-scheidingsteken wordt gebruikt, wordt de eigenschap TimeGenerated van elke record die zijn opgeslagen in Azure Monitor worden ingevuld met de datum/tijd opgegeven voor die vermelding in het logboekbestand.  Als een nieuwe regel als scheidingsteken wordt gebruikt, wordt TimeGenerated gevuld met de datum en tijd Azure Monitor de vermelding die worden verzameld.


1. Klik op **Bladeren** en blader naar een voorbeeldbestand.  Houd er rekening mee dat deze knop kan mogelijk worden gelabeld **bestand kiezen** in sommige browsers.
2. Klik op **volgende**.
3. De Wizard voor aangepaste logboek wordt het bestand uploaden en vermelden van de records die wordt geïdentificeerd.
4. Het scheidingsteken dat wordt gebruikt voor identificatie van een nieuwe record en selecteert u het scheidingsteken dat beste de records in het logboekbestand identificeert wijzigen.
5. Klik op **volgende**.

### <a name="step-3-add-log-collection-paths"></a>Stap 3. Paden van logboekverzamelingen toevoegen
U moet een of meer paden definiëren op de agent waar het aangepaste logboek kunnen worden gevonden.  U kunt ofwel een specifiek pad en de naam voor het logboekbestand opgeven of kunt u een pad met jokertekens voor de naam opgeven. Dit biedt ondersteuning voor toepassingen die een nieuw bestand maken elke dag of wanneer een bestand een bepaalde grootte heeft bereikt. U kunt ook meerdere paden opgeven voor één logboekbestand.

Bijvoorbeeld, een toepassing kan een logboekbestand elke dag maken met de datum die is opgenomen in de naam zoals log20100316.txt in. Een patroon voor een logboek met dergelijke mogelijk *log\*.txt* die wilt toepassen op een logbestand de toepassing na de naamgeving van schema.

>[!NOTE]
> Als uw toepassing een nieuw logboekbestand maakt per dag of wanneer het een bepaalde grootte bereikt, detecteert de Log Analytics-agent voor Linux geen ze pas na het opnieuw is opgestart. Dit is omdat de agent alleen inventariseren en begint met de bewaking van patronen met de opgegeven zich aanmeldt bij het opstarten en als gevolg hiervan u van plan bent om het moet door het automatiseren van het opnieuw opstarten van de agent.  Deze beperking bestaat niet met de Log Analytics-agent voor Windows.  
>

De volgende tabel bevat voorbeelden van geldige patronen om op te geven van de verschillende logboekbestanden.

| Description | Pad |
|:--- |:--- |
| Alle bestanden in *C:\Logs* met de extensie .txt op Windows-agent |C:\Logs\\\*.txt |
| Alle bestanden in *C:\Logs* met een naam die begint met logboek- en een .txt-extensie op Windows-agent |C:\Logs\log\*.txt |
| Alle bestanden in */var/log/audit* met de extensie .txt op Linux-agent |/var/log/audit/*.txt |
| Alle bestanden in */var/log/audit* met een naam die begint met logboek- en een .txt-extensie op Linux-agent |/var/log/audit/log\*.txt |

1. Selecteer Windows of Linux om op te geven welke padindeling die u wilt toevoegen.
2. Typ in het pad en klik op de **+** knop.
3. Het proces herhalen voor elke extra paden.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Stap 4. Geef een naam en beschrijving op voor het logboek
De naam die u opgeeft wordt gebruikt voor het logboektype zoals hierboven is beschreven.  Het wordt altijd _CL ter onderscheiding als een aangepast logboek eindigen.

1. Typ een naam voor het logboek.  De  **\_CL** achtervoegsel automatisch wordt geleverd.
2. Een optionele **beschrijving**.
3. Klik op **volgende** om op te slaan van de definitie van de aangepaste logboek.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Stap 5. Valideren dat de aangepaste logboeken worden verzameld
Het kan een uur duren voor de initiële gegevens uit een nieuw aangepast logboek wordt weergegeven in Azure Monitor.  Start het verzamelen van gegevens van de logboeken die zijn gevonden in het pad dat u hebt opgegeven vanaf het moment dat u het aangepaste logboek gedefinieerd.  Deze de items die u hebt geüpload tijdens het maken van het aangepaste logboek niet behouden, maar wordt het bestaande vermeldingen in de logboekbestanden die wordt gezocht naar verzamelen.

Zodra Azure Monitor verzamelen van het aangepaste logboek is gestart, wordt de records worden beschikbaar met een logboekquery.  Gebruik de naam die u hebt opgegeven het aangepaste logboek als de **Type** in uw query.

> [!NOTE]
> Als de RawData-eigenschap in de query ontbreekt, moet u sluiten en opnieuw openen van uw browser.


### <a name="step-6-parse-the-custom-log-entries"></a>Stap 6. Parseren van de aangepaste logboekvermeldingen
De volledige logboekvermelding worden opgeslagen in één eigenschap, genaamd **RawData**.  U zult waarschijnlijk voor het scheiden van de verschillende soorten informatie in elk item in afzonderlijke eigenschappen voor elke record. Raadpleeg [parseren van tekstgegevens in Azure Monitor](../log-query/parse-text.md) voor opties voor het parseren van **RawData** in meerdere eigenschappen.

## <a name="removing-a-custom-log"></a>Een aangepast logboek verwijderen...
Gebruik het volgende proces in Azure portal te verwijderen van een aangepaste logboek dat u eerder hebt gedefinieerd.

1. Uit de **gegevens** in het menu in de **geavanceerde instellingen** voor uw werkruimte, selecteert u **aangepaste logboeken** om uw aangepaste logboeken weer te geven.
2. Klik op **verwijderen** naast het aangepaste logboek te verwijderen.


## <a name="data-collection"></a>Gegevensverzameling
Azure Monitor worden nieuwe gegevens worden verzameld van elke aangepaste logboek ongeveer elke 5 minuten.  De agent wordt plaats daarvan worden opgenomen in elke logboekbestand die het verzamelt uit.  Als de agent voor een bepaalde periode offline gaat, klikt u vervolgens verzamelt Azure Monitor gegevens van waar het laatste afgebroken, zelfs als deze items zijn gemaakt terwijl de agent offline was.

De volledige inhoud van de logboekvermelding worden geschreven naar één eigenschap, genaamd **RawData**.  Zie [parseren van tekstgegevens in Azure Monitor](../log-query/parse-text.md) geïmporteerd voor methoden voor het parseren van elke logboekvermelding in meerdere eigenschappen.

## <a name="custom-log-record-properties"></a>Het aangepaste logboek record-eigenschappen
Aangepaste logboekrecords zijn een type met de naam van het logboek dat u opgeeft en de eigenschappen in de volgende tabel.

| Eigenschap | Description |
|:--- |:--- |
| TimeGenerated |Datum en tijd waarop de record is verzameld door Azure Monitor.  Als het logboek een scheidingsteken wordt gebruikt op basis van tijd is dit de tijd die worden verzameld van de vermelding. |
| SourceSystem |Type van de record van agent is verzameld. <br> OpsManager – Windows-agent, rechtstreeks verbinding maken of System Center Operations Manager <br> Linux: alle Linux-agents |
| RawData |Volledige tekst van de verzamelde post. Waarschijnlijk wilt u [parseren van deze gegevens in afzonderlijke eigenschappen](../log-query/parse-text.md). |
| ManagementGroupName |Naam van de beheergroep van System Center Operations Manage agents.  Voor andere agents is dit AOI -\<werkruimte-ID\> |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Walkthrough voor voorbeeld van een aangepast logbestand toevoegen
De volgende sectie leert u hoe via een voorbeeld van een aangepaste logboek te maken.  De verzamelde voorbeeldlogboek heeft een afzonderlijke vermelding op elke regel die begint met een datum en tijd en klik vervolgens met door komma's gescheiden velden voor code, status en het bericht.  Enkele voorbeelden van vermeldingen worden hieronder weergegeven.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Uploaden en een voorbeeldlogboek parseren
We voorzien in een van de logboekbestanden en de gebeurtenissen die zullen worden verzameld kunt zien.  In dit geval is de nieuwe regel een voldoende scheidingsteken.  Als één vermelding in het logboek echter meerdere regels omvatten kan, klikt u vervolgens moet een timestamp-scheidingsteken worden gebruikt.

![Uploaden en een voorbeeldlogboek parseren](media/data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Paden van logboekverzamelingen toevoegen
De logboekbestanden bevindt zich *C:\MyApp\Logs*.  Een nieuw bestand wordt gemaakt elke dag met een naam die de datum in het patroon bevat *appYYYYMMDD.log*.  Een voldoende patroon voor dit logboek worden *C:\MyApp\Logs\\\*.log*.

![Verzameling logboekpad](media/data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Geef een naam en beschrijving op voor het logboek
We gebruiken een naam op van *MyApp_CL* en typt u in een **beschrijving**.

![Logboeknaam](media/data-sources-custom-logs/log-name.png)

### <a name="validate-that-the-custom-logs-are-being-collected"></a>Valideren dat de aangepaste logboeken worden verzameld
We gebruiken een query voor *Type = MyApp_CL* voor alle records uit de verzamelde logboek.

![Logboek query's uitvoeren met geen aangepaste velden](media/data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Parseren van de aangepaste logboekvermeldingen
We aangepaste velden gebruiken voor het definiëren van de *EventTime*, *Code*, *Status*, en *bericht* velden en zien we het verschil in de records die worden geretourneerd door de query.

![Logboek query's uitvoeren met aangepaste velden](media/data-sources-custom-logs/query-02.png)

## <a name="alternatives-to-custom-logs"></a>Alternatieven voor aangepaste logboeken
Terwijl aangepaste logboeken handig zijn als uw gegevens geschikt is voor de criteria over, maar er zijn gevallen, zoals het volgende wanneer u een andere strategie nodig:

- De gegevens past niet de vereiste structuur en dus bijvoorbeeld de tijdstempel in een andere indeling.
- Het logboekbestand voldoen niet aan vereisten zoals bestandscodering of een niet-ondersteunde mapstructuur.
- De gegevens vereist voorverwerking of filteren voordat de verzameling. 

In gevallen waar uw gegevens kan niet worden verzameld met aangepaste logboeken, houd rekening met de volgende alternatieve strategieën:

- Een aangepast script of andere methode gebruiken om te schrijven naar [Windows-gebeurtenissen](data-sources-windows-events.md) of [Syslog](data-sources-syslog.md) die door Azure Monitor worden verzameld. 
- De gegevens rechtstreeks naar de met behulp van Azure Monitor verzenden [HTTP Data Collector API](data-collector-api.md). Een voorbeeld met behulp van runbooks in Azure Automation wordt geleverd [verzamelen logboekgegevens in Azure Monitor met een Azure Automation-runbook](runbook-datacollect.md).

## <a name="next-steps"></a>Volgende stappen
* Zie [parseren van tekstgegevens in Azure Monitor](../log-query/parse-text.md) geïmporteerd voor methoden voor het parseren van elke logboekvermelding in meerdere eigenschappen.
* Meer informatie over [query's bijgehouden](../log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.