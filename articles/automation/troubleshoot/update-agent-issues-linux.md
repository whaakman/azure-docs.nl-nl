---
title: Inzicht in de resultaten van Linux-agent in beheer van Azure-updates
description: Informatie over het oplossen van problemen met de updatebeheer-agent.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 11/06/2018
ms.topic: conceptual
ms.service: automation
ms.component: update-management
manager: carmonm
ms.openlocfilehash: 028a06a7fb627fd69bdd2f0a2084bbdef11eaed4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53077236"
---
# <a name="understand-the-linux-agent-check-results-in-update-management"></a>Inzicht in de resultaten van Linux-agent in beheer van updates

Mogelijk zijn er veel oorzaken waardoor uw Azure-machine wordt niet weergegeven **gereed** in beheer van updates. U kunt de status van een Hybrid Worker-agent om te bepalen van het onderliggende probleem controleren in Update Management. In dit artikel wordt beschreven hoe u de probleemoplosser uitvoert vanuit de Azure-portal en in offlinescenario's.

## <a name="start-the-troubleshooter"></a>De probleemoplosser voor starten

Door te klikken op de **oplossen** koppeling onder de **gereedheid voor Update-Agent** kolom in de portal, start u de **Update-Agent oplossen** pagina. Deze pagina ziet u problemen met de agent en een koppeling naar dit artikel helpt u bij het oplossen van uw problemen.

![de pagina van de VM-lijst](../media/update-agent-issues-linux/vm-list.png)

> [!NOTE]
> De controles moeten de virtuele machine worden uitgevoerd. Als de virtuele machine wordt niet uitgevoerd krijgt u een knop **Start de virtuele machine**.

Op de **Update-Agent oplossen** pagina, klikt u op **uitvoeren controleert**, de probleemoplosser voor starten. Maakt gebruik van de probleemoplosser voor [RunCommand-](../../virtual-machines/linux/run-command.md) een script uitvoeren op de machine om te controleren of de afhankelijkheden die de agent heeft. Wanneer de probleemoplosser voor voltooid is, wordt het resultaat van de controles.

![Pagina oplossen](../media/update-agent-issues-linux/troubleshoot-page.png)

Als u klaar bent, worden de resultaten worden geretourneerd in het venster. De [Controleer gedeelte](#pre-requisistes-checks) bevatten informatie over wat elke controle zoekt.

![Update-agent controleert op pagina](../media/update-agent-issues-linux/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Controles van vereisten

### <a name="operating-system"></a>Besturingssysteem

De OS-controle alleen gecontroleerd of de Hybrid Runbook Worker wordt uitgevoerd een van de volgende besturingssystemen:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|CentOS 6 (x86/x64) en 7 (x64)      | Linux-agents moeten toegang hebben tot een opslagplaats voor updates. Patch toepassen op classificatie gebaseerde vereist 'yum' om terug te keren beveiligingsgegevens die CentOS heeft geen buiten het vak.         |
|Red Hat Enterprise 6 (x86/x64) en 7 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Ubuntu 14.04 LTS, 16.04 LTS en 18.04 LTS (x86/x64)      |Linux-agents moeten toegang hebben tot een opslagplaats voor updates.         |

## <a name="monitoring-agent-service-health-checks"></a>Monitoring agent-service-statuscontroles

### <a name="oms-agent"></a>OMS-agent

Deze controles zorgt ervoor dat de OMS-Agent voor Linux is geïnstalleerd. Zie voor instructies over hoe u deze installeert, [de agent voor Linux installeren](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux
).

### <a name="oms-agent-status"></a>Status van de OMS-Agent

Deze controle zorgt ervoor dat de OMS-Agent voor Linux wordt uitgevoerd. U kunt de volgende opdracht uit om te proberen opnieuw te starten uitvoeren als de agent wordt niet uitgevoerd. Zie voor meer informatie over het oplossen van de agent [Linux Hybrid Runbook worker oplossen](hybrid-runbook-worker.md#linux)

```bash
sudo /opt/microsoft/omsagent/bin/service_control restart
```

### <a name="multihoming"></a>Multihoming

Deze controle wordt bepaald of de agent is rapporteert aan meerdere werkruimten. Multihoming wordt niet ondersteund door beheer van updates.

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker

Hiermee wordt gecontroleerd om te controleren of dat de OMS-Agent voor Linux is de Hybrid Runbook Worker-pakket. Dit pakket is vereist voor het beheer van updates om te werken.

### <a name="hybrid-runbook-worker-status"></a>Hybrid Runbook Worker-status

Deze controle zorgt ervoor dat de Hybrid Runbook Worker wordt uitgevoerd op de machine. De volgende processen moet aanwezig zijn als de Hybrid Runbook Worker correct wordt uitgevoerd. Zie voor meer informatie, [probleemoplossing van de Log Analytics-Agent voor Linux](hybrid-runbook-worker.md#oms-agent-not-running).

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

## <a name="connectivity-checks"></a>Controles voor connectiviteit

### <a name="general-internet-connectivity"></a>Algemene verbinding met internet

Deze controle zorgt ervoor dat de computer toegang tot internet heeft.

### <a name="registration-endpoint"></a>Eindpunt voor clientregistratie

Deze controle bepaalt als de agent correct met de agent-service communiceren kan.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met het eindpunt voor clientregistratie toestaan. Zie voor een lijst van adressen en poorten te openen, [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="operations-endpoint"></a>Operations-eindpunt

Deze controle bepaalt als de agent correct met de taak-Runtime-gegevens-Service communiceren kan.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met de taak Runtime Data-Service toestaan. Zie voor een lijst van adressen en poorten te openen, [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning)

### <a name="log-analytics-endpoint-1"></a>Log Analytics-eindpunt 1

Deze controle alleen gecontroleerd dat de computer toegang tot de eindpunten die nodig zijn voor de Log Analytics-agent heeft.

### <a name="log-analytics-endpoint-2"></a>Log Analytics-eindpunt 2

Deze controle alleen gecontroleerd dat de computer toegang tot de eindpunten die nodig zijn voor de Log Analytics-agent heeft.

### <a name="log-analytics-endpoint-3"></a>Log Analytics-eindpunt 3

Deze controle alleen gecontroleerd dat de computer toegang tot de eindpunten die nodig zijn voor de Log Analytics-agent heeft.

## <a name="troubleshoot-offline"></a>Offline oplossen

U kunt de probleemoplosser voor offline gebruiken op een Hybrid Runbook Worker door het script lokaal worden uitgevoerd. Het python-script [update_mgmt_health_check.py](https://gallery.technet.microsoft.com/scriptcenter/Troubleshooting-utility-3bcbefe6) kunt u vinden in de Script Center. Een voorbeeld van de uitvoer van dit script wordt weergegeven in het volgende voorbeeld:

```output
Debug: Machine Information:   Static hostname: LinuxVM2
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 00000000000000000000000000000000
           Boot ID: 00000000000000000000000000000000
    Virtualization: microsoft
  Operating System: Ubuntu 16.04.5 LTS
            Kernel: Linux 4.15.0-1025-azure
      Architecture: x86-64


Passed: Operating system version is supported

Passed: Microsoft Monitoring agent is installed

Debug: omsadmin.conf file contents:
        WORKSPACE_ID=00000000-0000-0000-0000-000000000000
        AGENT_GUID=00000000-0000-0000-0000-000000000000
        LOG_FACILITY=local0
        CERTIFICATE_UPDATE_ENDPOINT=https://00000000-0000-0000-0000-000000000000.oms.opinsights.azure.com/ConfigurationService.Svc/RenewCertificate
        URL_TLD=opinsights.azure.com
        DSC_ENDPOINT=https://scus-agentservice-prod-1.azure-automation.net/Accou            nts/00000000-0000-0000-0000-000000000000/Nodes\(AgentId='00000000-0000-0000-0000-000000000000'\)
        OMS_ENDPOINT=https://00000000-0000-0000-0000-000000000000.ods.opinsights            .azure.com/OperationalData.svc/PostJsonDataItems
        AZURE_RESOURCE_ID=/subscriptions/00000000-0000-0000-0000-000000000000/re            sourcegroups/myresourcegroup/providers/microsoft.compute/virtualmachines/linuxvm            2
        OMSCLOUD_ID=0000-0000-0000-0000-0000-0000-00
        UUID=00000000-0000-0000-0000-000000000000


Passed: Microsoft Monitoring agent is running

Passed: Machine registered with log analytics workspace:['00000000-0000-0000-0000-000000000000']

Passed: Hybrid worker package is present

Passed: Hybrid worker is running

Passed: Machine is connected to internet

Passed: TCP test for {scus-agentservice-prod-1.azure-automation.net} (port 443)             succeeded

Passed: TCP test for {eus2-jobruntimedata-prod-su1.azure-automation.net} (port 4            43) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.ods.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {00000000-0000-0000-0000-000000000000.oms.opinsights.azure.            com} (port 443) succeeded

Passed: TCP test for {ods.systemcenteradvisor.com} (port 443) succeeded

```

## <a name="next-steps"></a>Volgende stappen

Aanvullende problemen met de Hybrid Runbook Workers, Zie [problemen oplossen - Hybrid Runbook Workers](hybrid-runbook-worker.md)
