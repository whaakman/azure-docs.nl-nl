---
title: IIS-logboeken in Azure Log Analytics | Microsoft Docs
description: Internet Information Services (IIS) slaat gebruikersactiviteit in logboekbestanden die kunnen worden verzameld door Log Analytics.  In dit artikel wordt beschreven hoe het configureren van de verzameling van IIS-logboeken en de details van de records die ze in de Log Analytics-werkruimte maken.
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
ms.date: 06/12/2018
ms.author: bwren
ms.openlocfilehash: 42509f98126ebb489fe17c58e6d58b37d7451cf0
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53189069"
---
# <a name="iis-logs-in-log-analytics"></a>IIS-logboeken in Log Analytics
Internet Information Services (IIS) slaat gebruikersactiviteit in logboekbestanden die kunnen worden verzameld door Log Analytics.  

![IIS-logboeken](media/data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Configureren van IIS-logboeken
Log Analytics verzamelt gegevens uit logboekbestanden gemaakt door IIS, dus u moet [IIS configureren voor logboekregistratie](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics wordt alleen ondersteuning biedt voor IIS-logboekbestanden opgeslagen in de W3C-indeling en biedt geen ondersteuning voor aangepaste velden of IIS-geavanceerde registratie.  
Log Analytics verzamelt geen logboeken in systeemeigen NCSA- of IIS-indeling.

Configureren van IIS-logboeken in Log Analytics van de [gegevens in het menu in-logboekanalyse-instellingen](agent-data-sources.md#configuring-data-sources).  Er is geen configuratie vereist dan selecteren **verzamelen W3C-indeling IIS-logboekbestanden**.


## <a name="data-collection"></a>Gegevensverzameling
Log Analytics verzamelt IIS-logboekvermeldingen van elke agent die telkens wanneer het logboek wordt gesloten en er wordt een nieuwe gemaakt. Deze frequentie wordt bepaald door de **schema voor logboekregistraties bestand Rollover** instellen voor de IIS-site die één keer per dag standaard is. Bijvoorbeeld, als de instellingen is **per uur**, en vervolgens de Log Analytics verzamelt het logboek voor elk uur.  Als de instelling **dagelijkse**, en vervolgens de Log Analytics verzamelt het logboek elke 24 uur.


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

## <a name="log-searches-with-iis-logs"></a>Zoekopdrachten in Logboeken met IIS-logboeken
De volgende tabel bevat voorbeelden van Logboeken-query's die IIS logboekrecords ophalen.

| Query’s uitvoeren | Description |
|:--- |:--- |
| W3CIISLog |Alle IIS-logboek records. |
| W3CIISLog &#124; waar scStatus 500 == |Alle IIS-logboek records met een status van het resultaat van 500. |
| W3CIISLog &#124; count() samenvatten op overschrijving |Telling van IIS-logboekvermeldingen op client-IP-adres. |
| W3CIISLog &#124; waar csHost == 'www.contoso.com' &#124; count() samenvatten op csUriStem |Telling van IIS logboekvermeldingen op URL voor de host www.contoso.com. |
| W3CIISLog &#124; sum(csBytes) per Computer samenvatten &#124; 500000 nemen |Totaal aantal bytes dat is ontvangen door elke IIS-computer. |

## <a name="next-steps"></a>Volgende stappen
* Log Analytics voor het verzamelen van andere configureren [gegevensbronnen](agent-data-sources.md) voor analyse.
* Meer informatie over [zoekopdrachten](../../azure-monitor/log-query/log-query-overview.md) om de gegevens die worden verzameld van gegevensbronnen en oplossingen te analyseren.
* Waarschuwingen configureren in Log Analytics om proactief te waarschuwen u van belang voorwaarden gevonden in de IIS-logboeken.
