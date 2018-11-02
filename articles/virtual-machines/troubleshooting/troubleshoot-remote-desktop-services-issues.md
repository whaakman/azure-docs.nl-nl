---
title: Extern bureaublad-Services is niet gestart op een Azure-VM | Microsoft Docs
description: Meer informatie over het oplossen van problemen met extern bureaublad-Services wanneer u verbinding met een virtuele machine maakt | Microsoft Docs
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
ms.openlocfilehash: 756417ee2f98549d648386c2471baa74889245a4
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914019"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Extern bureaublad-Services is niet gestart op een Azure VM

In dit artikel wordt beschreven hoe u problemen oplossen wanneer u verbinding met een Azure-machine (VM maakt) en extern bureaublad-Services, of Terminal Server, is niet gestart of niet kan worden gestart.

> [!NOTE]  
> Azure heeft twee verschillende implementatiemodellen te maken en te werken met resources: [Azure Resource Manager en klassiek](../../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel wordt beschreven met behulp van de Resource Manager-implementatiemodel. U wordt aangeraden dat u dit model voor nieuwe implementaties in plaats van het klassieke implementatiemodel gebruikt.

## <a name="symptoms"></a>Symptomen

Wanneer u probeert verbinding maken met een virtuele machine, treden de volgende scenario's:

- De VM-schermafbeelding ziet u dat het besturingssysteem is volledig geladen en in afwachting van referenties.

    ![Schermafbeelding van de VM-status](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- U weergeven op afstand de gebeurtenislogboeken op de virtuele machine met behulp van Logboeken. U ziet dat extern bureaublad-Services, Terminal Server, is niet gestart of niet kan worden gestart. Het volgende logboek is een voorbeeld:

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

    U kunt ook de toegang tot de seriële Console-functie gebruiken om te zoeken voor deze fouten met de volgende query uit te voeren: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Oorzaak
 
Dit probleem treedt op omdat Extern bureaublad-Services niet wordt uitgevoerd op de virtuele machine. De oorzaak kan afhankelijk zijn van de volgende scenario's: 

- De Terminal Server-service is ingesteld op **uitgeschakelde**. 
- De Terminal Server-service is gecrasht of vastgelopen. 

## <a name="solution"></a>Oplossing

Om dit probleem wilt oplossen, moet u de seriële Console gebruiken. Of anders [herstel de virtuele machine offline](#repair-the-vm-offline) door het koppelen van de besturingssysteemschijf van de virtuele machine aan een virtuele machine voor herstel.

### <a name="use-serial-console"></a>Seriële Console gebruiken

1. Toegang tot de [seriële Console](serial-console-windows.md) hiervoor **ondersteuning en probleemoplossing** > **seriële console**. Als de functie is ingeschakeld op de virtuele machine, kunt u de virtuele machine is verbinden.

2. Maak een nieuw kanaal voor een CMD-exemplaar. Voer **CMD** naar het kanaal begint en naam van het kanaal.

3. Schakel over naar het kanaal dat u de CMD-exemplaar wordt uitgevoerd. In dit geval moet het kanaal 1:

   ```
   ch -si 1
   ```

4. Selecteer **Enter** opnieuw en voer een geldige gebruikersnaam en wachtwoord, lokaal of domein-ID voor de virtuele machine.

5. De status van de Terminal Server-service niet opvragen:

   ```
   sc query TermService
   ```

6. Als de status van de service **gestopt**, probeert de service te starten:

    ```
    sc start TermService
     ``` 

7. Een query in de service opnieuw om het ervoor te zorgen dat de service wordt gestart:

   ```
   sc query TermService
   ```
8. Als de service niet kan worden gestart, volgt u de oplossing op basis van de fout is weergegeven:

    |  Fout |  Suggestie |
    |---|---|
    |5 - TOEGANG IS GEWEIGERD |Zie [Terminal Server-service is gestopt vanwege een fout toegang geweigerd](#termService-service-is-stopped-because-of-an-access-denied-error). |
    |1058 - ERROR_SERVICE_DISABLED  |Zie [Terminal Server-service is uitgeschakeld](#termService-service-is-disabled).  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.|
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.    |
    |1070 - ERROR_SERVICE_START_HANG   | [Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.  |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Zie [Terminal Server-service is uitgeschakeld](#termService-service-is-disabled).  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost. |
    |1753   |[Neem contact op met ondersteuning voor](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.   |

#### <a name="termservice-service-is-stopped-because-of-an-access-denied-problem"></a>Terminal Server-service is gestopt vanwege een probleem met de toegang geweigerd

1. Verbinding maken met [seriële Console](serial-console-windows.md#) en open een PowerShell-exemplaar.
2. Download het hulpprogramma procesmonitor het volgende script uit te voeren:

   ```
   remove-module psreadline  
   $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
   $destination = "c:\temp\ProcessMonitor.zip" 
   $wc = New-Object System.Net.WebClient 
   $wc.DownloadFile($source,$destination) 
   ```

3. Nu beginnen met een **procmon** trace:

   ```
   procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
   ```

4. Reproduceer het probleem door het starten van de service die wordt aangeboden **toegang geweigerd**: 

   ```
   sc start TermService 
   ```

   Wanneer deze is mislukt, de trace-Monitor voor processen beëindigd:

   ```   
   procmon /Terminate 
   ```

5. Verzamelen van het bestand **c:\temp\ProcMonTrace.PML**. Open het met behulp van **procmon**. Vervolgens filteren op **resultaat is toegang geweigerd**, zoals weergegeven in de volgende schermafbeelding:

    ![Filteren op resultaat in proces bewaken](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Corrigeer de registersleutels, mappen of bestanden die zich op de uitvoer. Dit probleem wordt meestal veroorzaakt wanneer de aanmeldingsaccount die wordt gebruikt op de service beschikt niet over ACL-machtiging voor toegang tot deze objecten. Als u wilt weten de juiste ACL-machtiging voor het account aanmelden, kunt u controleren op een VM in orde. 

#### <a name="termservice-service-is-disabled"></a>Terminal Server-service is uitgeschakeld

1. Herstellen van de service op de standaardwaarde voor opstarten:

   ```
   sc config TermService start= demand 
   ```

2. Start de service:

   ```
   sc start TermService
   ```

3. Query uitvoeren op de status opnieuw om te controleren of dat de service wordt uitgevoerd:

   ```
   sc query TermService 
   ```

4. Probeer verbinding maken met virtuele machine met behulp van extern bureaublad.


### <a name="repair-the-vm-offline"></a>Herstel de virtuele machine offline

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>De besturingssysteemschijf koppelen aan een virtuele machine voor herstel

1. [De besturingssysteemschijf koppelen aan een virtuele machine voor herstel](../windows/troubleshoot-recovery-disks-portal.md).
2. Start een externe bureaubladverbinding met de virtuele machine voor herstel. Zorg ervoor dat de gekoppelde schijf is gemarkeerd als **Online** in de Schijfbeheer-console. Houd er rekening mee de stationsletter die toegewezen aan de gekoppelde besturingssysteemschijf.
3.  Open een opdrachtprompt met verhoogde bevoegdheid-exemplaar (**als administrator uitvoeren**). Voer het volgende script. We gaan ervan uit dat de stationsletter die toegewezen aan de gekoppelde besturingssysteemschijf **F**. Vervang deze door de juiste waarde in uw virtuele machine. 

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
   REM Set default values back on the broken service 
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
   ```

4. [De OS-schijf loskoppelen en opnieuw maken van de virtuele machine](../windows/troubleshoot-recovery-disks-portal.md). Controleer vervolgens of het probleem is opgelost.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem is opgelost.
