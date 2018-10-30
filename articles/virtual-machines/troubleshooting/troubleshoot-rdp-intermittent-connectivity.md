---
title: Extern bureaublad verbinding verbreekt vaak in virtuele Azure-machine | Microsoft Docs
description: Informatie over het oplossen van regelmatige verbroken verbindingen van extern bureaublad in virtuele Azure-machine.
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
ms.date: 10/24/2018
ms.author: genli
ms.openlocfilehash: 8c3e0b11a9a16ec338c6e11fecc48460862acad4
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50214846"
---
# <a name="remote-desktop-disconnects-frequently-in-azure-vm"></a>Extern bureaublad verbinding verbreekt vaak in Azure VM

In dit artikel wordt uitgelegd hoe u om op te lossen frequente verbroken verbindingen met een Azure-machine (VM) via RDP Remote Desktop Protocol).

> [!NOTE] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over het Resource Manager-implementatiemodel. U wordt aangeraden dat u dit model gebruikt voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptom"></a>Symptoom

U wordt geconfronteerd problemen voor RDP-connectiviteit tijdens uw sessies. U kunt in eerste instantie verbinding maken met de virtuele machine, maar vervolgens wordt de verbinding geweigerd.

## <a name="cause"></a>Oorzaak

Dit probleem kan optreden als de RDP-Listener is onjuist geconfigureerd. Normaal gesproken dit probleem doet zich op een virtuele machine die gebruikmaakt van een een aangepaste installatiekopie.

## <a name="solution"></a>Oplossing

Voordat u deze stappen volgen [maakt u een momentopname van de besturingssysteemschijf](../windows/snapshot-copy-managed-disk.md) van de betrokken virtuele machine als een back-up. 

Seriële besturingselement gebruiken om dit probleem wilt oplossen, of [herstel de virtuele machine offline](#repair-the-vm-offline) door het koppelen van de besturingssysteemschijf van de virtuele machine aan een virtuele machine voor herstel.

### <a name="serial-control"></a>Seriële besturingselement

1. Verbinding maken met [seriële Console- en CMD-instantie openen](./serial-console-windows.md). Voer de volgende opdrachten voor het opnieuw instellen van de RDP-configuraties. Als de seriële Console niet is ingeschakeld op de virtuele machine, gaat u naar de volgende stap.
2. Verlaag de RDP-beveiligingslaag op 0. Met deze instelling wordt de communicatie tussen de server en de client de RDP-codering gebruikt.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
3. Verlaag het versleutelingsniveau dat de instelling voor het minimale om toe te staan van verouderde clients van RDP verbinding te maken.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
4. Stel RDP laden van de Gebruikersconfiguratie van de clientcomputer.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
5. De RDP-keepalive beheer in te schakelen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
6. Het besturingselement RDP-verbinding instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f
7. De tijd voor RDP-sessie-besturingselement instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
8. Stel het besturingselement RDP verbroken verbinding: 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f
        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
9. Het besturingselement van de tijd voor RDP-verbinding instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
10. RDP-sessie niet-actieve tijd van het besturingselement instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
11. 'Het maximum aantal gelijktijdige verbindingen beperken' van het besturingselement instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

12. Start de VM opnieuw op en probeer het opnieuw verbinding maken met behulp van RDP.

### <a name="repair-the-vm-offline"></a>Herstel de virtuele machine offline

1. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).
2. Nadat de besturingssysteemschijf is gekoppeld aan de virtuele machine voor herstel, zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf.
3. Op een schijf met het besturingssysteem die u hebt gekoppeld, gaat u naar de **\windows\system32\config** map. Kopieer alle bestanden in deze map als een back-up, als een terugdraaiactie moet uitgevoerd worden.
4. Start de Register-Editor (regedit.exe).
5. Selecteer de **HKEY_LOCAL_MACHINE** sleutel. Selecteer in het menu **bestand** > **Component laden**:
6. Blader naar de **\windows\system32\config\SYSTEM** map op de OS-schijf die u hebt gekoppeld. Voer voor de naam van de component, **BROKENSYSTEM**. De nieuwe registercomponent wordt weergegeven onder de **HKEY_LOCAL_MACHINE** sleutel. Vervolgens de software-Component laden **\windows\system32\config\SOFTWARE** onder de **HKEY_LOCAL_MACHINE** sleutel. Voer voor de naam van de hive-software, **BROKENSOFTWARE**. 
7. Open een opdrachtpromptvenster (**als administrator uitvoeren**), en voer de opdrachten uit in de resterende stappen voor het opnieuw instellen van de RDP-configuraties. 
8. De RDP-beveiligingslaag op 0 verlagen zodat het RDP-codering voor communicatie tussen de server en client gebruikt:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
9. Het versleutelingsniveau dat de instelling voor het minimale om toe te staan van verouderde clients van RDP om verbinding te verlagen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
10. Stel RDP laden van de Gebruikersconfiguratie van de clientcomputer.

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
11. De RDP-keepalive beheer in te schakelen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
12. Het besturingselement RDP-verbinding instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f

13. De tijd voor RDP-sessie-besturingselement instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
14. Stel het besturingselement RDP verbroken verbinding:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
15. Het besturingselement van de tijd voor RDP-verbinding instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
16. RDP-sessie niet-actieve tijd van het besturingselement instellen: REG ADD ' HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp "/v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
17. 'Het maximum aantal gelijktijdige verbindingen beperken' van het besturingselement instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f
18. Start de VM opnieuw op en probeer het opnieuw verbinding maken met behulp van RDP.

## <a name="need-help"></a>Hulp nodig? 
Neem contact op met ondersteuning. Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.





