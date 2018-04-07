---
title: Gebruik Azure Automation verticaal schalen Windows virtuele machines | Microsoft Docs
description: Een virtuele Windows-Machine verticaal te schalen in reactie op waarschuwingen met Azure Automation bewaken
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abbbcce2ae72ac77ba6f6c21e626b34bcded7909
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Windows-VM's met Azure Automation verticaal te schalen

Verticaal schalen, is het proces van het vergroten of verkleinen van de resources van een machine in reactie op de workload. In Azure kunt dit doen door de grootte van de virtuele Machine. Dit kan helpen bij de volgende scenario 's

* Als de virtuele Machine niet vaak wordt gebruikt, kunt u het formaat naar beneden op een kleinere om uw maandelijkse kosten te verlagen
* Als de virtuele Machine een piekbelasting ziet is, het formaat kunt wijzigen in een groter formaat om de capaciteit te vergroten

Het overzicht voor de stappen om dit te bereiken is als hieronder

1. Azure Automation instellen voor toegang tot uw virtuele Machines
2. De verticale schaal van Azure Automation-runbooks importeert in uw abonnement
3. Een webhook toevoegen aan uw runbook
4. Een waarschuwing toevoegen aan uw virtuele Machine

> [!NOTE]
> Vanwege de grootte van de eerste virtuele Machine, de grootten die deze kan worden geschaald, mogelijk beperkt omdat de beschikbaarheid van de andere grootten in het cluster die in huidige virtuele Machine is geïmplementeerd. In de gepubliceerde automation-runbooks in dit artikel gebruikt we zorgt voor deze aanvraag en slechts worden opgeschaald binnen de onderstaande paren van VM-grootte. Dit betekent dat een Standard_D1v2 virtuele Machine niet plotseling worden uitgebreid naar Standard_G5 of verkleind tot Basic_A0.
> 
> | VM-grootten paar schalen |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1v2 |Standard_D5v2 |
> | Standard_D11v2 |Standard_D14v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Azure Automation instellen voor toegang tot uw virtuele Machines
Het eerste wat dat u moet doen is een Azure Automation-account die als host voor de runbooks gebruikt fungeert voor het schalen van een virtuele Machine maken. De Automation-service is onlangs geïntroduceerd de functie 'Run As-account', waardoor de instelling van de Service-Principal voor het automatisch uitvoeren van de runbooks namens de gebruiker heel eenvoudig. U kunt meer lezen over deze in het artikel hieronder:

* [Runbooks verifiëren met een Azure Uitvoeren als-account](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>De verticale schaal van Azure Automation-runbooks importeert in uw abonnement
De runbooks die nodig zijn voor uw virtuele Machine verticaal te schalen zijn al gepubliceerd in de galerie van Azure Automation Runbook. U moet ze importeren in uw abonnement. U kunt informatie over het importeren van runbooks met het volgende artikel lezen.

* [Runbook- en galerieën voor Azure Automation](../../automation/automation-runbook-gallery.md)

De runbooks die moeten worden geïmporteerd worden in de onderstaande afbeelding weergegeven

![Importeren van runbooks](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook
Zodra u hebt geïmporteerd toevoegen de runbooks u moet een webhook aan het runbook, zodat deze kan worden geactiveerd door een waarschuwing van een virtuele Machine. De details van het maken van een webhook voor uw Runbook kunnen hier worden gelezen

* [Azure Automation-webhooks.](../../automation/automation-webhooks.md)

Zorg ervoor dat u de webhook kopiëren voordat u sluit het dialoogvenster webhook want u dit in de volgende sectie moet.

## <a name="add-an-alert-to-your-virtual-machine"></a>Een waarschuwing toevoegen aan uw virtuele Machine
1. Instellingen voor virtuele Machine selecteren
2. Selecteer 'Waarschuwingsregels'
3. Selecteer 'Waarschuwing toevoegen'
4. Selecteer een waarde voor het starten van de waarschuwing in
5. Selecteer een voorwaarde die tijdens wordt voldaan ertoe leiden dat de waarschuwing moet worden gestart
6. Selecteer een drempelwaarde voor de voorwaarde in stap 5. moet worden voldaan
7. Selecteer een periode gedurende welke de bewakingsservice wordt gecontroleerd voor de voorwaarde en de drempelwaarde in stap 5 en 6
8. Plak in de webhook die u hebt gekopieerd uit de vorige sectie.

![Waarschuwing toevoegen aan de virtuele Machine 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Waarschuwing toevoegen aan de virtuele Machine 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

