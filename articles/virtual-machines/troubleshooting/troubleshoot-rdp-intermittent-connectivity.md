---
title: Extern bureaublad verbinding verbreekt vaak in virtuele Azure-machine | Microsoft Docs
description: Informatie over het oplossen van het probleem waarbij extern bureaublad verbinding vaak in virtuele Azure-machine verbreekt.
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
ms.openlocfilehash: b65cd4d7e6f68f9444ca0264892bcca31cfbe674
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142388"
---
# <a name="remote-desktop-disconnects-frequently-in-azure-vm"></a>Extern bureaublad verbinding verbreekt vaak in Azure VM

In dit artikel laat zien hoe het probleem waarbij extern bureaublad verbinding vaak in Azure VM verbreekt oplossen.

> [!NOTE] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt beschreven met behulp van de Resource Manager-implementatiemodel, dat wordt u aangeraden voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptom"></a>Symptoom 

U wordt geconfronteerd onregelmatige RDP-connectiviteit tijdens uw sessies. U kunt verbinding maken met de virtuele machine, maar vervolgens de verbinding wordt verzonden.

## <a name="cause"></a>Oorzaak

Dit probleem kan worden veroorzaakt door het RDP-Listener is onjuist geconfigureerd. Dit probleem treedt meestal op de aangepaste virtuele machine.

## <a name="solution"></a>Oplossing  

Voordat u de stappen volgen [maakt u een momentopname van de besturingssysteemschijf](../windows/snapshot-copy-managed-disk.md) van de betrokken virtuele machine als een back-up. 

Seriële besturingselement gebruiken om dit probleem wilt oplossen, of [herstel de virtuele machine offline](#repair-the-vm-offline) door het koppelen van de besturingssysteemschijf van de virtuele machine aan een virtuele machine voor herstel.

### <a name="reset-rdp-configurations"></a>RDP-configuratie opnieuw instellen

1. Verbinding maken met de [seriële Console- en CMD-instantie openen](./serial-console-windows.md), voer de volgende opdrachten in de sectie opnieuw instellen van RDP-configuraties. Als de seriële Console niet is ingeschakeld op de virtuele machine, verplaatst naar de volgende stap. 
2. Verlaag de RDP-beveiligingslaag op 0, zodat de communicatie tussen server en client de RDP-codering wordt gebruikt.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
3. Het versleutelingsniveau dat het minimum aan verouderde RDP clients toestaan verbinding te verlagen.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
4. RDP laden van de Gebruikersconfiguratie van de van de virtuele clientmachine wordt ingesteld.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
5. RDP Keep-Alive-beheer in te schakelen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
6. Besturingselement RDP-verbinding instellen

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f
7. Tijd van RDP-sessie besturingselement instellen 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
8. Besturingselement voor RDP verbroken verbinding instellen 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f
        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
9. Besturingselement van de tijd voor RDP-verbinding instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
10. RDP-sessie niet-actieve tijd besturingselement instellen:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
11. Stel het maximum aantal gelijktijdige verbindingen beperken:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

12. Start de virtuele machine en Zie als kunt u het verbinden met behulp van RDP.

### <a name="repair-the-vm-offline"></a>Herstel de virtuele machine offline

1. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).
2. Zodra de besturingssysteemschijf is gekoppeld aan de virtuele machine voor herstel, zorg ervoor dat de schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf.
3. Blader naar de **\windows\system32\config** map. Kopieer alle bestanden in deze map als een back-up, als een terugdraaiactie moet uitgevoerd worden.
4. Start de Register-Editor (regedit.exe).
5. Selecteer de **HKEY_LOCAL_MACHINE** sleutel. Selecteer in het menu **bestand** > **Component laden**:
6. Blader naar de **\windows\system32\config\SYSTEM** map op de OS-schijf die u hebt gekoppeld. Voer voor de naam van de component, **BROKENSYSTEM**. De nieuwe registercomponent wordt weergegeven onder de **HKEY_LOCAL_MACHINE** sleutel.
7. Blader naar de **\windows\system32\config\SOFTWARE** map op de OS-schijf die u hebt gekoppeld. Voer voor de naam van de hive-software, **BROKENSOFTWARE**.
8. Open een venster met verhoogde opdracht (als administrator uitvoeren), voert u opdrachten uit in de rest-stappen voor het opnieuw instellen van de RDP-configuraties. 
9. Verlaag de RDP-beveiligingslaag op 0, waar de communicatie tussen server en client de RDP-codering wordt gebruikt:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
10. Het versleutelingsniveau dat het minimum aan verouderde RDP clients toestaan verbinding te verlagen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
11. RDP laden van de Gebruikersconfiguratie van de van de virtuele clientmachine wordt ingesteld.

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
12. RDP Keep-Alive-beheer in te schakelen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
13. Besturingselement RDP-verbinding instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f

14. Tijd van RDP-sessie besturingselement instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
15. RDP verbroken verbinding besturingselement instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
16. Besturingselement van de tijd voor RDP-verbinding instellen:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
17. RDP-sessie niet-actieve tijd besturingselement instellen: REG ADD ' HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp "/v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
18. Stel het maximum aantal gelijktijdige verbindingen beperken:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f
19. Start de virtuele machine en Zie als kunt u het verbinden met behulp van RDP.

## <a name="need-help"></a>Hulp nodig? 
Neem contact op met ondersteuning. Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.





