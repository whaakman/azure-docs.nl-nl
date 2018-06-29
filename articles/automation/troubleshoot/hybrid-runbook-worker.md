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
ms.openlocfilehash: 1cae7253a4bfcb4f83baf003a4d9d3c367d8f014
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063846"
---
# <a name="troubleshoot-hybrid-runbook-workers"></a>Problemen oplossen met hybride Runbook Workers

In dit artikel bevat informatie over het oplossen van problemen met Hybrid Runbook Workers.

## <a name="general"></a>Algemeen

De hybride Runbook Worker is afhankelijk van een agent om te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Deze agent is voor Windows, Microsoft Monitoring Agent. Voor Linux is de OMS-Agent voor Linux.

###<a name="runbook-execution-fails"></a>Scenario: De uitvoering van Runbook is mislukt.

#### <a name="issue"></a>Probleem

Runbook-uitvoering mislukt en u het volgende foutbericht:

```
"The job action 'Activate' cannot be run, because the process stopped unexpectedly. The job action was attempted three times."
```

Uw runbook is onderbroken kort na het uitvoeren van deze drie keer. Er zijn voorwaarden, die het runbook is voltooid, kunnen worden onderbroken en het bijbehorende foutbericht bevat geen aanvullende informatie die aangeeft waarom.

#### <a name="cause"></a>Oorzaak

Hier volgen de mogelijke oorzaken:

* De runbooks verifiëren niet met lokale bronnen

* De hybride worker zich achter een proxy of firewall

* De runbooks verifiëren niet met lokale bronnen

* De computer moeten worden uitgevoerd van de Hybrid Runbook Worker-functie voldoet aan de minimale hardwarevereisten.

#### <a name="resolution"></a>Oplossing

Controleer of dat de computer heeft uitgaande toegang tot de *.azure automation.net op poort 443.

Computers met de hybride Runbook Worker aan de minimale hardwarevereisten voldoen voordat u deze als host voor deze functie. Gebruik anders, afhankelijk van het Resourcegebruik van andere achtergrondprocessen en conflicten veroorzaakt door runbooks tijdens het uitvoeren van de computer wordt overbelast en ervoor zorgen dat de runbook-taak vertragingen of time-outs.

Controleer de computer moeten worden uitgevoerd van de Hybrid Runbook Worker-functie voldoet aan de minimale hardwarevereisten. Zo ja, monitor CPU en geheugen gebruik om te bepalen een correlatie tussen de prestaties van Hybrid Runbook Worker-processen en vensters. Als er geheugen of CPU-belasting, dit kan duiden op hoeft bij te werken of toevoegen van extra processors of geheugen voor het adres van de resource-knelpunt en los de fout te verhogen. Selecteer een andere berekeningsresource die de minimum vereisten en schaal ondersteunen kan wanneer werkbelasting eisen duiden op dat een verhoging is nodig.

Controleer de **Microsoft SMA** gebeurtenislogboek voor een overeenkomstige gebeurtenis met beschrijving *Win32 proces is afgesloten met code [4294967295]*. De oorzaak van deze fout is u dat nog niet hebt geconfigureerd verificatie in uw runbooks of de Run As-referenties voor de Hybrid worker-groep is opgegeven. Bekijk [Runbook-machtigingen](../automation-hrw-run-runbooks.md#runbook-permissions) om te bevestigen dat u verificatie juist hebt geconfigureerd voor uw runbooks.

## <a name="linux"></a>Linux

De Linux hybride Runbook Worker is afhankelijk van de OMS-Agent voor Linux om te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:

###<a name="oms-agent-not-running"></a>Scenario: De OMS-Agent voor Linux wordt niet uitgevoerd.

Als de OMS-Agent voor Linux wordt niet uitgevoerd, dit voorkomt dat de Linux hybride Runbook Worker communicatie met Azure Automation. Controleer of de agent wordt uitgevoerd met de volgende opdracht: `ps -ef | grep python`. Ziet u uitvoer ziet er als volgt, de python-processen met **nxautomation** gebruikersaccount. Als de oplossingen updatebeheer of Azure Automation zijn niet ingeschakeld, wordt geen van de volgende processen worden uitgevoerd.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

De volgende lijst bevat de processen die worden gestart voor een Linux hybride Runbook Worker. Ze bevinden de `/var/opt/microsoft/omsagent/state/automationworker/` directory.

* **OMS.conf** -dit is het werkproces manager kunt u dit rechtstreeks vanaf DSC is gestart.

* **worker.conf** -dit proces is het werkproces automatisch geregistreerd hybride, het is gestart door de manager van de werknemer. Dit proces wordt gebruikt door updatebeheer en is transparant voor de gebruiker. Dit proces is niet aanwezig als de oplossing voor beheer van de Update niet is ingeschakeld op de machine.

* **diy/worker.conf** -dit proces is het werkproces ZELFOPLOSSING hybride. Het werkproces ZELFOPLOSSING hybride wordt gebruikt voor het uitvoeren van runbooks van de gebruiker op de hybride Runbook Worker. Deze alleen verschilt van de automatische hybride werkproces geregistreerd in de belangrijkste details die is gebruikt een andere configuratie. Dit proces is niet aanwezig als de Azure Automation-oplossing is niet ingeschakeld en de ZELFOPLOSSING Linux hybride Worker is niet geregistreerd.

Als de OMS-Agent voor Linux niet actief is, voer de volgende opdracht om de service te starten: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

###<a name="class-does-not-exist"></a>Scenario: De opgegeven klasse bestaat niet

Als u de volgende fout: **de opgegeven klasse bestaat niet...** in de `/var/opt/microsoft/omsconfig/omsconfig.log` en vervolgens de OMS-Agent voor Linux moet worden bijgewerkt. Voer de volgende opdracht om de OMS-Agent opnieuw installeren:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

## <a name="windows"></a>Windows

De Windows hybride Runbook Worker is afhankelijk van de Microsoft Monitoring Agent te communiceren met uw Automation-account voor het registreren van de werknemer, ontvangen van runbooktaken en status rapporteren. Als de registratie van de werknemer is mislukt, volgen hier enkele mogelijke oorzaken voor de fout:

###<a name="mma-not-running"></a>Scenario: Microsoft Monitoring Agent wordt niet uitgevoerd.

#### <a name="issue"></a>Probleem

De `healthservice` service wordt niet uitgevoerd op de hybride Runbook Worker-machine.

#### <a name="cause"></a>Oorzaak

Als de Microsoft Monitoring Agent Windows-service niet wordt uitgevoerd, dit voorkomt dat de hybride Runbook Worker communicatie met Azure Automation.

#### <a name="resolution"></a>Oplossing

Controleer of de agent wordt uitgevoerd met de volgende opdracht in PowerShell: `Get-Service healthservice`. Als de service wordt gestopt, voert u de volgende opdracht in PowerShell om de service te starten: `Start-Service healthservice`.

###<a name="event-4502"></a> Gebeurtenis 4502 in Operations Manager-logboek

#### <a name="issue"></a>Probleem

In de **toepassingen en Services Logs\Operations Manager** gebeurtenislogboek, ziet u gebeurtenis 4502 en EventMessage met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**met de volgende beschrijving: *het certificaat dat is doorgegeven door de service \<wsid\>. oms.opinsights.azure.com is niet uitgegeven door een certificeringsinstantie die wordt gebruikt voor Microsoft-services. Neem contact op met de netwerkbeheerder om te controleren of er een proxy die TLS/SSL-communicatie wordt onderschept worden uitgevoerd. Artikel KB3126513 bevat extra informatie over probleemoplossing voor problemen met de netwerkverbinding.*

#### <a name="cause"></a>Oorzaak

Dit kan worden veroorzaakt door uw proxy- of -firewall blokkeert communicatie met Microsoft Azure. Controleer of dat de computer heeft uitgaande toegang tot de *.azure automation.net op poort 443.

#### <a name="resolution"></a>Oplossing

Logboeken worden lokaal opgeslagen op elke worker hybride op C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes. U kunt controleren of er zijn waarschuwingen of fouten die zijn geschreven naar de **toepassingen en Services Logs\Microsoft-SMA\Operations** en **toepassingen en Services Logs\Operations Manager** in het gebeurtenislogboek die duidt dit op een verbindings- of andere probleem met betrekking tot de voorbereiding van de rol aan Azure Automation of probleem tijdens het normale bewerkingen uitvoeren.

[Runbook uitvoer en berichten](../automation-runbook-output-and-messages.md) worden verzonden naar Azure Automation van hybrid workers net als runbooktaken uitvoeren in de cloud. Ook kunt u de stromen uitgebreid en voortgang van de dezelfde manier als voor andere runbooks.

## <a name="next-steps"></a>Volgende stappen

Als u hebt uw probleem niet wordt weergegeven of niet uw probleem kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een incident voor ondersteuning van Azure kunt opslaan. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
