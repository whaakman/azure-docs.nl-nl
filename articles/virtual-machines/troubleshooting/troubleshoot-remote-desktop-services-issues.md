---
title: Extern bureaublad-Services wordt niet gestart op een Azure-VM | Microsoft Docs
description: Meer informatie over het oplossen van problemen met extern bureaublad-Services bij het verbinden met een virtuele Machine | Microsoft Docs
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
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 39b793e2722766f3f28829b4dc48534054abd97e
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49989194"
---
# <a name="remote-desktop-services-is-not-starting-on-an-azure-vm"></a>Extern bureaublad-Services wordt niet gestart op een Azure VM

In dit artikel wordt beschreven hoe u problemen met verbinding naar een Azure-virtuele Machine (VM) wanneer Extern bureaublad-Services (Terminal Server) is niet gestart of niet te starten.

>[!NOTE]
>Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt beschreven met behulp van de Resource Manager-implementatiemodel. U wordt aangeraden dat u dit model gebruikt voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptoms"></a>Symptomen

Wanneer u probeert verbinding maken met een virtuele machine, treden de volgende scenario's:

- De VM-schermafbeelding ziet u dat het besturingssysteem is volledig geladen en in afwachting van referenties.
- Alle toepassingen op de virtuele machine zijn werken zoals verwacht en toegankelijk is.
- De virtuele machine reageert op de TCP-verbinding op de Microsoft Remote Desktop Protocol (RDP)-poort (standaard 3389).
- U niet gevraagd om referenties wanneer u probeert een RDP-verbinding te maken.

## <a name="cause"></a>Oorzaak

Dit probleem treedt op omdat Extern bureaublad-Services niet wordt uitgevoerd op de virtuele Machine. De oorzaak kan afhankelijk zijn van de volgende scenario's:

- De Terminal Server-service is ingesteld op **uitgeschakelde**.
- De Terminal Server-service is gecrasht of vastgelopen.

## <a name="solution"></a>Oplossing

U lost dit probleem, gebruikt u een van de volgende oplossingen of één voor één van de oplossingen proberen:

### <a name="solution-1-using-the-serial-console"></a>Oplossing 1: Met behulp van de seriële Console

1. Toegang tot de [seriële Console](serial-console-windows.md) hiervoor **ondersteuning en probleemoplossing** > **seriële console (Preview)**. Als de functie is ingeschakeld op de virtuele machine, kunt u de virtuele machine is verbinden.

2. Maak een nieuw kanaal voor een CMD-exemplaar. Type **CMD** starten van het kanaal om op te halen van de naam van het kanaal.

3. Schakel over naar het kanaal dat het exemplaar CMD uitgevoerd, in dit geval moet het kanaal 1.

   ```
   ch -si 1
   ```

4. Druk op **Enter** opnieuw en geef een geldige gebruikersnaam en wachtwoord (lokaal of domein-ID) voor de virtuele machine.

5. De status van de Terminal Server-service niet opvragen.

   ```
   sc query TermService
   ```

6. Als de status van de service **gestopt**, probeert de service te starten.

   ```
   sc start TermService
   ```

7. Query uitvoeren op de service opnieuw om het ervoor te zorgen dat de service is gestart.

   ```
   sc query TermService
   ```

### <a name="solution-2-using-a-recovery-vm-to-enable-the-service"></a>Oplossing 2: Met behulp van een virtuele machine voor herstel de service inschakelen

[Maak een back-up van de besturingssysteemschijf](../windows/snapshot-copy-managed-disk.md) en [de besturingssysteemschijf koppelen aan een redden VM](../windows/troubleshoot-recovery-disks-portal.md). Vervolgens opent u een verhoogde CMD-exemplaar en de volgende scripts uitvoeren op de redden VM:

>[!NOTE]
>We gaan ervan uit dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf F. vervangen met de juiste waarde in uw virtuele machine. Nadat dit is gebeurd, de schijf loskoppelen van de virtuele machine voor herstel en [opnieuw maken van uw virtuele machine](../windows/create-vm-specialized.md). Voor meer informatie, kunt u **oplossing 1** omdat de seriële Console is ingeschakeld.

```
reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM

REM Enable Serial Console
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} displaybootmenu yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} timeout 10
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} bootems yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /ems {<Boot Loader Identifier>} ON
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

REM Get the current ControlSet from where the OS is booting
for /f "tokens=3" %x in ('REG QUERY HKLM\BROKENSYSTEM\Select /v Current') do set ControlSet=%x
set ControlSet=%ControlSet:~2,1%

REM Suggested configuration to enable OS Dump
set key=HKLM\BROKENSYSTEM\ControlSet00%ControlSet%\Control\CrashControl
REG ADD %key% /v CrashDumpEnabled /t REG_DWORD /d 2 /f
REG ADD %key% /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
REG ADD %key% /v NMICrashDump /t REG_DWORD /d 1 /f

REM Set default values back on the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Image Path>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v type /t REG_DWORD /d 16 /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v type /t REG_DWORD /d 16 /f

REM Enable default dependencies from the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg unload HKLM\BROKENSYSTEM
```

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
