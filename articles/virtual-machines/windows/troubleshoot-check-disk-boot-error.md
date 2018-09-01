---
title: Bestandssysteem controleren bij het opstarten van een Azure-VM | Microsoft Docs
description: Informatie over het oplossen van het probleem dat de virtuele machine bestandssysteem controleren weergeven bij het opstarten | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/31/2018
ms.author: genli
ms.openlocfilehash: 71619bed63d5d53e6d24eac434ce5a696a445165
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345177"
---
# <a name="windows-shows-checking-file-system-when-booting-an-azure-vm"></a>Windows bevat 'bestandssysteem controleren' wanneer een Azure-VM wordt opgestart
Dit artikel beschrijft de fout 'Bestandssysteem controleren', die optreden kunnen tijdens het opstarten van een Windows virtuele Machine (VM) in Microsoft Azure.

> [!NOTE] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt beschreven met behulp van de Resource Manager-implementatiemodel, dat wordt u aangeraden voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptom"></a>Symptoom 

Een Windows-VM niet wordt gestart. Wanneer u de schermafbeeldingen opstarten controleren in [diagnostische gegevens over opstarten](./boot-diagnostics.md), ziet u dat de schijf controleren-proces (chkdsk.exe) wordt uitgevoerd met een van de volgende berichten:

- Scannen en herstellen van station (C:)
- Controleren of bestandssysteem op C:

## <a name="cause"></a>Oorzaak

Als een NTFS-fout in het bestandssysteem wordt gevonden, wordt Windows controleren en herstellen van de consistentie van de schijf op het systeem opnieuw is opgestart. Dit gebeurt doorgaans als de virtuele machine heeft een onverwacht opnieuw opgestart, of als het afsluiten van de virtuele machine is abrupt onderbroken.

## <a name="solution"></a>Oplossing 

Windows normaal opgestart nadat het proces controleren op schijf is voltooid. Als de virtuele machine is mislukt tijdens het controleren op schijf, probeert uit te voeren van de schijf controleren op de virtuele machine offline:
1.  Een momentopname van de besturingssysteemschijf van de betrokken VM als een back-up. Zie voor meer informatie, [momentopname maken van een schijf](snapshot-copy-managed-disk.md).
2.  [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](.\troubleshoot-recovery-disks-portal.md).  
3.  Voer op de virtuele machine voor herstel, schijf controleren op de gekoppelde besturingssysteemschijf. In het volgende voorbeeld wordt de stationsletter van de gekoppelde besturingssysteemschijf E: 
        
        chkdsk E: /f
4.  Nadat de schijf controleren is voltooid, de schijf loskoppelen van de virtuele machine voor herstel en koppelt u de schijf met de betreffende virtuele machine als een besturingssysteemschijf opnieuw. Zie voor meer informatie, [een virtuele Windows-machine oplossen door de besturingssysteemschijf koppelen aan een virtuele machine voor herstel](.\troubleshoot-recovery-disks-portal.md).
