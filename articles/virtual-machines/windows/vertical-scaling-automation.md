---
title: Azure Automation gebruiken voor Windows virtuele machines verticaal schalen | Microsoft Docs
description: Windows virtuele machines verticaal schalen in reactie op waarschuwingen met Azure Automation
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 4f964713-fb67-4bcc-8246-3431452ddf7d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: kasing
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7cccd36c619e58b8dedb9a52e70c478dc7b857c
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707929"
---
# <a name="vertically-scale-windows-vms-with-azure-automation"></a>Windows virtuele machines met Azure Automation verticaal schalen

Verticaal schalen is het proces van vergroten of verkleinen van de resources van een virtuele machine in reactie op de workload. In Azure kunt dit doen door de grootte van de virtuele Machine te wijzigen. Dit kan helpen in de volgende scenario 's

* Als de virtuele Machine is niet vaak worden gebruikt, kunt u het formaat tot een kleinere om uw maandelijkse kosten te verlagen
* Als de virtuele Machine een piekbelasting ziet is, kan het worden gewijzigd in een groter formaat om de capaciteit te vergroten

Het overzicht voor de stappen om dit te doen is als hieronder

1. Azure Automation voor toegang tot uw virtuele Machines instellen
2. De verticale schaalbaarheid van Azure Automation-runbooks in uw abonnement importeren
3. Een webhook toevoegen aan uw runbook
4. Een waarschuwing toevoegen aan uw virtuele Machine

## <a name="scale-limitations"></a>Beperkingen van de schaal

Vanwege de grootte van de eerste virtuele Machine, de grootte die het kan worden geschaald, mogelijk beperkt omdat de beschikbaarheid van de andere grootten in het cluster die in huidige virtuele Machine is geïmplementeerd. In de gepubliceerde automation-runbooks die worden gebruikt in dit artikel we in dit geval regelt en alleen schalen binnen de hieronder paren van VM-grootte. Dit betekent dat een Standard_D1v2 virtuele Machine niet plotseling worden opgeschaald naar Standard_G5 of omlaag worden geschaald naar Basic_A0. Ook wordt beperkte virtuele Machine grootten omhoog/omlaag schalen niet ondersteund. 

U kunt kiezen om te schalen tussen de volgende sets grootten:

* [A-serie](#a-series)
* [B-serie](#b-series)
* [Uit de D-serie](#d-series)
* [E-serie](#e-series)
* [F-serie](#f-series)
* [G-serie](#g-series)
* [H-serie](#h-series)
* [L-serie](#l-series)
* [M-serie](#m-series)
* [N-serie](#n-series)

### <a name="a-series"></a>A-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Basic_A0 | Basic_A1 |
| Basic_A1 | Basic_A2 |
| Basic_A2 | Basic_A3 |
| Basic_A3 | Basic_A4 |
| Standard_A0 | Standard_A1 |
| Standard_A1 | Standard_A2 |
| Standard_A2 | Standard_A3 |
| Standard_A3 | Standard_A4 |
| Standard_A5 | Standard_A6 |
| Standard_A6 | Standard_A7 |
| Standard_A8 | Standard_A9 |
| Standard_A10 | Standard_A11 |
| Standard_A1_v2 | Standard_A2_v2 |
| Standard_A2_v2 | Standard_A4_v2 |
| Standard_A4_v2 | Standard_A8_v2 |
| Standard_A2m_v2 | Standard_A4m_v2 |
| Standard_A4m_v2 | Standard_A8m_v2 |

### <a name="b-series"></a>B-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_B1s | Standard_B2s |
| Standard_B1ms | Standard_B2ms |
| Standard_B2ms | Standard_B4ms |
| Standard_B4ms | Standard_B8ms |

### <a name="d-series"></a>D-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_D1 | Standard_D2 |
| Standard_D2 | Standard_D3 |
| Standard_D3 | Standard_D4 |
| Standard_D11 | Standard_D12 |
| Standard_D12 | Standard_D13 |
| Standard_D13 | Standard_D14 |
| Standard_DS1 | Standard_DS2 |
| Standard_DS2 | Standard_DS3 |
| Standard_DS3 | Standard_DS4 |
| Standard_DS11 | Standard_DS12 |
| Standard_DS12 | Standard_DS13 |
| Standard_DS13 | Standard_DS14 |
| Standard_D1_v2 | Standard_D2_v2 |
| Standard_D2_v2 | Standard_D3_v2 |
| Standard_D3_v2 | Standard_D4_v2 |
| Standard_D4_v2 | Standard_D5_v2 |
| Standard_D11_v2 | Standard_D12_v2 |
| Standard_D12_v2 | Standard_D13_v2 |
| Standard_D13_v2 | Standard_D14_v2 |
| Standard_DS1_v2 | Standard_DS2_v2 |
| Standard_DS2_v2 | Standard_DS3_v2 |
| Standard_DS3_v2 | Standard_DS4_v2 |
| Standard_DS4_v2 | Standard_DS5_v2 |
| Standard_DS11_v2 | Standard_DS12_v2 |
| Standard_DS12_v2 | Standard_DS13_v2 |
| Standard_DS13_v2 | Standard_DS14_v2 |
| Standard_D2_v3 | Standard_D4_v3 |
| Standard_D4_v3 | Standard_D8_v3 |
| Standard_D8_v3 | Standard_D16_v3 |
| Standard_D16_v3 | Standard_D32_v3 |
| Standard_D32_v3 | Standard_D64_v3 |
| Standard_D2s_v3 | Standard_D4s_v3 |
| Standard_D4s_v3 | Standard_D8s_v3 |
| Standard_D8s_v3 | Standard_D16s_v3 |
| Standard_D16s_v3 | Standard_D32s_v3 |
| Standard_D32s_v3 | Standard_D64s_v3 |
| Standard_DC2s | Standard_DC4s |

### <a name="e-series"></a>E-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_E2_v3 | Standard_E4_v3 |
| Standard_E4_v3 | Standard_E8_v3 |
| Standard_E8_v3 | Standard_E16_v3 |
| Standard_E16_v3 | Standard_E20_v3 |
| Standard_E20_v3 | Standard_E32_v3 |
| Standard_E32_v3 | Standard_E64_v3 |
| Standard_E2s_v3 | Standard_E4s_v3 |
| Standard_E4s_v3 | Standard_E8s_v3 |
| Standard_E8s_v3 | Standard_E16s_v3 |
| Standard_E16s_v3 | Standard_E20s_v3 |
| Standard_E20s_v3 | Standard_E32s_v3 |
| Standard_E32s_v3 | Standard_E64s_v3 |

### <a name="f-series"></a>F-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_F1 | Standard_F2 |
| Standard_F2 | Standard_F4 |
| Standard_F4 | Standard_F8 |
| Standard_F8 | Standard_F16 |
| Standard_F1s | Standard_F2s |
| Standard_F2s | Standard_F4s |
| Standard_F4s | Standard_F8s |
| Standard_F8s | Standard_F16s |
| Standard_F2s_v2 | Standard_F4s_v2 |
| Standard_F4s_v2 | Standard_F8s_v2 |
| Standard_F8s_v2 | Standard_F16s_v2 |
| Standard_F16s_v2 | Standard_F32s_v2 |
| Standard_F32s_v2 | Standard_F64s_v2 |
| Standard_F64s_v2 | Standard_F7s_v2 |

### <a name="g-series"></a>G-reeks

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_G1 | Standard_G2 |
| Standard_G2 | Standard_G3 |
| Standard_G3 | Standard_G4 |
| Standard_G4 | Standard_G5 |
| Standard_GS1 | Standard_GS2 |
| Standard_GS2 | Standard_GS3 |
| Standard_GS3 | Standard_GS4 |
| Standard_GS4 | Standard_GS5 |

### <a name="h-series"></a>H-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_H8 | Standard_H16 |
| Standard_H8m | Standard_H16m |

### <a name="l-series"></a>L-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_L4s | Standard_L8s |
| Standard_L8s | Standard_L16s |
| Standard_L16s | Standard_L32s |
| Standard_L8s_v2 | Standard_L16s_v2 |
| Standard_L16s_v2 | Standard_L32s_v2 |
| Standard_L32s_v2 | Standard_L64s_v2 |
| Standard_L64s_v2 | Standard_L80s_v2 |

### <a name="m-series"></a>M-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_M8ms | Standard_M16ms |
| Standard_M16ms | Standard_M32ms |
| Standard_M32ms | Standard_M64ms |
| Standard_M64ms | Standard_M128ms |
| Standard_M32ls | Standard_M64ls |
| Standard_M64s | Standard_M128s |
| Standard_M64 | Standard_M128 |
| Standard_M64m | Standard_M128m |

### <a name="n-series"></a>N-serie

| Oorspronkelijke grootte | Grootte opschalen | 
| --- | --- |
| Standard_NC6 | Standard_NC12 |
| Standard_NC12 | Standard_NC24 |
| Standard_NC6s_v2 | Standard_NC12s_v2 |
| Standard_NC12s_v2 | Standard_NC24s_v2 |
| Standard_NC6s_v3 | Standard_NC12s_v3 |
| Standard_NC12s_v3 | Standard_NC24s_v3 |
| Standard_ND6 | Standard_ND12 |
| Standard_ND12 | Standard_ND24 |
| Standard_NV6 | Standard_NV12 |
| Standard_NV12 | Standard_NV24 |
| Standard_NV6s_v2 | Standard_NV12s_v2 |
| Standard_NV12s_v2 | Standard_NV24s_v2 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Azure Automation voor toegang tot uw virtuele Machines instellen
Het eerste wat dat u moet doen is een Azure Automation-account dat als voor de runbooks die worden gebruikt host fungeert voor het schalen van een virtuele Machine maken. Onlangs toegevoegd de Automation-service de 'Uitvoeren als-account'-functie die is het instellen van de Service-Principal voor het automatisch uitvoeren van de runbooks namens de gebruiker heel eenvoudig. U kunt meer lezen over deze in het onderstaande artikel:

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

