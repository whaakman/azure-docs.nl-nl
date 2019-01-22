---
title: Probleemoplossing - Azure Automation Hybrid Runbook Workers
description: Dit artikel bevat informatie die het oplossen van Azure Automation Hybrid Runbook Workers
services: automation
ms.service: automation
ms.subservice: ''
author: georgewallace
ms.author: gwallace
ms.date: 12/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9f83a0cf97acfd0bed990cc832ac08eb23c29ef1
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54434455"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Hybrid Runbook Workers oplossen

In dit artikel bevat informatie over het oplossen van problemen met Hybrid Runbook Workers.

## <a name="general"></a>Algemeen

De Hybrid Runbook Worker is afhankelijk van een agent kan communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Deze agent is voor Windows, de Microsoft Monitoring Agent. Voor Linux is de OMS-Agent voor Linux.

### <a name="runbook-execution-fails"></a>Scenario: Uitvoering van Runbook is mislukt

#### <a name="issue"></a>Probleem

Uitvoering van Runbook is mislukt en u het volgende foutbericht:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Uw runbook wordt onderbroken, kort nadat deze probeert uit te voeren deze drie keer. Er zijn die het runbook uitvoeren kunnen worden onderbroken. Als dit gebeurt, kan het bijbehorende foutbericht aanvullende informatie die waarom aangeeft niet bevatten.

#### <a name="cause"></a>Oorzaak

Hier volgen mogelijke oorzaken:

* De runbooks verifiëren niet met lokale bronnen

* De hybrid worker zich achter een proxy of firewall

* De runbooks verifiëren niet met lokale bronnen

* De computer die is geconfigureerd voor het uitvoeren van de functie Hybrid Runbook Worker voldoet aan de minimale hardwarevereisten.

#### <a name="resolution"></a>Oplossing

Controleer of dat de computer heeft uitgaande toegang tot *.azure-automation.net op poort 443.

Computers met de Hybrid Runbook Worker dient te voldoen aan de minimale hardwarevereisten voldoet voordat deze is geconfigureerd voor het hosten van deze functie. Runbooks en de achtergrondprocessen die ze gebruiken kunnen ertoe leiden dat het systeem wordt overbelast en ervoor zorgen dat de runbook-taak vertragingen of time-outs.

Controleer of de computer met de functie Hybrid Runbook Worker voldoet aan de minimale hardwarevereisten. Als dit het geval is, worden monitor CPU en geheugen gebruiken om te bepalen een correlatie tussen de prestaties van Hybrid Runbook Worker-processen en Windows. Als er geheugen of CPU druk te verlichten, kan dit duiden op de noodzaak om bij te werken van resources. U kunt ook een andere compute-resource die ondersteunen kan de minimale vereisten en schaal wanneer werklastvraag geven een toename van de benodigde selecteren.

Controleer de **Microsoft SMA** gebeurtenislogboek voor een overeenkomende gebeurtenis met beschrijving *Win32 proces is afgesloten met code [4294967295]*. De oorzaak van deze fout is u dit nog niet hebt geconfigureerd verificatie in runbooks uitvoert of de uitvoeren als-referenties voor de Hybrid worker-groep is opgegeven. Beoordeling [runbookmachtigingen](../automation-hrw-run-runbooks.md#runbook-permissions) om te bevestigen u verificatie juist hebt geconfigureerd voor uw runbooks.

### <a name="no-cert-found"></a>Scenario: Er is geen certificaat gevonden in het certificaatarchief op Hybrid Runbook Worker

#### <a name="issue"></a>Probleem

Een runbook uitgevoerd op een Hybrid Runbook Worker is mislukt met de volgende strekking weergegeven:

```error
Connect-AzureRmAccount : No certificate was found in the certificate store with thumbprint 0000000000000000000000000000000000000000
At line:3 char:1
+ Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -Appl ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Connect-AzureRmAccount], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.ConnectAzureRmAccountCommand
```

#### <a name="cause"></a>Oorzaak

Deze fout treedt op wanneer u probeert te gebruiken een [Run As-Account](../manage-runas-account.md) in een runbook dat wordt uitgevoerd op een Hybrid Runbook Worker die het certificaat Run As-Account van indien niet aanwezig zijn. Hybrid Runbook Workers hebben niet lokaal het certificaatasset standaard, die nodig is voor het uitvoeren als-Account te laten functioneren.

#### <a name="resolution"></a>Oplossing

Als uw Hybrid Runbook Worker een Azure-VM is, kunt u [beheerde identiteiten voor een Azure-Resources](../automation-hrw-run-runbooks.md#managed-identities-for-azure-resources) in plaats daarvan. In dit scenario kunt u verifiëren met Azure-resources met behulp van de beheerde identiteit van de virtuele machine van Azure in plaats van het Run As-Account, verificatie te vereenvoudigen. Wanneer de Hybrid Runbook Worker een on-premises machine is, moet u het Run As-Account-certificaat installeren op de machine. Voor informatie over het installeren van het certificaat, Zie de stappen om uit te voeren de [Export-RunAsCertificateToHybridWorker](../automation-hrw-run-runbooks.md#runas-script) runbook.

## <a name="linux"></a>Linux

De Hybrid Runbook Worker in Linux is afhankelijk van de OMS-Agent voor Linux om te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:

### <a name="oms-agent-not-running"></a>Scenario: De OMS-Agent voor Linux wordt niet uitgevoerd


Als de OMS-Agent voor Linux niet wordt uitgevoerd, kunnen de Hybrid Runbook Worker in Linux niet communiceren met Azure Automation. Controleer of de agent wordt uitgevoerd met de volgende opdracht: `ps -ef | grep python`. U ziet de uitvoer is vergelijkbaar met het volgende, het python-processen met **nxautomation** gebruikersaccount. Als de Update Management of Azure Automation-oplossingen zijn niet ingeschakeld, wordt geen van de volgende processen worden uitgevoerd.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

De volgende lijst bevat de processen die worden gestart voor een Hybrid Runbook Worker in Linux. Ze zich al in de `/var/opt/microsoft/omsagent/state/automationworker/` directory.


* **OMS.conf** -deze waarde is het werkproces van manager. Het rechtstreeks vanaf DSC gestart.

* **worker.conf** -dit proces is het werkproces automatisch geregistreerd hybride, deze is gestart door de manager van de werknemer. Dit proces wordt gebruikt door beheer van updates en is transparant voor de gebruiker. Dit proces is niet aanwezig als de oplossing Update Management is niet ingeschakeld op de machine.

* **diy/worker.conf** -dit proces is het proces van de werknemer zelf hybride. Het werkproces zelf hybride wordt gebruikt voor het uitvoeren van runbooks van de gebruiker op de Hybrid Runbook Worker. Alleen wijkt af van de automatische hybride werkproces geregistreerd in de belangrijkste details die u gebruikt een andere configuratie. Dit proces is niet aanwezig zijn als de Azure Automation-oplossing is uitgeschakeld en de en ZELFGEMAAKT Linux Hybrid Worker is niet geregistreerd.

Als de OMS-Agent voor Linux wordt niet uitgevoerd, de volgende opdracht uitvoeren om de service te starten: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Scenario: De opgegeven klasse bestaat niet

Als u de volgende fout ziet: **De opgegeven klasse bestaat niet...** in de `/var/opt/microsoft/omsconfig/omsconfig.log` en vervolgens de OMS-Agent voor Linux moet worden bijgewerkt. Voer de volgende opdracht om de OMS-Agent opnieuw te installeren:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

De Hybrid Runbook Worker voor Windows, is afhankelijk van de Microsoft Monitoring Agent om te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:

### <a name="mma-not-running"></a>Scenario: De Microsoft Monitoring Agent wordt niet uitgevoerd

#### <a name="issue"></a>Probleem

De `healthservice` service niet actief is op de Hybrid Runbook Worker-machine.

#### <a name="cause"></a>Oorzaak

Als de Microsoft Monitoring Agent Windows-service niet actief is, deze status wordt voorkomen dat de Hybrid Runbook Worker communicatie met Azure Automation.

#### <a name="resolution"></a>Oplossing

Controleer of de agent wordt uitgevoerd met de volgende opdracht in PowerShell: `Get-Service healthservice`. Als de service is gestopt, voert u de volgende opdracht uit in PowerShell om de service te starten: `Start-Service healthservice`.

### <a name="event-4502"></a> Gebeurtenis 4502 in Operations Manager-logboek

#### <a name="issue"></a>Probleem

In de **toepassings- en servicelogboeken\operations Manager** gebeurtenislogboek, ziet u gebeurtenis 4502 en EventMessage met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**met de volgende beschrijving: *Het certificaat dat door de service \<wsid\>. oms.opinsights.azure.com is niet uitgegeven door een certificeringsinstantie die wordt gebruikt voor Microsoft-services. Neem contact op met de netwerkbeheerder om te controleren of er een proxy waarmee TLS/SSL-communicatie wordt uitgevoerd. Artikel KB3126513 bevat extra informatie over probleemoplossing bij verbindingsproblemen.*

#### <a name="cause"></a>Oorzaak

Dit probleem kan worden veroorzaakt door uw proxy- of firewall blokkeren communicatie met Microsoft Azure. Controleer of dat de computer heeft uitgaande toegang tot *.azure-automation.net op poort 443.

#### <a name="resolution"></a>Oplossing

Logboeken worden lokaal opgeslagen op elke hybrid worker op C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. U kunt controleren of er zijn geen waarschuwingen of fouten gebeurtenissen in de **toepassingen en Services Logs\Microsoft-SMA\Operations** en **toepassings- en servicelogboeken\operations Manager** gebeurtenislogboek dat zou duiden op een verbindings- of ander probleem dat betrekking heeft op de onboarding van de rol voor Azure Automation of probleem terwijl onder normale bewerkingen.

[Runbook-uitvoer en berichten](../automation-runbook-output-and-messages.md) worden verzonden naar Azure Automation van hybride werknemers, net zoals die worden uitgevoerd in de cloud runbooktaken. Ook kunt u de stromen uitgebreid en voortgang van de dezelfde manier als voor andere runbooks.

### <a name="corrupt-cache"></a> Hybrid Runbook Worker niet rapporteren

#### <a name="issue"></a>Probleem

De Hybrid Runbook Worker-machine wordt uitgevoerd, maar er geen heartbeat-gegevens voor de machine in de werkruimte.

De volgende voorbeeldquery laat zien van de machines in een werkruimte en de laatste heartbeat:

```loganalytics
// Last heartbeat of each computer
Heartbeat 
| summarize arg_max(TimeGenerated, *) by Computer
```

#### <a name="cause"></a>Oorzaak

Dit probleem kan worden veroorzaakt door een beschadigd cache op de Hybrid Runbook Worker.

#### <a name="resolution"></a>Oplossing

U lost dit probleem, zich aanmelden bij de Hybrid Runbook Worker en voer het volgende script. Met dit script stopt de Microsoft Monitoring Agent, verwijdert u de cache en start de service opnieuw. Deze actie zorgt ervoor dat de Hybrid Runbook Worker te downloaden de configuratie van Azure Automation.

```powershell
Stop-Service -Name HealthService

Remove-Item -Path 'C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State' -Recurse

Start-Service -Name HealthService
```

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

