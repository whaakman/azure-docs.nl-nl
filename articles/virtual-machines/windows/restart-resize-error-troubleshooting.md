---
title: Virtuele machine opnieuw te starten of het formaat problemen in Azure | Microsoft Docs
description: Oplossen van problemen bij de implementatie met opnieuw te starten of het formaat van een bestaande Windows virtuele Machine in Azure Resource Manager
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 11/03/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2fecb9ebf359e3d3d23d5af0f60882cdfac5ff45
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Problemen oplossen implementatie met opnieuw te starten of het formaat van een bestaande Windows-machines in Azure
Wanneer u een gestopte Azure virtuele Machine (VM) start of het formaat van een bestaande virtuele machine in Azure, is de algemene fout die u tegenkomt een geheugentoewijzing is mislukt. Deze fout treedt op wanneer het cluster of de regio geen bronnen beschikbaar heeft of het aangevraagde VM-grootte kan niet worden ondersteund.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Registreert activiteit verzamelen
U start het oplossen van problemen door de activiteitenlogboeken om u te identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over het proces:

[Implementatiebewerkingen bekijken](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Activiteit-logboeken voor het beheren van Azure-resources bekijken](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Fout bij het starten van een gestopte VM
U probeert te starten van een gestopte VM maar een toewijzingsfout ophalen.

### <a name="cause"></a>Oorzaak
De aanvraag voor het starten van de gestopte virtuele machine moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Het cluster heeft echter geen vrije schijfruimte om de aanvraag te voldoen.

### <a name="resolution"></a>Oplossing
* Alle VM's in de beschikbaarheidsset Stop en start opnieuw op elke virtuele machine.
  
  1. Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheidsset*  >  **Virtuele Machines** > *uw virtuele machine* > **stoppen**.
  2. Nadat de virtuele machines stoppen, selecteert u elk van de gestopte virtuele machines en klik op Start.
* Probeer de aanvraag opnieuw opstarten op een later tijdstip.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Fout wanneer het formaat van een bestaande virtuele machine
U probeert te vergroten of verkleinen van een bestaande virtuele machine, maar een toewijzingsfout ophalen.

### <a name="cause"></a>Oorzaak
De aanvraag voor het formaat van de virtuele machine moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Het cluster biedt echter geen ondersteuning voor de aangevraagde VM-grootte.

### <a name="resolution"></a>Oplossing
* Probeer de aanvraag met een kleinere VM.
* Als de grootte van de aangevraagde virtuele machine kan niet worden gewijzigd:
  
  1. Stop de virtuele machines in de beschikbaarheidsset.
     
     * Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheidsset*  >  **Virtuele Machines** > *uw virtuele machine* > **stoppen**.
  2. Nadat de virtuele machines stoppen, de grootte van de gewenste VM in een groter formaat.
  3. Selecteer de VM formaat is gewijzigd en klik op **Start**, en start de gestopte virtuele machines.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt bij het maken van een nieuwe Windows VM in Azure, Zie [implementatieproblemen oplossen met een nieuwe virtuele machine voor Windows maken in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

