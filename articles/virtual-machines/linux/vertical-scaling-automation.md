---
title: Verticaal schalen van Azure-machine met Azure Automation | Microsoft Docs
description: Verticaal schalen van een virtuele Linux-Machine in reactie op waarschuwingen met Azure Automation
services: virtual-machines-linux
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: dcee199e-fa25-44d5-9b25-df564cee9b45
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/29/2016
ms.author: singhkay
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d58a9df519ea886372258dd0c7b012df8d8d3ae
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58579390"
---
# <a name="vertically-scale-azure-linux-virtual-machine-with-azure-automation"></a>Verticaal schalen Azure Linux-machine met Azure Automation
Verticaal schalen is het proces van vergroten of verkleinen van de resources van een virtuele machine in reactie op de workload. In Azure kunt dit doen door de grootte van de virtuele Machine te wijzigen. Dit kan helpen in de volgende scenario 's

* Als de virtuele Machine is niet vaak worden gebruikt, kunt u het formaat tot een kleinere om uw maandelijkse kosten te verlagen
* Als de virtuele Machine een piekbelasting ziet is, kan het worden gewijzigd in een groter formaat om de capaciteit te vergroten

Het overzicht voor de stappen om dit te doen is als hieronder

1. Azure Automation voor toegang tot uw virtuele Machines instellen
2. De verticale schaalbaarheid van Azure Automation-runbooks in uw abonnement importeren
3. Een webhook toevoegen aan uw runbook
4. Een waarschuwing toevoegen aan uw virtuele Machine

> [!NOTE]
> Vanwege de grootte van de eerste virtuele Machine, de grootte die het kan worden geschaald, mogelijk beperkt omdat de beschikbaarheid van de andere grootten in het cluster die in huidige virtuele Machine is geïmplementeerd. In de gepubliceerde automation-runbooks die worden gebruikt in dit artikel we in dit geval regelt en alleen schalen binnen de hieronder paren van VM-grootte. Dit betekent dat een Standard_D1v2 virtuele Machine niet plotseling worden opgeschaald naar Standard_G5 of omlaag worden geschaald naar Basic_A0. Ook wordt beperkte virtuele Machine grootten omhoog/omlaag schalen niet ondersteund. U kunt kiezen om te schalen tussen de volgende sets grootten:
> 
> | VM-grootten paar schalen |  |
> | --- | --- |
> | Basic_A0 |Basic_A4 |
> | Standard_A0 |Standard_A4 |
> | Standard_A5 |Standard_A7 |
> | Standard_A8 |Standard_A9 |
> | Standard_A10 |Standard_A11 |
> | Standard_A1_v2 |Standard_A8_v2 |
> | Standard_A2m_v2 |Standard_A8m_v2  |
> | Standard_B1s |Standard_B2s |
> | Standard_B1ms |Standard_B8ms |
> | Standard_D1 |Standard_D4 |
> | Standard_D11 |Standard_D14 |
> | Standard_DS1 |Standard_DS4 |
> | Standard_DS11 |Standard_DS14 |
> | Standard_D1_v2 |Standard_D5_v2 |
> | Standard_D11_v2 |Standard_D14_v2 |
> | Standard_DS1_v2 |Standard_DS5_v2 |
> | Standard_DS11_v2 |Standard_DS14_v2 |
> | Standard_D2_v3 |Standard_D64_v3 |
> | Standard_D2s_v3 |Standard_D64s_v3 |
> | Standard_DC2s |Standard_DC4s |
> | Standard_E2v3 |Standard_E64v3 |
> | Standard_E2sv3 |Standard_E64sv3 |
> | Standard_F1 |Standard_F16 |
> | Standard_F1s |Standard_F16s |
> | Standard_F2sv2 |Standard_F72sv2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> | Standard_H8 |Standard_H16 |
> | Standard_H8m |Standard_H16m |
> | Standard_L4s |Standard_L32s |
> | Standard_L8s_v2 |Standard_L80s_v2 |
> | Standard_M8ms  |Standard_M128ms |
> | Standard_M32ls  |Standard_M64ls |
> | Standard_M64s  |Standard_M128s |
> | Standard_M64  |Standard_M128 |
> | Standard_M64m  |Standard_M128m |
> | Standard_NC6 |Standard_NC24 |
> | Standard_NC6s_v2 |Standard_NC24s_v2 |
> | Standard_NC6s_v3 |Standard_NC24s_v3 |
> | Standard_ND6s |Standard_ND24s |
> | Standard_NV6 |Standard_NV24 |
> | Standard_NV6s_v2 |Standard_NV24s_v2 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Azure Automation voor toegang tot uw virtuele Machines instellen
Het eerste wat dat u moet doen is maken van een Azure Automation-account dat als voor de runbooks die worden gebruikt host fungeert voor het schalen van de VM-Schaalset instanties. Onlangs toegevoegd de Automation-service de 'Uitvoeren als-account'-functie die is het instellen van de Service-Principal voor het automatisch uitvoeren van de runbooks namens de gebruiker heel eenvoudig. U kunt meer lezen over deze in het onderstaande artikel:

* [Runbooks verifiëren met een Azure Uitvoeren als-account](../../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>De verticale schaalbaarheid van Azure Automation-runbooks in uw abonnement importeren
De runbooks die nodig zijn voor het verticaal schalen van uw virtuele Machine zijn al gepubliceerd in de Azure Automation Runbook Gallery. U moet ze importeren in uw abonnement. U kunt informatie over het importeren van runbooks met het lezen van het volgende artikel.

* [Runbook- en modulegalerieën voor Azure Automation](../../automation/automation-runbook-gallery.md)

De runbooks die moeten worden geïmporteerd worden in de onderstaande afbeelding weergegeven

![Runbooks importeren](./media/vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook
Nadat u hebt geïmporteerd toevoegen de runbooks u moet, een webhook aan het runbook, zodat deze kan worden geactiveerd door een waarschuwing van een virtuele Machine. De details van het maken van een webhook voor het Runbook kunnen hier worden gelezen.

* [Azure Automation-webhooks](../../automation/automation-webhooks.md)

Zorg ervoor dat u de webhook kopiëren voordat de webhook-dialoogvenster te sluiten als u dit in de volgende sectie moet.

## <a name="add-an-alert-to-your-virtual-machine"></a>Een waarschuwing toevoegen aan uw virtuele Machine
1. Instellingen voor virtuele machines selecteren
2. Selecteer "Waarschuwingsregels"
3. Selecteer 'Waarschuwing toevoegen'
4. Selecteer een metrische waarde voor de waarschuwing geactiveerd op
5. Selecteer een voorwaarde die bij wordt voldaan ertoe leiden dat de waarschuwing moet worden gestart
6. Selecteer een drempelwaarde voor de voorwaarde in stap 5. moet worden voldaan
7. Selecteer een periode waarover de bewakingsservice wordt gecontroleerd voor de voorwaarde en drempelwaarde in stap 5 en 6
8. Plak de webhook die u hebt gekopieerd uit de vorige sectie.

![Waarschuwing toevoegen aan de virtuele Machine 1](./media/vertical-scaling-automation/add-alert-webhook-1.png)

![Waarschuwing toevoegen aan de virtuele Machine 2](./media/vertical-scaling-automation/add-alert-webhook-2.png)

