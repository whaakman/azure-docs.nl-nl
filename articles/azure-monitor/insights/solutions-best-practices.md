---
title: Management-oplossing in best practices voor Azure | Microsoft Docs
description: ''
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: d6d2414935bb5d1f095ad2b200acafa97b3b9b32
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53192687"
---
# <a name="best-practices-for-creating-management-solutions-in-azure-preview"></a>Aanbevolen procedures voor het maken van oplossingen in Azure (Preview)
> [!NOTE]
> Dit is een voorlopige documentatie voor het maken van oplossingen in Azure die zich momenteel in preview. Er is geen schema die hieronder worden beschreven kan worden gewijzigd.  

In dit artikel bevat aanbevolen procedures voor [het maken van een oplossingsbestand management](solutions-solution-file.md) in Azure.  Deze informatie wordt bijgewerkt als aanvullende aanbevolen procedures worden geïdentificeerd.

## <a name="data-sources"></a>Gegevensbronnen
- Gegevensbronnen kunnen worden [geconfigureerd met een Resource Manager-sjabloon](../../azure-monitor/platform/template-workspace-configuration.md), maar ze moeten niet worden opgenomen in een oplossingsbestand.  De reden is dat het configureren van gegevensbronnen momenteel niet idempotent zijn is, wat betekent dat uw oplossing bestaande configuratie in de werkruimte van de gebruiker kan overschrijven.<br><br>Uw oplossing is bijvoorbeeld mogelijk waarschuwings- en gebeurtenissen van het logboek voor toepassingsgebeurtenissen.  Als u dit als een gegevensbron in uw oplossing opgeeft, riskeert u informatiegebeurtenissen wordt verwijderd als de gebruiker dit in orde in hun werkruimte heeft.  Als u alle gebeurtenissen opgenomen, kan klikt u vervolgens u worden verzameld overmatige informatiegebeurtenissen in de werkruimte van de gebruiker.

- Als uw oplossing vereist dat gegevens uit een van de standard-gegevensbronnen, moet klikt u vervolgens u definiëren dit als een vereiste.  Status in de documentatie bij de klant moet de gegevensbron configureren op hun eigen.  
- Voeg een [Flow gegevensverificatie](../../azure-monitor/platform/view-designer-tiles.md) bericht voor weergaven in uw oplossing om te geven van de gebruiker op gegevensbronnen die worden geconfigureerd voor de vereiste gegevens moeten te verzamelen.  Dit bericht wordt weergegeven op de tegel van de weergave als vereiste gegevens is niet gevonden.


## <a name="runbooks"></a>Runbooks
- Voeg een [Automation planning](../../automation/automation-schedules.md) voor elk runbook in de oplossing die moet worden uitgevoerd volgens een schema.
- Bevatten de [IngestionAPI module](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) in uw oplossing moet worden gebruikt door runbooks schrijven van gegevens naar de opslagplaats van Log Analytics.  Configureren van de oplossing [verwijzing](solutions-solution-file.md#solution-resource) deze resource zo dat deze blijven de oplossing wordt verwijderd.  Hiermee kunt meerdere oplossingen voor het delen van de module.
- Gebruik [Automation-variabelen](../../automation/automation-schedules.md) waarden aan de oplossing die gebruikers kunnen later wilt wijzigen op te geven.  Zelfs als de oplossing is geconfigureerd voor de variabele bevatten, kan de waarde nog steeds worden gewijzigd.

## <a name="views"></a>Weergaven
- Alle oplossingen moeten één weergave die wordt weergegeven in de portal van de gebruiker bevatten.  De weergave kan bevatten meerdere [visualisatie delen](../../azure-monitor/platform/view-designer-parts.md) ter illustratie van verschillende gegevenssets.
- Voeg een [Flow gegevensverificatie](../../azure-monitor/platform/view-designer-tiles.md) bericht voor weergaven in uw oplossing om te geven van de gebruiker op gegevensbronnen die worden geconfigureerd voor de vereiste gegevens moeten te verzamelen.
- Configureren van de oplossing [bevatten](solutions-solution-file.md#solution-resource) de weergave, zodat deze de verwijderd als de oplossing wordt verwijderd.

## <a name="alerts"></a>Waarschuwingen
- De lijst met geadresseerden als een parameter in het oplossingsbestand definiëren, zodat de gebruiker ze definiëren wanneer ze de oplossing installeren.
- Configureren van de oplossing [verwijzing](solutions-solution-file.md#solution-resource) waarschuwingsregels zodat van die gebruiker hun configuratie kunt wijzigen.  Ze willen aanbrengen van wijzigingen, zoals het wijzigen van de lijst met geadresseerden, te wijzigen van de drempelwaarde van de waarschuwing of uitschakelen van de waarschuwingsregel. 


## <a name="next-steps"></a>Volgende stappen
* Doorloop het basisproces [ontwerpen en bouwen van een oplossing voor](solutions-creating.md).
* Meer informatie over het [maken van een oplossingsbestand](solutions-solution-file.md).
* [Opgeslagen zoekopdrachten en waarschuwingen toevoegen](solutions-resources-searches-alerts.md) aan uw oplossing.
* [Weergaven toevoegen](solutions-resources-views.md) aan uw oplossing.
* [Automation-runbooks en andere resources toevoegen](solutions-resources-automation.md) aan uw oplossing.

