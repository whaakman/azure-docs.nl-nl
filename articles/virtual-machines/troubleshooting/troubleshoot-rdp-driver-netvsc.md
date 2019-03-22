---
title: Een probleem netvsc.sys oplost als u extern verbinding maken met een Windows 10 of Windows Server 2016-VM in Azure | Microsoft Docs
description: Informatie over het oplossen van een RDP met betrekking tot netsvc.sys uitgeven wanneer u verbinding maken met een Windows 10 of Windows Server 2016-VM in Azure.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/19/2018
ms.author: genli
ms.openlocfilehash: e6685a5e77d92bb9e05ab9578e48c99e80a64b74
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57994632"
---
# <a name="cannot-connect-remotely-to-a-windows-10-or-windows-server-2016-vm-in-azure-because-of-netvscsys"></a>Kan niet extern verbinding maken met een Windows 10 of Windows Server 2016-VM in Azure vanwege netvsc.sys

In dit artikel wordt uitgelegd hoe u een probleem waarbij er geen netwerkverbinding is wanneer u verbinding met een Windows 10 of Windows Server 2016 Datacenter virtuele machine (VM) op een host met Hyper-V Server 2016 maakt op te lossen.

## <a name="symptoms"></a>Symptomen

U kan geen verbinding maken met een Azure Windows 10 of Windows Server 2016-VM met behulp van Remote Desktop Protocol (RDP). In [diagnostische gegevens over opstarten](boot-diagnostics.md), het scherm ziet u een rode x via de netwerkinterfacekaart (NIC). Hiermee wordt aangegeven dat de virtuele machine geen verbinding heeft nadat het besturingssysteem volledig geladen is.

Dit probleem treedt meestal, op in Windows [bouwen 14393](https://support.microsoft.com/help/4093120/) en [15063 bouwen](https://support.microsoft.com/help/4015583/). Als de versie van het besturingssysteem hoger dan deze versies is, geldt dit artikel niet op uw scenario. Om te controleren of de versie van het systeem, opent u een CMD-sessie in [de toegang tot de seriële Console-functie](serial-console-windows.md), en voer **Ver**.

## <a name="cause"></a>Oorzaak

Dit probleem kan optreden als de versie van het systeem geïnstalleerde netvsc.sys bestand **10.0.14393.594** of **10.0.15063.0**. Deze versies van netvsc.sys mogelijk te voorkomen dat het systeem kunnen communiceren met het Azure-platform.


## <a name="solution"></a>Oplossing

Voordat u deze stappen volgen [een momentopname van de systeemschijf](../windows/snapshot-copy-managed-disk.md) van de betrokken virtuele machine als een back-up. Om dit probleem wilt oplossen, gebruikt u de seriële Console of [herstel de virtuele machine offline](#repair-the-vm-offline) door het koppelen van de schijf van de virtuele machine aan een virtuele machine voor herstel.


### <a name="use-the-serial-console"></a>Gebruik de seriële Console

Verbinding maken met [de seriële Console, open een PowerShell-exemplaar](serial-console-windows.md), en voer deze stappen uit.

> [!NOTE]
> Als de seriële Console niet is ingeschakeld op de virtuele machine, gaat u naar de [herstel de virtuele machine offline](#repair-the-vm-offline) sectie.

1. Voer de volgende opdracht uit in een PowerShell-sessie voor de versie van het bestand (**c:\windows\system32\drivers\netvsc.sys**):

   ```
   (get-childitem "$env:systemroot\system32\drivers\netvsc.sys").VersionInfo.FileVersion
   ```

2. Download de update naar een nieuwe of bestaande gegevensschijf die is gekoppeld aan een werkende virtuele machine uit dezelfde regio bevinden:

   - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) of een latere update
   - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) of een latere update

3. Het hulpprogramma schijf loskoppelen van de werkende virtuele machine, en koppel deze vervolgens aan de verbroken VM.

4. Voer de volgende opdracht om de update installeren op de virtuele machine:

   ```
   dism /ONLINE /add-package /packagepath:<Utility Disk Letter>:\<KB .msu or .cab>
   ```

5. Start de VM opnieuw.

### <a name="repair-the-vm-offline"></a>Herstel de virtuele machine Offline

1. [De schijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).

2. Start een externe bureaubladverbinding met de virtuele machine voor herstel.

3. Zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die is toegewezen aan de gekoppelde schijf.

4. Maak een kopie van de **\Windows\System32\config** map in geval een rollback bij de wijzigingen die nodig is.

5. Start de Register-Editor (regedit.exe) op de virtuele machine redden.

6. Selecteer de **HKEY_LOCAL_MACHINE** sleutel, en selecteer vervolgens **bestand** > **Component laden** in het menu.

7. Ga naar het bestand SYSTEM in de **\Windows\System32\config** map.

8. Selecteer **Open**, type **BROKENSYSTEM** voor de naam, vouw de **HKEY_LOCAL_MACHINE** sleutel en zoek vervolgens de extra sleutel met de naam **BROKENSYSTEM** .

9. Ga naar de volgende locatie:

   ```
   HKLM\BROKENSYSTEM\ControlSet001\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}
   ```

10. In elke subsleutel (zoals 0000), Controleer de **DriverDesc** waarde die wordt weergegeven als **Microsoft HYPER-V-netwerkadapter**.

11. In de subsleutel onderzoekt de **DriverVersion** waarde die de versie van het stuurprogramma van de netwerkadapter van de virtuele machine.

12. Download de juiste update:

    - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) of een latere update
    - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) of een latere update

13. De schijf koppelen als een gegevensschijf op een redden virtuele machine waarop u kunt de update te downloaden.

14. Voer de volgende opdracht om de update installeren op de virtuele machine:

    ```
    dism /image:<OS Disk letter>:\ /add-package /packagepath:c:\temp\<KB .msu or .cab>
    ```

15. Voer de volgende opdracht ontkoppelen van de componenten:

    ```
    reg unload HKLM\BROKENSYSTEM
    ```

16. [De schijf loskoppelen en opnieuw maken van de virtuele machine](../windows/troubleshoot-recovery-disks-portal.md).

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [Neem contact op met ondersteuning voor Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
