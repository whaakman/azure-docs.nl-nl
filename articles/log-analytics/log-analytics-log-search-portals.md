---
title: Portals voor het maken en bewerken van logboek-query's in Azure Log Analytics | Microsoft Docs
description: In dit artikel beschrijft de portals die u kunt gebruiken in Azure-logboekanalyse maken en bewerken Meld zoekopdrachten.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: magoedte; bwren
ms.component: na
ms.openlocfilehash: e2ea0bf1fb3f1c63f4e6f037e465e8fdfd9a4374
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132929"
---
# <a name="portals-for-creating-and-editing-log-queries-in-azure-log-analytics"></a>Portals voor het maken en bewerken van logboek-query's in Azure Log Analytics

Logboek van zoekopdrachten in een veelheid aan plaatsen in Log Analytics kunt u gegevens ophalen uit de werkruimte.  Voor het daadwerkelijk maken en bewerken van query's werkt interactief met de geretourneerde gegevens niet alleen maar, hebt u twee opties die hieronder worden beschreven.  

## <a name="log-search"></a>Zoekopdrachten in logboeken 
De zoekpagina logboek is toegankelijk vanuit de Azure-portal.  Het is geschikt voor het maken van eenvoudige query's die kunnen worden gemaakt op één regel.  Logboek zoeken kan worden gebruikt zonder het starten van een externe portal en kunt u het uitvoeren van een breed scala aan functies met logboek zoekopdrachten inclusief maken van regels voor waarschuwingen, computergroepen maken en exporteren van de resultaten van de query.  

Logboek Search biedt meerdere functies voor het bewerken van de query zonder een volledige kennis van de querytaal.  U krijgt een overzicht van deze functies in [maken logboek zoekopdrachten in Azure-logboekanalyse logboek zoekactie](log-analytics-log-search-log-search-portal.md).


![Meld u zoekpagina](media/log-analytics-log-search-portals/log-search-portal.png)

## <a name="advanced-analytics-portal"></a>Geavanceerde Analytics-portal
De geavanceerde analyses portal is een speciale portal geavanceerde functionaliteit niet beschikbaar in het logboek zoeken vanuit de Azure-portal.  Voorbeelden van functies zijn de mogelijkheid om een query van meerdere regels te bewerken, code selectief uit te voeren, contextafhankelijke Intellisense en slimme analyse.  De portal Advanced Analytics is het meest geschikt is voor het ontwerpen van complexe query's die zijn ofwel opgeslagen als een zoekopdracht logboek of gekopieerd en geplakt in andere Log Analytics-elementen.  U start de Advanced Analytics-portal met een koppeling op de pagina logboek zoeken.

![Geavanceerde Analytics-portal](media/log-analytics-log-search-portals/advanced-analytics-portal.png)


Vanwege de geavanceerde functies, meestal gebruikt u de portal geavanceerde analyses als uw primaire hulpprogramma voor het maken en bewerken van query's.  Eenmaal hebt u vastgesteld dat de query werkt zoals verwacht en u zult Kopieer en plak deze ergens anders zoals logboek zoekpagina of Designer bekijken.  

### <a name="firewall-requirements"></a>Firewallvereisten
Uw browser vereist toegang tot de volgende adressen toegang tot de Advanced Analytics-portal.  Als uw browser de Azure-portal via een firewall benaderen is, moet u toegang tot deze adressen inschakelen.

| URI | IP | Poorten |
|:---|:---|:---|
| portal.loganalytics.io | Dynamisch | 80,443 |
| api.loganalytics.io    | Dynamisch | 80,443 |
| docs.loganalytics.io   | Dynamisch | 80,443 |


## <a name="next-steps"></a>Volgende stappen

- Helpt u bij een zelfstudie over het gebruik van [logboek zoeken](log-analytics-tutorial-viewdata.md) voor informatie over het gebruik van de query language-query's maken
- Bekijk de [Advanced Analytics-portal](https://go.microsoft.com/fwlink/?linkid=856587) geavanceerde query's maken en gebruiken als een ontwikkelomgeving voor uw zoekacties logboek.

