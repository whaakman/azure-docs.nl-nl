---
title: Windows opnieuw opstarten lus op een Azure-VM | Microsoft Docs
description: Informatie over het oplossen van de lus voor opnieuw opstarten van Windows | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/15/2018
ms.author: genli
ms.openlocfilehash: 032bc1b9c4b1b0e3bf8040ed52bf4db65ba7b6c7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58085589"
---
# <a name="windows-reboot-loop-on-an-azure-vm"></a>Windows lus voor opnieuw opstarten op een Azure VM
Dit artikel beschrijft de lus opnieuw opstarten die kan optreden op een Windows virtuele Machine (VM) in Microsoft Azure.

## <a name="symptom"></a>Symptoom

Bij het gebruik [diagnostische gegevens over opstarten](./boot-diagnostics.md) voor de schermafbeeldingen van een virtuele machine kunt u de virtuele machine wordt opgestart, maar wordt het opstartproces ophalen onderbroken, waardoor het proces wordt gestart via vinden.

![Startscherm 1](./media/troubleshoot-reboot-loop/start-screen-1.png)

## <a name="cause"></a>Oorzaak

De lus opnieuw opstarten treedt op vanwege de volgende oorzaken hebben:

### <a name="cause-1"></a>Oorzaak 1

Er is een service van derden die is gemarkeerd als kritiek en kan niet worden gestart. Dit zorgt ervoor dat het besturingssysteem opnieuw op te starten.

### <a name="cause-2"></a>Oorzaak 2

Sommige wijzigingen zijn aangebracht in het besturingssysteem. Deze zijn meestal met betrekking tot installatie van een update, de installatie van toepassing of een nieuw beleid. Mogelijk moet u controleren van de volgende logboeken voor meer informatie:

- Gebeurtenislogboeken
- CBS.logWindows
- Update.log

### <a name="cause-3"></a>Oorzaak 3

Beschadigingen van het bestandssysteem kan ervoor zorgen dat dit. Het is echter moeilijk voor het opsporen en identificeren van de wijziging die ervoor zorgt de beschadiging van het besturingssysteem dat.

## <a name="solution"></a>Oplossing

Dit probleem op te lossen [maakt u een back-up van de besturingssysteemschijf](../windows/snapshot-copy-managed-disk.md), en [de besturingssysteemschijf koppelen aan een redden VM](../windows/troubleshoot-recovery-disks-portal.md), en volgt u de oplossingsopties dienovereenkomstig, of probeer de oplossingen één voor één.

### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

1. Zodra de besturingssysteemschijf is gekoppeld aan een werkende virtuele machine, zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console en noteer de stationsletter van de partitie met de **\Windows** map.

2. Als de schijf is ingesteld op **Offline**, vervolgens instellen op **Online**.

3. Maak een kopie van de **\Windows\System32\config** map in geval een rollback bij de wijzigingen die nodig is.

4. Open de Windows-register-Editor (regedit) op de virtuele machine redden.

5. Selecteer de **HKEY_LOCAL_MACHINE** ingedrukt en selecteer vervolgens **bestand** > **Component laden** in het menu.

6. Blader naar het bestand SYSTEM in de **\Windows\System32\config** map.

7. Selecteer **Open**, type **BROKENSYSTEM** voor de naam, vouw de **HKEY_LOCAL_MACHINE** sleutel, en vervolgens u ziet een sleutel met de naam **BROKENSYSTEM** .

8. Controleer welke ControlSet het opstarten van de computer. Hier ziet u het belangrijkste nummer in de volgende registersleutel.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\Select\Current`

9. Controleer welke het kritieke karakter van de VM-agent-service via de volgende registersleutel is.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\RDAgent\ErrorControl`

10. Als de waarde van de registersleutel niet is ingesteld op **2**, klikt u vervolgens gaat u naar de volgende risicobeperking.

11. Als de waarde van de registersleutel is ingesteld op **2**, wijzig de waarde van **2** naar **1**.

12. Als een van de volgende sleutels bestaan en ze hebben een waarde **2** of **3**, en wijzig deze waarden aan **1** dienovereenkomstig:

    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupCoordinatorSvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupInquirySvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupPluginSvc\ErrorControl`

13. Selecteer de **BROKENSYSTEM** ingedrukt en selecteer vervolgens **bestand** > **Component laden** in het menu.

14. Loskoppelen van de besturingssysteemschijf van de VM voor probleemoplossing.

15. Verwijder de schijf uit de VM voor probleemoplossing en wacht u ongeveer 2 minuten voor Azure om deze schijf vrij te geven.

16. [Maak een nieuwe VM op basis van de besturingssysteemschijf](../windows/create-vm-specialized.md).

17. Als het probleem is opgelost, wordt mogelijk moet u opnieuw de [RDAgent](https://blogs.msdn.microsoft.com/mast/2014/04/07/install-the-vm-agent-on-an-existing-azure-vm/) (WaAppAgent.exe).

### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

De virtuele machine herstellen naar de laatste bekende juiste configuratie, volg de stappen in [Azure Windows VM starten met de laatst bekende juiste configuratie](https://support.microsoft.com/help/4016731/).

### <a name="solution-for-cause-3"></a>Oplossing voor oorzaak 3

1. Zodra de schijf is gekoppeld aan een virtuele machine voor probleemoplossing, zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console.

2. Maak een kopie van de **\Windows\System32\config** map in geval een rollback bij de wijzigingen die nodig is.

3. Kopieer de bestanden in de **\Windows\System32\config\regback** map en vervang de bestanden in de **\Windows\System32\config** map.

4. Verwijder de schijf uit de VM voor probleemoplossing en wacht u ongeveer 2 minuten voor Azure om deze schijf vrij te geven.

5. [Maak een nieuwe VM op basis van de besturingssysteemschijf](../windows/create-vm-specialized.md).

>[!NOTE]
>De volgende procedure moet alleen worden gebruikt als laatste resource. Tijdens het terugzetten vanuit regback mogelijk toegang herstellen naar de machine, het besturingssysteem wordt niet beschouwd als stabiel omdat er gegevens verloren gaan in het register tussen de tijdstempel van de component en de huidige dag. U moet een nieuwe virtuele machine maken en plannen om gegevens te migreren.
