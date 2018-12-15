---
title: IIS-logboeken in Azure Monitor | Microsoft Docs
description: Internet Information Services (IIS) slaat gebruikersactiviteit in logboekbestanden die kunnen worden verzameld door Azure Monitor.  In dit artikel wordt beschreven hoe u de verzameling van IIS-logboeken en de details van de records die zij in Azure Monitor maken configureren.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: bwren
ms.openlocfilehash: cd28eef249ae6b07b9e3f74b80c32a4b53370215
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436735"
---
# <a name="collect-iis-logs-in-azure-monitor"></a>Verzamelen van IIS-logboeken in Azure Monitor
Internet Information Services (IIS) slaat gebruikersactiviteit in logboekbestanden die kunnen worden verzameld door Azure Monitor en opgeslagen als [logboekgegevens](data-collection.md).

![IIS-logboeken](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Configureren van IIS-logboeken
Azure Monitor worden gegevens verzameld uit logboekbestanden gemaakt door IIS, dus u moet [IIS configureren voor logboekregistratie](https://technet.microsoft.com/library/hh831775.aspx).

Azure Monitor alleen ondersteuning biedt voor IIS-logboekbestanden opgeslagen in de W3C-indeling en biedt geen ondersteuning voor aangepaste velden of geavanceerde IIS-registratie. De functie verzamelt geen logboeken in systeemeigen NCSA- of IIS-indeling.

Configureren van IIS-logboeken in Azure Monitor uit de [geavanceerde instellingen menu](agent-data-sources.md#configuring-data-sources).  Er is geen configuratie vereist dan selecteren **verzamelen W3C-indeling IIS-logboekbestanden**.


## <a name="data-collection"></a>Gegevensverzameling
Azure Monitor verzamelt IIS-logboekvermeldingen van elke agent die telkens wanneer het logboek wordt gesloten en er wordt een nieuwe gemaakt. Deze frequentie wordt bepaald door de **schema voor logboekregistraties bestand Rollover** instellen voor de IIS-site die één keer per dag standaard is. Bijvoorbeeld, als de instellingen is **per uur**, en vervolgens Azure Monitor het logboek voor elk uur verzamelt.  Als de instelling **dagelijkse**, en vervolgens Azure Monitor het logboek elke 24 uur verzamelt.


## <a name="iis-log-record-properties"></a>Record-eigenschappen van IIS-logboek
IIS-logboekrecords zijn een type **W3CIISLog** en hebben de eigenschappen in de volgende tabel:

| Eigenschap | Description |
|:--- |:--- |
| Computer |De naam van de computer waarop de gebeurtenis is verzameld. |
| Overschrijving |IP-adres van de client. |
| csMethod |Methode van de aanvraag, zoals GET of POST. |
| csReferer |Site of de gebruiker een koppeling tussen de huidige site hebt gevolgd. |
| csUserAgent |Het type van de browser van de client. |
| csUserName |De naam van de geverifieerde gebruiker die toegang krijgen de server tot. Anonieme gebruikers worden aangeduid met een afbreekstreepje. |
| csUriStem |Het doel van de aanvraag, zoals een webpagina. |
| csUriQuery |Een query uitvoert, indien van toepassing, dat de client probeerde uit te voeren. |
| ManagementGroupName |De naam van de beheergroep van Operations Manager-agents.  Voor andere agents is dit AOI -\<werkruimte-ID\> |
| RemoteIPCountry |Land van het IP-adres van de client. |
| RemoteIPLatitude |De breedtegraad van de client-IP-adres. |
| RemoteIPLongitude |De lengtegraad van de client-IP-adres. |
| scStatus |HTTP-statuscode. |
| scSubStatus |Fout-substatuscode. |
| scWin32Status |Windows-statuscode. |
| sIP |IP-adres van de webserver. |
| SourceSystem |OpsMgr |
| sPort |De poort op de server de client die is verbonden. |
| sSiteName |De naam van de IIS-site. |
| TimeGenerated |De datum en tijdstip waarop die de vermelding is vastgelegd. |
| timeTaken |Lengte van de tijd voor het verwerken van de aanvraag in milliseconden. |

## <a name="log-queries-with-iis-logs"></a>Logboeken-query's met IIS-logboeken
De volgende tabel bevat voorbeelden van Logboeken-query's die IIS logboekrecords ophalen.

| Query’s uitvoeren | Description |
|:--- |:--- |
| W3CIISLog |Alle IIS-logboek records. |
| W3CIISLog &#124; waar scStatus 500 == |Alle IIS-logboek records met een status van het resultaat van 500. |
| W3CIISLog &#124; count() samenvatten op overschrijving |Telling van IIS-logboekvermeldingen op client-IP-adres. |
| W3CIISLog &#124; waar csHost == 'www.contoso.com' &#124; count() samenvatten op csUriStem |Telling van IIS logboekvermeldingen op URL voor de host www.contoso.com. |
| W3CIISLog &#124; sum(csBytes) per Computer samenvatten &#124; 500000 nemen |Totaal aantal bytes dat is ontvangen door elke IIS-computer. |

## <a name="next-steps"></a>Volgende stappen
* Configureer Azure Monitor voor het verzamelen van andere [gegevensbronnen](agent-data-sources.md) voor analyse.
* Meer informatie over [query's bijgehouden](../../log-analytics/log-analytics-queries.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.
