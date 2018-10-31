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
ms.openlocfilehash: a9967aec61aaab5bc6b4517407f36e2a6c7342c8
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238859"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Extern bureaublad-Services is niet gestart op een Azure VM

In dit artikel wordt beschreven hoe u problemen met verbinding naar een Azure-virtuele Machine (VM) wanneer Extern bureaublad-Services (Terminal Server) is niet gestart of niet kan worden gestart.

>[!NOTE]
>Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt beschreven met behulp van de Resource Manager-implementatiemodel. U wordt aangeraden dat u dit model gebruikt voor nieuwe implementaties in plaats van het klassieke implementatiemodel.

## <a name="symptoms"></a>Symptomen

Wanneer u probeert verbinding maken met een virtuele machine, treden de volgende scenario's:

- De VM-schermafbeelding ziet u dat het besturingssysteem is volledig geladen en in afwachting van referenties.

    ![Schermafbeelding van de VM-status](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- U op afstand de gebeurtenislogboeken weergeven in de virtuele machine met behulp van Logboeken, ziet u dat extern bureaublad-Services (TermServ) niet is gestart of niet te starten. Hier volgt een voorbeeldlogboek:

    **Meld u de naam**: systeem </br>
    **Bron**: Service Control Manager </br>
    **Datum**: 16-12-2017 11:19:36 uur</br>
    **Gebeurtenis-ID**: 7022</br>
    **Taak categorie**: geen</br>
    **Niveau**: fout</br>
    **Trefwoorden**: klassieke</br>
    **Gebruiker**: N.V.T.</br>
    **Computer**: vm.contoso.com</br>
    **Beschrijving**: de extern bureaublad-Services-service is vastgelopen bij het starten. 

    U kunt ook de toegang tot de seriële Console-functie gebruiken om te zoeken naar deze fouten met behulp van de volgende query uit: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Oorzaak
 
Dit probleem treedt op omdat Extern bureaublad-Services niet wordt uitgevoerd op de virtuele machine. De oorzaak kan afhankelijk zijn van de volgende scenario's: 

- De Terminal Server-service is ingesteld op **uitgeschakelde**. 
- De Terminal Server-service is gecrasht of vastgelopen. 

## <a name="solution"></a>Oplossing

Om dit probleem wilt oplossen, gebruikt u de seriële Console of [herstel de virtuele machine offline](#repair-the-vm-offline) door het koppelen van de besturingssysteemschijf van de virtuele machine aan een virtuele machine voor herstel.

### <a name="use-serial-console"></a>Seriële Console gebruiken

1. Toegang tot de [seriële Console](serial-console-windows.md) hiervoor **ondersteuning en probleemoplossing** > **seriële console**. Als de functie is ingeschakeld op de virtuele machine, kunt u de virtuele machine is verbinden.

2. Maak een nieuw kanaal voor een CMD-exemplaar. Type **CMD** starten van het kanaal om op te halen van de naam van het kanaal.

3. Schakel over naar het kanaal dat het exemplaar CMD uitgevoerd. In dit geval moet het kanaal 1 zijn.

   ```
   ch -si 1
   ```

4. Druk op **Enter** opnieuw en voer een geldige gebruikersnaam en wachtwoord (lokaal of domein-ID) voor de virtuele machine.

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
    Als de service niet kan worden gestart, volgt u de oplossing op basis van de fout is weergegeven:

    |  Fout |  Suggestie |
    |---|---|
    |5 - TOEGANG IS GEWEIGERD |Zie [Terminal Server-service is gestopt vanwege de fout toegang geweigerd](#termService-service-is-stopped-because-of-access-denied-error) |
    |1058 - ERROR_SERVICE_DISABLED  |Zie [Terminal Server-service is uitgeschakeld.](#termService-service-is-disabled)  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost|
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost    |
    |1070 - ERROR_SERVICE_START_HANG   | [Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost  |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Zie [Terminal Server-service is uitgeschakeld](#termService-service-is-disabled)  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost |
    |1753   |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel opgelost   |

#### <a name="termservice-service-is-stopped-because-of-access-denied-error"></a>Terminal Server-service is gestopt vanwege de fout toegang geweigerd

1. Verbinding maken met [seriële Console](serial-console-windows.md#) en open een PowerShell-exemplaar.
2. Download het hulpprogramma procesmonitor het volgende script uit te voeren:

        remove-module psreadline  
        $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
        $destination = "c:\temp\ProcessMonitor.zip" 
        $wc = New-Object System.Net.WebClient 
        $wc.DownloadFile($source,$destination) 
3. Nu een tracering procmon starten:

        procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
4. Reproduceren het probleem door het starten van de service die het verlenen van toegang weigeren: 

        sc start TermService 
        
    Wanneer deze is mislukt, gaat u verder en beëindigen van de trace-Monitor voor processen:

        procmon /Terminate 
5. Verzamelen van het bestand **c:\temp\ProcMonTrace.PML**, opent u het met behulp van procmon en vervolgens filteren op **resultaat is toegang geweigerd** als de volgende schermafbeelding ziet:

    ![Filteren op resultaat in proces bewaken](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Corrigeer de registersleutels, mappen of bestanden die zich op de uitvoer. Meestal dit probleem wordt veroorzaakt door het logboek voor het account dat wordt gebruikt voor de service geen ACL-machtiging voor toegang tot deze objecten. Als u wilt weten de juiste ACL-machtiging voor het logboek voor account, kunt u controleren op een VM in orde. 

#### <a name="termservice-service-is-disabled"></a>Terminal Server-service is uitgeschakeld

1.  Herstellen van de service op de standaardwaarde voor opstarten:

        sc config TermService start= demand 
        
2.  Start de service:

        sc start TermService 
3.  Query uitvoeren op de status opnieuw om ervoor te zorgen de service wordt uitgevoerd: sc-query Terminal Server 
4.  Probeer te conntet aan virtuele machine met behulp van extern bureaublad.


### <a name="repair-the-vm-offline"></a>Herstel de virtuele machine offline

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>De besturingssysteemschijf koppelen aan een virtuele machine voor herstel

1. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).
2. Start een externe bureaubladverbinding met de virtuele machine voor herstel. Zorg ervoor dat de gekoppelde schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf.
3.  Open een opdrachtprompt met verhoogde bevoegdheid-exemplaar (**als administrator uitvoeren**), en voer het volgende script. We gaan ervan uit dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteemschijf F. vervangen met de juiste waarde in uw virtuele machine. 

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
        REM Set default values back on the broken service 
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
4. [De OS-schijf loskoppelen en opnieuw maken van de virtuele machine](../windows/troubleshoot-recovery-disks-portal.md), en controleer vervolgens of het probleem opgelost is.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
