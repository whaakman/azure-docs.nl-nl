---
title: Verzamelen en analyseren van Windows-gebeurtenislogboeken in Azure Log Analytics | Microsoft Docs
description: Windows-gebeurtenislogboeken zijn een van de meest voorkomende gegevensbronnen die worden gebruikt door Log Analytics.  In dit artikel wordt beschreven hoe het configureren van de verzameling van Windows-gebeurtenislogboeken en details van de records die ze in de Log Analytics-werkruimte maken.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 4275b734a30310c896c397a5ef9cc1b218d89476
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842540"
---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windows-gebeurtenislogboek gegevensbronnen in Log Analytics
Windows-gebeurtenislogboeken zijn een van de meest voorkomende [gegevensbronnen](agent-data-sources.md) voor het verzamelen van gegevens met behulp van Windows-agents, omdat veel toepassingen naar het Windows-gebeurtenislogboek schrijven.  U kunt gebeurtenissen verzamelen van standard logboeken zoals systeem- en naast het opgeven van een aangepaste logboeken die zijn gemaakt door toepassingen die u nodig hebt om te controleren.

![Windows-gebeurtenissen](media/data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Configuratie Windows gebeurtenislogboeken
Configureren van Windows-gebeurtenislogboeken vanuit de [gegevens in het menu in-logboekanalyse-instellingen](agent-data-sources.md#configuring-data-sources).

Log Analytics verzamelt alleen gebeurtenissen uit de Windows-gebeurtenislogboeken die zijn opgegeven in de instellingen.  U kunt een gebeurtenislogboek toevoegen door te klikken en naam van het logboek typen **+**.  Voor elk logboek, worden alleen de gebeurtenissen met de geselecteerde ernstcategorieën verzameld.  Raadpleeg de ernstcategorieën voor het logboek dat u wenst te verzamelen.  U kunt aanvullende criteria om te filteren van gebeurtenissen kan niet opgeven.

Terwijl u de naam van een gebeurtenislogboek typt, biedt Log Analytics suggesties van algemene namen van het gebeurtenislogboek. Als het logboek dat u wilt toevoegen niet wordt weergegeven in de lijst, kunt u deze nog steeds toevoegen door in de volledige naam van het logboek te typen. U vindt de volledige naam van het logboek met behulp van Logboeken. Open in de logboeken de *eigenschappen* pagina voor het logboek en kopieer de tekenreeks van de *volledige naam* veld.

![Windows-gebeurtenissen configureren](media/data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Gegevensverzameling
Log Analytics verzamelt elke gebeurtenis die overeenkomt met een geselecteerde ernst van een bewaakte gebeurtenislogboek als de gebeurtenis wordt gemaakt.  De agent registreert plaats daarvan in elk logboek voor systeemgebeurtenissen die het verzamelt uit.  Als de agent voor een bepaalde periode offline gaat, klikt u vervolgens verzamelt Log Analytics gebeurtenissen van waar het laatste afgebroken, zelfs als de gebeurtenissen die zijn gemaakt terwijl de agent offline was.  Er is het mogelijk om deze gebeurtenissen niet worden verzameld als het gebeurtenislogboek wordt verpakt met niet-geïnde gebeurtenissen wordt overschreven terwijl de agent offline is.

>[!NOTE]
>Log Analytics verzamelt geen controlegebeurtenissen die zijn gemaakt door SQL Server vanuit de bron *MSSQLSERVER* met gebeurtenis-ID 18453 met trefwoorden - *klassieke* of *controle geslaagd* en sleutelwoord *0xa0000000000000*.
>

## <a name="windows-event-records-properties"></a>Windows-gebeurtenis registreert eigenschappen
Windows-gebeurtenis legt vast zijn een type **gebeurtenis** en hebben de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--- |:--- |
| Computer |De naam van de computer waarop de gebeurtenis is verzameld. |
| Culture |Categorie van de gebeurtenis. |
| EventData |Alle gebeurtenisgegevens in een onbewerkte indeling. |
| gebeurtenis-id |Nummer van de gebeurtenis. |
| eventLevel |Ernst van de gebeurtenis in numerieke vorm. |
| EventLevelName |Ernst van de gebeurtenis in de tekstvorm. |
| Gebeurtenislogboek |De naam van het gebeurtenislogboek dat de gebeurtenis is verzameld. |
| ParameterXml |Gebeurtenis parameterwaarden in XML-indeling. |
| ManagementGroupName |Naam van de beheergroep van System Center Operations Manager-agents.  Voor andere agents is deze waarde AOI-<workspace ID> |
| RenderedDescription |Beschrijving van gebeurtenis met parameterwaarden |
| Bron |Bron van de gebeurtenis. |
| SourceSystem |Het type van de agent die de gebeurtenis is verzameld. <br> OpsManager – Windows-agent, rechtstreeks verbinding maken of Operations Manager worden beheerd <br> Linux: alle Linux-agents  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated |De datum en tijd waarop die de gebeurtenis is gemaakt in Windows. |
| Gebruikersnaam |De naam van de gebruiker van het account waarmee de gebeurtenis is vastgelegd. |

## <a name="log-searches-with-windows-events"></a>Zoekopdrachten in Logboeken met Windows-gebeurtenissen
De volgende tabel bevat voorbeelden van zoekopdrachten die Windows-gebeurtenis legt vast ophalen.

| Query’s uitvoeren | Beschrijving |
|:---|:---|
| Gebeurtenis |Alle Windows-gebeurtenissen. |
| Gebeurtenis &#124; waar EventLevelName == "error" |Alle Windows-gebeurtenissen met de ernst van fout. |
| Gebeurtenis &#124; count() by Source samenvatten |Aantal van de Windows-gebeurtenissen op bron. |
| Gebeurtenis &#124; waar EventLevelName == "error" &#124; count() by Source samenvatten |Aantal van de Windows-foutgebeurtenissen door bron. |


## <a name="next-steps"></a>Volgende stappen
* Log Analytics voor het verzamelen van andere configureren [gegevensbronnen](agent-data-sources.md) voor analyse.
* Meer informatie over [zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.  
* Gebruik [aangepaste velden](../../log-analytics/log-analytics-custom-fields.md) parseren van de gebeurtenis legt vast in afzonderlijke velden.
* Configureer [verzamelen van prestatiemeteritems](data-sources-performance-counters.md) van uw Windows-agents.
