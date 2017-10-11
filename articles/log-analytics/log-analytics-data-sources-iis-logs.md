---
title: IIS-logboeken in Log Analytics | Microsoft Docs
description: Internet Information Services (IIS) slaat gebruikersactiviteit in de logboekbestanden die kunnen worden verzameld door logboekanalyse.  Dit artikel wordt beschreven voor het configureren van de verzameling van IIS-logboeken en details van de records die ze in de OMS-opslagplaats maken.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: cec5ff0a-01f5-4262-b2e8-e3db7b7467d2
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/12/2017
ms.author: bwren
ms.openlocfilehash: 2114bdafb3b9fe2eb0632271840b8b70a76d10f1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="iis-logs-in-log-analytics"></a>IIS-logboeken in Log Analytics
Internet Information Services (IIS) slaat gebruikersactiviteit in de logboekbestanden die kunnen worden verzameld door logboekanalyse.  

![IIS-logboeken](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Configureren van IIS-logboeken
Log Analytics worden gegevens verzameld uit logboekbestanden gemaakt door IIS, dus u moet [IIS configureren voor logboekregistratie](https://technet.microsoft.com/library/hh831775.aspx).

Log Analytics IIS-logboekbestanden opgeslagen in de W3C-indeling ondersteunt alleen en biedt geen ondersteuning voor aangepaste velden of geavanceerde IIS-registratie.  
Log Analytics verzamelt geen logboeken in systeemeigen NCSA of IIS-indeling.

Configureren van IIS-logboeken in logboekanalyse uit de [menu Data in logboekanalyse-instellingen](log-analytics-data-sources.md#configuring-data-sources).  Er is geen configuratie vereist dan selecteren **verzamelen W3C-indeling IIS-logboekbestanden**.

U wordt aangeraden wanneer u IIS-logboekverzameling inschakelt, u de IIS-logboek rollover-instelling op elke server configureren moet.

## <a name="data-collection"></a>Gegevensverzameling
Log Analytics verzamelt IIS logboekvermeldingen van elke bron verbonden ongeveer elke 15 minuten.  De agent registreert plaats daarvan in elk logboek die worden verzameld uit.  Als de agent offline gaat, klikt u vervolgens verzamelt Log Analytics gebeurtenissen van waar het laatst werd afgebroken, zelfs als de gebeurtenissen die zijn gemaakt terwijl de agent offline was.

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
| timeTaken |Tijdsduur voor de verwerking van de aanvraag in milliseconden. |

## <a name="log-searches-with-iis-logs"></a>Logboek van zoekopdrachten met IIS-logboeken
De volgende tabel bevat voorbeelden van logboek-query's die IIS-logboekrecords ophalen.

| Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| Type W3CIISLog = |Alle records voor IIS-logboek. |
| Type = W3CIISLog scStatus = 500 |Alle IIS-logboekrecords met een status van het resultaat van 500. |
| Type = W3CIISLog &#124; Meting count() door overschrijving |Telling van IIS logboekvermeldingen op client-IP-adres. |
| Type = W3CIISLog csHost = 'www.contoso.com' &#124; Meting count() door csUriStem |Telling van IIS logboekvermeldingen met URL voor de host www.contoso.com. |
| Type = W3CIISLog &#124; Meting Sum(csBytes) door Computer &#124; Top 500000 |Totaal aantal bytes dat is ontvangen door elke IIS-computer. |

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe querytaal van Log Analytics](log-analytics-log-search-upgrade.md), worden bovenstaande query's gewijzigd in het volgende.

> | Query’s uitvoeren | Beschrijving |
|:--- |:--- |
| W3CIISLog |Alle records voor IIS-logboek. |
| W3CIISLog &#124; waar scStatus == 500 |Alle IIS-logboekrecords met een status van het resultaat van 500. |
| W3CIISLog &#124; count() overschrijving samenvatten |Telling van IIS logboekvermeldingen op client-IP-adres. |
| W3CIISLog &#124; waar csHost == 'www.contoso.com' &#124; count() csUriStem samenvatten |Telling van IIS logboekvermeldingen met URL voor de host www.contoso.com. |
| W3CIISLog &#124; overzicht van sum(csBytes) door Computer &#124; 500000 duren |Totaal aantal bytes dat is ontvangen door elke IIS-computer. |

## <a name="next-steps"></a>Volgende stappen
* Configureren van logboekanalyse voor het verzamelen van andere [gegevensbronnen](log-analytics-data-sources.md) voor analyse.
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren.
* Waarschuwingen configureren in logboekanalyse proactief om u te waarschuwen van belangrijke voorwaarden gevonden in de IIS-logboeken.
