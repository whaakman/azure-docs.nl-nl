---
title: Virtuele machine opnieuw te starten of het formaat van problemen | Microsoft Docs
description: Problemen met klassieke implementatie bij opnieuw starten of het formaat van een bestaande Windows-virtuele Machine in Azure
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 06/15/2018
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 400b20e474257650a22e04c89ddde581bf0552f4
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091727"
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Problemen met klassieke implementatie bij opnieuw starten of het formaat van een bestaande Windows-virtuele Machine in Azure
> [!div class="op_single_selector"]
> * [Klassiek](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

Wanneer u probeert een gestopte Azure virtuele Machine (VM) start, vergroten of verkleinen van een bestaande VM in Azure, is de algemene fout die u ondervindt een toewijzingsfout. Deze fout treedt op wanneer het cluster of de regio heeft geen beschikbare bronnen of kan de aangevraagde VM-grootte niet ondersteunen.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../../azure-resource-manager/resource-manager-deployment-model.md).  Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Auditlogboeken verzamelen
Verzamelen de auditlogboeken voor het identificeren van de fout die is gekoppeld aan het probleem wilt oplossen.

Klik in de Azure-portal op **Bladeren** > **virtuele machines** > *uw Windows-machine*  >   **Instellingen voor** > **auditlogboeken**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Fout bij het starten van een gestopte virtuele machine
U probeert een gestopte virtuele machine starten, maar treedt er een toewijzingsfout.

### <a name="cause"></a>Oorzaak
De aanvraag voor het starten van de gestopte virtuele machine moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Het cluster heeft echter geen vrije ruimte beschikbaar is voor de aanvraag niet voltooien.

### <a name="resolution"></a>Oplossing
* Een nieuwe cloudservice maken en deze koppelen aan een regio of een virtueel netwerk voor op basis van regio, maar niet een affiniteitsgroep.
* Verwijder de gestopte virtuele machine.
* De virtuele machine in de nieuwe service in de cloud met behulp van de schijven opnieuw.
* Start de opnieuw gemaakte virtuele machine.

Als u een fout optreedt bij het maken van een nieuwe cloudservice, probeer het later opnieuw of wijzigen van de regio voor de cloudservice.

> [!IMPORTANT]
> De nieuwe cloudservice heeft een nieuwe naam en VIP-adres, daarom u moet die gegevens voor de afhankelijkheden die gebruikmaken van die informatie voor de bestaande cloudservice wijzigen.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Fout bij het vergroten of verkleinen van een bestaande virtuele machine
U probeert te vergroten of verkleinen van een bestaande virtuele machine, maar treedt er een toewijzingsfout.

### <a name="cause"></a>Oorzaak
De aanvraag voor het formaat van de virtuele machine moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Het cluster biedt echter geen ondersteuning voor de aangevraagde VM-grootte.

### <a name="resolution"></a>Oplossing
De aangevraagde VM-grootte te beperken en probeer de aanvraag voor formaat wijzigen.

* Klik op **door alles bladeren** > **virtuele machines (klassiek)** > *uw virtuele machine* > **instellingen**  >  **Grootte**. Zie voor gedetailleerde stappen [grootte van de virtuele machine](https://msdn.microsoft.com/library/dn168976.aspx).

Als het niet mogelijk om de VM-grootte te beperken, volg deze stappen:

* Een nieuwe cloudservice maken, is ervoor te zorgen dat deze niet is gekoppeld aan een affiniteitsgroep en niet zijn gekoppeld aan een virtueel netwerk dat is gekoppeld aan een affiniteitsgroep.
* Maak een nieuwe, grotere grootte VM erin.

U kunt alle virtuele machines in dezelfde cloudservice samenvoegen. Als uw bestaande cloudservice gekoppeld aan een virtueel netwerk voor op basis van regio is, kunt u de nieuwe service in de cloud verbinden met een bestaand virtueel netwerk.

Als de bestaande cloudservice niet gekoppeld aan een virtueel netwerk voor op basis van regio is, hebt u de virtuele machines in de bestaande cloudservice verwijderen en opnieuw maken in de nieuwe cloudservice van de schijven. Het is echter belangrijk te weten dat de nieuwe service in de cloud hebben een nieuwe naam en VIP-adres, dus moet u deze voor alle afhankelijkheden die momenteel gebruikmaken van deze informatie voor de bestaande cloudservice bijwerken.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt bij het maken van een Windows-VM in Azure, Zie [probleemoplossing voor implementatieproblemen bij het maken van een Windows-machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

