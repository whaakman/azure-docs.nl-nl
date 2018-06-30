---
title: Geregistreerd in Azure Log Analytics | Microsoft Docs
description: Internet Information Services (IIS) slaat gebruikersactiviteit in de logboekbestanden die kunnen worden verzameld door logboekanalyse.  In dit artikel wordt beschreven hoe verzameling van IIS-logboeken en details van de records die ze in de werkruimte voor logboekanalyse maken configureren.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: bwren
ms.comopnent: na
ms.openlocfilehash: 65320e7d3cc97a3d53fd1a00fbbeab5559c02fce
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132953"
---
# <a name="iis-logs-in-log-analytics"></a>IIS-logboeken in Log Analytics
Internet Information Services (IIS) slaat gebruikersactiviteit in de logboekbestanden die kunnen worden verzameld door logboekanalyse.  

![IIS-logboeken](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Configureren van IIS-logboeken
Log Analytics worden gegevens verzameld uit logboekbestanden gemaakt door IIS, dus u moet [IIS configureren voor logboekregistratie](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics IIS-logboekbestanden opgeslagen in de W3C-indeling ondersteunt alleen en biedt geen ondersteuning voor aangepaste velden of geavanceerde IIS-registratie.  
Log Analytics verzamelt geen logboeken in systeemeigen NCSA of IIS-indeling.

Configureren van IIS-logboeken in logboekanalyse uit de [menu Data in logboekanalyse-instellingen](log-analytics-data-sources.md#configuring-data-sources).  Er is geen configuratie vereist dan selecteren **verzamelen W3C-indeling IIS-logboekbestanden**.


## <a name="data-collection"></a>Gegevensverzameling
Log Analytics verzamelt IIS logboekvermeldingen van elke agent telkens wanneer het logboek wordt gesloten en een nieuw wordt gemaakt. Deze frequentie wordt bepaald door de **logboekregistraties bestand Rollover** instellen voor de IIS-site die één keer per dag standaard is. Bijvoorbeeld, als de instellingen is **per uur**, en vervolgens Log Analytics het logboek voor elk uur verzamelt.  Als de instelling **dagelijkse**, en vervolgens Log Analytics het logboek elke 24 uur verzamelt.


## <a name="iis-log-record-properties"></a>Eigenschappen van IIS-logboek record
IIS-logboekrecords hebben een soort **W3CIISLog** en hebben de eigenschappen in de volgende tabel:

| Eigenschap | Beschrijving |
|:--- |:--- |
| Computer |Naam van de computer waarop de gebeurtenis is verzameld. |
| Overschrijving |IP-adres van de client. |
| csMethod |De methode van de aanvraag, zoals GET of POST. |
| csReferer |Site dat de gebruiker een koppeling tussen de huidige site gevolgd. |
| csUserAgent |Het type van de browser van de client. |
| csUserName |Naam van de geverifieerde gebruiker die toegang de server tot. Anonieme gebruikers worden aangeduid met een koppelteken. |
| csUriStem |Doel van de aanvraag is, zoals een webpagina. |
| csUriQuery |Query uitvoeren op, indien van toepassing, de client probeerde uit te voeren. |
| ManagementGroupName |De naam van de beheergroep van Operations Manager-agents.  Dit is voor andere agents AOI -\<werkruimte-ID\> |
| RemoteIPCountry |Land van het IP-adres van de client. |
| RemoteIPLatitude |De breedte van de IP-adres van de client. |
| RemoteIPLongitude |Lengtegraad van IP-adres van de client. |
| scStatus |HTTP-statuscode. |
| scSubStatus |Fout-substatuscode geregistreerd. |
| scWin32Status |Windows-statuscode geregistreerd. |
| sIP |IP-adres van de webserver. |
| SourceSystem |OpsMgr |
| sPort |Poort op de server van de client is verbonden met. |
| sSiteName |Naam van de IIS-site. |
| TimeGenerated |De datum en tijd die de vermelding is geregistreerd. |
| TimeTaken |Tijdsduur voor de verwerking van de aanvraag in milliseconden. |

## <a name="log-searches-with-iis-logs"></a>Logboek van zoekopdrachten met IIS-logboeken
De volgende tabel bevat voorbeelden van logboek-query's die IIS-logboekrecords ophalen.

| Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| W3CIISLog |Alle records voor IIS-logboek. |
| W3CIISLog &#124; waar scStatus == 500 |Alle IIS-logboekrecords met een status van het resultaat van 500. |
| W3CIISLog &#124; count() overschrijving samenvatten |Telling van IIS logboekvermeldingen op client-IP-adres. |
| W3CIISLog &#124; waar csHost == 'www.contoso.com' &#124; count() csUriStem samenvatten |Telling van IIS logboekvermeldingen met URL voor de host www.contoso.com. |
| W3CIISLog &#124; sum(csBytes) door Computer samenvatten &#124; 500000 duren |Totaal aantal bytes dat is ontvangen door elke IIS-computer. |

## <a name="next-steps"></a>Volgende stappen
* Configureren van logboekanalyse voor het verzamelen van andere [gegevensbronnen](log-analytics-data-sources.md) voor analyse.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.
* Waarschuwingen configureren in logboekanalyse proactief om u te waarschuwen van belangrijke voorwaarden gevonden in de IIS-logboeken.
