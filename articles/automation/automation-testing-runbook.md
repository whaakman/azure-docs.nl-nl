---
title: Testen van een runbook in Azure Automation | Microsoft Docs
description: Voordat u een runbook in Azure Automation publiceren, kunt u het testen om ervoor te zorgen dat werkt zoals verwacht.  In dit artikel wordt beschreven hoe een runbook testen en weergeven van de uitvoer ervan weergegeven.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 7f7db785-52c0-4613-aa12-b02fd32a5182
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;bwren
ms.openlocfilehash: 49e8dfa341940386f15932ec4346c8811effbf0b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="testing-a-runbook-in-azure-automation"></a>Testen van een runbook in Azure Automation
Wanneer u een runbook test de [conceptversie](automation-creating-importing-runbook.md#publishing-a-runbook) wordt uitgevoerd en acties die worden uitgevoerd, zijn voltooid. Er wordt geen Taakgeschiedenis is gemaakt, maar de [uitvoer](automation-runbook-output-and-messages.md#output-stream) en [waarschuwingen en fouten](automation-runbook-output-and-messages.md#message-streams) stromen worden weergegeven in de Test deelvenster Uitvoer. Berichten naar de [uitgebreide stroom](automation-runbook-output-and-messages.md#message-streams) worden weergegeven in het deelvenster Uitvoer alleen als de [variabele $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) is ingesteld om verder te gaan.

Hoewel de conceptversie wordt uitgevoerd, wordt het runbook nog steeds normaal uitgevoerd van de werkstroom en worden eventuele acties op basis van bronnen in de omgeving wordt uitgevoerd. Daarom kunt testen u runbooks alleen op niet-productiebronnen.

De procedure voor het testen van elk [type runbook](automation-runbook-types.md) is hetzelfde, en er is geen verschil tussen de teksteditor en de grafische editor in Azure portal tests.  

## <a name="to-test-a-runbook-in-the-azure-portal"></a>Een runbook te testen in de Azure portal
U kunt werken met een [runbooktype](automation-runbook-types.md) in de Azure portal.

1. Open de conceptversie van het runbook in ofwel de [teksteditor](automation-edit-textual-runbook.md) of [grafische editor](automation-graphical-authoring-intro.md).
2. Klik op de **Test** knop om de blade Test te openen.
3. Als het runbook parameters heeft, wordt deze weergegeven in het linkerdeelvenster waarin u de waarden moeten worden gebruikt voor de test kunt opgeven.
4. Als u de test uitvoert wilt op een [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), wijzigt u vervolgens **instellingen uitvoeren** naar **Hybrid Worker** en selecteer de naam van de doelgroep.  Gebruik anders de standaardwaarde **Azure** de test wilt uitvoeren in de cloud.
5. Klik op de **Start** knop om de test te starten.
6. Als het runbook is [PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks) of [grafisch](automation-runbook-types.md#graphical-runbooks), kunt u stoppen of onderbreken deze terwijl deze wordt getest met de knoppen onder het deelvenster Uitvoer. Wanneer u het runbook onderbreekt, is de huidige activiteit voltooid voordat het wordt onderbroken. Zodra het runbook wordt onderbroken, kunt u stoppen of het programma opnieuw.
7. Inspecteer de uitvoer van het runbook in het deelvenster Uitvoer.

## <a name="next-steps"></a>Volgende stappen
* Zie voor informatie over het maken of importeren van een runbook, [maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md)
* Zie voor meer informatie over grafisch ontwerpen [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie voor meer informatie over het configureren van runbooks om terug te keren statusberichten en fouten, met inbegrip van aanbevolen procedures, [Runbook uitvoer en berichten in Azure Automation](automation-runbook-output-and-messages.md)

