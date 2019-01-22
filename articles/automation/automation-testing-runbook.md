---
title: Een runbook testen in Azure Automation
description: Voordat u een runbook in Azure Automation publiceren, kunt u testen om ervoor te zorgen dat werkt zoals verwacht.  In dit artikel wordt beschreven hoe u een runbook testen en de uitvoer weer te geven.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 95e3f4426fab8ed3ff28877607dee8694962e79f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54422468"
---
# <a name="testing-a-runbook-in-azure-automation"></a>Een runbook testen in Azure Automation
Wanneer u een runbook test de [conceptversie](automation-creating-importing-runbook.md#publishing-a-runbook) wordt uitgevoerd en de acties die worden uitgevoerd zijn voltooid. Er wordt geen Taakgeschiedenis wordt gemaakt, maar de [uitvoer](automation-runbook-output-and-messages.md#output-stream) en [waarschuwings- en](automation-runbook-output-and-messages.md#message-streams) stromen worden weergegeven in de Test deelvenster Uitvoer. Berichten naar de [uitgebreide Stream](automation-runbook-output-and-messages.md#message-streams) worden weergegeven in het deelvenster Uitvoer alleen als de [variabele $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) is ingesteld om verder te gaan.

Hoewel de conceptversie wordt uitgevoerd, wordt het runbook nog steeds normaal uitvoeren van de werkstroom en worden eventuele acties op basis van resources in de omgeving wordt uitgevoerd. Daarom moet u alleen runbooks op niet-productiebronnen testen.

De procedure voor het testen van elke [type runbook](automation-runbook-types.md) is hetzelfde, en er is geen verschil in de testfase tussen de teksteditor en de grafische editor in Azure portal.  

## <a name="to-test-a-runbook-in-the-azure-portal"></a>Een runbook testen in Azure portal
U kunt werken met een [runbooktype](automation-runbook-types.md) in Azure portal.

1. De conceptversie van het runbook te openen in een de [teksteditor](automation-edit-textual-runbook.md) of [grafische editor](automation-graphical-authoring-intro.md).
2. Klik op de **Test** knop om de blade Test te openen.
3. Als het runbook parameters heeft, wordt deze weergegeven in het linkerdeelvenster waarin u de waarden die moeten worden gebruikt voor de test kunt opgeven.
4. Als u de test uitvoeren wilt op een [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md), wijzig de **instellingen uitvoeren** naar **Hybrid Worker** en selecteer de naam van de doelgroep.  Anders de standaardwaarde **Azure** om uit te voeren van de test in de cloud.
5. Klik op de **Start** knop om de test te starten.
6. Als het runbook is [PowerShell-werkstroom](automation-runbook-types.md#powershell-workflow-runbooks) of [grafisch](automation-runbook-types.md#graphical-runbooks), kunt u stoppen of deze onderbreken terwijl deze wordt getest met de knoppen onder het deelvenster Uitvoer. Wanneer u het runbook onderbreekt, wordt de huidige activiteit voltooid voordat het wordt onderbroken. Zodra het runbook wordt onderbroken, kunt u voorkomen dat deze of het programma opnieuw.
7. Inspecteer de uitvoer van het runbook in het deelvenster Uitvoer.

## <a name="next-steps"></a>Volgende stappen
* Zie voor informatie over het maken of importeren van een runbook, [maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md)
* Zie voor meer informatie over grafisch ontwerpen [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie voor meer informatie over het configureren van runbooks om te retourneren van statusberichten en fouten, met inbegrip van aanbevolen procedures, [Runbook-uitvoer en berichten in Azure Automation](automation-runbook-output-and-messages.md)


