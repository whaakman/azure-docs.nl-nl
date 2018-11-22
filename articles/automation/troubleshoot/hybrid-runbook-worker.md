---
title: Probleemoplossing - Azure Automation Hybrid Runbook Workers
description: Dit artikel bevat informatie die het oplossen van Azure Automation Hybrid Runbook Workers
services: automation
ms.service: automation
ms.component: ''
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 38e2589365c2f1c88145fbf068d3ed267d4a4621
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284560"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Hybrid Runbook Workers oplossen

In dit artikel bevat informatie over het oplossen van problemen met Hybrid Runbook Workers.

## <a name="general"></a>Algemeen

De Hybrid Runbook Worker is afhankelijk van een agent kan communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Deze agent is voor Windows, de Microsoft Monitoring Agent. Voor Linux is de OMS-Agent voor Linux.

### <a name="runbook-execution-fails"></a>Scenario: De uitvoering van Runbook is mislukt.

#### <a name="issue"></a>Probleem

Uitvoering van Runbook is mislukt en u het volgende foutbericht:

```error
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Uw runbook is onderbroken kort na het uitvoeren van deze drie keer. Er zijn die het runbook voltooid kunnen worden onderbroken en het bijbehorende foutbericht bevat geen eventuele aanvullende informatie die aangeeft waarom.

#### <a name="cause"></a>Oorzaak

Hier volgen mogelijke oorzaken:

* De runbooks verifiëren niet met lokale bronnen

* De hybrid worker zich achter een proxy of firewall

* De runbooks verifiëren niet met lokale bronnen

* De computer die is aangewezen om uit te voeren van de functie Hybrid Runbook Worker voldoet aan de minimale hardwarevereisten.

#### <a name="resolution"></a>Oplossing

Controleer of dat de computer heeft uitgaande toegang tot *.azure-automation.net op poort 443.

Computers met de Hybrid Runbook Worker moeten voldoen aan de minimale hardwarevereisten voldoet voordat u deze als host voor deze functie aanwijst. Anders, afhankelijk van het Resourcegebruik van andere achtergrondprocessen en conflicten veroorzaakt door runbooks tijdens het uitvoeren van de computer wordt overbelast en wordt ervoor zorgen dat de runbook-taak vertragingen of time-outs.

Controleer of de computer die is aangewezen om uit te voeren van de functie Hybrid Runbook Worker voldoet aan de minimale hardwarevereisten. Als dit het geval, bewaken gebruik van de CPU en geheugen om te bepalen van een correlatie tussen de prestaties van Hybrid Runbook Worker-processen en Windows. Als er geheugen of CPU druk te verlichten, kan dit duiden op de noodzaak om te upgraden of extra processors toevoegen of meer geheugen voor het adres van het knelpunt resource en los de fout. Of Selecteer een andere compute-resource die ondersteunen kan de minimale vereisten en schaal wanneer werklastvraag geven dat een toename van de nodig is.

Controleer de **Microsoft SMA** gebeurtenislogboek voor een overeenkomende gebeurtenis met beschrijving *Win32 proces is afgesloten met code [4294967295]*. De oorzaak van deze fout is u dit nog niet hebt geconfigureerd verificatie in runbooks uitvoert of de uitvoeren als-referenties voor de Hybrid worker-groep is opgegeven. Beoordeling [runbookmachtigingen](../automation-hrw-run-runbooks.md#runbook-permissions) om te bevestigen u verificatie juist hebt geconfigureerd voor uw runbooks.

## <a name="linux"></a>Linux

De Hybrid Runbook Worker in Linux is afhankelijk van de OMS-Agent voor Linux om te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:

### <a name="oms-agent-not-running"></a>Scenario: De OMS-Agent voor Linux wordt niet uitgevoerd.

Als de OMS-Agent voor Linux wordt niet uitgevoerd, dit voorkomt dat de Hybrid Runbook Worker in Linux communicatie met Azure Automation. Controleer of de agent wordt uitgevoerd met de volgende opdracht: `ps -ef | grep python`. U ziet de uitvoer is vergelijkbaar met het volgende, het python-processen met **nxautomation** gebruikersaccount. Als de Update Management of Azure Automation-oplossingen zijn niet ingeschakeld, wordt geen van de volgende processen worden uitgevoerd.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

De volgende lijst bevat de processen die worden gestart voor een Hybrid Runbook Worker in Linux. Ze zich al de `/var/opt/microsoft/omsagent/state/automationworker/` directory.

* **OMS.conf** -dit is het werkproces manager, deze rechtstreeks vanuit DSC wordt gestart.

* **worker.conf** -dit proces is het werkproces automatisch geregistreerd hybride, deze is gestart door de manager van de werknemer. Dit proces wordt gebruikt door beheer van updates en is transparant voor de gebruiker. Dit proces is niet aanwezig zijn als de oplossing Update Management is niet ingeschakeld op de machine.

* **diy/worker.conf** -dit proces is het proces van de werknemer zelf hybride. Het werkproces zelf hybride wordt gebruikt voor het uitvoeren van runbooks van de gebruiker op de Hybrid Runbook Worker. Alleen wijkt af van de automatische hybride werkproces geregistreerd in de belangrijkste details die u gebruikt een andere configuratie. Dit proces is niet aanwezig zijn als de Azure Automation-oplossing niet is ingeschakeld en de en ZELFGEMAAKT Linux Hybrid Worker is niet geregistreerd.

Als de OMS-Agent voor Linux wordt niet uitgevoerd, de volgende opdracht uitvoeren om de service te starten: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="class-does-not-exist"></a>Scenario: De opgegeven klasse bestaat niet

Als u een foutbericht weergegeven: **de opgegeven klasse bestaat niet...** in de `/var/opt/microsoft/omsconfig/omsconfig.log` en vervolgens de OMS-Agent voor Linux moet worden bijgewerkt. Voer de volgende opdracht om de OMS-Agent opnieuw te installeren:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

De Hybrid Runbook Worker voor Windows, is afhankelijk van de Microsoft Monitoring Agent om te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:

### <a name="mma-not-running"></a>Scenario: De Microsoft Monitoring Agent wordt niet uitgevoerd.

#### <a name="issue"></a>Probleem

De `healthservice` service wordt niet uitgevoerd op de Hybrid Runbook Worker-machine.

#### <a name="cause"></a>Oorzaak

Als de Microsoft Monitoring Agent Windows-service wordt niet uitgevoerd, dit voorkomt dat de Hybrid Runbook Worker communicatie met Azure Automation.

#### <a name="resolution"></a>Oplossing

Controleer of de agent wordt uitgevoerd met de volgende opdracht in PowerShell: `Get-Service healthservice`. Als de service is gestopt, voert u de volgende opdracht uit in PowerShell om de service te starten: `Start-Service healthservice`.

### <a name="event-4502"></a> Gebeurtenis 4502 in Operations Manager-logboek

#### <a name="issue"></a>Probleem

In de **toepassings- en servicelogboeken\operations Manager** gebeurtenislogboek, ziet u gebeurtenis 4502 en EventMessage met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**met de volgende beschrijving: *het certificaat dat door de service \<wsid\>. oms.opinsights.azure.com is niet uitgegeven door een certificeringsinstantie die wordt gebruikt voor Microsoft-services. Neem contact op met de netwerkbeheerder om te controleren of er een proxy waarmee TLS/SSL-communicatie wordt uitgevoerd. Artikel KB3126513 bevat extra informatie over probleemoplossing bij verbindingsproblemen.*

#### <a name="cause"></a>Oorzaak

Dit kan worden veroorzaakt door uw proxy- of firewall blokkeren communicatie met Microsoft Azure. Controleer of dat de computer heeft uitgaande toegang tot *.azure-automation.net op poort 443.

#### <a name="resolution"></a>Oplossing

Logboeken worden lokaal opgeslagen op elke hybrid worker op C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. U kunt controleren of er zijn waarschuwingen of fouten die zijn geschreven naar de **toepassingen en Services Logs\Microsoft-SMA\Operations** en **toepassings- en servicelogboeken\operations Manager** in het gebeurtenislogboek die duidt een verbinding of een ander probleem dat die betrekking hebben op de onboarding van de rol van Azure Automation of probleem tijdens het uitvoeren van normale bewerkingen.

[Runbook-uitvoer en berichten](../automation-runbook-output-and-messages.md) worden verzonden naar Azure Automation van hybride werknemers net als de runbook-taken uitvoeren in de cloud. Ook kunt u de stromen uitgebreid en voortgang van de dezelfde manier als voor andere runbooks.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet wordt gezien of kan niet aan het oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
