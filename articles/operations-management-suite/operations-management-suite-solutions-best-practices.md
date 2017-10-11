---
title: Aanbevolen procedures voor OMSManagement oplossing | Microsoft Docs
description: 
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: b3d07ad3164609a5628c0d9805de55a32870ab94
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="best-practices-for-creating-management-solutions-in-operations-management-suite-oms-preview"></a>Aanbevolen procedures voor het maken van oplossingen voor het beheer in Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Dit is voorlopige documentatie voor het maken van oplossingen voor het beheer in OMS die zich momenteel in preview. De hieronder beschreven schema kan worden gewijzigd.  

Dit artikel vindt u aanbevolen procedures voor [maken van een bestand van de oplossing management](operations-management-suite-solutions-solution-file.md) in Operations Management Suite (OMS).  Deze informatie wordt bijgewerkt extra aanbevelingen zijn aangeduid.

## <a name="data-sources"></a>Gegevensbronnen
- Gegevensbronnen kunnen worden [geconfigureerd met een Resource Manager-sjabloon](../log-analytics/log-analytics-template-workspace-configuration.md), maar ze moeten niet worden opgenomen in een oplossingsbestand.  De reden is dat gegevensbronnen configureren momenteel niet idempotent is, wat betekent dat uw oplossing bestaande configuratie in de werkruimte van de gebruiker kan overschreven.<br><br>Uw oplossing moet mogelijk waarschuwings- en gebeurtenissen van het logboek voor toepassingsgebeurtenissen.  Als u dit als een gegevensbron in uw oplossing opgeeft, kunnen gegevens gebeurtenissen wordt verwijderd als de gebruiker dit niet is geconfigureerd in de werkruimte heeft.  Als u alle gebeurtenissen worden opgenomen, klikt u mogelijk worden overmatige informatie en gebeurtenissen verzamelen in de werkruimte van de gebruiker.

- Als uw oplossing vereist dat gegevens uit een van de standaard gegevensbronnen, moet vervolgens u definiëren dit als een vereiste.  Status in de documentatie of de klant de gegevensbron op hun eigen configureren moet.  
- Voeg een [stromen gegevensverificatie](../log-analytics/log-analytics-view-designer-tiles.md) bericht voor weergaven in uw oplossing in zodat de gebruiker op de gegevensbronnen die worden geconfigureerd voor de vereiste gegevens moeten te verzamelen.  Dit bericht wordt weergegeven op de tegel van de weergave wanneer de vereiste gegevens is niet gevonden.


## <a name="runbooks"></a>Runbooks
- Voeg een [Automation planning](../automation/automation-schedules.md) voor elk runbook in uw oplossing die moet worden uitgevoerd volgens een schema.
- Bevatten de [IngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) in uw oplossing moet worden gebruikt door runbooks schrijven van gegevens naar de opslagplaats logboekanalyse.  De oplossing configureren [verwijzing](operations-management-suite-solutions-solution-file.md#solution-resource) deze bron zodanig dat deze blijft als de oplossing is verwijderd.  Hierdoor kunnen meerdere oplossingen voor het delen van de module.
- Gebruik [Automation-variabelen](../automation/automation-schedules.md) naar waarden voor de oplossing die gebruikers kunnen later wilt wijzigen.  Zelfs als de oplossing is geconfigureerd voor de variabele bevatten, kan de waarde nog worden gewijzigd.

## <a name="views"></a>Weergaven
- Alle oplossingen moeten één weergave die wordt weergegeven in de gebruikersportal bevatten.  De weergave kan bevatten meerdere [visualisatie delen](../log-analytics/log-analytics-view-designer-parts.md) ter illustratie van verschillende sets van gegevens.
- Voeg een [stromen gegevensverificatie](../log-analytics/log-analytics-view-designer-tiles.md) bericht voor weergaven in uw oplossing in zodat de gebruiker op de gegevensbronnen die worden geconfigureerd voor de vereiste gegevens moeten te verzamelen.
- De oplossing configureren [bevatten](operations-management-suite-solutions-solution-file.md#solution-resource) de weergave zodanig dat deze de verwijderd als de oplossing wordt verwijderd.

## <a name="alerts"></a>Waarschuwingen
- De Ontvangerslijst te definiëren als een parameter in het oplossingsbestand zodat de gebruiker ze definiëren kunt bij de installatie van de oplossing.
- De oplossing configureren [verwijzing](operations-management-suite-solutions-solution-file.md#solution-resource) waarschuwing regels zodat van die gebruiker hun configuratie kunt wijzigen.  Ze willen aanbrengen, zoals het wijzigen van de adreslijst, te wijzigen van de drempelwaarde van de waarschuwing of uitschakelen van de waarschuwingsregel. 


## <a name="next-steps"></a>Volgende stappen
* Het basisproces voor doorlopen [ontwerpen en bouwen van een beheeroplossing](operations-management-suite-solutions-creating.md).
* Meer informatie over hoe [maken van een oplossingsbestand](operations-management-suite-solutions-solution-file.md).
* [Opgeslagen zoekopdrachten en waarschuwingen toevoegen](operations-management-suite-solutions-resources-searches-alerts.md) bij uw systeem.
* [Weergaven toevoegen](operations-management-suite-solutions-resources-views.md) bij uw systeem.
* [Automation-runbooks en andere resources toevoegen](operations-management-suite-solutions-resources-automation.md) bij uw systeem.

