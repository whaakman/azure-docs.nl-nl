---
title: Verzamelen en analyseren van Windows-gebeurtenislogboeken in Azure Log Analytics | Microsoft Docs
description: Windows-gebeurtenislogboeken zijn een van de meest voorkomende gegevensbronnen die worden gebruikt door logboekanalyse.  In dit artikel wordt beschreven hoe de verzameling van Windows-gebeurtenislogboeken en details van de records die ze in de werkruimte voor logboekanalyse maken configureren.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: bwren
ms.openlocfilehash: 7a7deb4d7a287b2e9613e6035a7ffd7bb6f14f9c
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Gegevensbronnen van de Windows-gebeurtenislogboek in Log Analytics
Windows-gebeurtenislogboeken zijn een van de meest voorkomende [gegevensbronnen](log-analytics-data-sources.md) voor het verzamelen van gegevens met behulp van Windows-agents omdat veel toepassingen naar het Windows-gebeurtenislogboek schrijven.  U kunt gebeurtenissen verzamelen uit standaard logboeken zoals systeem en toepassing naast het opgeven van een aangepaste logboeken gemaakt door toepassingen die u wilt bewaken.

![Windows-gebeurtenissen](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Configureren Windows gebeurtenislogboeken
Configureren van Windows-gebeurtenislogboeken vanuit de [menu Data in logboekanalyse-instellingen](log-analytics-data-sources.md#configuring-data-sources).

Log Analytics verzamelt alleen gebeurtenissen uit de Windows-gebeurtenislogboeken die zijn opgegeven in de instellingen.  U kunt een gebeurtenislogboek toevoegen door te typen in de naam van het logboek en te klikken op  **+** .  Voor elk logboek worden alleen de gebeurtenissen met de geselecteerde ernst verzameld.  Controleer de ernst voor de specifieke logboek die u wilt verzamelen.  U kunt aanvullende criteria gebeurtenissen filteren kan niet opgeven.

Terwijl u de naam van een gebeurtenislogboek typt, bevat logboekanalyse suggesties van algemene namen van gebeurtenislogboeken. Als het logboek dat u wilt toevoegen in de lijst niet wordt weergegeven, kunt u dit nog steeds toevoegen door in de volledige naam van het logboek te typen. U vindt de volledige naam van het logboek in Logboeken. Open in Logboeken, het *eigenschappen* pagina voor het logboek en kopieer de tekenreeks van de *volledige naam* veld.

![Windows-gebeurtenissen configureren](media/log-analytics-data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Gegevensverzameling
Log Analytics verzamelt elke gebeurtenis die overeenkomt met een geselecteerde ernst van een bewaakte gebeurtenislogboek als de gebeurtenis wordt gemaakt.  De agent registreert plaats daarvan in elk logboek die worden verzameld uit.  Als de agent voor een bepaalde periode offline gaat, klikt u vervolgens verzamelt Log Analytics gebeurtenissen van waar het laatst werd afgebroken, zelfs als de gebeurtenissen die zijn gemaakt terwijl de agent offline was.  Er is het mogelijk om deze gebeurtenissen niet worden verzameld als het gebeurtenislogboek met niet-geïnde gebeurtenissen wordt overschreven doorloopt als de agent offline is.

>[!NOTE]
>Log Analytics verzamelt geen controlegebeurtenissen die zijn gemaakt door SQL Server van bron *MSSQLSERVER* met gebeurtenis-ID 18453 met trefwoorden - *klassieke* of *Audit geslaagd* en trefwoord *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Windows-gebeurtenis registreert eigenschappen
Gebeurtenisrecords Windows hebben een soort **gebeurtenis** en hebben de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--- |:--- |
| Computer |Naam van de computer waarop de gebeurtenis is verzameld. |
| Culture |De categorie van de gebeurtenis. |
| EventData |Alle gebeurtenisgegevens in een onbewerkte indeling. |
| Gebeurtenis-id |Nummer van de gebeurtenis. |
| eventLevel |Ernst van de gebeurtenis in de numerieke vorm. |
| EventLevelName |Ernst van de gebeurtenis in de tekstvorm. |
| Gebeurtenislogboek |Naam van het gebeurtenislogboek dat de gebeurtenis is verzameld. |
| ParameterXml |Gebeurtenis parameterwaarden in XML-indeling. |
| ManagementGroupName |De naam van de beheergroep voor System Center Operations Manager-agents.  Voor andere agents is deze waarde AOI-<workspace ID> |
| RenderedDescription |Beschrijving van gebeurtenis met parameterwaarden |
| Bron |De bron van de gebeurtenis. |
| SourceSystem |Type van de agent die de gebeurtenis is verzameld. <br> OpsManager – Windows-agent, ofwel direct verbinding maken met of Operations Manager worden beheerd <br> Linux – alle Linux-agents  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated |De datum en tijd waarop die de gebeurtenis is gemaakt in Windows. |
| Gebruikersnaam |Gebruikersnaam van het account waarmee de gebeurtenis is vastgelegd. |

## <a name="log-searches-with-windows-events"></a>Logboek van zoekopdrachten met Windows-gebeurtenissen
De volgende tabel bevat voorbeelden van logboek-zoekopdrachten waarmee u Windows-gebeurtenis records ophalen.

| Query’s uitvoeren | Beschrijving |
|:---|:---|
| Gebeurtenis |Alle Windows-gebeurtenissen. |
| Gebeurtenis &#124; waar EventLevelName == "error" |Alle Windows-gebeurtenissen met de ernst van de fout. |
| Gebeurtenis &#124; count() door bron samenvatten |Telling van Windows-gebeurtenissen op de bron. |
| Gebeurtenis &#124; waar EventLevelName == "error" &#124; count() door bron samenvatten |Telling van Windows-foutgebeurtenissen door bron. |


## <a name="next-steps"></a>Volgende stappen
* Configureren van logboekanalyse voor het verzamelen van andere [gegevensbronnen](log-analytics-data-sources.md) voor analyse.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.  
* Gebruik [aangepaste velden](log-analytics-custom-fields.md) parseren van de gebeurtenis legt vast in afzonderlijke velden.
* Configureer [verzameling van prestatiemeteritems](log-analytics-data-sources-performance-counters.md) van uw Windows-agents.
