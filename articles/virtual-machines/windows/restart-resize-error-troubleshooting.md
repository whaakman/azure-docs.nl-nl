---
title: Problemen met virtuele machine opnieuw starten of het formaat wijzigen in Azure | Microsoft Docs
description: Probleemoplossing voor implementatieproblemen bij opnieuw starten of het formaat van een bestaande Windows-virtuele Machine in Azure Resource Manager
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4e0c77c03856b4851ee5fe49bd6ae54d47f6c31
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092176"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Probleemoplossing voor implementatieproblemen bij opnieuw starten of het formaat van een bestaande Windows-VM in Azure
Wanneer u probeert een gestopte Azure virtuele Machine (VM) start, vergroten of verkleinen van een bestaande VM in Azure, is de algemene fout die u ondervindt een toewijzingsfout. Deze fout treedt op wanneer het cluster of de regio heeft geen beschikbare bronnen of kan de aangevraagde VM-grootte niet ondersteunen.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Activiteitenlogboeken verzamelen
Als u wilt gaan met het oplossen van problemen, de activiteitenlogboeken voor het identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over het proces:

[Implementatiebewerkingen bekijken](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Activiteitenlogboeken bekijken voor het beheren van Azure-resources](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Probleem: Fout bij het starten van een gestopte virtuele machine
U probeert een gestopte virtuele machine starten, maar treedt er een toewijzingsfout.

### <a name="cause"></a>Oorzaak
De aanvraag voor het starten van de gestopte virtuele machine moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Het cluster heeft echter geen vrije ruimte beschikbaar is voor de aanvraag niet voltooien.

### <a name="resolution"></a>Oplossing
* Alle virtuele machines in de beschikbaarheidsset stoppen en opnieuw opstarten elke virtuele machine.
  
  1. Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheidsset*  >  **Virtuele Machines** > *uw virtuele machine* > **stoppen**.
  2. Nadat alle virtuele machines stoppen, selecteert u elk van de gestopte VM's en klik op Start.
* De aanvraag opnieuw opstarten op een later tijdstip opnieuw.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Probleem: Fout bij het vergroten of verkleinen van een bestaande virtuele machine
U probeert te vergroten of verkleinen van een bestaande virtuele machine, maar treedt er een toewijzingsfout.

### <a name="cause"></a>Oorzaak
De aanvraag voor het formaat van de virtuele machine moet worden uitgevoerd op het oorspronkelijke cluster die als host fungeert voor de cloudservice. Het cluster biedt echter geen ondersteuning voor de aangevraagde VM-grootte.

### <a name="resolution"></a>Oplossing
* Probeer de aanvraag met een kleinere virtuele machine.
* Als de grootte van de aangevraagde virtuele machine kan niet worden gewijzigd:
  
  1. Stop alle virtuele machines in de beschikbaarheidsset.
     
     * Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheidsset*  >  **Virtuele Machines** > *uw virtuele machine* > **stoppen**.
  2. Nadat alle virtuele machines stoppen, de grootte van de gewenste VM in een groter formaat.
  3. Selecteer de grootte virtuele machine en klikt u op **Start**, en start vervolgens elk van de gestopte VM's.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt bij het maken van een nieuwe Windows-VM in Azure, Zie [probleemoplossing voor implementatieproblemen bij het maken van een nieuwe Windows virtuele machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

